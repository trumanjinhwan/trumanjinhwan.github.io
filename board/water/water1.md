---
layout: post
title: "1. 수질오염원 데이터 처리"
date: 2025-04-29
---

# 1. 수질오염원 데이터 수집(Pandas, DB)

<div style="text-align: center;">
  <img src="/사진들/water/수질오염원csv.png" alt="" />
</div>

프로젝트의 범위를 강원도 춘천시의 "소양강"으로 제한했기 때문에 도로명 주소가 "강원도"만 들어가는 데이터들을 필터링합니다.

```python
import pandas as pd
import pymysql
import os

# ✅ CSV 파일 불러오기
df = pd.read_csv(os.path.join(os.path.dirname(__file__), "data", "수질오염원_정보(2023년11월).csv"), encoding="cp949")

# ✅ 필요한 열만 추출
columns_needed = ["bsnm_nm", "induty_nm", "bsns_detail_road_addr", "web_bplc_x_katec", "web_bplc_y_katec"]
df = df[columns_needed]

# ✅ "강원도 춘천시" 데이터만 필터링
df = df[df["bsns_detail_road_addr"].str.contains("강원도", na=False)]

# ✅ NaN 값이 있는 행 제거
df = df.dropna(subset=['web_bplc_x_katec', 'web_bplc_y_katec'])

# NaN 값 확인
print(df.isnull().sum())

# NaN 값을 0으로 대체
df['web_bplc_x_katec'].fillna(0, inplace=True)
df['web_bplc_y_katec'].fillna(0, inplace=True)

# ✅ MySQL 연결 정보
host = "www.lifeslike.org"    # MySQL 서버 주소
user = "water"                # MySQL 사용자명
password = "water1111"        # MySQL 비밀번호
database = "weather_db"       # 사용할 데이터베이스
charset = "utf8mb4"

# ✅ MySQL 연결
conn = pymysql.connect(host=host, user=user, password=password, database=database, charset=charset, use_unicode=True)
cursor = conn.cursor()

# ✅ 테이블 생성 쿼리 (존재하지 않으면 생성)
create_table_sql = """
CREATE TABLE IF NOT EXISTS pollution_sources (
    id INT AUTO_INCREMENT PRIMARY KEY,
    bsnm_nm VARCHAR(255),
    induty_nm VARCHAR(255),
    bsns_detail_road_addr VARCHAR(500),
    web_bplc_x_katec DOUBLE,
    web_bplc_y_katec DOUBLE
);
"""

cursor.execute(create_table_sql)
conn.commit()

# ✅ 데이터 삽입 쿼리
insert_sql = """
INSERT INTO pollution_sources (bsnm_nm, induty_nm, bsns_detail_road_addr, web_bplc_x_katec, web_bplc_y_katec)
VALUES (%s, %s, %s, %s, %s)
"""
# ✅ 데이터프레임을 튜플 리스트로 변환 후 삽입
data_to_insert = df.values.tolist()
cursor.executemany(insert_sql, data_to_insert)
  
# ✅ 변경사항 저장
conn.commit()

print(f"✅ 테이블 'pollution_sources' 생성 및 {len(data_to_insert)}개 데이터 삽입 완료!")

# ✅ 연결 종료
cursor.close()
conn.close()
```

<br>

그 다음에 파이썬의 'pandas'로 데이터 분석을 위한 프레임을 짜고(필요한 행만 추출), 'pymysql'로 DB에 저장합니다. JDBC랑 많이 달라서 익숙해지는 데 시간이 좀 걸렸습니다...

<br>

<div style="text-align: center;">
  <img src="/사진들/water/수질오염원DB.png" alt="" />
</div>

<br>

---
# 2. 수질오염원 데이터 RestAPI(백엔드: Spring boot)

```
//build.gradlew 파일의 의존성 부분
dependencies {
    runtimeOnly 'com.mysql:mysql-connector-j' //MySQL 커넥터
}

//application.properties 파일의 DB 설정부분
spring.datasource.url=jdbc:mysql://www.lifeslike.org:3306/weather_db?serverTimezone=Asia/Seoul&characterEncoding=UTF-8
spring.datasource.username=water
spring.datasource.password=water1111
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

Spring boot 프로젝트에서 DB로 접근하여 CRUD작업을 수행하기 위해 `build.gradlew`에서는 각자의 DB에 맞는 커넥터를 추가하고 `application.properties`파일에서는 DB관련 정보들을 추가합니다.( ex 접속 URL, 유저이름, 비밀번호, 드라이버)

<br>

## Controller
```java
package org.water.ex1.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.water.ex1.service.PollutionSourceService;
import org.water.ex1.model.PollutionSource;

import java.util.List;

@RestController
public class PollutionSourceController {
    @Autowired
    private PollutionSourceService pollutionSourceService;
    //GET http://localhost:8080/pollution-sources
    @GetMapping("/pollution-sources")
    public List<PollutionSource> getPollutionSources() {
        return pollutionSourceService.getAllPollutionSources();
    }
}
```

`@RestController`과 `@GetMapping`어노테이션을 이용해서 `http://localhost:8080/pollution-sources`URL로 접속하면 RestAPI가 나오도록 설정해줍니다.

<br>

## Service

```java
package org.water.ex1.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.water.ex1.model.PollutionSource;
import org.water.ex1.repository.PollutionSourceRepository;
import org.water.ex1.util.CoordinateConverter;

import java.util.ArrayList;
import java.util.List;

@Service
public class PollutionSourceService {
    @Autowired
    private PollutionSourceRepository pollutionSourceRepository;
    
    public List<PollutionSource> getAllPollutionSources() {
		List<PollutionSource> pollutionSources = pollutionSourceRepository.findAll();
        // WGS84로 변환된 결과를 담을 리스트
        List<PollutionSource> convertedSources = new ArrayList<>();
        for (PollutionSource source : pollutionSources) {
            // UTM-K 좌표를 WGS84로 변환
            double[] wgs84Coordinates = CoordinateConverter.convertTMToWGS84(source.getWeb_bplc_x_katec(), source.getWeb_bplc_y_katec());
            // 새로운 PollutionSource 객체 생성하여 변환된 좌표로 설정
            PollutionSource convertedSource = new PollutionSource();
            convertedSource.setId(source.getId());
            convertedSource.setBsnm_nm(source.getBsnm_nm()); // 회사명 설정
            convertedSource.setInduty_nm(source.getInduty_nm()); // 산업명 설정
            convertedSource.setBsns_detail_road_addr(source.getBsns_detail_road_addr()); // 주소 설정
            convertedSource.setWeb_bplc_x_katec(wgs84Coordinates[0]); // 변환된 X 좌표
            convertedSource.setWeb_bplc_y_katec(wgs84Coordinates[1]); // 변환된 Y 좌표
            // 변환된 객체를 리스트에 추가
            convertedSources.add(convertedSource);
        }
        return convertedSources; // 변환된 리스트 반환
    }
}
```
`Service`계층에서 실제 기능을 구현합니다. 후술할 `PollutionSourceRepository`클래스를 사용해서 CRUD작업을 할 수 있는데요. 지금 이 `Service`단에서는 `Repository` 계층의 인스턴스를 생성하고 `pollutionSourceRepository.findAll();` 메서드로 `Read`작업을 수행하고 있습니다. 정리하자면, `Service`계층은 `Repository`계층의 메서드로 실제 기능을 일으키는 단이라고 볼 수 있습니다.

<br>

## Repository
```java
package org.water.ex1.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.water.ex1.model.PollutionSource;

public interface PollutionSourceRepository extends JpaRepository<PollutionSource, Long> {
    // 추가적인 쿼리 메서드를 정의할 수 있습니다.
}
```

`extends JpaRepository<PollutionSource, Long>`이 부분에 주목해주시기 바랍니다. JPA에서 미리 만들어놓은 인터페이스를 구현하고 그 안에 제네릭 타입은 `PollutionSource`입니다. 즉, `PollutionSource`에 관련된 데이터 타입을 가지고 CRUD작업을 할 수 있는 메서드들이 `JpaRepository`인터페이스에는 미리 디폴트 메서드로 정의 되어 있는 것입니다. 앞서 본 `Service`계층의 `findAll()`메서드처럼 말이죠.

<br>

## Model
```java
package org.water.ex1.model;

import jakarta.persistence.*;

@Entity
@Table(name = "pollution_sources") // 테이블 이름을 명시적으로 지정
public class PollutionSource {
    @Id
    @Column(name = "id") // 데이터베이스의 칼럼명과 매핑
    private Long id;

    @Column(name = "bsnm_nm") // 데이터베이스의 칼럼명과 매핑
    private String bsnm_nm;

    @Column(name = "induty_nm") // 데이터베이스의 칼럼명과 매핑
    private String induty_nm;

    @Column(name = "bsns_detail_road_addr") // 데이터베이스의 칼럼명과 매핑
    private String bsns_detail_road_addr;

    @Column(name = "web_bplc_x_katec") // 데이터베이스의 칼럼명과 매핑
    private Double web_bplc_x_katec;

    @Column(name = "web_bplc_y_katec") // 데이터베이스의 칼럼명과 매핑
    private Double web_bplc_y_katec;

    // WGS84 좌표 추가
    private Double wgs84_x; // WGS84 X 좌표
    private Double wgs84_y; // WGS84 Y 좌표

    // Getter 및 Setter 추가
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getBsnm_nm() {
        return bsnm_nm;
    }

    public void setBsnm_nm(String bsnm_nm) {
        this.bsnm_nm = bsnm_nm;
    }

    public String getInduty_nm() {
        return induty_nm;
    }

    public void setInduty_nm(String induty_nm) {
        this.induty_nm = induty_nm;
    }

    public String getBsns_detail_road_addr() {
        return bsns_detail_road_addr;
    }
    
    public void setBsns_detail_road_addr(String bsns_detail_road_addr) {
        this.bsns_detail_road_addr = bsns_detail_road_addr;
    }

    public Double getWeb_bplc_x_katec() {
        return web_bplc_x_katec;
    }

    public void setWeb_bplc_x_katec(Double web_bplc_x_katec) {
        this.web_bplc_x_katec = web_bplc_x_katec;
    }

    public Double getWeb_bplc_y_katec() {
        return web_bplc_y_katec;
    }

    public void setWeb_bplc_y_katec(Double web_bplc_y_katec) {
        this.web_bplc_y_katec = web_bplc_y_katec;
    }
}
```

`Model`계층은 DB의 테이블구조를 자바의 클래스 구조(필드, 메서드)와 매핑해줍니다. `@Entity`어노테이션으로 이 클래스가 DB와 매핑될 클래스임을 명시하고 `@Table`어노테이션으로 무슨 테이블인지 컴파일러에게 알려줍니다. `@ID`어노테이션은 Primary Key임을 의미라고 `@Column`어노테이션으로 각각의 열들과 매핑해줍니다.

<br>

## 정리

```
[PollutionSourceController] <--- HTTP 요청 처리
       |
       v
[PollutionSourceService] <--- 비즈니스 로직 처리
       |
       v
[PollutionSourceRepository] <--- 데이터베이스 접근
       |
       v
[PollutionSource] <--- 데이터 모델
```
  이렇게 도식화하여 정리할 수 있겠습니다.
  
---

