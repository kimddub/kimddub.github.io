---
layout: default
title: draft-4
grand_parent: Blog
parent: draft
permalink: /docs/blog/draft/git
nav_order: 3
---

# Git

### bare repostory
서버에 git 설치
```
# 패키지 리스트를 업데이트합니다.
sudo apt-get install git
# git 설치
sudo apt install git
# 설치된 git 버전 확인
git --version
# git repository 디렉토리 생성 후 init
git init --bare remote
```

### Hook
husky?
특정 동작 단계에 script를 실행하도록 설정해두는 것

### Cherry-pick 
여러개의 브랜치 또는 잘못 커밋된 브랜치에 대해서 새 브랜치에 올바른 커밋만 가져갈 수 있다.
```
git cherry-pick [체크섬]
```

### Tag
커밋에 특정 태그와 주석을 작성한다. 사용 목적은 다양하다고 한다. 예를들어,  release branch에 버전을 태그하는 등의 목적으로 사용 가능하다. 

- 기본 명령어
    ```
    # 태그 조회
    git tag

    # 태그 작성
    git tag [tag명]

    # 태그 및 주석 작성
    git tag -am [주석] [tag명]

    # 태그 삭제
    git tag -d [tag명]

    # 이전 커밋에 태그
    git tag [tag명] [commit id]
    ```

- 원격지에 태그 전송
    로컬에서 생성한 태그가 원격저장소에 전송되지는 않기 때문에 태그도 push가 필요하다.
    ```
    # 원격저장소 태그 push
    git push origin [tag명]
    
    # 원격저장소 태그 전체 push
    git push origin --tags
    ```



### diff
```
# master 브랜치와 sychronize
git diff master
```

### Log
커밋과 관련된 정보를 보여준다.
```
# 상세한 커밋 정보
git log

# 태그된 커밋에 대한 정보
git show [tag명] 

# 간단한 커밋 정보
git log --decorate -1

# 각 커밋 diff 정보
git log -p 

# 최근 두 개 결과
git log -2

# 삭제된 커밋까지 조회
git reflog
```

### 체크섬(Checksum)
Git에서 사용하는 가장 기본적인 데이터 단위. 파일 상태나 커밋 정보 등을 포함한다. Git은 SHA-1 해시를 사용하여 체크섬을 만든다. 40자 길이의 16진수 문자열.
- 세 가지 상태
    1. committed : 데이터가 로컬저장소에 안전하게 저장된 상태
    2. Staged : 수정한 파일을 저장 목록에 올려놓은 상태. 로컬저장소에 저장되기 전.
    3. Modified : 파일을 수정만 한 상태. 로컬저장소에 저장되기 전.

### stash
작업 트리에 수정한 파일이 있는 상태에서 브랜치를 checkout 할 때 사용한다.
일반적으로는 작업 트리의 내용이 checkout 한 브랜치로 옮겨간다.
stash를 사용하면 이전 브랜치의 작업 트리가 저장되고 마지막 커밋상태로 돌아간다. 이후에 stash로 원하는 브랜치에서 불러올 수 있다.

```
# 현재 작업 임시저장
git stash 

# 저장목록 확인
git stash list

# stash 불러오기
git stash apply [stash명]

# 최근 stash 불러오기
git stash apply

# stage 상태까지 반영해서 불러오기
git stash apply --index

# 임시저장 한 작업 되돌리기
git stash pop

# 임시저장 한 작업 삭제
git stash drop

# 임시저장 한 작업 전체삭제
git stash clear
```

### Branch
- 목적에 따른 branch 분류
    - master branch
    제품으로 출시될 수 있는 브랜치
    - develop branch
    다음 출시 버전을 개발하는 브랜치
    - feature branch
    기능을 개발하는 브랜치
    - release branch
    이번 출시 버전을 준비하는 브랜치
    - hotfix branch
    출시 버전에서 발생한 버그를 수정하는 브랜치

- 기본 명령어
    ```
    # 브랜치 조회
    git branch
    # 원격지 브랜치 조회
    git branch -r
    # 전체 브랜치 조회
    git branch -a

    # 원격지 브랜치 참조
    git checkout origin/[브랜치]
    # 원격지 브랜치 가져오기
    git checkout -t origin/[브랜치]
    # 원격지 브랜치 새로운 브랜치명으로 가져오기
    git checkout -b branch2 origin/[브랜치]

    # 로컬 브랜치 삭제
    git branch -d [브랜치]
    # 원격지 브랜치 삭제
    git push origin --delete [브랜치]
    ```

- local 과 remote branch
    ```
    # 로컬에 생성 후 이동
    git checkout -b branch-1

    # 원격지에 생성
    git push origin branch-1 

    # 이미 존재하는 로컬 branch를 원격지 branch 추적 및 연동
    git branch -t origin/[브랜치]
    git branch --set-upstream-to origin/[브랜치]

    ```

    
### Full-request
develop 권한자가 Upstream Repository의 master 권한자에게 merge 요청을 보내는 작업. 프로젝트를 origin(원격저장소)으로 Fork -> 브랜치 수정 -> push 까지 작업하였을 때, 이를 compare & pull request 하면 master 권한자가 커밋 내용을 확인 후 merge 하는 과정을 거친다.

### fork, clone


### pull, fetch, merge, upstream
- pull
    fetch + merge

- fetch
     원격지의 변경점을 가져온다.
    ```
    # 모든 원격지 fetch
    git remote update

    # origin 원격지만 fetch
    git fetch origin
    ```

- merge 
     커밋 또는 브랜치 소스를 합친다.

- upstream
    remote repository를 fork 뜨는 경우 upstream이 기존 원격지이고, 내가 fork 뜬 원격지가 origin이 된다.
    fork repo를 작업하다가 upstream 버전업이 된 경우 fetch 하여 origin에 반영한다.  



### reset, revert, rebase
- reset
    이전 커밋으로 돌아가는 대표적인 방법
    ```
    # 특정 커밋으로 reset
    git reset [commit id]
    # N번째 뒤 커밋으로 reset
    git reset HEAD~1
    # 이동 후 변경사항은 unstaged, staging, commit 시 기존 상태로 돌아오는 옵션
    git reset --soft [commit id]
    # 변경사항 모두 제거 후 이동
    git reset --hard [commit id]
    ```
- rebase
    특정 커밋으로 돌아가 변경사항 남길 시

### status
1.git restore --staged [filename/ path]
로컬에서 modified 된 파일을 이전 커밋 상태로 되돌리는 기능같다.
    ```
    # git bach guide 
    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
    ```

2.git update-index --assume-unchanged [filename (with path)]
원격저장소(서버)에는 파일이 있고 로컬에도 파일이 있지만 로컬에서의 변동 추적을 중지하고 싶은 경우
    ```
    # 무시 취소
    git update-index --no-assume-unchanged [file path]
    # 무시 내용 조회
    git ls-files -v|grep '^h'
    ```

3.git rm --cached [filename]
로컬에 있는 특정 파일의 변동 추적을 중지하고 싶은 경우
만약 원격저장소에 파일이 있다면 원격 저장소에서의 파일은 삭제한다.

4.git rm [filename]
로컬에 있는 특정 파일의 변동 추적을 중지하고 더 나아가 아예 삭제하고 싶은 경우
만약 원격저장소에 파일이 있다면 원격 저장소에서의 파일은 삭제한다.

### 권한설정
```
# password 오류로 Access denied 발생 시 해결
git config --system --unset credential.helper

# 자동 로그인 설정
git config credential.helper store
git push https://...git

# 설정
git config --global user.name "kimddub"
git config --global user.email kimddub@vaiv.kr
```

### commit
- untracked files
    작업 트리의 파일 상태에서 기존에 없던 새로운 소스가 status 내역에 표시된다. 
    ```
    # untraced files 삭제
    git clean -f
    # untraced files 디렉토리까지 삭제
    git clean -fd
    ```
- .gitignore
    특정 경로나 파일을 무시하고 커밋할 수 있는 문서.

    아래 방법은 내 경로에서 파일이 삭제되지는 않지만, 삭제한것으로 간주되어 push됨.
    ```
    # 이미 관리중이던 파일을 무시할 때
    git rm --chached [파일]
    # 경로를 무시할 때
    git rm --chached [경로]/ -r
    ```

### 작업 트리(Work tree)와 인덱스(Index)
 작업 트리(sandbox) <-> 인덱스(staging area) <-> 로컬저장소 <-> 원격저장소 
1. 위의 프로세스에서 내가 방금 수정한 파일이 작업 트리에 속한다.
2. 작업 트리를 add를 통해 staging 시킨 상태가 인덱스에 속한다.
    index에 staging 하는 목적은 수정한 파일의 일부를 선택해서 올릴 수 있는 것이다.
3. 인덱스에 있는 파일들을 commit을 통해 로컬에 저장하면 로컬저장소에 속한다.
4. 로컬저장소에 있는 파일들을 push를 톨해 원격저장소에 저장한다.

### 원격저장소
Upstream Repository 외에도 여러 Repository 를 리모트 할 수 있다.
```
# 원격저장소 추가
git remote add [별명] [URL]

# 원격저장소 현황
git remote -v
```