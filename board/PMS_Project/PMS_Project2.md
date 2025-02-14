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

---