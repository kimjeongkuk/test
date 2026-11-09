# 🖥️ 개발 워크스테이션 구축 미션

## 📌 목차
1. [프로젝트 개요](#1-프로젝트-개요)
2. [실행 환경](#2-실행-환경)
3. [수행 항목 체크리스트](#3-수행-항목-체크리스트)
4. [터미널 조작 로그](#4-터미널-조작-로그)
5. [권한 실습](#5-권한-실습)
6. [Docker 설치 및 기본 점검](#6-docker-설치-및-기본-점검)
7. [Docker 기본 운영 명령](#7-docker-기본-운영-명령)
8. [컨테이너 실행 실습](#8-컨테이너-실행-실습)
9. [커스텀 이미지 제작 Dockerfile](#9-커스텀-이미지-제작-dockerfile)
10. [포트 매핑 및 접속 증거](#10-포트-매핑-및-접속-증거)
11. [Docker 볼륨 영속성 검증](#11-docker-볼륨-영속성-검증)
12. [Git 설정 및 GitHub 연동](#12-git-설정-및-github-연동)
13. [검증 방법 및 결과](#13-검증-방법-및-결과)
14. [트러블슈팅](#14-트러블슈팅)

---

## 1. 프로젝트 개요

본 미션은 개발자로서 필수적인 **로컬 개발 환경(워크스테이션)을 직접 구축**하고 검증하는 과정이다.  
터미널 조작, 파일 권한 관리, Docker 컨테이너 운영, Dockerfile 기반 커스텀 이미지 제작,  
포트 매핑 및 볼륨 영속성 확인, Git/GitHub 연동까지 개발 환경의 전 과정을 실습하고 문서화한다.

**핵심 목표**
- 터미널 기본 명령 및 파일 권한 이해
- Docker를 활용한 컨테이너 빌드·운영·검증
- Git 로컬 버전관리 및 GitHub 원격 협업 플랫폼 연동

---

## 2. 실행 환경

| 항목 | 내용 |
|------|------|
| **OS** | Linux (WSL2) — `6.18.54.1-microsoft-standard-WSL2` |
| **아키텍처** | x86_64 |
| **쉘/터미널** | bash (WSL2 Ubuntu) |
| **Docker 버전** | 29.9.2 (build a82e528) |
| **Docker 엔진** | Docker Desktop (Server Version 29.9.2) |
| **Git 버전** | 2.53.0 |
| **작업 디렉토리** | `~/projects/test` |

### 2-1. 환경 확인 명령 및 결과

```bash
$ uname -a
Linux GNIA-C201-F02 6.18.54.1-microsoft-standard-WSL2 #1 SMP PREEMPT_DYNAMIC \
Tue Oct 6 19:13:07 UTC 2026 x86_64 GNU/Linux
---

## 3. 수행 항목 체크리스트

| # | 항목 | 완료 여부 |
|---|------|-----------|
| 1 | 터미널 기본 명령어 실습 (pwd, ls, mkdir, touch, cp, mv, rm) | ✅ |
| 2 | 파일 권한 확인 및 변경 (chmod) | ⬜ |
| 3 | Docker 설치 확인 및 기본 점검 | ⬜ |
| 4 | Docker 기본 운영 명령 실습 | ⬜ |
| 5 | 컨테이너 실행 실습 (nginx, ubuntu 등) | ⬜ |
| 6 | Dockerfile 작성 및 커스텀 이미지 빌드 | ⬜ |
| 7 | 포트 매핑 및 브라우저 접속 확인 | ⬜ |
| 8 | Docker 볼륨 영속성 검증 | ⬜ |
| 9 | Git 초기 설정 및 GitHub 원격 연동 | ⬜ |

---

## 4. 터미널 조작 로그

### 4-1. 현재 위치 확인

```bash
$ pwd
/home/viablevision3530/projects/test
```

### 4-2. 파일 목록 확인 (실습 전)

```bash
$ ls -la
total 24
drwxr-xr-x 3 viablevision3530 viablevision3530 4096 Nov  9 17:35 .
drwxr-xr-x 4 viablevision3530 viablevision3530 4096 Nov  9 17:35 ..
drwxr-xr-x 8 viablevision3530 viablevision3530 4096 Nov  9 17:36 .git
-rw-r--r-- 1 viablevision3530 viablevision3530 4664 Nov  9 17:35 .gitignore
-rw-r--r-- 1 viablevision3530 viablevision3530 2108 Nov  9 17:44 README.md
```

### 4-3. 폴더 및 파일 생성·복사·이동·삭제

```bash
$ mkdir practice_dir          # 폴더 생성
$ touch practice_file.txt     # 빈 파일 생성
$ cp practice_file.txt practice_file_copy.txt  # 파일 복사
$ mv practice_file_copy.txt renamed_file.txt   # 파일 이름 변경
$ cat practice_file.txt       # 파일 내용 확인 (빈 파일이므로 출력 없음)
$ rm renamed_file.txt         # 파일 삭제
```

### 4-4. 파일 목록 확인 (실습 후)

```bash
$ ls -la
total 28
drwxr-xr-x 4 viablevision3530 viablevision3530 4096 Nov  9 17:52 .
drwxr-xr-x 4 viablevision3530 viablevision3530 4096 Nov  9 17:35 ..
drwxr-xr-x 8 viablevision3530 viablevision3530 4096 Nov  9 17:36 .git
-rw-r--r-- 1 viablevision3530 viablevision3530 4664 Nov  9 17:35 .gitignore
-rw-r--r-- 1 viablevision3530 viablevision3530 2108 Nov  9 17:44 README.md
drwxr-xr-x 2 viablevision3530 viablevision3530 4096 Nov  9 17:51 practice_dir
-rw-r--r-- 1 viablevision3530 viablevision3530    0 Nov  9 17:51 practice_file.txt
```
---

## 5. 권한 실습

### 5-1. 권한(Permission) 개념

리눅스의 파일 권한은 **소유자 / 그룹 / 기타 사용자** 3가지로 구분된다.

```
-  rw-  r--  r--
│   │    │    │
│   │    │    └── 기타 사용자 권한
│   │    └── 그룹 권한
│   └── 소유자 권한
└── 파일 종류 (- = 파일, d = 폴더)
```

| 기호 | 의미 | 숫자 값 |
|------|------|---------|
| `r` | 읽기 (Read) | 4 |
| `w` | 쓰기 (Write) | 2 |
| `x` | 실행 (Execute) | 1 |
| `-` | 권한 없음 | 0 |

### 5-2. 권한 변경 실습

```bash
# 변경 전 권한 확인
$ ls -la practice_file.txt
-rw-r--r-- 1 viablevision3530 viablevision3530 0 Nov  9 17:51 practice_file.txt
#  ↑ 644: 소유자(rw-) 그룹(r--) 기타(r--)

# 755로 변경 (실행 권한 추가)
$ chmod 755 practice_file.txt
$ ls -la practice_file.txt
-rwxr-xr-x 1 viablevision3530 viablevision3530 0 Nov  9 17:51 practice_file.txt
#  ↑ 755: 소유자(rwx) 그룹(r-x) 기타(r-x)

# 644로 복구 (원래 상태)
$ chmod 644 practice_file.txt
$ ls -la practice_file.txt
-rw-r--r-- 1 viablevision3530 viablevision3530 0 Nov  9 17:51 practice_file.txt
#  ↑ 644: 소유자(rw-) 그룹(r--) 기타(r--)
```

### 5-3. 권한 변경 요약

| 단계 | 명령어 | 권한 기호 | 숫자 |
|------|--------|-----------|------|
| 초기 상태 | — | `-rw-r--r--` | 644 |
| 실행 권한 추가 | `chmod 755` | `-rwxr-xr-x` | 755 |
| 원래대로 복구 | `chmod 644` | `-rw-r--r--` | 644 |
---

## 6. Docker 환경 점검

### 6-1. Docker 버전 확인

```bash
$ docker --version
Docker version 29.9.2, build a82e528
```

### 6-2. Docker 시스템 정보

```bash
$ docker info
```

| 항목 | 값 |
|------|-----|
| Docker 버전 | 29.9.2 |
| 컨테이너 수 | 3개 (실행 중: 1, 정지: 2) |
| 이미지 수 | 2개 |
| 스토리지 드라이버 | overlayfs |
| 운영체제 | Docker Desktop (WSL2) |
| CPU | 24코어 |
| 메모리 | 15.25 GiB |
| 아키텍처 | x86_64 |
| 플러그인 | buildx, compose, scout 등 |

### 6-3. 컨테이너 목록 확인

```bash
$ docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS                    NAMES
3cb4d26975db   hello-world   "/hello"   32 minutes ago   Exited (0) 32 minutes ago                            serene_hellman
d87907d6bb4e   nginx         "/docker…" 34 minutes ago   Up 34 minutes               0.0.0.0:8080->80/tcp     wonderful_galois
bc3db0776aa7   hello-world   "/hello"   35 minutes ago   Exited (0) 35 minutes ago                            quirky_mcclintock
```

| 컨테이너 이름 | 이미지 | 상태 | 포트 |
|--------------|--------|------|------|
| wonderful_galois | nginx | ✅ 실행 중 | 8080 → 80 |
| serene_hellman | hello-world | ⏹ 정상 종료 (0) | — |
| quirky_mcclintock | hello-world | ⏹ 정상 종료 (0) | — |

### 6-4. 이미지 목록 확인

```bash
$ docker images
IMAGE                ID             DISK USAGE   CONTENT SIZE
hello-world:latest   c567b231f656       25.9kB         9.49kB
nginx:latest         165ddde8751e          243MB        66.8MB
```

| 이미지 | 용도 | 크기 |
|--------|------|------|
| hello-world | Docker 동작 확인용 테스트 이미지 | 25.9 kB |
| nginx | 웹 서버 (포트 8080 서비스 중) | 243 MB |