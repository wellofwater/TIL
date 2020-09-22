## Branch 기초 명령어

- 브랜치 생성

  ```bash
  $ git branch {브랜치이름}
  ```

- 브랜치 목록

  ```bash
  $ git branch
  * master
    test $
  ```

- 브랜치 생성 및 이동

  ```bash
  $ git checkout -b test2
  Switched to a new branch 'test2'
  (test2) $
  ```

- 브랜치 병합

  ```bash
  $ git merge test2
  ```

- 브랜치 삭제

  ```bash
  $ git branch -d test2
  Deleted branch test2 (was 55be5e7).
  ```





## pull request

1. branch Push

   ```bash
   $ git checkout -b feature/chtting
   $ touch chatting.html
   $ git add .
   $ git commit -m 'Complete chat'
   $ git push origin feature/chatting
   ```

2. Pull Request 생성(요청)

3. merge

   - 확인:: 리더급