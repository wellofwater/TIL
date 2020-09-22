## 빅데이터

> 기존 데이터베이스 관리도구의 데이터 수집, 저장, 관리, 분석하는 역량을 넘어서는 데이터 (데이터의 규모)
>
> 다양한 종류의 대규모 데이터로부터 저렵한 비용으로 가치를 추출하고, 데이터의 빠른 수집, 발굴, 분석을 지원하도록 고안된 차세대 기술 및 아키텍쳐 (업무 수행 방식)

### 3v + 

- Volume (크기) : 대용량 데이터 (수만 테라바이트)
- Velocity (속도) : 큰 용량의 데이터를 빨리 처리해야함
- Variety (다양성) : 계량화 및 수치화가 어려운 비정형적 데이터를 포함함
- Veracity (정확성) : 분석에서 목적에 맞는 데이터를 선별하고 수집하는 것이 분석 결과의 정확성에 영향을 미침
- Value (가치) : 빅데이터를 통해 어떤 문제를 해결할 수 있는가



## 하둡(Hadoop)

> 대용량 데이터를 분산 처리할 수 있는 자바 기반의 오픈소스 프레임워크

- 분산처리 시스템인 GFS(Google File System)와 맵리듀스(MapReduce)를 구현
- 하둡 분산 파일 시스템(HDFS; Hadoop Distributed File System)에 데이터를 저장하고, 분산 처리 시스템인 맵리듀스를 이용해 데이터를 병렬처리



#### 하둡 에코시스템(Hadoop Ecosystem)

> 하둡의 서브 프로젝트가 상용화되며 구성되었다. 하둡 생태계라고도 표현



#### 하둡의 문제점

- 고가용성(HA; High Availability)
- 파일 네임스페이스 제한
- 데이터 수정 불가
  - 기존 데이터에 덧붙임(append)는 가능



## HDFS Architecture (Master / Worker)

- Namenode (NN): Master
  - 메타 데이터 저장 및 관리
- Secondary Namenode (2NN): NN의 백업 (대체X)
- Data Node: 실제 데이터를 저장





### 하둡 설치 방식

- 독립 실행 모드

- 가상 분산 모드 : 한 대의 장비에 Namenode, Secondary Namenode, Data Node를 모두 설치
- 완전 분산 모드 : 여러 대의 장비에 하둡을 설치





## 가상 분산 모드 Hadoop 설치

1. 방화벽 해제

   ```bash
   systemctl stop firewalld
   systemctl disable firewalld
   ```

   

2. hostname, ip 변경

   - test 환경 설정값: ip = 192.168.111.119 / hostname = hadoopserver

   ```bash
   hostnamectl set-hostname hadoopserver
   vi /etc/hosts
   
         4 192.168.111.119 hadoopserver
   ```

   ```bash
   vi /etc/sysconfig/network-scripts/ifcfg-ens33
   
         4 BOOTPROTO="none"
         5 IPADDR="192.168.111.119"
         6 NETMASK="255.255.255.0"
         7 GATEWAY="192.168.111.2"
         8 DNS1="192.168.111.2"
   ```

3. Java 설치

   ```bash
   # oracle.com에서 Linux x64 Compressed Archive(tar.gz) 설치
   tar xvf jdk-8u261-linux-x64.tar.gz 
   mv jdk1.8.0_261 jdk1.8.0
   cp -r jdk1.8.0 /usr/local
   # 기존 java(openjdk) 심볼릭링크 삭제
   cd /usr/bin
   rm java
   # 설치한 java 심볼릭링크 생성
   ln -s /usr/local/jdk1.8.0/bin/java java
   ```

   ```bash
   vi /etc/ptofile
   
        52 JAVA_HOEM=/usr/local/jdk1.8.0
        53 CLASSPATH=/usr/local/jdk1.8.0/lib
        54 export JAVA_HOME CLASSPATH
        55 PATH=$JAVA_HOME/bin:.:$PATH
   ```

   ```bash
   java -version
   java version "1.8.0_261"
   Java(TM) SE Runtime Environment (build 1.8.0_261-b12)
   Java HotSpot(TM) 64-Bit Server VM (build 25.261-b12, mixed mode)
   ```

   

4. hadoop 설치

   ```bash
   cd 다운로드
   wget https://archive.apache.org/dist/hadoop/common/hadoop-1.2.1/hadoop-1.2.1.tar.gz
   tar xvf hadoop-1.2.1.tar.gz
   
   # /usr/local에 복사
   cp -r hadoop-1.2.1 /usr/local
   ```

   ```bash
   #HADOOP_HOME 지정
   vi /etc/profile
   
        52 JAVA_HOME=/usr/local/jdk1.8.0
        53 CLASSPATH=/usr/local/jdk1.8.0/lib
        54 HADOOP_HOME=/usr/local/hadoop-1.2.1
        55 export JAVA_HOME CLASSPATH HADOOP_HOME
        56 PATH=$JAVA_HOME/bin:$HADOOP_HOME/bin:.:$PATH
   
   ```

   

5. 보안 설정

   ```bash
   ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
   cd .ssh
   cat id_dsa.pub >> authorized_keys
   ```

   ```bash
   ssh hadoopserver
   # password 입력 없이 로그인시 보안설정 완료
   # hadoopserver 로그아웃
   exit
   ```

   

6. hadoop 환경설정

   ```bash
   cd /usr/local/hadoop-1.2.1/conf
   ls
   capacity-scheduler.xml      hadoop-policy.xml      slaves
   configuration.xsl           hdfs-site.xml          ssl-client.xml.example
   core-site.xml               log4j.properties       ssl-server.xml.example
   fair-scheduler.xml          mapred-queue-acls.xml  task-log4j.properties
   hadoop-env.sh               mapred-site.xml        taskcontroller.cfg
   hadoop-metrics2.properties  masters
   ```

   ```bash
   vi core-site.xml
   
   <configuration>
   	<property>
   		<name>fs.default.name</name>
   		<value>hdfs://localhost:9000</value>
   	</property>
   	<property>
   		<name>hadoop.tmp.dir</name>
   		<value>/usr/local/hadoop-1.2.1/tmp</value>
   	</property>
   </configuration>
   ```

   ```bash
   vi hdfs-site.xml
   
   <configuration>
   	<property>
   		<name>dfs.replication</name>
   		<value>1</value> 
   	</property>
   	<property>
   		<name>dfs.webhdfs.enabled</name>
   		<value>true</value>
   	</property>
   	<property>
   		<name>dfs.name.dir</name>
   		<value>/usr/local/hadoop-1.2.1/name</value>
   	</property>
   	<property>
   		<name>dfs.data.dir</name>
   		<value>/usr/local/hadoop-1.2.1/data</value>
   	</property>
   </configuration>
   ```

   ```bash
   vi mapred-site.xml
   
   <configuration>
   	<property>
   		<name>mapred.job.tracker</name>
   		<value>localhost:9001</value>
   	</property>
   </configuration>
   ```

   ```bash
   vi hadoop-env.sh
   
        9 export JAVA_HOME=/usr/local/jdk1.8.0
        10 export HADOOP_HOME_WARN_SUPPRESS="TRUE"
   ```

   

7. hadoop 실행

   ```bash
   # 초기화
   hadoop namenode -format
   # 하둡과 관련된 모든 데몬 실행
   start-all.sh
   # 하둡 데몬 실행 여부 확인
   jps
   3923 SecondaryNameNode
   3688 NameNode
   3817 DataNode
   4185 Jps
   4012 JobTracker
   4127 TaskTracker
   ```




### 하둡 웹인터페이스

- http://{NameNode서버IP}:50070



## Hadoop 실행

- 하둡 파일 시스템에 파일 저장

  ```bash
  cd /usr/local/hadoop-1.2.1/
  
  hadoop fs -mkdir /test
  hadoop fs -put README.txt /test
  ```



- 하둡 폴더 삭제

  ```bash
  hadoop fs -rmr {폴더}
  ```

  

- 하둡 예제 실행

  ```bash
  # -jar 파일에 있는 wordcount 클래스를 실행
  # 입력값은 test 폴더, 출력값은 output폴더
  hadoop jar hadoop-examples-1.2.1.jar wordcount /test /output
  ```

  

