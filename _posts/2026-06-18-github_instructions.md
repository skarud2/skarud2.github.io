---
title: "깃허브 초기 설정 및 SSH 연결 정리"
description: 맨날 까먹는 깃허브 초기 환경 설정 방법을 정리해본다.
author:
date: 2026-06-18 15:45:00 +0900
categories: [Study, github]
tags: [github]
pin: false
image:
---

<br>
### SSH 연결하기
#### 1. SSH 키 생성
`ssh-keygen -t ed25519 -C "[이메일 주소]" -f ~/.ssh/[키 이름]`
<br><br>
키 생성 확인<br>
`ls -al ~/.ssh/`
<br>
<br>
#### 2. public key를  깃허브 ssh키에 등록
public key 확인명령어<br>
`cat ~/.ssh/[퍼블릭키 이름].pub`
<br>
#### 3. SSH config 파일 설정
계정별로 호스트를 구분하기 위해 ~/.ssh/config 파일을 생성 및 수정한다.<br>
`vim ~/.ssh/config`
```terminal
Host github-main
    HostName github.com
    User git
    IdentityFile ~/.ssh/[퍼블릭키 이름]
```
<br>
원격 깃허브 계정과 연결 확인<br>
`ssh -T git@github-main`
<br>
<br>
#### 4. 레포지토리 연결 
SSH 방식<br>
`git remote add origin git@[config파일에 설정해놓은 이름]:[계정이름]/[원격저장소이름].git`<br>
 여기서 config파일에 설정해놓은 이름 =>github-main
 <br><br>
https 방식<br>
`git remote add origin https://github.com/계정이름/board-practice.git`
<br><br>
연결 확인<br>
`git remote -v`
<br><br>
처음 push<br>
`git push -u origin main`
<br><br>
이후 push는 그냥 `git push` 로 가능
<br>
<br>

#### 5. 클론 (SSH 방식)
`git clone git@github-sub:[계정이름]/[레포지토리 이름].git (저장할 폴더 이름)`
<br><br>
여러 깃허브 계정 사용시 해당 폴더 안에서만 쓸 사용자 정보를 설정해야 한다.<br>
```terminal
# 해당 프로젝트 폴더에서
git config user.name "사용자이름"
git config user.email "이메일주소"
```
<br>
<br>
<br>
### 명령어
#### 1. 로그 확인
로그 한줄 요약 (커밋 기록 보기)<br>
`git log --oneline` 
<br><br>
커밋 내역이 예쁘게 트리 구조로 나오게 하는 방법<br>
=> `git lg`로 조회 가능하도록 단축어 설정<br>
`git config --global alias.lg "log --oneline --graph --all”`
<br><br>
커밋 취소한 로그까지 모두 보기<br>
`git reflog` 
<br>
<br>
#### 2. 취소
add된 파일 취소<br>
`git rm --cached [파일명]` 
<br><br>
커밋만 취소(add된 상태로)<br>
`git reset --soft HEAD~1` 
<br><br>
커밋+add된 거 취소<br>
`git reset --mixed HEAD~1` 
<br><br>
커밋 +add+파일 내용까지 취소(파일 수정 후 저장한 것도 취소) ⇒ 완전 되돌리기<br>
`git reset --hard HEAD~1` 
<br><br>
취소 취소<br>
`git reset --hard [해시]` 
<br><br>
변경사항 확인<br>
`git diff` 
<br>
<br>
<br>
### 브랜치
#### 1. 새로운 브랜치 생성
로컬에서 새로운 브랜치 만들고 그 브랜치로 바꾸기 (-c:`--create`의 약자)<br>
`git switch -c [브랜치명]` 
<br><br>
원격에 새로 만든 브랜치 알려주기<br>
`git push origin develop` 
<br><br>
원격 브랜치 확인<br>
`git branch -r` 
<br><br>
원격, 로컬 브랜치 확인<br>
`git branch --all` 
<br><br>
#### 2. 브랜치 삭제
로컬에 있는 브랜치 삭제<br>
`git branch -D [브랜치명]`  
<br><br>
원격에 있는 브랜치 삭제<br>
`git push origin --delete` 

