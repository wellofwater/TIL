# Git

> Git은 분산형 버전관리시스템(DVCS) 중 하나이다.

## Git 사전 준비

> Git을 사용하기 전에 커밋을 남기는 사람에 대한 정보를 설정한다(최초 1회)

```bash
$ git config --global user.name 'wellofwater'
$ git config --global user.email 'cumulonimbus96@gmail.com'
```

- 추후에 commit을 하면, 작성한 사람(author)로 저장된다.
- email 정보는 github에 등록된 이메일로 설정을 하는 것을 추천(잔디밭)
- 설정 내용을 확인하기 위해서는 아래의 명령어를 입력한다.

```bash
$ git config --global -l
user.email=cumulonimbus96@gmail.com
user.name=wellofwater
```

> git bash 설치 [링크](https://gitforwindows.org/)

## 기초 흐름

> 작업 -> add -> commit

### 0. 저장소 설정

```bash
$ git init
Initialized empty Git repository in C:/Users/LSR/Desktop/gittest/.git/
```

- git 저장소를 만들게 되면 해당 디렉토리 내에 `.git/` 폴더가 생성
- git bash에서는 `(master)`로 현재 작업중인 브랜치가 표기된다.

### 1. `add`

> 커밋을 위한 파일 목록 (staging area)

```bash
$ git add .				# 현재 디렉토리의 모든 파일 및 폴더
$ git add a.txt			# 특정 파일
$ git add md-images/	# 특정 폴더
$ git status
# master 브랜치에 있다.
On branch master
# 커밋이 될 변경사항들(changes)
# Staging area 단계
Changes to be committed:
  # unstage를 하기 위해서... 명령어
  # working directory 단계
  (use "git restore --staged <file>..." to unstage)
        new file:   b.txt
        
# 트래킹 X 파일들
# git으로 아직 관리를 하지 않음
# working directory 단계
Untracked files:
  # 커밋이 될 것에 추가하려면
  # staging area로 보내려면 commit
  (use "git add <file>..." to include in what will be committed)
        c.txt

```

### 2. `commit`

> 버전을 기록(스냅샷)

```bash
$ git commit -m '커밋메시지'
```

- 커밋 메시지는 현재 버전을 알 수 있도록 명확하게 작성한다.

- 커밋 이력을 남기기 확인하기 위해서는 아래의 명령어를 입력한다.

  ```bash
  $ git log
  $ git log -1			# 최근 한개의 버전
  $ git log --oneline		# 한줄로 간단하게 표현
  $ git log -1 --oneline
  ```

## 상태 확인

> git에 대한 모든 정보는 status에서 확인할 수 있다.

```bash
$ git status
```



## 버전별 기록

- sql, java 등 여러 작업 파일이 동시에 생성

- 버전을 따로 기록한다.
  - 'DB 작업'
  - '스프링 코드'

```bash
$ git add a.sql
$ git commit -m 'DB작업'
$ git add b.java
$ git commit -m '스프링코드'
```

- 폴더로 생성시

```bash
$ git add DB/
$ git commit -m 'DB작업'
$ git add SPRING/
$ git commit -m '스프링코드'
```



## 원격 저장소 활용하기

> 원격 저장소를 제공하는 서비스는 github, gitlab, bitbucket 등이 있다.

### 1. 원격 저장소 설정하기

```bash
$ git remote add origin {URL}
```

- 깃아, 원격(remote)저장소로 추가해줘(add) origin이라는 이름으로 URL을
- 원격저장소 삭제하기 위해서는 아래의 명령어를 사용한다.

```bash
$ git remote rm origin
```



### 2. 원격 저장소 확인하기

```bash
$ git remote -v
origin  https://github.com/wellofwater/git-test.git (fetch)
origin  https://github.com/wellofwater/git-test.git (push)
```



### 3. push

```bash
$ git push origin master
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 257 bytes | 128.00 KiB/s, done.
Total 2 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/wellofwater/git-test.git
   621cf38..50c36e0  master -> master
```

>  GitHub은 버전들을 업로드하는 것이다.