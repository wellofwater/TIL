# Amazon EC2 Linux Instans

> Amazon EC2; Amazon Elastic Compute Cloud: AWS 클라우드의 가상 서비스



### 1. 인스턴스 생성: [AWS 설명서](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/EC2_GetStarted.html)

### 2. 보안 그룹 설정

- 생성한 인스턴스의 보안 그룹을 선택 -> 인바운드 규칙 편집 선택

  ![image-20201106174454565](C:%5CUsers%5CLSR%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20201106174454565.png)

- 유형 SSH 선택한 후, 접속할 IP 주소를 입력한다 (test 중이기에 위치 무관 선택)

  ![image-20201106174652265](C:%5CUsers%5CLSR%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20201106174652265.png)

### 3. 인스턴스 연결

- 인스턴스를 선택 -> 연결

- 사용자 이름은 `ec2-user`가 기본값

  ![image-20201106175720513](C:%5CUsers%5CLSR%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20201106175720513.png)



#### 1. window 내장 openSSH

1. [시작] 우클릭 -> [앱 및 기능] -> [선택적 기능] -> [기능 추가]

2. `OpenSSH 서버`, `OpenSSH 클라이언트` 설치

3. 재부팅

   ```bash
   ssh -i C:\dev\key\awsFisrt.pem ec2-user@[public ip address]
   ```

   

#### 2. PuTTY

1. PuTTYgen 실행: 키생성 프로그램

   1. Load -> `키페어.pem` 파일 클릭
   2. 키 타입: RSA 선택, 없을 경우 SSH-2 RSA
   3. Save private key 클릭

2. PuTTY 실행

   1. Session
      - HostName : username@ipaddress
      - Port : 22
      - Saved Sessions: 현재 세션 정보를 저장할 수 있다
   2. Connection -> SSH -> Auth
      - Private key ...: PuTTYgen에서 저장한 `.ppk` 파일 선택
   3. Connection -> SSH -> Tunnels : VNC 서버 연결시 설정

   ![image-20201106181950250](C:%5CUsers%5CLSR%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20201106181950250.png)



### 4. GUI 설치: [AWS 설명서](https://aws.amazon.com/ko/premiumsupport/knowledge-center/ec2-linux-2-install-gui/)



### 5. VNC viewer 설치 및 실행

- JDK, Eclipse, Tomcat 설치