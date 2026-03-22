---
layout: post
title: "4. Blue"
date: 2026-03-22
---

## 프로젝트 개요
본 프로젝트는 **TryHackMe**의 'Blue' 머신을 대상으로 진행한 모의해킹 실습입니다. 윈도우 시스템의 대표적인 취약점인 **EternalBlue(MS17-010)**를 이용해 시스템 권한을 획득하고, Metasploit 프레임워크를 활용한 포스트 익스플로잇(Post-Exploit) 및 해시 크래킹 과정을 체계적으로 실습하였습니다.

---

## 키워드(keyword)
`nmap`, `Metasploit`, `MS17-010`, `EternalBlue`, `Meterpreter`, `Hashdump`, `CrackStation`

## 1. 정보 수집 (Information Gathering)
대상 시스템의 운영체제 및 활성화된 서비스를 확인하기 위해 `nmap` 스캐닝을 수행했습니다.

* **수행 내용:** 모든 포트 및 서비스 버전 스캔 ("-sC", "-sV")
* **결과:** 135, 139, 445(SMB), 3389(RDP) 포트 등이 열려 있음을 확인했습니다. 특히 **Windows 7 Professional 7601 Service Pack 1** 버전이 구동 중임을 식별하여 SMB 취약점 가능성을 파악했습니다.

```bash
nmap -sC -sV 10.66.188.35
```

<div style="text-align: center;">
  <img src="/사진들/Hacking/Blue/1. 정보수집(nmap).png" alt="Nmap Scan Result" />
</div>

---

## 2. 서비스 분석 (Service Analysis)
식별된 Windows 7 시스템의 SMB 취약점을 공격하기 위해 `Metasploit` 프레임워크를 활용했습니다.

* **수행 내용:** MS17-010(EternalBlue) 관련 익스플로잇 모듈 검색
```bash
msfconsole
search ms17-010
```

<div style="text-align: center;">
  <img src="/사진들/Hacking/Blue/2-1. 서비스 분석(msfconsole).png" alt="Launching Metasploit" />
</div>

* **결과:** 다양한 모듈 중 `exploit/windows/smb/ms17_010_eternalblue` 모듈이 타겟 시스템에 적합함을 확인했습니다.

<div style="text-align: center;">
  <img src="/사진들/Hacking/Blue/2-2. 서비스 분석(Poc).png" alt="Search MS17-010 Modules" />
</div>

---

## 3. 침투 수행 (Exploit)
선택한 익스플로잇 모듈을 사용하여 대상 시스템의 제어권을 획득했습니다.

* **수행 내용:** 타겟 IP(RHOSTS) 설정 및 공격 수행
```bash
set RHOSTS 10.66.188.35
exploit
```
* **결과:** 공격 성공 후 대상 시스템의 일반 커맨드 쉘 세션을 획득하는 데 성공했습니다.

<div style="text-align: center;">
  <img src="/사진들/Hacking/Blue/3. exploit(쉘 획득).png" alt="Shell Gained via EternalBlue" />
</div>

---

## 4. 권한 상승 및 사후 분석 (Post-Exploit)
획득한 일반 쉘을 더욱 강력한 기능을 가진 **Meterpreter** 쉘로 업그레이드하고, 시스템 정보를 탈취했습니다.

* **Meterpreter 전환:** "shell_to_meterpreter" 모듈을 사용하여 `NT AUTHORITY\SYSTEM` 권한의 세션을 확보했습니다.

```bash
use post/multi/manage/shell_to_meterpreter
run
sessions -i 2
getuid
```

<div style="text-align: center;">
  <img src="/사진들/Hacking/Blue/4-1. post-exploit(meterpreter 쉘 획득).png" alt="Upgrade to Meterpreter Session" />
</div>

* **비밀번호 해시 탈취:** 시스템 내부의 사용자 계정 비밀번호 해시를 덤프했습니다.
```bash
hashdump
```

<div style="text-align: center;">
  <img src="/사진들/Hacking/Blue/4-2. post-exploit(비밀번호크랙킹1).png" alt="Dumping Password Hashes" />
</div>

* **해시 크래킹:** 탈취한 'Jon' 계정의 NTLM 해시를 **CrackStation**을 통해 크래킹한 결과, 평문 비밀번호가 `alqfa22`임을 확인했습니다.

<div style="text-align: center;">
  <img src="/사진들/Hacking/Blue/4-3 post-exploit(비밀번호크랙킹2).png" alt="Password Cracking via CrackStation" />
</div>

---

## [총평: Blue 침투 분석]
본 시스템 취약점의 핵심은 **오래된 윈도우 시스템의 패치 미비**였습니다. SMBv1 프로토콜의 설계 결함인 **MS17-010(EternalBlue)** 취약점이 방치되어 있어, 별도의 계정 정보 없이도 원격 코드 실행(RCE)을 통한 시스템 침투가 가능했습니다.

침투 이후 과정에서는 Metasploit의 강력한 기능을 통해 일반 쉘을 **Meterpreter**로 신속하게 전환하고, 시스템 최고 권한을 유지한 상태에서 `hashdump`를 통해 사용자 자격 증명까지 탈취할 수 있었습니다. 특히 복잡하지 않은 암호화 방식(NTLM)과 취약한 비밀번호 설정은 해시 크래킹에 의해 쉽게 무력화됨을 확인했습니다. 보안 패치와 강력한 비밀번호 정책의 중요성을 다시 한번 상기시키는 사례였습니다.

---

## 마치며
이번 Blue 실습을 통해 윈도우 취약점 분석의 기본 흐름과 Metasploit 프레임워크의 효율성을 깊이 이해할 수 있었습니다. 특히 권한 상승 이후의 정보 탈취 과정은 실제 침투 사고 발생 시 데이터 유출이 얼마나 빠르게 일어날 수 있는지를 잘 보여주었습니다.

[결과 보고서 다운로드 받기](/files/hacking/Blue/결과보고서(Blue).docx)
