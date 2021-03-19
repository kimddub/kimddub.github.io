---
layout: default
title: draft-4
grand_parent: Blog
parent: draft
permalink: /docs/blog/draft/draft-4
nav_order: 3
---

Git
===========
### Hook
husky?

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
    git tag [tag명] [체크섬]
    ```

- 원격지에 태그 전송
    로컬에서 생성한 태그가 원격저장소에 전송되지는 않기 때문에 태그도 push가 필요하다.
    ```
    # 원격저장소 태그 push
    git push origin [tag명]
    
    # 원격저장소 태그 전체 push
    git push origin --tags
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

### Log
커밋과 관련된 정보를 보여준다.
```
# 태그된 커밋에 대한 정보
git show [tag명] 

# 간단한 커밋 정보
git log --decorate -l
```

### diff
```
# master 브랜치와 sychronize
git diff master
```

### stash
로컬에서 

### Full-request
develop 권한자가 Upstream Repository의 master 권한자에게 merge 요청을 보내는 작업. 프로젝트를 origin(원격저장소)으로 Fork -> 브랜치 수정 -> push 까지 작업하였을 때, 이를 compare & pull request 하면 master 권한자가 커밋 내용을 확인 후 merge 하는 과정을 거친다.

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


### 작업 트리(Work tree)와 인덱스(Index)
 작업 트리 <-> 인덱스 <-> 로컬저장소 <-> 원격저장소 
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