---
layout: post
title: "2. 서버배포(Deployment)"
date: 2025-02-13
---
>이번 항목에서는 실제 외부 IP가 접속하도록 클라우드 서버에 war파일을 배포를 하는 법을 정리해보겠습니다.

# 배포환경

클라우드 호스팅 서비스는 GCP를 기준으로 하고 VM의 운영체제는 우분투 20.04이고 JDK는 17버전이고 아파치 톰캣은 9.0.98버전이고 SQL은 MySQL 8.0.41 기준으로 배포합니다.

#### 개발환경 상세     
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

<table>
  <thead>
    <tr>
      <th>환경</th>
      <th>사용</th>
      <th>버전</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>OS</td>
      <td><img src="https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" alt="Ubuntu"></td>
      <td>ubuntu 20.04</td>
    </tr>
    <tr>
      <td>Server</td>
      <td><img src="https://img.shields.io/badge/apache%20tomcat-%23F8DC75.svg?style=for-the-badge&logo=apache-tomcat&logoColor=black" alt="Apache Tomcat"></td>
      <td>Apache Tomcat v9.0</td>
    </tr>
    <tr>
      <td>Language</td>
      <td><img src="https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white" alt="Java"></td>
      <td>JDK: JavaSE-17</td>
    </tr>
    <tr>
      <td>DB</td>
      <td><img src="https://img.shields.io/badge/mysql-4479A1.svg?style=for-the-badge&logo=mysql&logoColor=white" alt="MySQL"></td>
      <td>mysql-8.0.41</td>
    </tr>
  </tbody>
</table>



# VM 인스턴스 생성하기

<div style="text-align: center;">
	<img src="/사진들/서버배포/프로젝트 선택 버튼.png" alt="alt text" />
</div>

<br>

```프로젝트 선택```버튼을 클릭합니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/새 프로젝트.png" alt="alt text" />
</div>

<br>

```새 프로젝트```버튼을 눌러줍니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/프로젝트 만들기.png" alt="alt text" />
</div>

<br>

프로젝트 이름을 정해주고 위치는 상관안하셔도 됩니다. 그리고 ```만들기```버튼을 클릭합니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/프로젝트 선택.png" alt="alt text" />
</div>

<br>

위에서 만든 프로젝트로 들어갑니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/Computer Engine.png" alt="alt text" />
</div>

<br>

```Computer Engine``` 버튼을 눌러줍니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/Computer Engine API 사용.png" alt="alt text" />
</div>

<br>

```사용```버튼을 눌러줍니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/인스턴스 만들기.png" alt="alt text" />
</div>

<br>

```인스턴스 만들기```버튼을 클릭합니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/머신구성.png" alt="alt text" />
</div>

<br>

이름을 지어주고 리전은 ```us-central1(아이오와)```, 영역은 ```us-central-a```로 설정합니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/OS 및 스토리지.png" alt="alt text" />
</div>

<br>

OS 및 스토리지 항목에 들어가서 ```변경```버튼을 눌러줍니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/부팅디스크 변경.png" alt="alt text" />
</div>

<br>

운영체제는 Ubuntu, 버전은 Ubuntu 20.04 LTS로 해줍니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/네트워킹.png" alt="alt text" />
</div>

<br>

```HTTP 트래픽 허용```이랑 ```HTTPS 트래픽 허용``` 두 개의 체크박스를 선택해줍니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/보안&만들기.png" alt="alt text" />
</div>

<br>

```모든 Cloud API에 대한 전체 액세스 허용``` 라디오 버튼을 체크해주고 ```만들기```버튼을 누릅니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/SSH 버튼 클릭.png" alt="alt text" />
</div>

<br>

```연결``` 컬럼의 ```SSH```버튼을 눌러줍니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/VM 접속 성공.png" alt="alt text" />
</div>

<br>

이 화면이 뜨면 VM 생성과 접속이 잘 된 것입니다!

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/VPC 네트워크.png" alt="alt text" />
</div>

<br>

다시 홈 화면으로 돌아간다음 ```VPC 네트워크```버튼을 클릭합니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/방화벽.png" alt="alt text" />
</div>

<br>

 ```방화벽```항목을 클릭합니다.
 
<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/방화벽 규칙 만들기.png" alt="alt text" />
</div>

<br>

 ```방화벽 규칙 만들기```버튼을 클릭합니다.
 
<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/8080 포트 허용.png" alt="alt text" />
</div>

<br>

이름은 아무거나 해도 상관 없지만, 대상, 소스범위, 프로토콜(TCP)를 8080으로 허용하는 건 다 지켜주시고 "만들기" 눌러줍니다.

---

# 우분트 기본 패키지 다운로드, 설정

```bash
sudo apt upgrade

sudo apt update
```

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/우분트 기본패키지.png" alt="alt text" />
</div>

<br>

우분트의 기본 패키지들을 다운 받아 줍니다.

# Java(JDK) 

<https://www.oracle.com/kr/java/technologies/downloads/archive/>

위의 링크에서 본인의 실행환경과 맞는 JDK를 선택하는게 최고입니다. 이 글에서는 일단 JDK 17버전으로 설치를 하겠습니다.

<br>

```bash
wget https://download.oracle.com/java/17/archive/jdk-17.0.12_linux-x64_bin.tar.gz

sudo tar -xvzf jdk-17.0.12_linux-x64_bin.tar.gz
```
<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/JDK 다운&압축풀기.png" alt="alt text" />
</div>

<br>

리눅스에 호환되는 JDK를 설치하고 압축을 풀어줍니다.

<br>

```bash
wget https://download.oracle.com/java/17/archive/jdk-17.0.12_linux-x64_bin.tar.gz

sudo tar -xvzf jdk-17.0.12_linux-x64_bin.tar.gz

sudo mkdir -p /usr/lib/jvm

sudo mv jdk-17.0.12 /usr/lib/jvm/

sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk-17.0.12/bin/java 1

sudo update-alternatives --config java

java -version
```
<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/JVM폴더에 옮기기.png" alt="alt text" />
</div>

<br>

JDK는 보통 /usr/lib/jvm 폴더 밑에 위치하게 하는 관습이 있으므로 /usr/lib 폴더 밑에 jvm 폴더를 만들어주고 그 곳에다가 JDK를 위치시켜줍니다. ```java -version``` 쳤을 때 자바 버전이 잘 나오면 설치가 잘 된 것입니다.

<br>

```bash
sudo apt-get install openjdk-17-jdk
```
<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/openjdk 설치.png" alt="alt text" />
</div>

<br>

openjdk도 설치를 해줍니다. 중간에 "y" 누르는 부분 있습니다.

<br>

```bash
sudo apt-get install openjdk-17-jdk
```

# Tomcat

<https://tomcat.apache.org/whichversion.html>

위의 링크에서 본인의 실행환경과 맞는 Tomcat을 선택하는게 최고입니다. 이 글에서는 일단 tomcat 9버전으로 설치를 하겠습니다.
<br>

```bash
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.99/bin/apache-tomcat-9.0.99.tar.gz

tar -xvzf apache-tomcat-9.0.99.tar.gz
```
<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/tomcat 다운&압축풀기.png" alt="alt text" />
</div>

<br>

tomcat을 설치하고 압축풀어줍니다.

<br>

```bash
cd ~/ 

nano .bashrc #아래 꺼 맨 밑에 복사 붙여넣기

CATALINA_HOME="~/tomcat/apache-tomcat-9.0.99"
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64

CATALINA_HOME="~/tomcat/apache-tomcat-9.0.99"
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export JRE_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export PATH=$PATH:$JAVA_HOME/bin

source ~/.bashrc
```
<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/시스템 환경변수.png" alt="alt text" />
</div>

<br>

시스템의 환경변수를 설정합니다.
```bash
CATALINA_HOME="~/tomcat/apache-tomcat-9.0.99"
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64

CATALINA_HOME="~/tomcat/apache-tomcat-9.0.99"
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export JRE_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export PATH=$PATH:$JAVA_HOME/bin
```

이 부분은 bashrc 파일을 열고 맨 아랫줄에 붙여넣어줍니다.

<br>

```bash
cd apache-tomcat-9.0.99/bin

vi ~/apache-tomcat-9.0.99/bin/setenv.sh

export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export JRE_HOME=/usr/lib/jvm/java-17-openjdk-amd64
CATALINA_PID="/run/tomcat.pid"
```
<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/톰캣 환경변수.png" alt="alt text" />
</div>

<br>

이번에는 톰캣 부분에서 환경변수를 설정해줍니다.

<br>

```bash
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export JRE_HOME=/usr/lib/jvm/java-17-openjdk-amd64
CATALINA_PID="/run/tomcat.pid"
```

이 부분을 복사 붙여넣기 해줍니다. vi 편집기에서 저장 후 나가려면 ```ESC``` 한번 누르고 ```:wq```입니다. 

<br>

```bash
cd apache-tomcat-9.0.99/bin

vi ~/apache-tomcat-9.0.99/bin/setenv.sh

export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export JRE_HOME=/usr/lib/jvm/java-17-openjdk-amd64
CATALINA_PID="/run/tomcat.pid"
```
<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/톰캣 환경변수.png" alt="alt text" />
</div>

<br>

이번에는 톰캣 부분에서 환경변수를 설정해줍니다.

# 데이터베이스(MySQL)

<br>

```bash
sudo apt-get install mysql-server
```
<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/MySQL 설치.png" alt="alt text" />
</div>

<br>

MySQL을 깔아줍니다. 우분투에서는 기본 패키지에 MySQL이 내장 되어있으니 그걸 이용하면 편리합니다. 중간에 "y" 눌러줍니다.

<br>

```bash
sudo systemctl start mysql

sudo systemctl enable mysql
```
<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/SQL 서버시작&유지.png" alt="alt text" />
</div>

<br>

MySQL 서버를 시작해주고 서버 시작을 자동적으로 유지해줍니다.

<br>

```bash
sudo apt-get install mysql-server

sudo systemctl start mysql

sudo systemctl enable mysql

sudo mysql -u root -p

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '1111';

GRANT GRANT OPTION ON *.* TO 'root'@'localhost';

FLUSH PRIVILEGES;

CREATE USER 'apple'@'localhost' IDENTIFIED BY '1111';

GRANT ALL PRIVILEGES ON *.* TO 'apple'@'localhost';

FLUSH PRIVILEGES;

exit
```
<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/apple 유저 생성&권한.png" alt="alt text" />
</div>

<br>

일단 "root" 계정으로 들어가서 일반 사용자를 만들어주고 권한을 부여해야합니다. 일반 사용자 이름은 여러분들이 로컬에서 웹 프로젝트 만들 적에 썼던 유저이름과 비밀번호로 해줘야합니다!!!
저는 "apple"이 유저 이름이고 "1111"이 비밀번호입니다.

<br>

```bash
sudo mysql -u apple -p

CREATE DATABASE gapi;

USE gapi;

// #create, insert 구문 스크립트 복붙
```
<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/apple 계정 스키마&인스턴스.png" alt="alt text" />
</div>

<br>

해당 유저이름으로 데이터베이스 만들고 웹 프로젝트 할 적에 해놨던 create,insert 스크립트 복사,붙여넣기해줍니다.

# Github 이용하기

war파일을 VM에 올리기 위해 git 레파지토리를 만들고 git clone 하는 방식을 사용할 것 입니다.
그러므로 지금부터는 여러분들이 github계정을 가지고 있다고 가정하고 진행할 것이니 github계정이 없으시다면 계정을 먼저 만들고 진행해주세요.

<br>

```bash
cd ~/

sudo apt install git

git --version

cd ~/.ssh

ssh-keygen -t rsa -C 'trumanjinhwan@gmail.com' #본인 깃허브계정 이메일

cat id_rsa.pub
```
<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/깃허브 ssh.png" alt="alt text" />
</div>

<br>

```bash 
sh-keygen -t rsa -C 'trumanjinhwan@gmail.com'
```

이거 한 다음에 뭐 입력해야 될 것 처럼 나오지만 그냥 아무것도 입력안하고 엔터 키 눌러도 되니까 한 3번 정도는 그냥 엔터 키만 치셔도 됩니다. 그리고 

```bash 
cat id_rsa.pub
```
이거 한 다음 나오는 거 메모장 같은 곳에다가 복사 붙여넣기 해줍니다. 저로 예를 들면
```bash
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDWptUKblKCs8QkWYUzU0za/rlv+S5xxSUm2MuVvrZ6DJEN/bh6lSSLh3/IWe3s4Fc6ZNTnA9EcvJKFV+LlrTewLJVLJ1
ya1jhNKEqVn8HEW1d7aHk+cUhdqrH94U8vKyloKt8XXOZU75yPykIM2Lfd8AHjSjL4+DqN12+qQzS0lZ7yYDNfjqEVoe/SARtdlZ9WVX7hjIwGHVsOY8Wwfeynob6R8uJRM
PhBt1fX3E39KZTKcU/CzVoDX33aa+j971OB0+23OPqdslpuHgzcd5HBtog6zKtk+afYP7Q+g0OL3MqipLBGZTXvC3OGl9OFykfg1w8TrmLB7RVYLjg1pIXIYW5AOmzHcCb4g1
WjQRy1pdYDDq6OSzLDadY6MFmOjGWJbzj/TJQgC23HAGbvlvRlJh716V9s2plj3CQoBFR+qXzWM2c8xxqcuZ8xOV/d4zwgezc6bExkKccYzKixy7YNbujOtYoFPV8I45o6JkPX
ax/Z8F6EDMfSYNtFkmAz2ws= trumanjinhwan@gmail.com
```

이런식으로 나옵니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/New ssh key.png" alt="alt text" />
</div>

<br>

깃허브 들어가서 우측 상단에 프로필 -> Settings -> SSH and GPG keys -> New SSH key 복사한 SSH 키 붙여넣어봅시다 .

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/깃 레파지토리 생성.png" alt="alt text" />
</div>

<br>

깃 레파지토리를 생성해줍니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/uploading an existing file.png" alt="alt text" />
</div>

<br>

깃 레파지토리에 war파일 올려줄겁니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/war파일 올리기.png" alt="alt text" />
</div>

<br>

해당 war파일을 선택 후 커밋 해줍니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/깃 레파지토리 주소 복사.png" alt="alt text" />
</div>

<br>

깃 레파지토리 주소를 복사 해줍니다.

<br>

```bash
cd ~/

git clone https://github.com/trumanjinhwan/myserver.git

cd myserver

cp Project4team_PMS.war /home/trumanjinhwan/apache-tomcat-9.0.99/webapps/

cd ~/apache-tomcat-9.0.99/webapps
```

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/webapps 폴더 밑에 war파일.png" alt="alt text" />
</div>

<br>

깃 클론 해주고 war파일을 ```webapps```폴더 밑에 복사시켜줍니다.

<br>

```bash
#서버 켜기
sudo sh /home/trumanjinhwan/apache-tomcat-9.0.99/bin/startup.sh

#서버 끄기
sudo sh /home/trumanjinhwan/apache-tomcat-9.0.99/bin/shutdown.sh
```

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/서버 켜기&끄기.png" alt="alt text" />
</div>

<br>

서버를 켜고 끄기는 명령어는 위의 2개입니다.

<br>

<div style="text-align: center;">
	<img src="/사진들/서버배포/서버 배포 성공.png" alt="alt text" />
</div>

<br>

이런식으로 서버 배포에 성공했습니다!

