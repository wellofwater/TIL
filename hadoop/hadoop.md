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
  - 하둡 = HDFS + 맵리듀스



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





## 하둡 설치 방식

- 독립 실행(Standalone) 모드

- 가상 분산(Pseudo-distributed) 모드 : 한 대의 장비에 Namenode, Secondary Namenode, Data Node를 모두 설치
- 완전 분산(Fully distributed) 모드 : 여러 대의 장비에 하둡을 설치






### 하둡 웹인터페이스

- http://{NameNode서버IP}:50070

