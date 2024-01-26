# Framework
최초 작성일 : 2024-01-23  
마지막 수정일 : 2024-01-25

## 0. Overview
하둡 에코 시스템(Hadoop Ecosystem)은 대규모 데이터를 저장, 처리 및 분석하기 위한 Apache Hadoop 프레임워크의 여러 컴포넌트 및 관련 기술들의 모음을 의미한다. 이 에코 시스템은 다양한 작업을 수행하고 다양한 비즈니스 및 데이터 분석 요구 사항을 해결하기 위해 다양한 툴과 기술로 구성되어 있다. 이 글에서는 하둡 에코시스템을 구성하는 프레임워크의 설명과 간단한 사용 예제에 대해 알아본다.

<img src="./images/logo_hadoop.png" width="360" height="120" alt="Textbook">


하둡(Hadoop)은 대규모 데이터 집합을 분산하여 처리하고 저장할 수 있는 오픈 소스 분산 컴퓨팅 프레임워크이다. 아파치 소프트웨어 재단에서 개발하고 관리하며, 빅데이터 처리와 저장을 위한 핵심 도구 중 하나이며, 하둡은 크게 두 가지 핵심 구성 요소로 구성되어 있습니다.

- Hadoop Distributed File System (HDFS)  
  HDFS는 대용량 데이터를 분산하여 저장하기 위한 파일 시스템이며, 이 파일 시스템은 데이터를 여러 노드에 나누어 저장하고, 데이터 손실을 방지하기 위한 복제 기능을 제공한다. 이를 통해 빅데이터를 안정적으로 저장하고 관리하는 데 중요한 역할을 수행한다.

- MapReduce
  MapReduce는 데이터를 처리하고 분석하는 데 사용되는 프로그래밍 모델과 프레임워크이다. MapReduce 작업은 두 단계로 나누어지는데, "맵" 단계에서 데이터를 분할하고 처리하며, "리듀스" 단계에서 중간 결과를 집계하고 최종 결과를 생성한다. 이 과정을 통해 병렬 처리와 분산 데이터 처리를 수행한다.

하둡은 하드웨어 클러스터를 활용하여 대용량 데이터를 효율적으로 처리하고 저장할 수 있는 기능을 제공한다. 이러한 특징으로 하둡은 대규모 데이터 처리, 데이터 분석, 머신러닝, 그리고 데이터 웨어하우스 등 다양한 빅데이터 작업에 사용된다. 또한 하둡 생태계는 다양한 관련 프로젝트와 도구를 포함하고 있어, 다양한 빅데이터 요구 사항에 맞게 확장하여 사용할 수 있다.

## Table of Contents

1. [Hadoop Distributed File System](#1.-Hadoop-Distributed-File-System)
2. [ZooKeeper](#2.-ZooKeeper)
3. [Flume](#3.-Flume)
4. [Kafka](#3.-Kafka)
## 1. Hadoop Distributed File System  

Hadoop Distributed File System (HDFS)는 아파치 하둡(Hadoop) 프레임워크의 핵심 구성 요소 중 하나로, 대용량 데이터를 저장하고 관리하기 위한 분산 파일 시스템이다. HDFS는 주로 빅데이터 처리를 위한 분산 스토리지 시스템으로 사용되며, 하둡 클러스터의 여러 노드에 데이터를 블록(일반적으로 128MB 또는 256MB 크기) 단위로 나누어 분산 저장하여 고가용성과 데이터 병렬 처리를 할 수 있도록 한다. 

아래에서는 HDFS를 사용하기에 자주 사용되는 명령어에 대해서 알아본다. 먼저, HDFS 명령어에는 자주 사용되는 접두사가 있는데, 각 명령어 요소는 아래와 같은 의미이다.

|Prefix|Description|
|--|--|
|hdfs|Hadoop Distributed File System에 접근하기 위한 Hadoop 명령어|
|dfs|Distributed File System의 약어로, Hadoop의 분산 파일 시스템|
|fsck|File System Check의 약어로, 파일 시스템 체크를 수행하는 옵션|

이 외 사용되는 명령어는 UNIX 또는 LINUX 명령어와 유사하다.

### 1-1. HDFS에 파일 저장
HDFS를 사용하기 위해서, Putty를 이용하여 Server02에 SSH 접속을 하고, FileZilla에 의해 업로드된 샘플 파일(Sample.txt)이 있는 위치로 이동해야 한다.

```
cd /home/bigdata
```
이 후, Linux(Local)에 있는 Sample.txt 파일을 HDFS에서 파일을 관리할 수 있도록 put 명령을 이용하여, 파일을 업로드한다.

```
$ hdfs dfs -put Sample.txt /tmp
```

### 1-2. HDFS에 저장한 파일 확인
/tmp 경로에 있는 파일 및 디렉토리 목록을 나열함으로써, 파일(Sample.txt)이 정상적으로 업로드 되었는지 확인한다.
```
$ hdfs dfs -ls /tmp
```

### 1-3. HDFS에 저장한 파일 내용 보기
LINUX 명령어와 유사하게 -cat 명령어는 파일의 내용을 표시하거나 출력하는 명령어이다.
```
$ hdfs dfs -cat /tmp/Sample.txt
```

### 1-4. HDFS에 저장한 파일 상태 확인
```
$ hdfs dfs -stat '%b %o %u %n' /tmp/Sample.txt
```

파일크기(%b), 파일 블록 크기(%o), 복제 수(%r), 소유자명(%u), 파일명(%n) 정보를 출력한다.

### 1-5. HDFS에 저장한 파일의 이름 바꾸기
```
$ hdfs dfs -mv /tmp/Sample.txt /tmp/Sample2.txt
```

### 1-6. HDFS의 파일 시스템 상태 검사
HDFS의 파일 블록 및 데이터 무결성을 검사하는 데 사용된다. 파일 시스템 내의 각 파일 블록에 대한 정보를 표시하고, 전체 크기, 디렉터리 수, 파일 수, 블록의 복제 상태, 블록 크기, 블록 위치 등과 같은 세부 정보를 출력한다.
 

```
$ hdfs fsck /
```
특히, 아래에서 확인해야할 부분은 STATUS가 HEALTHY인지 MIS,나 Corrupt된 블록이 있는지를 중점적으로 출력값을 확인한다.

```
Connecting to namenode via http://server01.hadoop.com:9870/fsck?ugi=root&path=%2F
FSCK started by root (auth:SIMPLE) from /000.000.000.000 for path / at Tue Jan 23 15:49:09 KST 2024

Status: HEALTHY
 Number of data-nodes:  2
 Number of racks:               1
 Total dirs:                    15
 Total symlinks:                0

Replicated Blocks:
 Total size:    235169738 B
 Total files:   2
 Total blocks (validated):      3 (avg. block size 78389912 B)
 Minimally replicated blocks:   3 (100.0 %)
 Over-replicated blocks:        0 (0.0 %)
 Under-replicated blocks:       0 (0.0 %)
 Mis-replicated blocks:         0 (0.0 %)
 Default replication factor:    1
 Average block replication:     1.0
 Missing blocks:                0
 Corrupt blocks:                0
 Missing replicas:              0 (0.0 %)
 Blocks queued for replication: 0

Erasure Coded Block Groups:
 Total size:    0 B
 Total files:   0
 Total block groups (validated):        0
 Minimally erasure-coded block groups:  0
 Over-erasure-coded block groups:       0
 Under-erasure-coded block groups:      0
 Unsatisfactory placement block groups: 0
 Average block group size:      0.0
 Missing block groups:          0
 Corrupt block groups:          0
 Missing internal blocks:       0
 Blocks queued for replication: 0
FSCK ended at Tue Jan 23 15:49:09 KST 2024 in 2 milliseconds
```

명령어는 HDFS 클러스터의 전반적인 상태 및 데이터 노드와 관련된 정보를 출력한다. 아래 명령어를 사용하면, 데이터 노드의 수, 용량, 사용 가능한 용량, 블록의 수 등과 같은 클러스터 전반적인 상태 정보를 확인할 수 있다. 주로 클러스터의 리소스 사용 및 가용성을 모니터링하고, 클러스터 상태를 확인하는 데 사용된다.
```
$ hdfs dfsadmin -report
```
```
Configured Capacity: 50186701210 (46.74 GB)
Present Capacity: 26371350528 (24.56 GB)
DFS Remaining: 26134228992 (24.34 GB)
DFS Used: 237121536 (226.14 MB)
DFS Used%: 0.90%
Replicated Blocks:
        Under replicated blocks: 0
        Blocks with corrupt replicas: 0
        Missing blocks: 0
        Missing blocks (with replication factor 1): 0
        Low redundancy blocks with highest priority to recover: 0
        Pending deletion blocks: 0
Erasure Coded Block Groups:
        Low redundancy block groups: 0
        Block groups with corrupt internal blocks: 0
        Missing block groups: 0
        Low redundancy blocks with highest priority to recover: 0
        Pending deletion blocks: 0

-------------------------------------------------
Live datanodes (2):

Name: 000.000.000.000:0000 (server01.hadoop.com)
Hostname: server01.hadoop.com
Rack: /default
Decommission Status : Normal
Configured Capacity: 25093350605 (23.37 GB)
DFS Used: 53248 (52 KB)
Non DFS Used: 13218301133 (12.31 GB)
DFS Remaining: 11590402048 (10.79 GB)
DFS Used%: 0.00%
DFS Remaining%: 46.19%
Configured Cache Capacity: 363855872 (347 MB)
Cache Used: 0 (0 B)
Cache Remaining: 363855872 (347 MB)
Cache Used%: 0.00%
Cache Remaining%: 100.00%
Xceivers: 2
Last contact: Tue Jan 23 15:56:57 KST 2024
Last Block Report: Tue Jan 23 15:48:03 KST 2024


Name: 000.000.000.000:0000 (server02.hadoop.com)
Hostname: server02.hadoop.com
Rack: /default
Decommission Status : Normal
Configured Capacity: 25093350605 (23.37 GB)
DFS Used: 237068288 (226.09 MB)
Non DFS Used: 10027861197 (9.34 GB)
DFS Remaining: 14543826944 (13.54 GB)
DFS Used%: 0.94%
DFS Remaining%: 57.96%
Configured Cache Capacity: 363855872 (347 MB)
Cache Used: 0 (0 B)
Cache Remaining: 363855872 (347 MB)
Cache Used%: 0.00%
Cache Remaining%: 100.00%
Xceivers: 2
Last contact: Tue Jan 23 15:56:59 KST 2024
Last Block Report: Tue Jan 23 11:18:05 KST 2024
```


###  1-7. HDFS에 저장된 파일을 로컬 파일 시스템으로 가져오기
-put 명령어는 로컬에 있는 파일을 HDFS에 업로드 하였다면, 반대로 HDFS에 업로드된 파일을 로컬로 다운로드하는 작업도 -get 명령어를 통해 실행할 수 있다.
```
$ hdfs dfs -get /tmp/Sample2.txt
```

###  1-8. HDFS의 저장한 파일 삭제(휴지통)
```
$ hdfs dfs -rm /tmp/Sample2.txt
```
전환되었다면, 강제 안전 모드를 해제함으로써, "Safe Mode"를 빠져나와야 한다. -->
삭제 명령을 실행하면 먼저 파일은 휴지통으로 임시 삭제되며, 복구가 가능하다. 그러나 휴지통으로 임시 삭제된 파일은 특정 시간이 지나면 자동으로 완전히 삭제된다. 휴지통에 임시 삭제가 필요 없을 때는 -skipTrash 옵션을 사용하여 완전 삭제를 할 수 있다.

HDFS 파일이 비정상적인 상태로 식별되는 경우, CORRUPT CLOCK, MISSING BLOCK, MISSING SIZE, CORRUPT BLOCKS 등의 항목과 함께 숫자가 표시된다. 이러한 상황이 지속되면 하둡과 연결된 HBase, Hive 등 다른 서비스에서 문제가 발생할 수 있기 때문에 비정상적인 파일 블록이 식별된 경우, 사용자는 다른 노드로 복구를 시도하거나 이슈를 해결하기 위해 강제 삭제 또는 이동 명령을 사용해야 한다.

Safe Mode는 HDFS에서 데이터의 안정성을 유지하고 클러스터의 상태를 관리하기 위한 모드이다. 일반적으로 클러스터의 유지 보수 또는 복구 작업 중에 활성화된다. 안전 모드로 전환된 경우, "Safe Mode"를 빠져나오기 위해 강제 안전 모드 해제를 실행해야 한다.

```
$ hdfs dfsamin -safemode leave
```

하둡은 파일 저장 시 Replication 기술을 사용하여 데이터의 안정성을 확보한다. Replication은 주 파일(main file)과 백업 파일(sub file) 간의 복제 및 교체 과정을 의미하며, 파일을 저장할 때 Server01과 Server02에 동시에 파일을 저장하는 방식을 채택하여, 한쪽 서버의 파일이 손상될 경우 나머지 한쪽 서버에서 파일을 복제하여 손상된 파일(MISSING BLOCK, CORRUPT CLOCK)을 대체한다. 그러나 양쪽 서버에 동일한 파일이 손상될 경우, 복제 및 복구 작업이 불가능하므로 이 경우 사용자가 아래와 같은 명령어를 이용하여 파일을 수동으로 삭제해야 한다.

```
$ hdfs fsck / -delete
```

위 명령어는 root 디렉토리에 존재하는 파일 중 손상된 파일을 모두 삭제하는 명령어이다. 그러나 손상된 파일 중에는 시스템에서 복구 작업이 진행 중인 파일도 있을 수 있기 때문에 손상된 파일을 /lost + found 디렉터리로 이동시키는 명령어를 사용하여 파일 및 블록을 안전하게 보존해야 한다. 

```
$ hdfs fsck / -move
```


## 2. ZooKeeper
Apache ZooKeeper는 분산 시스템의 조율 및 동기화를 위한 오픈 소스 분산 코디네이션 서비스입니다. 주로 분산 환경에서의 데이터 관리, 동기화, 공유 및 분산 시스템 관리를 위해 사용된다.

ZooKeeper를 실행하여, 간단한 작업을 수행해보자.

PuTTy 프로그램 실행
Server02에 root 계정으로 SSH 접속

Server02에 root 계정으로 로그인한 후 다음 명령어를 실행한다.

1. zookeeper-client 실행
$ zookeeper-client
를 실행하게 되면, 설치된 주키퍼에 의해 주피커 shell 화면으로 전환된다.

2. zookeer Z노드 등록/조회/삭제
전환된 shell 화면에서 아래 명령어를 통해 pilot-pjt 노드를 생성/조회/출력/삭제를 차례대로 실행할 수 있다. 

[zk: localhost:2181(CONNECTED) 0]: create \pilot-pjt bigdata
[zk: localhost:2181(CONNECTED) 0]: ls \
[zk: localhost:2181(CONNECTED) 0]: get \pilot-pjt
[zk: localhost:2181(CONNECTED) 0]: delete \pilot-pjt

zookeeper-client 실행

## 3. Flume  
빅데이터를 수집하는 기술 중, Flume은 원천에 있는 데이터를 파일, DB, API, socker 등의 다양한 유형으로부터 데이터를 수집할 때, 프로토콜나 메세지폼의 주기를 고려해야하지만, Flume은 이러한 고려 대상을 호환한다
또한, Flume은 이러한 데이터를 수집하여, HDFS에 저장하는 기능을 수행한다. Flume의 주요 구성 요소를 아래와 같다.

- Source : 다양한 원천 시스템의 데이터를 수집하기 위해 Avro, Thrift, JMS, Spool Dir, Kafka 등 주요 컴포넌트를 제공하며, 수집한 데이터를 Channel로 전달
- Sink : 수집한 데이터를 Channel로부터 전달받아 최종 목적지에 저장하기 위한 기능으로 HDFS, Hive, Logger, Avro, ElasticSearch, Thrift 등에 제공
- Channel : Source와 Sink를 연결하여, 데이퍼를 버퍼링하는 컴포넌트로 메모리, 파일, 데이터베이스를 채널의 저장소로 이용
- Interceptor : Source와 Channel 사이에서 데이터 필터링 및 가공하는 컴포넌트로 timestamp, Host, Regex Filtering 등을 기본 제공하며, 필요시 사용자 정의 Interceptor를 추가
- Agent : Source -> (Intercept) -> Channel -> Sink 컴포넌트 순으로 구성된 작업 단위로 독립된 인스턴스를 생성

플럼 에이전트
Source -> Channel -> Sink
플럼을 어떻게 구조하는가에 따라 다양한 데이터 파이프라인을 구현할 수 있다.

## 4. Kafka  
카프카(Kafka)는 대규모로 발생하는 작은 데이터를 처리하기 위한 데이터 스트리밍 플랫폼으로, 초당 수십, 수백, 수천 건의 데이터가 발생하는 상황에서 비동기 방식으로 데이터를 효율적으로 처리한다. 이를 위해 큐(Queue)를 활용하며, 이 큐는 데이터의 버퍼 역할을 수행한다. 또한, 카프카는 데이터 처리 과정에서 버퍼 처리와 트랜잭션 처리를 수행하여 대규모 메시지 처리를 빠르고 안정적으로 수행할 수 있으며, 대규모 분산 임시 저장소 역할로서도 수행합니다. 예를 들어, 데이터를 수집한 후 바로 HBase와 같은 시스템으로 데이터를 적재하는 경우, HBase에 장애가 발생하면 데이터가 손실될 수 있다. 그러나 카프카를 중간에 두고 데이터를 버퍼링하면 장애 발생 시에도 데이터를 안전하게 보관하고 나중에 처리할 수 있다.

이처럼 카프카는 데이터 수집 직후에 문제나 장애가 예상되는 상황에서 유용하게 활용될 수 있으며, 데이터를 안전하게 보존하고 처리할 수 있는 이러한 특징은 신뢰성 있는 데이터 스트리밍을 구축하는 데 도움을 준다. 카프카의 주요 구성 요소는 아래와 같다.

- Broker : 카프카의 인스턴스로서, 다수의 Broker를 클러스터로 구성하고 Topic이 생성되는 물리적 서버
- Topic : Broker에서 데이터의 발행/소비 처리를 위한 중간 저장소
- Provider : Broker의 특정 Topic에 데이터를 전송(발행)하는 역할로서 애플리케이션에서 카프카 라이브리러를 이용해 구현
- Consumer : Broker의 특정 Topic에서 데이터를 수신(소비)하는 역할로서 애플리케이션에서 카프카 라이브러리를 이용해 구현
