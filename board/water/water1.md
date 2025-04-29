---
layout: post
title: "1. 데이터수집하기"
date: 2025-04-29
---

# 수질오염원 데이터 수집

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

이렇게 테이블이 만들어지고 이걸 토대로 Spring boot(백엔드 서버)로 넘겨서 Rest API를 만들어줍니다.

---


