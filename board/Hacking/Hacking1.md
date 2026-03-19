---
layout: post
title: "1. Ignite"
date: 2026-03-18
---

## 📋 프로젝트 개요
본 프로젝트는 **TryHackMe**의 'Ignite' 머신을 대상으로 진행한 모의해킹 실무 프로세스 학습 과정입니다. 정보 수집부터 권한 상승, 그리고 공격 이후의 지속성 유지 단계까지 침투 테스트의 전체 생명주기(Lifecycle)를 체계적으로 수행하였습니다.

---

## 1. 정보 수집 (Information Gathering)
공격 대상을 파악하기 위해 네트워크 스캐닝 도구인 `nmap`을 사용하여 열려 있는 포트와 실행 중인 서비스를 확인했습니다.

* **수행 내용:** 대상 IP에 대한 서비스 및 스크립트 스캔 (`-sC`, `-sV`)
* **결과:** 80번 포트(HTTP)가 활성화되어 있으며, **Apache 2.4.18** 서버에서 **FUEL CMS**가 구동 중임을 확인했습니다.

<div style="text-align: center;">
  <img src="/사진들/Hacking/ignite/1.정보수집(nmap).png" alt="Nmap Scan Result" />
</div>

---

## 2. 서비스 분석 (Service Analysis)
식별된 웹 서비스의 구조와 잠재적인 취약점을 분석하는 단계입니다.

* **웹 인터페이스 확인:** FUEL CMS 1.4 버전이 구동 중인 메인 페이지를 확인했습니다.
* **취약점 검색:** `searchsploit` 도구를 통해 해당 버전(v1.4)에서 발생 가능한 취약점을 검색한 결과, **Remote Code Execution (RCE)** 취약점이 다수 존재함을 식별했습니다.

<div style="text-align: center;">
  <img src="/사진들/Hacking/ignite/2-1. 서비스 분석.png" alt="Web Interface" />
</div>

<div style="text-align: center;">
  <img src="/사진들/Hacking/ignite/2-2. 서비스 분석(searchsploit).png" alt="Searchsploit Result" />
</div>

---

## 3. 침투 수행 (Exploit)
분석된 취약점을 바탕으로 대상 시스템의 쉘(Shell) 권한을 획득하는 단계입니다.

* **공격 수행:** 식별된 RCE 취약점 코드(`50477.py`)를 로컬로 가져와 대상 URL에 실행했습니다.
* **결과:** 대상 서버로부터 `www-data` 권한의 원격 명령 실행 권한을 확보하는 데 성공했습니다.

<div style="text-align: center;">
  <img src="/사진들/Hacking/ignite/3. exploit(쉘 획득).png" alt="Exploit Success" />
</div>

---

## 4. 권한 상승 (Post-Exploit)
일반 웹 사용자 권한(`www-data`)에서 시스템 최고 권한인 `root`로 상승시키는 과정입니다.

* **SUID 및 설정 분석:** 시스템 내 설정 오류나 취약한 바이너리를 조사했습니다.
* **PwnKit 활용:** 시스템 내 `pkexec`의 취약점을 이용하는 **CVE-2021-4034 (PwnKit)** 익스플로잇 코드를 작성 및 컴파일하여 실행했습니다.
* **결과:** 최종적으로 시스템의 최고 권한인 **Root Shell**을 획득했습니다.

<div style="text-align: center;">
  <img src="/사진들/Hacking/ignite/4-1. post-exploit(setuid 확인).png" alt="Privilege Escalation Research" />
</div>

<div style="text-align: center;">
  <img src="/사진들/Hacking/ignite/4-2. post-exploit(root탈취).png" alt="Root Access Granted" />
</div>

---

## 5. 지속성 유지 (Persistence)
관리자의 조치나 시스템 재부팅 후에도 접근 권한을 유지하기 위한 백도어를 설정했습니다.

* **메커니즘:** Linux의 예약 작업 관리자인 `crontab`을 활용했습니다.
* **수행 내용:** 매 분마다 공격자 서버(`192.168.216.48:9001`)로 리버스 쉘을 연결하도록 스케줄을 등록하여, 연결이 끊기더라도 자동으로 재연결되도록 구성했습니다.

<div style="text-align: center;">
  <img src="/사진들/Hacking/ignite/5. 백도어(crontab).png" alt="Backdoor Persistence" />
</div>

---

## 💡 마치며
이번 실습을 통해 서비스 취약점 분석부터 최종 권한 탈취까지의 일련의 과정을 직접 수행하며 **침투 테스트의 메커니즘**을 깊이 있게 이해할 수 있었습니다. 특히, 단순한 툴 사용에 그치지 않고 시스템의 취약한 설정을 파고드는 권한 상승 기법과 사후 관리의 중요성을 학습하는 계기가 되었습니다.
