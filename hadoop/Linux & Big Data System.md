# Linux & Big Data System



1. hive 서버 설치

   1. 방화벽 해제

      ```bash
      systemctl stop firewalld
      systemctl disable firewalld
      ```

   2. hostname, ip 변경

      - 환경 설정값: ip = 192.168.111.130 / hostname = hive

        ```bash
        hostnamectl set-hostname hadoopserver
        vi /etc/hosts
        # 마지막에 추가
        192.168.111.119 hadoopserver
        ```

        ```bash
        vi /etc/sysconfig/network-scripts/ifcfg-ens33
        
              4 BOOTPROTO="none"
              5 IPADDR="192.168.111.119"
              6 NETMASK="255.255.255.0"
              7 GATEWAY="192.168.111.2"
              8 DNS1="192.168.111.2"
        ```

2. 프로그램 설치

   - JDK

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
     
     # 환경변수 설정
     vi /etc/profile
     
          52 JAVA_HOEM=/usr/local/jdk1.8.0
          53 CLASSPATH=/usr/local/jdk1.8.0/lib
          54 export JAVA_HOME CLASSPATH
          55 PATH=$JAVA_HOME/bin:.:$PATH
     ```

     ```bash
     # 설치 확인
     java -version
     java version "1.8.0_261"
     Java(TM) SE Runtime Environment (build 1.8.0_261-b12)
     Java HotSpot(TM) 64-Bit Server VM (build 25.261-b12, mixed mode)
     ```

     

   - Tomcat

     ```bash
     cp -r apa(tab) /usr/local
     ln -s /usr/local/apa(tab)/bin/startup.sh starttomcat
     ln -s /usr/local/apa(tab)/bin/shutdown.sh stoptomcat
     cd /usr/local/apa(tab)/conf
     vi server.xml
     
     69 port 8080-> 80
     ```

     

   - MariaDB

     - client, common, server download
       - `https://downloads.mariadb.com/MariaDB/mariadb-10.0.15/yum/centos7-amd64/rpms/`

     ```bash
     yum -y remove mariadb-libs
     yum -y localinstall Maria*
     
     systemctl restart mysql
     systemctl status mysql
     
     chkconfig mysql on
     
     # root pwd 설정
     mysqladmin -u root password '111111'
     # MariaDB root로 로그인
     mysql -h localhost  -u  root  -p
     use mysql;
     # user, host 확인
     SELECT  user, host  FROM  user;
     # 사용자 생성 : hive, pwd=111111
     # localhost, hive, 127.0.0.1, 192.168.111.130
     GRANT   ALL   ON   *.*  TO   hive@'127.0.0.1'  IDENTIFIED  BY  '111111';
     
     # hive_db database 생성
     mysql -h localhost  -u  hive  -p
     CREATE   DATABASE   hive_db;
     
     use   hive_db
     ```

     

   - Hadoop

     ```bash
     cd 다운로드
     wget https://archive.apache.org/dist/hadoop/common/hadoop-1.2.1/hadoop-1.2.1.tar.gz
     tar xvf hadoop-1.2.1.tar.gz
     
     # /usr/local에 복사
     cp -r hadoop-1.2.1 /usr/local
     
     #HADOOP_HOME 지정
     vi /etc/profile
     
          52 JAVA_HOME=/usr/local/jdk1.8.0
          53 CLASSPATH=/usr/local/jdk1.8.0/lib
          54 HADOOP_HOME=/usr/local/hadoop-1.2.1
          55 export JAVA_HOME CLASSPATH HADOOP_HOME
          56 PATH=$JAVA_HOME/bin:$HADOOP_HOME/bin:.:$PATH
     ```

     ```bash
     # 보안설정
     ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
     cd .ssh
     cat id_dsa.pub >> authorized_keys
     ssh hive
     # password 입력 없이 로그인시 보안설정 완료
     # hadoopserver 로그아웃
     exit
     ```

     - 환경설정

       ```bash
       cd /usr/local/hadoop-1.2.1/conf
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

       - 확인 (hadoop 실행)

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

         

   - Hive

     - HIVE 다운로드

       ```bash
       wget https://archive.apache.org/dist/hive/hive-1.0.1/apache-hive-1.0.1-bin.tar.gz
       tar xvf apache-hive-1.0.1-bin.tar.gz
       mv apache-hive-1.0.1-bin hive
       cp -r hive /usr/local
       ```

     - HIVE_HOME 설정

       ```bash
       vi /etc/profile
       
            52 JAVA_HOME=/usr/local/jdk1.8.0
            53 CLASSPATH=/usr/local/jdk1.8.0/lib
            54 HADOOP_HOME=/usr/local/hadoop-1.2.1
            55 HIVE_HOME=/usr/local/hive
            56 
            57 export JAVA_HOME CLASSPATH HADOOP_HOME HIVE_HOME
            58 PATH=$JAVA_HOME/bin:$HADOOP_HOME/bin:$HIVE_HOME/bin:.:$PATH
       ```

     - MySQL을 메타스토어 데이터베이스로 사용

       ```bash
       # mariadb jdbc driver를 하이브의 lib 디렉터리에 복사
       cp mariadb-java-client-1.3.5.jar /usr/local/hive/lib
       
       # hive-site 생성
       cd /usr/local/hive/conf
       vi hive-site.xml
       
       <?xml version="1.0"?>
       <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
       
       <configuration>
           <property>
               <name>hive.metastore.local</name>
               <value>false</value>
               <description>controls whether to connect to remove metastore server or open a new metastore server in Hive Client JVM</description>
           </property>
           
           <property>
               <name>javax.jdo.option.ConnectionURL</name>
               <value>jdbc:mariadb://localhost:3306/hive_db?createDatabaseIfNotExist=true</value>
               <description>JDBC connect string for a JDBC metastore</description>
           </property>
       
           <property>
               <name>javax.jdo.option.ConnectionDriverName</name>
               <value>org.mariadb.jdbc.Driver</value>
               <description>Driver class name for a JDBC metastore</description>
           </property>
           # 계정명
           <property>
               <name>javax.jdo.option.ConnectionUserName</name>
               <value>hive</value>
               <description>username to use against metastore database</description>
           </property>
           # pwd
           <property>
               <name>javax.jdo.option.ConnectionPassword</name>
               <value>111111</value>
               <description>password to use against metastore database</description>
           </property>
       
       </configuration>
       ```

       ```bash
       # HIVE DIRECTORY SETTING
       hadoop fs -mkdir /tmp
       hadoop fs -mkdir /user/root/warehouse
       hadoop fs -chmod 777 /tmp
       hadoop fs -chmod 777 /user/root/warehouse
       hadoop fs -mkdir /tmp/hive
       hadoop fs -chmod 777 /tmp/hive
       ```

3. Web Application

   1. 기획

      - 집의 전원이 켜지고 꺼질 때를 기록한다
        1. 위치: 어디의 전원이 켜지고 꺼졌는지
        2. 어떠한 방식(수동 / 앱이용)을 사용했는지
        3. 집에 있는 인원은 몇명인지

   2. 구현

   3. Log

      - Data Structure

        ```bash
        CREATE TABLE dataclick(
        date STRING,
        fn STRING,
        id STRING,
        item STRING,
        price INT,
        age INT,
        gender STRING
        )
        PARTITIONED BY (logdate STRING)
        ROW FORMAT DELIMITED
        FIELDS TERMINATED BY ','
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;
        ```

        