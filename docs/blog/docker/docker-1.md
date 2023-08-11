---
layout: default
title: "Docker"
grand_parent: Blog
parent: msa
permalink: /docs/blog/docker/docker-1
nav_order: 1
date: 2023-07-07 10:00
---

# Docker

## 설치   
- - -
### Window WSL2 설치
윈도우10 이상에는 기본적으로 wsl 명령어가 포함되어 있어 파워셀 관리자 모드에서 아래 명령어를 실행한다.
```shell
    # WSL 활성화
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
    
    # winver 빌드 18362 이상만 지원
    winver
    
    # VM platform 옵션 활성화
    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
    
    # PC 재부팅 후 WSL2 기본 버전 설정
    wsl --set-default-version 2
    
    # 명령어 안먹는 경우 윈도우 하위 시스템 설정 
    wsl --install -d Ubuntu --web-download
    
    # Microsoft Store > Ubuntu 배포판 설치
    # Ubuntu 구동 시 오류 발생하면 커널 업데이트 패키지 다운로드
    # 설치 버전 확인
    wsl -l -v     
```

> WSL2로 리눅스를 사용하고자 하는 경우, 배포판 설치 등의 추가 설정 필요  

> [참고] [hanjuli94님 블로그](https://velog.io/@hanjuli94/%EC%9C%88%EB%8F%84%EC%9A%B0%EC%97%90%EC%84%9C-%EB%8F%84%EC%BB%A4-%EC%8B%A4%EC%8A%B5%ED%95%98%EA%B8%B0)

### WSL2 Docker 설치
