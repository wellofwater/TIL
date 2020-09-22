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

- origin 원격저장소의 master 브래치로 push



# GitHub

>  GitHub은 버전들을 업로드하는 것이다.



# git status

```bash
On branch master
# 2) 
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    1.txt
        modified:   README.md
# 1) untracked
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        new.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

- working directory
  - untracked - 깃이 관리하지 않고 있는 파일
    - 파일 생성(new file) 등
  - tracked - 이전 커밋에 포함된 적 있는 파일
    - modified - modified / deleted
    - unmodified - 수정 X (status에 안 뜸)



# 원격 저장소 활용하기

## 충돌상황

- 원격 저장소의 이력과 로컬 저장소의 이력이 다르다.

```bash
$ git push origin master
To https://github.com/wellofwater/remote.git
 ! [rejected]        master -> master (fetch first)
# 에러!
error: failed to push some refs to 'https://github.com/wellofwater/remote.git'
# rejected(거절) - 원격저장소의 작업이 로컬에 없다.
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
# 너는 원할 것 같다. 원격저장소의 변경사항을 먼저 통합(integrate) 다시 push하기 전에
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

- 해결방법

  ```bash
   $ git pull origin master
  ```

  - 이렇게 하면, vim 창으로 커밋 메시지를 작성하도록 뜬다.

    ```bash
  Merge branch 'master' of https://github.com/edutak/remote into master
    
    ```
  # Please enter the commit message for your changes. Lines starting
    # with '#' will be ignored, and an empty message aborts the commit.
    #
    # Date:      Fri Sep 18 10:11:30 2020 +0900
    #
    # On branch master
    # Your branch is up to date with 'origin/master'.
    #
    # Changes to be committed:
    #       new file:   remote.txt
    #
    ```
  
  - 자동으로 작성된 커밋메시지를 확인하고, `:wq` 로 저장하고 나간다.
  
  - 그 이후 log를 확인하고, push를 한다.
  
    ```bash
    $ git log --oneline
    9becb7f (HEAD -> master) Merge branch 'master' of https://github.aster
    ff12057 로컬에서 작업함
    98e57d9 (origin/master) Create remote.txt
    548230d Update
    a526519 Update
    bb71743 Add README
    ```






## [CS50](http://ide.cs50.io/)

```bash
$ git pull origin master	# 실행시 기본 편집기가 nano로 되어 있다

$ git config --global core.editor "vim"		# vim 편집기로 실행
```





# Git 명령어 취소

1. add 취소

   ```bash
   $ git add .
   $ git status
   On branch master
   Changes to be committed:
    # unstage.. add를 취소하려면
    # git restore --staged
     (use "git restore --staged <file>..." to unstage)
           new file:   a.txt
           new file:   b.txt
   ```

   ```bash
   $ git restore --staged <파일명>	# 최신
   $ git reset HEAD <파일명>			# 구버전 (2019)
   ```

   ```bash
   $ git restore --staged b.txt
   $ git status
   On branch master
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           new file:   a.txt
   
   Untracked files:
     (use "git add <file>..." to include in what will be committed)
           b.txt
   ```

2. `commit` 메시지 변경

   > 주의! 커밋 메시지를 변경하면, hash 값이 변경된다.
   >
   > 즉, 공개된 저장소에 push를 한 이후에는 하지 않는

   ```bash
   $ git commit --amend
   ```

   - vim 편집기 창에서 직접 메시지를 수정하고 저장

     ```bash
     $ git log --oneline
     03ca0cb (HEAD -> master) c.txt 추가
     538d35d Add b.txt
     3867506 Add a.txt
     76b305f README
     
     $ git commit --amend
     [master 30ca836] Add c.txt
      Date: Fri Sep 18 16:12:38 2020 +0900
      1 file changed, 0 insertions(+), 0 deletions(-)
      create mode 100644 c.txt
     
     $ git log --oneline
     30ca836 (HEAD -> master) Add c.txt
     538d35d Add b.txt
     3867506 Add a.txt
     76b305f READMExxxxxxxxxx
     
     $ git log --oneline
     03ca0cb (HEAD -> master) c.txt 추가
     538d35d Add b.txt
     3867506 Add a.txt
     76b305f README
     
     $ git commit --amend
     [master 30ca836] Add c.txt
     Date: Fri Sep 18 16:12:38 2020 +0900
     1 file changed, 0 insertions(+), 0 deletions(-)
     create mode 100644 c.txt
     
     $ git log --oneline
     30ca836 (HEAD -> master) Add c.tx
     t538d35d Add b.txt
     3867506 Add a.txt
     76b305f README
     ```

3. 커밋 변경

   > 주의! 커밋 메시지를 변경하면, hash 값이 변경된다.
   >
   > 즉, 공개된 저장소에 `push`를 한 이후에는 하지 않는다.
   >
   > 2번과 동일한 명령어

   - 예) 내가 어떠한 파일을 빠뜨리고 커밋했을 때

     ```bash
     $ touch d.txt
     $ touch omit.txt
     $ git add d.txt
     $ git commit -m 'Add d&omit'
     $ git status On branch master Untracked files:
       (use "git add ..." to include in what will be committed)
       		omit.txt
        nothing added to commit but untracked files present
        		(use "git add" to track)
        		
     ```

   - 해결 방법

     ```bash
     $ git add omit.txt
     $ git status
     On branch master
     Changes to be committed:
       (use "git restore --staged <file>..." to unstage)
             new file:   omit.txt
     $ git commit --amend
     [master 7844532] Add d & omit
      Date: Fri Sep 18 16:20:18 2020 +0900
      2 files changed, 0 insertions(+), 0 deletions(-)
      create mode 100644 d.txt
      # 들어가있죠!
      create mode 100644 omit.txt
     $ git status
     On branch master
     nothing to commit, working tree clean
     ```

4. `working directory` 변경사항 삭제

   > 주의! 아래의 명령어를 입력하면 절대 과거로 돌아갈 수는 없다.
   >
   > 커밋한 내용만 복구 가능

   ```bash
   $ git restore <파일명>
   ```

   ```bash
   $ git status
   On branch master
   Changes not staged for commit:
     (use "git add/rm <file>..." to update what will be committed)
     # WD 있는 변경사항들을(changes) 버리기 위해서는...
     # git restore <파일>
     (use "git restore <file>..." to discard changes in working directory)
           deleted:    README.md
           deleted:    a.txt
           deleted:    b.txt
           deleted:    c.txt
           deleted:    d.txt
           deleted:    omit.txt
   
   no changes added to commit (use "git add" and/or "git commit -a")
   ```

   ```bash
   $ git restore .
   $ git status
   On branch master
   nothing to commit, working tree clean
   ```



## 이력 되돌리기 (reset vs revert)

- 두 명령어는 특정 시점의 상태로 커밋을 되돌리는 작업을 한다.

- `reset` : 이력을 삭제

  - `--mixed` : (default) 해당 커밋 이후 변경사항을 보관

  - `--hard` : 해당 커밋 이후 변경사항을 모두 삭제 (주의!)

  - `--soft` : 해당 커밋 이후 변경사항 및 WD 내용까지 보관

    ```bash
    $ git log --oneline
    da0ae77 (HEAD -> master) Update README
    7844532 Add d & omit
    30ca836 Add c.txt
    $ git reset 7844532
    $ git log --oneline
    7844532 (HEAD -> master) Add d & omit
    30ca836 Add c.txt
    ```

- `revert` : 되돌렸다는 이력을 남긴다.

  ```bash
  $ git revert 
  Removing omit.txt
  Removing d.txt
  [master f050b8e] Revert "Add d & omit"
   2 files changed, 0 insertions(+), 0 deletions(-)
   delete mode 100644 d.txt
   delete mode 100644 omit.txt
   
  $ git log --oneline
  f050b8e (HEAD -> master) Revert "Add d & omit"
  7844532 Add d & omit
  30ca836 Add c.txt
  ```

  

# Stash

> 지금까지 작업했던 내용을 임시적으로 저장하는 공간

## 기본 명령어

- stash 보관

  ```bash
  $ git stash
  ```

- stash 반영

  ```bash
  $ git stash pop	# 반영 + 삭제
  # $ git stash
  ```

  