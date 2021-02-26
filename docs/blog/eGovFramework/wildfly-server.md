---
layout: default
title: WildFly(JBoss) 서버 구동하기
grand_parent: Blog
parent: eGovFramework
permalink: /docs/blog/eGovFramework/wildfly
nav_order: 2
date: 2021-02-17 8:58
---


WildFly(JBoss) 서버 구동하기
============



## WildFly란
JBoss는 JAVA 기반의 오픈 소스 미들웨어 플랫폼.
엔터프라이즈 환경에서 클러스터링, 분산 캐시 등의 기술 제공.
JBoss가 현재 WildFly로 변경(?)되면서,
JDK 버전에 따라 1.7 이하에서 JBoss, 1.8 이상에서 WildFly 이름으로 제공.

### standalone 모드
운영모드를 관리 측면에서 봤을 때 아래와 같은 차이가 있다.
- standalone 모드 : 각 서버 인스턴스별로 관리.
- domain 모드 : 도메인 마스터 서버에서 여러 서버 인스턴스를 중앙 관리 가능.

*참고 : <https://blueyikim.tistory.com/282>*


- - -

## IDE에서 WildFly 애플리케이션 실행

tomcat server 설정을 WildFly 설정으로 세팅하고, message 경로 설정 이슈에 대한 소스를 수정한다. WildFly 서버 환경설정 후 실행한다. 자세한 내용은 아래와 같다.

### 1. pom.xml
pom.xml에 다음과 같이 추가한다.

```
<dependency>
    <groupId>egovframework.rte</groupId>
    <artifactId>egovframework.rte.ptl.mvc</artifactId>
    <version>${egovframework.rte.version}</version>
    <!-- 추가 -->
    <exclusions>
        <exclusion>
            <artifactId>commons-logging</artifactId>
            <groupId>commons-logging</groupId>
        </exclusion>
        <exclusion>
            <artifactId>spring-modules-validation</artifactId>
            <groupId>org.springmodules</groupId>
        </exclusion>
    </exclusions>
    </dependency>
        <dependency>
            <groupId>egovframework.rte</groupId>
            <artifactId>spring-modules-validation</artifactId>
            <version>0.9</version>
    <!-- 끝 -->
</dependency>
```

### 2. jboss-web.xml
WEB-INF/ 하위에 jboss-web.xml 파일 생성 후 다음과 같이 작성한다.
```
<?xml version="1.0" encoding="UTF-8"?>
    <jboss-web>
        <context-root>/</context-root>
    </jboss-web>
```

*참고 : <https://way-be-developer.tistory.com/14>*
    

- - -

### 3. context-common.xml

- - -

### 4. Eclipse에 JBoss 플러그인 설치
Eclipse > help > Market Place > JBoss Tool 4.4 install

*eGovFrame 3.9 환경에서는 marketplac에서 플러그인 설치가 안된다.
이때는 아래의 방법으로 수동으로 install 후 서버 선택 가능하다.
server > new > wildfly 검색해서 install 진행. 또는, Help > Check For Updates 진행을 통해 install 할 수도 있다.

참고 : <https://way-be-developer.tistory.com/9>*

### 5. Server에 WildFly 추가
WildFly 18 버전으로 진행
*최신 버전(WildFly 22.0.1)은 사용중인 Eclipse(2019-12) 버전과 호환이 안된다.*

*참고 : <https://waspro.tistory.com/264>*

- - -

### 6. 관리자 계정

- - - 

## WildFly(JBoss) 애플리케이션 배포

1. WildFly 배포 파일 관리
    - Managed 방식 : JBoss가 애플리케이션 배포 파일 관리
        - Archive 방식 : .war, .jar, ... 등
    - Unmanaged 방식 : 사용자가 애플리케이션 배포 파일 관리
        - Archive 방식
        - Exploded 방식 : 특정 파일만 변경하여 배포하고자 할 경우

2. WildFly 배포 방법
    - 배포 스캐너 이용(standalone 모드만 지원)
        1. 설정 확인
            deploy-scanner 태그에서 배포 폴더 등을 설정한다.
            - 설정문서 : ...\standalone\configuration\standalone.xml
            - default 배포 폴더 : ...\standalone\deployments\
        2. 서버 실행
            ...\bin\standalone.sh 파일 실행.
            배포 폴더로 설정된 경로의 war 목록 자동 배포한다.
        3. 서버 종료
            ``` 
            // WildFly CLI
            $> Ctrl+C 

            // WildFly CLI 종료되었을 때, window shell에서 프로세스 taskkill
            $> netstat -a -n -o -p tcp
            $> taskkill /f /pid [해당 포트의 PID]
            ```
    - CLI 이용
        1. 서버 실행
            ...\bin\standalone.sh 파일 실행.
        2. CLI 실행
            ```
            [disconnected /] connect
            ```
        3. 배포
            Achive 방식이 아닌 Exploded 방식 사용 시 애플리케이션 명과 확장자까지 작성한다. ex) [NAME].[확장자]
            ```
            # 설정
            [standalone@localhost:9999 /] /deployment=[NAME].war:add(runtime-name=“[NAME].war”, content=[{“path”=>”[PATH/NAME].war”, “archive”=>false}])
            # 배포
            [standalone@localhost:9999 /] /deployment=[NAME].war:deploy
            ```
        4. 배포중지
            ```
            # 배포중지
            [standalone@localhost:9999 /] /deployment=[NAME].war:undeploy
            # 배포설정 삭제
            [standalone@localhost:9999 /]/deployment=[NAME].war:remove
            ```

    - Management console 이용

    *참고 : <https://vaert.tistory.com/187>


3. 환경구성 해두고 사용하는 방식
    
*참고 : <https://linux.systemv.pe.kr/jboss-eap-6-standalone-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%84%B1%ED%95%98%EA%B8%B0/>
