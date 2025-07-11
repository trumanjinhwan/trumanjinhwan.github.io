---
layout: post  
title: "4. Nginx와 Certbot(Let's Encrypt)으로 도메인 + HTTPS 적용하기"  
date: 2025-07-05
---

> 이전 글에서는 GitHub Actions를 이용해 CI/CD 파이프라인을 구축했습니다. 이번 글에서는 **Nginx 리버스 프록시**를 도입하고, **Certbot**을 이용해 무료 SSL 인증서를 발급받아 **HTTPS를 적용**하는 전체 과정을 정리합니다.
>
> # 전체 흐름 개요

<div style="text-align: center;">
    <img src="/사진들/ShopSphere/CICD3.png" alt="alt text" />
</div>

## 1. 아키텍처 개요

### HTTPS 적용 아키텍처

기존에는 사용자가 Docker 컨테이너의 포트로 직접 접근했지만, 이제는 중간에 **Nginx**를 둔 **리버스 프록시(Reverse Proxy)** 구조로 변경합니다.

#### 구성요소 역할 분담

- **사용자**: `https://shopsphere123.duckdns.org`로 접속 (443 포트)
    
- **Nginx**: SSL 처리 및 리버스 프록시 역할 (443 → 8080)
    
- **Spring Boot 앱 (Docker 컨테이너)**: HTTP 요청(8080) 처리 전담
    

---

## 2. HTTPS 적용을 위한 사전 준비

### 2-1. 무료 도메인 설정 (DuckDNS)

1. [https://www.duckdns.org/](https://www.duckdns.org/) 접속
    
2. GitHub 계정 등으로 로그인
    
3. 원하는 서브도메인 등록 (예: `shopsphere123`)
    
4. EC2의 공인 IP를 입력한 뒤 `update ip` 클릭
    
5. IP 자동 갱신을 위한 `cron` 등록:
    

```bash
mkdir -p ~/duckdns

# DuckDNS 갱신 스크립트 생성
echo 'echo url="https://www.duckdns.org/update?domains=shopsphere123&token=YOUR_TOKEN&ip=" | curl -k -o ~/duckdns/duck.log -K -' > ~/duckdns/duck.sh

chmod 700 ~/duckdns/duck.sh

crontab -e
# 아래 내용 추가
*/5 * * * * ~/duckdns/duck.sh
```

### 2-2. AWS 보안 그룹(방화벽) 설정

|유형|프로토콜|포트 범위|소스|설명|
|---|---|---|---|---|
|HTTP|TCP|80|0.0.0.0/0|일반 웹 접속 및 Certbot 인증용|
|HTTPS|TCP|443|0.0.0.0/0|보안 웹 접속용|
|SSH|TCP|22|내 IP (권장)|EC2 접속용|
|ICMP|ICMP|전체|0.0.0.0/0|Ping 테스트용|

EC2 콘솔 → 인스턴스 → 보안 탭 → 보안 그룹 클릭 → 인바운드 규칙 편집

---

## 3. Nginx 및 Certbot 설치

### 3-1. Nginx 설치 및 실행 확인

```bash
sudo apt-get update
sudo apt-get install nginx -y
sudo systemctl status nginx
```

- 브라우저에서 `http://shopsphere123.duckdns.org` 접속 → "Welcome to nginx!" 페이지 확인
    

### 3-2. Certbot 설치 (Let's Encrypt)

```bash
sudo snap install core; sudo snap refresh core
sudo apt-get remove certbot
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

---

## 4. Nginx와 Certbot 연동하여 HTTPS 적용

### 4-1. Nginx 설정 파일 수정

```bash
sudo nano /etc/nginx/sites-available/default
```

- `server_name _;` → `server_name shopsphere123.duckdns.org;`로 변경
    
- 저장 후 문법 확인 및 재시작
    

```bash
sudo nginx -t
sudo systemctl restart nginx
```

### 4-2. Certbot 실행

```bash
sudo certbot --nginx
```

- 이메일 입력, 약관 동의(Y)
    
- 리디렉션 여부: **2번 (Redirect)** 선택
    
- 성공 시: "Congratulations!" 메시지 출력 → HTTPS 적용 완료
    

---

## 5. Nginx 리버스 프록시 설정

```bash
sudo nano /etc/nginx/sites-enabled/default
```

`location /` 블록을 다음과 같이 수정:

```nginx
location / {
    proxy_pass http://127.0.0.1:8080;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

- 저장 후 문법 검사 및 재시작
    

```bash
sudo nginx -t
sudo systemctl restart nginx
```

---

## 최종 확인

- 사용자는 `https://shopsphere123.duckdns.org`로 안전하게 접속 가능
    
- Nginx는 SSL 처리 및 리버스 프록시 역할
    
- 내부 Spring Boot 애플리케이션은 기존처럼 8080 포트에서 동작
    
- CI/CD 파이프라인은 여전히 `app.jar`을 EC2로 전송 및 실행

<style>
  table {
    width: 100%;
    border-collapse: collapse;
    margin: 20px 0;
  }
  th, td {
    border: 2px solid #333;
    padding: 12px;
    text-align: center;
  }
  th {
    background-color: #f4f4f4;
    font-weight: bold;
  }
  td {
    background-color: #fafafa;
  }
  table th, table td {
    border: 1px solid #ddd;
  }
</style>
    
