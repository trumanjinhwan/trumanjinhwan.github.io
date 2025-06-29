---
layout: post  
title: "2. GitHub Actions + Docker로 ShopSphere 프로젝트 CI/CD 자동화 구축"  
date: 2025-06-29
---

> 이번 글에서는 ShopSphere 프로젝트의 배포 과정을 자동화하기 위해 GitHub Actions와 EC2 서버를 이용한 CI/CD 구축 절차를 정리합니다.

> 백엔드(Spring Boot)와 프론트엔드(React) 모두에 대해 적용했으며, 서버 접속부터 배포 완료까지의 전체 흐름을 기록합니다.


# CI/CD 구축 목적

ShopSphere 프로젝트에서는 두 가지 작업이 반복적으로 발생했습니다.

- **백엔드 변경 후:**
    
    - Spring Boot 애플리케이션 재빌드
        
    - Docker 이미지 재생성 및 재실행
        
- **프론트엔드 변경 후:**
    
    - React 앱 다시 build
        
    - Spring Boot static 폴더에 복사
        
    - Docker 이미지 재생성 및 재시작
        

이러한 수동 과정을 **GitHub Actions + SSH 자동화**로 통합했습니다.



# EC2 서버 준비

## 1. Java 설치 (백엔드용)

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
java -version
```

## 2. Node.js 설치 (프론트엔드 빌드용)

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
node -v
npm -v
```

## 3. Docker 설치 (배포용)

```bash
sudo apt update
sudo apt install ca-certificates curl gnupg -y
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```



# EC2에 GitHub 레포지토리 클론

```bash
# 백엔드
git clone https://github.com/Sahmyook-4-team/ShopSphere-Backend.git

# 프론트엔드
git clone https://github.com/Sahmyook-4-team/ShopSphere-Frontend.git
```



# EC2 SSH Key 준비 (GitHub Actions용)

```bash
ssh-keygen -t rsa -b 4096 -C "github-actions"
cat ~/.ssh/github-actions
chmod 600 ~/.ssh/authorized_keys
```

**GitHub Secrets에 등록할 값들:**

|이름|값|
|---|---|
|HOST|EC2 퍼블릭 IP|
|USERNAME|ubuntu|
|PRIVATE_KEY|위에서 cat으로 출력된 전체 Private Key 내용|

# Dockerfile 설정 (백엔드)

ShopSphere-Backend 프로젝트 루트에 다음과 같은 Dockerfile이 존재합니다:

```
# Java 17~23 중 서버에 맞게 선택
FROM openjdk:17-jdk-slim

# JAR 넣기
ARG JAR_FILE=build/libs/*.jar
COPY ${JAR_FILE} app.jar

# 포트 오픈
EXPOSE 8080

# 실행 명령어
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

### 핵심 포인트:

- Gradle로 JAR 빌드 후 `/build/libs/`에 생성된 JAR 파일을 Docker 이미지에 포함
    
- 컨테이너 실행 시 바로 Spring Boot 앱 구동
    



# Backend 레포 `.github/workflows/deploy.yml`

**Trigger:** `ShopSphere-Backend` 레포에 푸시될 때마다 자동 실행.

```yaml
name: Deploy Backend to EC2

on:
  push:
    branches:
      - master

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Backend Repo
        uses: actions/checkout@v3

      - name: SSH into EC2 and Deploy Backend
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USERNAME }}
          key: ${{ secrets.PRIVATE_KEY }}
          script: |
            cd ~/ShopSphere-Backend
            git pull
            sudo chmod 777 gradlew
            ./gradlew bootJar -x test
            sudo docker stop $(sudo docker ps -q) || true
            sudo docker rm $(sudo docker ps -aq) || true
            sudo docker build --no-cache -t shopsphere-backend .
            sudo docker run -d -p 8080:8080 shopsphere-backend
```



# Frontend 레포 `.github/workflows/deploy.yml`

**Trigger:** `ShopSphere-Frontend` 레포에 푸시될 때마다 자동 실행.

```yaml
name: Deploy Frontend to EC2

on:
  push:
    branches:
      - master

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Frontend Repo
        uses: actions/checkout@v3

      - name: SSH into EC2 and Build Frontend
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USERNAME }}
          key: ${{ secrets.PRIVATE_KEY }}
          script: |
            cd ~/ShopSphere-Frontend
            git pull
            rm -rf build
            npm install
            npm run build

            rm -rf ~/ShopSphere-Backend/src/main/resources/static/*
            cp -r build/* ~/ShopSphere-Backend/src/main/resources/static/

            cd ~/ShopSphere-Backend
            sudo chmod 777 gradlew
            ./gradlew bootJar -x test
            sudo docker stop $(sudo docker ps -q) || true
            sudo docker rm $(sudo docker ps -aq) || true
            sudo docker build --no-cache -t shopsphere-backend .
            sudo docker run -d -p 8080:8080 shopsphere-backend
```


# EC2 디스크 공간 부족 시 수동 조치

용량이 부족해지면 아래 명령어로 Docker 캐시 강제 정리:

```bash
sudo docker stop $(sudo docker ps -q)
sudo docker rm $(sudo docker ps -aq)
sudo docker rmi $(sudo docker images -q)
sudo docker volume prune -f
sudo docker builder prune -af
sudo docker system prune -af --volumes
```



# 최종 정리

이제부터는 **Backend 레포에 push**  
→ 자동 백엔드 배포

 **Frontend 레포에 push**  
→ 자동 프론트 build + static 반영 + 백엔드 재배포

** 진짜 CI/CD 완성!**

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
