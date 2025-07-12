---
layout: post  
title: AWS RDS/Spring Boot 'Too Many Connections' 문제 해결 
date: 2024-07-12
---

> Spring Boot와 AWS RDS를 사용하며 `Too Many Connections` 오류를 마주쳤다면, 단순히 `max_connections` 값만 늘리는 것은 임시방편일 뿐입니다. 이 글에서는 문제의 진짜 원인인 '좀비 커넥션'을 진단하고, 애플리케이션과 데이터베이스 양쪽에서 방어벽을 구축하여 문제를 근본적으로 해결하는 전체 과정을 상세하게 기록합니다.


# 전체 흐름: 진단부터 해결까지


## 1. 문제 진단: '누가' 내 DB 연결을 독점하는가?

```sql
SHOW FULL PROCESSLIST;
```

### 실행 결과 분석 포인트

|컬럼|의미|주목 포인트|
|:--|:--|:--|
|Host|클라이언트 IP|동일한 IP에서 다중 연결 확인|
|Command|Sleep|연결 대기 상태|
|Time|대기 지속 시간(초)|10일 이상 = 좀비 커넥션|

- `Sleep` 상태 + `Time` 값이 비정상적으로 긴 경우 → **좀비 커넥션(Zombie Connection)**


## 2. 근본 원인: 좀비 커넥션은 왜 생기는가?

### 원인 1: 비정상 종료 (Connection Leak)

- 앱 강제 종료(Ctrl+C, IDE 중지 등)
    
- DB 연결 해제 신호 없이 종료 → 좀비 커넥션 생성
    

### 원인 2: 방어 설정 부재

- Spring Boot: HikariCP 설정 미비
    
- RDS: `wait_timeout` 값이 너무 길게 설정됨
    

## 3. 해결 방안: 3단계 완벽 방어 가이드

### 3-1. 1단계: [긴급 처치] 좀비 커넥션 즉시 제거

```sql
KILL 921004;
KILL 921426;
KILL 842898;
```

### 3-2. 2단계: [근본 해결 1] 애플리케이션 방어벽 구축 (HikariCP 설정)

```properties
spring.datasource.hikari.maximum-pool-size=25
spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.connection-timeout=30000
spring.datasource.hikari.max-lifetime=1800000
spring.datasource.hikari.idle-timeout=600000
```

- **maximum-pool-size**: 최대 연결 수 제한
    
- **max-lifetime**: 최대 수명 30분 설정
    
- **idle-timeout**: 유휴 10분 후 커넥션 제거
    

### 3-3. 3단계: [근본 해결 2] 데이터베이스 방어벽 구축 (RDS 파라미터 그룹 수정)

#### 3-A. wait_timeout 설정

- RDS → 파라미터 그룹 → `wait_timeout = 300` 또는 `600`
    
- 즉시 적용 가능
    

#### 3-B. max_connections 상향 (필요시)

- 새 파라미터 그룹 생성 → `max_connections = 원하는 값`
    
- DB 인스턴스 적용 → **즉시 적용 + 재부팅 필수**
    

## 최종 요약

1. **진단 및 긴급 처치**: `SHOW PROCESSLIST` → `KILL` 명령어
    
2. **애플리케이션 강화**: HikariCP 설정
    
3. **데이터베이스 강화**: RDS 파라미터 그룹 설정 (`wait_timeout`, `max_connections`)
    
