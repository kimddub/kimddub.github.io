---
layout: default
title: draft-2
grand_parent: Blog
parent: draft
permalink: /docs/blog/draft/note
nav_order: 2
---

작성목록
===========

퍼블리싱 
- 블로그 퍼블리싱 히스토리/ 저작권 또는 소스코드 등 오픈하기

0. 로컬 DB
[DB접근]
mysql -u root -p

[DB서버제어]
- 설정 초기화 mysqld --initialize-insecure
- 서비스 등록 "C:\Program Files\MySQL\MySQL Server 8.0\bin\mysqld.exe" --install
- 서비스 시작 net start mysql
- 서비스 종료 net stop mysql


CLI vs GUI

local vs remote 환경

1.
터미널 콘솔 쉘 출처:https://blog.naver.com/asianchairshot/221383363419
"다 까만 화면"
- 콘솔 : 서버 로컬장치의 입출력 장치(물리적 터미널)
- 터미널 : 서버 로컬 또는 원격으로 접속 가능한 콘솔 소프트웨어
- 쉘 : 실제로 명령어를 해석하고 OS와 주고받는 프로그램 (bash, sh, csh...)

2.
쉘(shell) 
커널(kernel)과 사용자간의 다리. 터미널 역할

powershell : .NET FramwWork 사용, cmdlet 명령어 체계 (리눅스, bash, ...)
- 최근 소프트웨어(명령어 多)
- 객체지향언어로 재사용 등 다양한 작업 가능
- 디지털서명 통한 신리된 스크립트로써 배포 가능
cmd(명령 프롬프트) : 절차적언어 (dos, ...)

명령어
- cd
- ls
- curl : 프로토콜을 이용하여 네트워크 명령 전송(HTTP, HTTPS, FTP, ...)

3.
-RubyGems에서 설치 -업데이트가 쉽고 관련없는 프로젝트 파일을 격리하여 작성에 집중할 수 있습니다.
-GitHub의 포크 -사용자 지정 개발에 편리하지만 업데이트하기 어렵고 웹 개발자에게만 적합합니다.

-깃 페이지(설치없이) 
(1) jekyll remote theme로 ruby 설치없이 실행

-깃 페이지(설치) 
(https://poiemaweb.com/jekyll-basics)
(https://jehuipark.github.io/blog/jekyll_blog_setup)
(https://rwb0104.github.io/posts/2020/08/install-jekyll/)

(1) repository clone
(2) ruby 설치 https://smartbase.tistory.com/42
*간단한 학습목적일 때 WSL 대신 windows 환경에 RubyInstaller를 사용해 Ruby 설치도 가능(https://smartbase.tistory.com/42)
(2-1) WSL(Windows Subsystem for Linux) 설치(리눅스용 윈도우 하위 시스템, 윈도우 운영체제와 통합된 리눅스 배포?, 가상환경 플랫폼 구성 요소 세팅)
VMWare나 VirtualBox를 통해 가상환경에서 리눅스를 실행했다면 WSL로 윈도에서 리눅스 가상환경 설치 가능하도록 구성하는 시스템?
(2-1-1) windows 기능 켜기/끄기 > Linux용 Windows 하위 시스템 활성화
(2-1-2) windows store > ubuntu 설치 > 터미널
(2-1-3) 명령어 
- apt-get : 우분투를 포함한 데비안 계열의 리눅스 패키지 관리 명령어 도구
- echo : 문자열 사이 \와 조합되는 escape 문자를 "로 묶어 인식(??), 환경변수 출력시 사용하며 [값] 자리에 $[환경변수] 와 같이 사용하면 값으로 출력
- export : 환경변수로 사용 [변수]=[값] 
	# rbenv 설치하기 
	git clone https://github.com/rbenv/rbenv.git ~/.rbenv 
	echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc 
	echo 'eval "$(rbenv init -)"' >> ~/.bashrc 
	exec $SHELL
	#환경변수 추가
	echo 'export PATH="[경로]:$PATH"' >> ~/.bashrc 
	exec $SHELL
▽우분투 환경에서 계속 진행(Ruby 런타임)
(2-2) 가상환경 설치(프로그래밍 언어 버전별 환경관리를 위해?) 
- rvm or rbenv(더 가벼운 실행환경)
(2-3) ruby build 설치
(2-4) ruby 설치 *rbenv 경로 하위로 이동해야 가능*
- gem update --system
- gem package 설치
(3) jekyll 설치
(4) jekyll site 생성
bundle exec jekyll serve
(5) 테마 적용 가이드 https://devlopr.netlify.app/get-started/#/

*가상환경에서 해봤자 퍼블리싱 할 에디터가 없다..
(6) WSL에서 VSCODE 작동방법
(6-1) VSCode > Extension > Remote-WSL 설치
(6-2) 리눅스 배포판에서 VSCode 서버 시작 시 필요한 라이브러리 없으면 패키지매니저 최신화
- sudo apt-get update
Wget (웹 서버에서 콘텐츠 검색) 및 ca 인증서 (SSL 기반 응용 프로그램에서 SSL 연결의 신뢰성을 확인할 수 있도록 하려면)를 추가해야 함
- sudo apt-get update, sudo apt-get install wget ca-certificates
(6-3) code 설치를 해야하는데 애가 알아듣지를 못함
>> https://yjcode.tistory.com/1
>> (옛날방법이어서 안되는거였음)microsoft 사의 GPG키를 VSCode 다운할 저장소 경로에 추가해준 후 패키지 업데이트 하면 설치가능해짐.....
sudo sh -c 'curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > /etc/apt/trusted.gpg.d/microsoft.gpg'
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
sudo apt-get update (아까랑 다르게 새로운 업데이트 진행)
sudo apt-get install code (아까는 code라는 항목 찾지못했었음) >> 우분투 환경에 vscode 활성화돼있음
sudo rm /etc/apt/sources.list.d/vscode.list (추가했던 저장소 삭제)
>> WSL2 설치
- ??https://www.44bits.io/ko/post/wsl2-install-and-basic-usage
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
- 커널 업데이트 패키지 설치(파워쉘) 및 활성화
x64 머신용 최신 WSL2 Linux 커널 업데이트 패키지 설치
- WSL2 적용
wsl --set-version Ubuntu 2
- WSL2 기본 세팅
wsl --set-default-version 2
- 강제종료 후 버전 확인
wsl -t Ubuntu
wsl -l -v
(6-4) code . 입력시 윈도우 vscode에서 wsl 상의 경로로 열림
드디어..ㅠㅠㅠ 버전 올리라고 말을 해주지..

(7) 새 테마 gem으로 적용하기
항목 별 설명 > https://jekyllrb-ko.github.io/tutorials/using-jekyll-with-bundler/
(7-1) 번들 초기화 : 프로젝트 경로 생성 후 
bundle init
(7-2) 번들 환경설정
bundle config set path 'vendor/bundle'
(7-3) Jekyll 추가
bundle add jekyll
(7-4) Jekyll 뼈대 생성
bundle exec jekyll new --force --skip-bundle .
bundle install
(7-5) 서버 실행
bundle exec jekyll serve
(7-6) 테마적용
> https://github.com/cotes2020/jekyll-theme-chirpy


*번들러 버전 바꿔야할 때
Bundler could not find compatible versions for gem "bundler":
  In Gemfile:
    rails (= 3.0.0) ruby depends on
      bundler (~> 1.0.0) ruby

  Current Bundler version:
    bundler (1.1.5)

This Gemfile requires a different version of Bundler.
> gem install bundler -v '~> 1.0.0'
> bundle _1.0.22_ install


*WSL ubuntu 비밀번호 재설정
1. 윈도우 cmd
	$> ubuntu.exe config --default-user root
	우분투 실행 시 root 권한으로 실행 설정

2. 우분투 실행
	$> passwd $[사용자계정]
	비밀번호 변경

3. 윈도우 cmmc
	$> ubuntu.exe config --default-user [사용자계정]
	다시 사용자 계정으로 실행하도록 설정

4.
wsl 관련?
- 가상환경 용량?
- 드라이브 마운트 https://webdir.tistory.com/542
: 특정 경로에서 파워쉘 혹은 cmd를 실행하면 자동으로 wsl에 마운트되도록 하는 작업




/home/kimddub/.rbenv/plugins/ruby-build/bin:/home/kimddub/.rbenv/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games


