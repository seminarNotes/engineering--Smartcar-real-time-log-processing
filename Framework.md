# Framework
최초 작성일 : 2024-01-23  
마지막 수정일 : 2024-01-25

## 0. Overview

데이터 엔지니어링 분야에서 빅데이터를 다루기 위해서는 관련된 프레임워크와 소프트웨어를 능숙하게 설치하고 유지보수하는 것이 기본 업무이자 가장 중요한 업무이다. 그러나 빅데이터와 관련된 프레임워크 및 시스템이 매우 복잡한 아키텍처를 가지고 있기 때문에 빅데이터 기술의 초기 설정과 관리는 심지어 경험이 풍부한 엔지니어들에게도 어려운 작업이다.

구체적으로, 빅데이터 솔루션은 대개 분산 시스템으로 구성되어 있으며, 다수의 컴퓨터 및 서버에서 동시에 작동한다. 이러한 복잡한 아키텍처를 이해하고 올바르게 설정하는 것은 많은 시간과 전문적인 지식이 요구된다. 또한, 각 빅데이터 솔루션/프레임워크는 다양한 구성 옵션과 세부 설정을 가지고 있으며, 어떤 환경에서 어떤 옵션을 선택해야 하는지, 어떻게 최적화해야 하는지 결정하는 것도 많은 경험을 요구한다. 또한, 성능 측면에서 적절한 자원 할당, 하드웨어 및 네트워크 환경을 구축하는 것은 빅데이터 처리 성능에 많은 영향을 미치기 때문에, 시스템을 최적화하여 대량의 데이터 및 처리량를 관리할 수 있도록 환경을 구성 해야 한다.

이러한 문제를 해결하기 위해, 본 프로젝트에서는 다양한 프레임워크를 활용하여, 아키텍처를 구성하고, 전체적인 구조를 설계하고 이해하는 것에 중점을 두었기 때문에 클라우데라(Cloudera)를 활용했다. 클라우데라는 종합적인 빅데이터 솔루션로, Hadoop 기반의 종합적인 빅데이터 솔루션을 제공하기 때문에, Hadoop을 중심으로 HDFS(분산 파일 시스템), MapReduce, YARN 및 기타 주요 빅데이터 프레임워크를 통합하여 데이터 수집, 저장, 처리 및 분석을 한 곳에서 관리하고 모니터링 할 수 있다. 또한, 다양한 프레임워크가 실행되는 동안 해당 컴퓨터의 자원을 실시간으로 모니터링할 수 있는 대시보드도 제공된다. 클라우데라는 아래와 같은 특징을 갖는다.

- 통합된 플랫폼: 클라우데라는 하둡, 스파크, Kafka 등 다양한 빅데이터 기술을 하나의 플랫폼에 통합하여 제공합니다. 이는 기술 간의 호환성 문제를 줄이고, 관리의 복잡성을 낮춥니다.

- 사용자 친화적인 관리 도구: 클라우데라 매니저와 같은 관리 도구를 통해 사용자는 시스템을 쉽게 모니터링, 관리, 설정할 수 있습니다. 이는 시스템 관리의 난이도를 크게 낮춰줍니다.

- 보안 및 거버넌스: 클라우데라는 데이터 보안, 거버넌스, 컴플라이언스를 위한 포괄적인 솔루션을 제공합니다. 이는 빅데이터를 안전하게 관리하고 규정 준수를 보장하는 데 도움이 됩니다.

- 확장성 및 성능 최적화: 클라우데라는 높은 확장성을 제공하며, 대규모 데이터 처리를 위해 최적화되어 있습니다. 이는 데이터 볼륨과 처리량이 증가해도 성능을 유지할 수 있게 해줍니다.


아래에서 설명하는 server01과 server02는 직접 linux 서버를 구성한 것을 기준으로 설명한 것으로, 각 프레임워크는 cloudera를 통해 구축하였으며, host의 구성은 아래와 같다.  


### 1. HBase
## 1.1. Hbase의 설명  
HBase는 Apache Hadoop 프로젝트의 일부로 개발된 오픈 소스 분산형 NoSQL 데이터베이스입니다. HBase는 대용량 데이터의 저장 및 검색에 특화된 분산형 데이터베이스로, 대규모 데이터를 실시간으로 처리하고 분석하기 위한 목적으로 설계되었습니다.HBase는 대규모 데이터의 실시간 처리와 분석에 사용되며, 웹 애플리케이션 로그, 센서 데이터, 소셜 미디어 데이터, 기계 학습 모델 등 다양한 분야에서 활용됩니다. HBase는 Hadoop과 통합하여 사용되며, Hadoop의 HDFS와 함께 빅데이터 처리 및 분석에 필요한 풍부한 생태계를 제공합니다.

## 1.2. HBase의 특징  
### 1.2.1. 분산 아키텍처  
HBase는 여러 서버에 데이터를 분산 저장하며, 데이터를 자동으로 분산하고 복제하여 고가용성을 제공합니다. 이러한 분산 아키텍처 덕분에 대용량 데이터를 처리할 수 있습니다. 

### 1.2.2. 열 지향 데이터베이스
HBase는 데이터를 테이블 형식으로 저장하며, 테이블은 로우(Row)와 열(Column)로 구성됩니다. 이러한 열 지향 데이터 모델은 읽기 작업 및 스캔에 특히 효율적입니다.

### 1.2.3. 스캔 및 실시간 쿼리
HBase는 대용량 데이터 세트를 스캔하고 실시간으로 쿼리할 수 있는 기능을 제공합니다. 이는 데이터를 빠르게 검색하고 분석할 수 있게 해줍니다.

### 1.2.4. 자동 분할과 복제
HBase는 데이터를 자동으로 분할하여 여러 리전(Region)으로 분산하고, 데이터의 복제를 통해 데이터의 가용성을 보장합니다. 이로 인해 데이터의 손상 및 데이터 손실을 방지할 수 있습니다.

### 1.2.5. 일관성
HBase는 데이터의 일관성을 유지하기 위한 강력한 일관성 모델을 제공합니다. ACID(Atomicity, Consistency, Isolation, Durability) 원칙을 준수하며, 데이터의 일관성을 보장합니다.

### 1.2.6. Java API  
HBase는 Java로 작성되었으며, Java 언어로 데이터를 조작하기 위한 API를 제공합니다. 이를 통해 개발자는 Java로 HBase와 상호 작용할 수 있습니다.

### 1.2.7. 확장성
HBase는 클러스터에 노드를 추가함으로써 쉽게 확장할 수 있습니다. 이로써 데이터베이스 성능을 향상시키고 대용량 데이터를 처리할 수 있습니다.

### 1.2.8. 컬럼 패밀리
HBase는 데이터를 테이블 내에서 컬럼 패밀리(Column Family)로 구성합니다. 각 컬럼 패밀리에는 여러 개의 열을 포함할 수 있으며, 이러한 구조는 데이터를 유연하게 모델링할 수 있도록 합니다.

## 1.3. HBase 구성요소
### 1.3.1. HTable  
칼럼 기반 데이터 구조를 정의한 테이블로서, 공통점이 있는 칼럼의 그룹을 묶는 칼럼 패밀리와 테이블의 로우를 식별하여 접근하기 위한 로우키 구성  
### 1.3.2. HMaster  
HRegion 서버를 관리하며, HRegion들이 속한 HRegion 서버의 메타 정보 관리
### 1.3.3. HRegion  
HTable의 크기에 따라 자동으로 수평 분할이 발생하고, 이때 분할된 블록을 HRegion 단위로 지정
### 1.3.4. HRegionServer  
분산 노드별 HRegionServer가 구성되며, 하나의 HRegionServer에는 다수의 HRegion이 생성되어 HRegion을 관리
### 1.3.5. Store  
하나의 Store에는 칼럼 패밀리가 저장 및 관리되며, MemStore와 HFile로 구성됨
### 1.3.6. MemStore  
Store 내 데이터 베이스 인메모리에 저장 및 관리하는 데이터 캐시 영역
### 1.3.7. HFile  
Store 내 데이터를 스토리지에 저장 및 관리하는 영욱 저장 영역

## 1.4. 유사 프레임워크
Big Table, Cassandra, MongoDB



|Name|Roles|
|--|--|
|server01.hadoop.com|HDFS Balancer|
|server01.hadoop.com|HDFS DataNode|
|server01.hadoop.com|HDFS NameNode|
|server01.hadoop.com|HDFS SecondaryNameNode|
|server01.hadoop.com|Cloudera Management Service Alert Publisher|
|server01.hadoop.com|Cloudera Management Service Event Server|
|server01.hadoop.com|Cloudera Management Service Host Monitor|
|server01.hadoop.com|Cloudera Management Service Service Monitor|
|server01.hadoop.com|YARN (MR2 Included) JobHistory Server|
|server01.hadoop.com|YARN (MR2 Included) ResourceManager|
|server02.hadoop.com|HDFS DataNode|
|server02.hadoop.com|YARN (MR2 Included) NodeManager|
|server02.hadoop.com|ZooKeeper Server|

![host](./images/allhost.png)



따라서, 아래 프레임워크에 대한 설명과 실행도 클라우데라를 기반으로 실행하여 설명한 것이다. 이외 대표적인 종합적인 빅데이터 솔루션/서비스로는 Amazon Web Services, Google Cloud Platform, Microsoft Azure 등이 있다.

## Table of Contents

1. [Hadoop Distributed File System](#1.-Hadoop-Distributed-File-System)
2. [ZooKeeper](#2.-ZooKeeper)
3. [Flume](#3.-Flume)
4. [Kafka](#3.-Kafka)
## 1. Hadoop Distributed File System 

하둡 에코 시스템(Hadoop Ecosystem)은 대규모 데이터를 저장, 처리 및 분석하기 위한 Apache Hadoop 프레임워크의 여러 컴포넌트 및 관련 기술들의 모음을 의미한다. 이 에코 시스템은 다양한 작업을 수행하고 다양한 비즈니스 및 데이터 분석 요구 사항을 해결하기 위해 다양한 툴과 기술로 구성되어 있다. 이 글에서는 하둡 에코시스템을 구성하는 프레임워크의 설명과 간단한 사용 예제에 대해 알아본다.

<img src="./images/logo_hadoop.png" width="360" height="120" alt="Textbook">


하둡(Hadoop)은 대규모 데이터 집합을 분산하여 처리하고 저장할 수 있는 오픈 소스 분산 컴퓨팅 프레임워크이다. 아파치 소프트웨어 재단에서 개발하고 관리하며, 빅데이터 처리와 저장을 위한 핵심 도구 중 하나이며, 하둡은 크게 두 가지 핵심 구성 요소로 구성되어 있습니다.

- Hadoop Distributed File System (HDFS)  
  HDFS는 대용량 데이터를 분산하여 저장하기 위한 파일 시스템이며, 이 파일 시스템은 데이터를 여러 노드에 나누어 저장하고, 데이터 손실을 방지하기 위한 복제 기능을 제공한다. 이를 통해 빅데이터를 안정적으로 저장하고 관리하는 데 중요한 역할을 수행한다.

- MapReduce
  MapReduce는 데이터를 처리하고 분석하는 데 사용되는 프로그래밍 모델과 프레임워크이다. MapReduce 작업은 두 단계로 나누어지는데, "맵" 단계에서 데이터를 분할하고 처리하며, "리듀스" 단계에서 중간 결과를 집계하고 최종 결과를 생성한다. 이 과정을 통해 병렬 처리와 분산 데이터 처리를 수행한다.

하둡은 하드웨어 클러스터를 활용하여 대용량 데이터를 효율적으로 처리하고 저장할 수 있는 기능을 제공한다. 이러한 특징으로 하둡은 대규모 데이터 처리, 데이터 분석, 머신러닝, 그리고 데이터 웨어하우스 등 다양한 빅데이터 작업에 사용된다. 또한 하둡 생태계는 다양한 관련 프로젝트와 도구를 포함하고 있어, 다양한 빅데이터 요구 사항에 맞게 확장하여 사용할 수 있다.



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


Topic 만들기
```
$ kafka-topics --create --zookeeper server02.hadoop.com:0000 --replication-factor 1 --partitions 1 --topic [Name-of-Topic]
```

앞의 Topic을 바라보고 있는 메세지 생산자(Producer) 사용
```
$ kafka-console-producer --broker-list server02.hadoop.com:0000 -topic [Name-of-Topic]
```

producer가 생상한 메세지를 consumer가 수신한다. 이 때, 생상자와 동일한 topic을 바라봐야한다.
```
$ kafka-console-consumer --bootstrap-server Server02.hadoop.com:0000 --topic [Name-of-Topic] --partition 0 --from-beginning
```




### 1. Hadoop
#### 1.1. 설명  
Apache Hadoop은 대용량 데이터를 저장하고 처리하기 위한 오픈소스 프레임워크입니다. Hadoop은 클러스터로 구성된 컴퓨터에서 대량의 데이터 집합을 분산 처리하는 데 사용됩니다. Hadoop의 핵심은 HDFS(Hadoop Distributed File System)와 MapReduce 프로그래밍 모델로 구성되어 있습니다.

#### 1.2. 특징  
- **분산 처리**: 데이터와 처리 작업을 여러 서버에 분산하여 처리할 수 있습니다.
- **확장성**: 서버를 추가함으로써 시스템을 쉽게 확장할 수 있습니다.
- **고가용성**: 데이터를 여러 노드에 복제하여 하나의 노드에 장애가 발생해도 시스템을 계속 운영할 수 있습니다.
- **결함 허용성**: 자동으로 데이터를 복제하고 장애가 발생한 노드를 복구합니다.

#### 1.3. 구성요소  
- **HDFS (Hadoop Distributed File System)**: 대용량 데이터를 저장하기 위한 분산 파일 시스템.
- **MapReduce**: 대용량 데이터를 병렬로 처리하기 위한 프로그래밍 모델.
- **YARN (Yet Another Resource Negotiator)**: 클러스터 자원 관리 및 작업 스케줄링을 담당.

#### 1.4. 유사 프레임워크  
- Apache Spark
- Apache Flink

### 2. Spark
#### 2.1. 설명  
Apache Spark는 대용량 데이터 처리를 위한 오픈소스 분산 컴퓨팅 시스템입니다. Spark는 Hadoop MapReduce와 호환되지만, 반복적인 데이터 흐름을 최적화하기 위해 메모리 내 컴퓨팅과 빠른 처리 속도를 지원하는 점에서 차별화됩니다.

#### 2.2. 특징  
- **고속 처리**: 인메모리 데이터 처리를 통해 빠른 성능을 제공합니다.
- **다양한 언어 지원**: Scala, Java, Python, R 등 다양한 프로그래밍 언어를 지원합니다.
- **확장성**: 작은 규모의 데이터부터 페타바이트급 데이터까지 처리할 수 있습니다.
- **다양한 데이터 소스 지원**: HDFS, HBase, Cassandra, Hive 등 다양한 데이터 소스를 지원합니다.

#### 2.3. 구성요소  
- **RDD (Resilient Distributed Datasets)**: 변하지 않는 분산된 데이터 컬렉션.
- **Spark SQL**: SQL과 DataFrame을 사용하여 데이터를 쿼리할 수 있는 모듈.
- **Spark Streaming**: 실시간 데이터 스트리밍 처리를 위한 모듈.
- **MLlib**: 머신러닝 알고리즘을 제공하는 모듈.
- **GraphX**: 그래프 처리를 위한 모듈.

#### 2.4. 유사 프레임워크  
- Apache Flink
- Apache Storm

### 3. Hive
#### 3.1. 설명  
Apache Hive는 Hadoop 상에서 데이터 요약, 쿼리, 분석을 위한 데이터 웨어하우스 인프라를 제공하는 프레임워크입니다. Hive는 SQL을 사용하여 데이터를 쿼리하는 HiveQL 인터페이스를 제공하므로, SQL에 익숙한 사용자가 Hadoop 데이터를 쉽게 처리할 수 있습니다.

#### 3.2. 특징  
- **SQL 지원**: HiveQL을 통해 SQL과 유사한 쿼리를 사용할 수 있습니다.
- **확장성**: Hadoop의 분산 저장 및 처리 기능을 활용하여 대용량 데이터를 처리할 수 있습니다.
- **다양한 데이터 형식 지원**: Text, Parquet, ORC 등 다양한 데이터 형식을 지원합니다.

#### 3.3. 구성요소  
- **Hive Metastore**: 메타데이터를 저장하는 컴포넌트.
- **Hive Driver**: 쿼리 실행 계획을 관리하고 실행하는 컴포넌트.
- **Hive Compiler**: HiveQL을 실행 계획으로 변환하는 컴포넌트.
- **Hive Execution Engine**: 쿼리를 실행하는 컴포넌트.

#### 3.4. 유사 프레임워크  
- Apache Impala
- Presto

### 4. Pig
#### 4.1. 설명  
Apache Pig는 Hadoop 데이터에 대한 고수준 스크립트 플랫폼입니다. Pig는 Hadoop의 복잡한 Java MapReduce 프로그램을 대신하여, 간단한 스크립트 언어 Pig Latin을 통해 데이터 분석 작업을 쉽게 할 수 있게 합니다.

#### 4.2. 특징  
- **간결한 스크립팅 언어**: Pig Latin을 사용하여 복잡한 데이터 변환을 간단한 단계로 표현할 수 있습니다.
- **확장성**: Hadoop 인프라를 활용하여 대용량 데이터를 처리할 수 있습니다.
- **다양한 데이터 소스 지원**: HDFS, HBase 등 다양한 데이터 소스를 지원합니다.

#### 4.3. 구성요소  
- **Pig Latin**: 데이터 처리 작업을 위한 스크립팅 언어.
- **Pig Engine**: Pig Latin 스크립트를 실행하는 실행 엔진.
- **UDF (User Defined Functions)**: 사용자가 정의한 함수를 지원하여 데이터 처리를 확장할 수 있습니다.

#### 4.4. 유사 프레임워크  
- Apache Hive
- Apache Spark


### 5. HBase
#### 5.1. 설명  
HBase는 Apache Hadoop 프로젝트의 일부로 개발된 오픈 소스 분산형 NoSQL 데이터베이스입니다. HBase는 대용량 데이터의 저장 및 검색에 특화된 분산형 데이터베이스로, 대규모 데이터를 실시간으로 처리하고 분석하기 위한 목적으로 설계되었습니다.

#### 5.2. 특징  
- **분산 아키텍처**: 여러 서버에 데이터를 분산 저장하며, 데이터를 자동으로 분산하고 복제하여 고가용성을 제공합니다.
- **열 지향 데이터베이스**: 데이터를 테이블 형식으로 저장하며, 테이블은 로우(Row)와 열(Column)로 구성됩니다.
- **스캔 및 실시간 쿼리**: 대용량 데이터 세트를 스캔하고 실시간으로 쿼리할 수 있는 기능을 제공합니다.

#### 5.3. 구성요소  
- **HMaster**: 클러스터 관리 및 조정.
- **HRegionServer**: 데이터 저장과 처리를 담당하는 서버.
- **HFile**: 실제 데이터 파일.
- **Zookeeper**: 클러스터 조정과 상태 관리.

#### 5.4. 유사 프레임워크  
- Cassandra
- Accumulo

### 6. Flink
#### 6.1. 설명  
Apache Flink은 대규모 분산 데이터 처리를 위한 오픈 소스 스트리밍 프로세스 프레임워크입니다. Flink는 빠른 계산과 낮은 지연 시간을 목표로 하며, 배치 및 스트리밍 데이터 처리를 모두 지원합니다.

#### 6.2. 특징  
- **스트리밍과 배치 처리**: 실시간 스트리밍 처리와 배치 처리를 모두 지원합니다.
- **고성능**: 빠른 처리 속도와 낮은 지연 시간을 제공합니다.
- **확장성**: 크고 작은 클러스터 모두에서 확장성 있게 작동합니다.

#### 6.3. 구성요소  
- **DataStream API**: 실시간 스트리밍 데이터를 처리하기 위한 API.
- **DataSet API**: 배치 데이터 처리를 위한 API.
- **Table API & SQL**: SQL과 유사한 쿼리 언어를 제공합니다.

#### 6.4. 유사 프레임워크  
- Apache Storm
- Apache Spark

### 7. Kafka
#### 7.1. 설명  
Apache Kafka는 대용량 데이터 스트림을 처리하기 위한 분산형 스트리밍 플랫폼입니다. Kafka는 데이터 파이프라인을 구축하고, 실시간 데이터 분석을 수행하는 데 사용됩니다.

#### 7.2. 특징  
- **고성능**: 높은 처리량을 제공하며, 대규모 메시지를 신속하게 처리할 수 있습니다.
- **분산 시스템**: 데이터를 여러 서버에 분산하여 저장하고 처리할 수 있습니다.
- **내구성**: 데이터를 디스크에 저장하고, 복제를 통해 데이터 손실에 대비합니다.

#### 7.3. 구성요소  
- **Producer**: 데이터를 생성하고 Kafka 클러스터에 보내는 컴포넌트.
- **Consumer**: Kafka 클러스터에서 데이터를 읽는 컴포넌트.
- **Broker**: Kafka 클러스터의 서버 노드.

#### 7.4. 유사 프레임워크  
- RabbitMQ
- Apache Pulsar

### 8. Cassandra
#### 8.1. 설명  
Apache Cassandra는 대규모 분산 데이터베이스 관리 시스템입니다. Cassandra는 고가용성과 확장성을 제공하며, 많은 기업에서 대규모 데이터를 관리하기 위해 사용됩니다.

#### 8.2. 특징  
- **분산 설계**: 데이터를 여러 노드에 걸쳐 분산 저장합니다.
- **확장성**: 데이터와 트래픽 증가에 따라 선형적으로 확장할 수 있습니다.
- **내구성**: 데이터를 여러 노드에 복제하여 고가용성을 보장합니다.

#### 8.3. 구성요소  
- **Node**: 데이터를 저장하는 기본 단위.
- **Cluster**: 여러 노드가 모여 구성된 집합.
- **Keyspace**: 데이터베이스 스키마와 유사한 역할을 하는 최상위 데이터 구조.

#### 8.4. 유사 프레임워크  
- HBase
- MongoDB

### 9. Beam (Apache Beam)
#### 9.1. 설명  
Apache Beam은 배치 및 실시간 데이터 처리를 위한 통합 모델을 제공하는 오픈 소스 프레임워크입니다. Beam은 다양한 실행 엔진(Flink, Spark, Google Cloud Dataflow 등) 위에서 동작할 수 있습니다.

#### 9.2. 특징  
- **모델의 일관성**: 배치 및 스트리밍 데이터 처리에 대해 일관된 프로그래밍 모델을 제공합니다.
- **다양한 백엔드 지원**: 여러 실행 엔진에서 동작할 수 있습니다.
- **휴대성**: 다양한 프로그래밍 언어와 플랫폼에서 작동합니다.

#### 9.3. 구성요소  
- **PCollection**: 데이터를 표현하는 핵심 데이터 구조.
- **PTransform**: 데이터 변환을 수행하는 작업.
- **Pipeline**: PTransform의 연속적인 집합을 정의하는 작업 흐름.

#### 9.4. 유사 프레임워크  
- Apache Flink
- Apache Spark

### 10. Kylin (Apache Kylin)
#### 10.1. 설명  
Apache Kylin은 대규모 데이터셋에 대한 OLAP(Online Analytical Processing) 쿼리를 지원하는 분산형 분석 데이터베이스입니다. Kylin은 Hadoop 상에서 동작하며, 대규모 데이터에 대해 빠른 쿼리 성능을 제공합니다.

#### 10.2. 특징  
- **고성능 OLAP**: 대규모 데이터셋에 대한 빠른 쿼리 응답 시간을 제공합니다.
- **확장성**: Hadoop 기반으로, 대용량 데이터를 처리할 수 있는 확장성을 제공합니다.
- **다차원 분석**: 다차원 데이터 분석을 위한 강력한 기능을 제공합니다.

#### 10.3. 구성요소  
- **Cube**: 데이터를 미리 집계하여 저장하는 데이터 구조.
- **Query Engine**: 사용자의 쿼리에 대한 응답을 생성하는 엔진.
- **Storage Layer**: Cube 데이터를 저장하는 계층.

#### 10.4. 유사 프레임워크  
- Druid
- ClickHouse


### 11. Tez (Apache Tez)
#### 11.1. 설명  
Apache Tez는 복잡한 데이터 처리 작업을 위한 범용, 유연한, 고성능의 데이터 처리 엔진입니다. Hadoop YARN 위에서 동작하며, 특히 데이터 처리의 복잡한 DAG(Directed Acyclic Graph)를 효율적으로 실행할 수 있도록 설계되었습니다.

#### 11.2. 특징  
- **효율성**: 복잡한 데이터 처리 작업을 위한 최적화된 경로를 제공합니다.
- **유연성**: 사용자 정의 가능한 데이터 처리 컴포넌트를 지원합니다.
- **확장성**: 대규모 데이터 처리 작업을 처리할 수 있는 높은 확장성을 제공합니다.

#### 11.3. 구성요소  
- **DAG**: 데이터 처리 작업을 DAG로 표현하여 복잡한 작업을 효율적으로 관리합니다.
- **Tez API**: 사용자가 Tez를 사용하여 데이터 처리 작업을 쉽게 정의할 수 있도록 돕는 API.
- **YARN Integration**: Hadoop YARN과의 통합을 통해 리소스 관리 및 작업 스케줄링을 지원합니다.

#### 11.4. 유사 프레임워크  
- Apache Spark
- Apache Flink

### 12. Storm
#### 12.1. 설명  
Apache Storm은 실시간 데이터 스트림 처리를 위한 분산 컴퓨팅 시스템입니다. Storm은 빠른 처리 속도, 확장성, 그리고 내결함성을 제공하며, 실시간 분석, 온라인 머신러닝, 지속적인 컴퓨팅 등 다양한 용도로 사용됩니다.

#### 12.2. 특징  
- **실시간 처리**: 데이터를 실시간으로 처리하고 분석할 수 있습니다.
- **확장성**: 클러스터에 노드를 추가함으로써 쉽게 확장할 수 있습니다.
- **내결함성**: 자동으로 장애 노드를 감지하고 복구합니다.

#### 12.3. 구성요소  
- **Spout**: 외부 데이터 소스로부터 데이터를 읽어 들이는 컴포넌트.
- **Bolt**: 데이터 처리 및 변환을 담당하는 컴포넌트.
- **Topology**: Spouts와 Bolts를 연결하여 전체 데이터 처리 구조를 정의하는 구성.

#### 12.4. 유사 프레임워크  
- Apache Flink
- Apache Samza

### 13. ZooKeeper
#### 13.1. 설명  
Apache ZooKeeper는 분산 시스템을 위한 중앙 집중식 서비스로, 네이밍, 구성 관리, 동기화, 그룹 서비스 등의 서비스를 제공합니다. 분산 애플리케이션의 조정을 위해 설계되었으며, 높은 가용성과 확장성을 제공합니다.

#### 13.2. 특징  
- **분산 조정**: 클러스터 내의 노드 간 동기화와 조정을 관리합니다.
- **서비스 디스커버리**: 시스템 구성 요소 간 서비스 디스커버리를 지원합니다.
- **구성 관리**: 시스템 구성 정보의 중앙 집중식 관리를 제공합니다.

#### 13.3. 구성요소  
- **ZNode**: ZooKeeper 데이터 모델의 기본 단위. 데이터와 함께 메타데이터를 저장합니다.
- **Leader and Follower**: 클러스터 노드들은 리더와 팔로워로 구성되며, 리더는 클러스터 조정을 담당합니다.
- **Watchers**: 클라이언트가 특정 ZNode의 변경사항을 감시할 수 있게 하는 메커니즘.

#### 13.4. 유사 프레임워크  
- etcd
- Consul

### 14. Impala
#### 14.1. 설명  
Apache Impala는 Hadoop 클러스터에서 대규모 데이터를 실시간으로 쿼리할 수 있는 SQL 엔진입니다. Impala는 분석 쿼리를 위해 최적화되어 있으며, Hive와 호환되는 메타스토어를 사용합니다.

#### 14.2. 특징  
- **실시간 쿼리**: 빠른 쿼리 응답 시간을 제공하여 대용량 데이터에 대해 실시간 분석을 가능하게 합니다.
- **SQL 지원**: 기존 SQL 지식을 활용하여 Hadoop 데이터에 접근할 수 있습니다.
- **확장성**: Hadoop의 분산 저장 및 처리 기능을 활용하여 대용량 데이터를 처리할 수 있습니다.

#### 14.3. 구성요소  
- **Daemon**: 데이터 노드에서 동작하며, 쿼리 실행을 담당합니다.
- **Statestore**: 클러스터 상태 정보를 관리하고, 노드 간 동기화를 담당합니다.
- **Catalog Service**: 메타데이터 정보를 관리하고, Impala Daemon과 Hive Metastore 간의 일관성을 유지합니다.

#### 14.4. 유사 프레임워크  
- Apache Drill
- Apache Hive


### 15. Pinot (Apache Pinot)
#### 15.1. 설명  
Apache Pinot는 실시간 분석을 위한 분산형 OLAP 데이터 스토어입니다. Pinot는 대규모 데이터셋에 대해 실시간 쿼리를 처리할 수 있으며, 사용자는 거의 실시간으로 데이터를 쿼리하고 분석할 수 있습니다.

#### 15.2. 특징  
- **실시간 데이터 색인**: 데이터를 실시간으로 색인하여 빠른 쿼리 응답 시간을 제공합니다.
- **확장성**: 대규모 데이터셋을 처리할 수 있도록 설계되었습니다.
- **고가용성**: 데이터를 복제하고 분산 저장하여 높은 가용성을 보장합니다.

#### 15.3. 구성요소  
- **Broker**: 쿼리를 받고 처리 결과를 반환합니다.
- **Server**: 데이터를 저장하고 쿼리를 처리합니다.
- **Controller**: 클러스터 관리와 세그먼트 할당을 담당합니다.

#### 15.4. 유사 프레임워크  
- Druid
- ClickHouse

### 16. Arrow (Apache Arrow)
#### 16.1. 설명  
Apache Arrow는 컬럼 기반의 인메모리 데이터 구조로, 다양한 데이터 시스템 간에 효율적인 데이터 교환을 가능하게 합니다. Arrow는 데이터 처리와 분석을 위한 표준 메모리 포맷을 제공하여, 성능을 향상시키고 복잡한 데이터 파이프라인을 간소화합니다.

#### 16.2. 특징  
- **메모리 최적화**: 인메모리 데이터 처리를 최적화하여 빠른 데이터 액세스를 지원합니다.
- **언어 독립성**: C++, Java, Python 등 다양한 프로그래밍 언어에서 사용할 수 있습니다.
- **데이터 교환 효율성**: 서로 다른 데이터 처리 시스템 간의 데이터 교환 오버헤드를 최소화합니다.

#### 16.3. 구성요소  
- **Arrow Format**: 데이터 구조 및 형식을 정의합니다.
- **Arrow Flight**: 고성능 데이터 서비스 프로토콜로, 대규모 데이터 전송을 위한 프레임워크입니다.
- **Libraries and Tools**: 다양한 언어와 통합을 지원하는 라이브러리와 도구들.

#### 16.4. 유사 프레임워크  
- Parquet
- ORC

### 17. Avro (Apache Avro)
#### 17.1. 설명  
Apache Avro는 데이터 직렬화 시스템으로, 데이터 구조를 정의하고, 이진 형식으로 데이터를 직렬화 및 역직렬화합니다. Avro는 특히 Hadoop 시스템에서 널리 사용되며, 스키마가 데이터와 함께 저장되는 것이 특징입니다.

#### 17.2. 특징  
- **스키마 진화**: 스키마 변경 시에도 데이터를 손실 없이 처리할 수 있습니다.
- **효율적인 직렬화**: 데이터를 컴팩트한 바이너리 형식으로 직렬화합니다.
- **언어 독립적**: 다양한 프로그래밍 언어로 구현할 수 있습니다.

#### 17.3. 구성요소  
- **Avro Data File**: 데이터와 스키마가 함께 저장되는 파일 형식.
- **Avro RPC**: 원격 프로시저 호출을 지원합니다.
- **Avro Schema**: 데이터 구조를 정의하는 JSON 형식의 스키마.

#### 17.4. 유사 프레임워크  
- Protocol Buffers
- Thrift

### 18. NiFi (Apache NiFi)
#### 18.1. 설명  
Apache NiFi는 데이터 플로우 자동화와 관리를 위한 시스템입니다. NiFi는 데이터 플로우의 설계, 제어, 자동화 및 모니터링을 지원하여, 데이터 전송과 변환 과정을 간소화합니다.

#### 18.2. 특징  
- **직관적인 사용자 인터페이스**: 데이터 플로우를 시각적으로 설계하고 관리할 수 있습니다.
- **확장성**: 소규모에서 대규모 데이터 플로우까지 지원합니다.
- **데이터 프로버넌스**: 데이터의 원본, 흐름, 처리 내역을 추적하고 기록합니다.

#### 18.3. 구성요소  
- **Processor**: 데이터를 처리하고 변환하는 구성요소.
- **Connection**: 프로세서 간 데이터 플로우를 연결합니다.
- **Flow Controller**: 데이터 플로우를 관리하고 조정합니다.

#### 18.4. 유사 프레임워크  
- Apache Camel
- Apache Kafka

### 19. Oozie (Apache Oozie)
#### 19.1. 설명  
Apache Oozie는 Hadoop 작업(예: MapReduce, Pig, Hive)의 워크플로우를 관리하고 조정하기 위한 시스템입니다. Oozie는 복잡한 데이터 처리 작업을 일련의 작업으로 정의하고, 이들을 순차적 혹은 병렬로 실행할 수 있게 합니다.

#### 19.2. 특징  
- **워크플로우 관리**: 다양한 Hadoop 작업을 통합하여 워크플로우를 구성하고 관리합니다.
- **스케줄링**: 작업의 실행을 시간이나 데이터 가용성에 따라 예약합니다.
- **확장성과 유연성**: 복잡한 데이터 처리 로직을 워크플로우로 설계하여 처리합니다.

#### 19.3. 구성요소  
- **Workflow Engine**: 워크플로우의 실행과 생명주기를 관리합니다.
- **Coordinator**: 워크플로우의 실행을 시간 혹은 데이터 이벤트에 따라 예약합니다.
- **Bundle**: 여러 워크플로우와 코디네이터를 묶어서 관리합니다.

#### 19.4. 유사 프레임워크  
- Apache Airflow
- Luigi

### 20. Sqoop (Apache Sqoop)
#### 20.1. 설명  
Apache Sqoop은 SQL 기반의 데이터베이스와 Hadoop HDFS 사이에 대용량 데이터를 효율적으로 전송할 수 있는 도구입니다. Sqoop은 데이터를 Hadoop 생태계의 다양한 데이터 스토리지 시스템(예: HDFS, Hive, HBase)과 동기화할 수 있습니다.

#### 20.2. 특징  
- **효율적인 데이터 전송**: 대용량의 데이터를 RDBMS와 Hadoop 사이에서 빠르고 효율적으로 전송합니다.
- **병렬 처리**: 데이터 전송 과정에서 병렬 처리를 지원하여 성능을 최적화합니다.
- **다양한 데이터 소스 지원**: 여러 SQL 데이터베이스에 대한 지원을 제공합니다.

#### 20.3. 구성요소  
- **Import**: RDBMS에서 Hadoop으로 데이터를 가져옵니다.
- **Export**: Hadoop에서 RDBMS로 데이터를 내보냅니다.
- **Connectors**: 다양한 데이터 소스와의 연결을 지원합니다.

#### 20.4. 유사 프레임워크  
- Apache Flume
- Apache Kafka Connect

### 21. Tika (Apache Tika)
#### 21.1. 설명  
Apache Tika는 문서 분석과 메타데이터 추출을 위한 도구입니다. Tika는 다양한 파일 포맷(예: PDF, Microsoft Office, HTML)에서 텍스트와 메타데이터를 추출할 수 있습니다.

#### 21.2. 특징  
- **다양한 파일 포맷 지원**: 수백 가지의 파일 포맷을 지원합니다.
- **메타데이터 추출**: 파일로부터 메타데이터를 추출할 수 있습니다.
- **언어 독립적**: 다양한 프로그래밍 언어에서 사용할 수 있습니다.

#### 21.3. 구성요소  
- **Parser**: 다양한 파일 포맷을 파싱하여 텍스트와 메타데이터를 추출합니다.
- **Detector**: 파일 포맷을 감지합니다.
- **Language Identification**: 텍스트의 언어를 식별합니다.

#### 21.4. 유사 프레임워크  
- Apache PDFBox
- Apache POI

### 22. Mahout (Apache Mahout)
#### 22.1. 설명  
Apache Mahout은 머신러닝 알고리즘을 구현하고 대규모 데이터셋에서 이러한 알고리즘을 실행하기 위한 스케일러블한 라이브러리입니다. Mahout은 분류, 클러스터링, 추천 시스템 등 다양한 머신러닝 기법을 지원합니다.

#### 22.2. 특징  
- **다양한 머신러닝 알고리즘**: 분류, 클러스터링, 추천 시스템 등을 지원합니다.
- **확장성**: Hadoop을 이용하여 대용량 데이터셋을 처리할 수 있습니다.
- **프로그래밍 언어 독립성**: Java 및 기타 언어를 통해 사용할 수 있습니다.

#### 22.3. 구성요소  
- **Algorithms**: 다양한 머신러닝 알고리즘을 제공합니다.
- **Math Library**: 수학적 연산을 위한 라이브러리를 제공합니다.
- **Integration**: Hadoop과의 통합을 지원하여 대규모 데이터 처리를 가능하게 합니다.

#### 22.4. 유사 프레임워크  
- MLlib (Apache Spark)
- Scikit-learn

### 23. Drill (Apache Drill)
#### 23.1. 설명  
Apache Drill은 대규모 분산 데이터 저장 시스템에서 구조화되지 않은 또는 반구조화된 데이터를 위한 SQL 쿼리 엔진입니다. Drill은 복잡한 데이터 분석을 위해 다양한 데이터 소스를 통합하고, 사용하기 쉬운 SQL 인터페이스를 제공합니다.

#### 23.2. 특징  
- **다양한 데이터 소스 지원**: HDFS, NoSQL, 클라우드 저장소 등 다양한 데이터 소스를 지원합니다.
- **스키마 온-더-플라이**: 데이터를 쿼리할 때 스키마를 동적으로 해석합니다.
- **확장성과 성능**: 대규모 데이터셋에 대해 빠른 쿼리 성능을 제공합니다.

#### 23.3. 구성요소  
- **Query Engine**: SQL 쿼리를 실행하는 엔진.
- **Storage Plugin**: 다양한 데이터 소스와의 연결을 관리합니다.
- **Execution Engine**: 쿼리 계획을 실행하고 결과를 처리합니다.

#### 23.4. 유사 프레임워크  
- Apache Hive
- Apache Impala

### 24. Calcite (Apache Calcite)
#### 24.1. 설명  
Apache Calcite은 데이터베이스와 스트리밍 시스템을 위한 독립적인 SQL 쿼리 엔진 및 프레임워크입니다. Calcite는 다양한 데이터 소스에 대해 통합된 SQL 인터페이스를 제공하며, 쿼리 최적화 및 실행 계획을 생성합니다.

#### 24.2. 특징  
- **플러그형 아키텍처**: 다양한 데이터 스토리지 및 처리 시스템에 쉽게 통합할 수 있습니다.
- **고급 쿼리 최적화**: 여러 최적화 규칙과 비용 기반 최적화를 제공합니다.
- **다양한 쿼리 언어 지원**: SQL 뿐만 아니라 다양한 쿼리 언어를 지원합니다.

#### 24.3. 구성요소  
- **SQL Parser**: SQL 쿼리를 파싱합니다.
- **Query Optimizer**: 쿼리를 최적화하고 실행 계획을 생성합니다.
- **Query Executor**: 최적화된 쿼리를 실행합니다.

#### 24.4. 유사 프레임워크  
- Apache Drill
- Apache Hive

### 25. Flume (Apache Flume)
#### 25.1. 설명  
Apache Flume은 대규모 로그 데이터를 수집하고, 집계하며, Hadoop 같은 데이터 저장소로 이동시키기 위한 분산 시스템입니다. Flume은 데이터 생성 소스에서 HDFS, HBase 등의 목적지까지 안정적으로 데이터를 전송합니다.

#### 25.2. 특징  
- **데이터 수집**: 다양한 소스로부터 로그 데이터와 이벤트를 수집합니다.
- **데이터 집계**: 수집된 데이터를 집계하고 효율적으로 처리합니다.
- **데이터 전송**: 데이터를 안전하고 신뢰성 있게 Hadoop 저장소로 전송합니다.

#### 25.3. 구성요소  
- **Agent**: 데이터를 수집하고 처리하는 독립적인 프로세스.
- **Source**: 데이터를 수집하는 시작점.
- **Channel**: 데이터를 임시로 저장하는 버퍼.
- **Sink**: 데이터를 최종 목적지로 전송하는 구성요소.

#### 25.4. 유사 프레임워크  
- Apache Kafka
- Logstash

### 26. Superset
#### 26.1. 설명  
Superset은 데이터 탐색 및 시각화를 위한 오픈 소스 비즈니스 인텔리전스 도구입니다. 사용자는 Superset을 사용하여 데이터를 쉽게 탐색하고, 대화형 대시보드를 생성하며, 다양한 시각화를 통해 데이터를 분석할 수 있습니다.

#### 26.2. 특징  
- **다양한 데이터 소스 지원**: SQL 데이터베이스, Druid, Google BigQuery 등 다양한 데이터 소스를 지원합니다.
- **강력한 시각화 도구**: 다양한 시각화 옵션과 사용자 정의 대시보드를 제공합니다.
- **사용자 친화적 인터페이스**: 직관적인 사용자 인터페이스를 통해 데이터 탐색과 시각화를 쉽게 수행할 수 있습니다.

#### 26.3. 구성요소  
- **Dashboard**: 다양한 시각화를 통합하여 보여주는 대시보드.
- **Chart**: 데이터를 시각화하는 다양한 차트 옵션.
- **SQL Lab**: SQL 쿼리를 작성하고 데이터를 탐색할 수 있는 도구.

#### 26.4. 유사 프레임워크  
- Grafana
- Tableau

### 27. Beats
#### 27.1. 설명  
Beats는 Elastic Stack의 경량 데이터 수집기로, 다양한 유형의 데이터를 수집하여 Logstash 또는 Elasticsearch로 전송할 수 있습니다. Beats는 시스템 로그, 네트워크 트래픽, 클라우드 서비스 데이터 등 다양한 데이터 소스에서 데이터를 수집할 수 있습니다.

#### 27.2. 특징  
- **경량성**: 시스템 자원을 많이 사용하지 않으면서 데이터를 효율적으로 수집합니다.
- **모듈성**: Filebeat, Metricbeat, Packetbeat 등 특정 데이터 유형에 특화된 다양한 Beats가 있습니다.
- **유연성**: 데이터를 Logstash로 전송하여 추가 처리를 할 수도 있고, 직접 Elasticsearch로 전송할 수도 있습니다.

#### 27.3. 구성요소  
- **Filebeat**: 로그 파일을 수집하기 위한 Beats.
- **Metricbeat**: 시스템, 서비스, 컨테이너의 메트릭을 수집하기 위한 Beats.
- **Packetbeat**: 네트워크 패킷 데이터를 수집하기 위한 Beats.

#### 27.4. 유사 프레임워크  
- Fluentd
- Logstash

### 28. Logstash
#### 28.1. 설명  
Logstash는 데이터 처리 파이프라인을 구축하는 데 사용되는 서버 사이드 데이터 처리 파이프라인 도구입니다. 로그, 이벤트 데이터 등을 수집, 변환, 저장하는 데 사용되며, Elastic Stack의 주요 구성 요소 중 하나입니다.

#### 28.2. 특징  
- **다양한 플러그인 지원**: 입력, 필터, 출력 플러그인을 통해 다양한 소스의 데이터를 수집하고 변환할 수 있습니다.
- **강력한 데이터 처리**: 데이터 변환, 정제, 풍부화 등 복잡한 데이터 처리 작업을 수행할 수 있습니다.
- **확장성**: 대량의 데이터를 처리할 수 있도록 설계되었습니다.

#### 28.3. 구성요소  
- **Input Plugins**: 다양한 소스로부터 데이터를 수집합니다.
- **Filter Plugins**: 데이터를 변환하고 풍부화하는 데 사용됩니다.
- **Output Plugins**: 처리된 데이터를 다양한 목적지로 전송합니다.

#### 28.4. 유사 프레임워크  
- Fluentd
- Apache NiFi

### 29. Elasticsearch
#### 29.1. 설명  
Elasticsearch는 높은 확장성을 갖춘 분산형 검색 및 분석 엔진입니다. Elasticsearch는 복잡한 검색 기능, 실시간 분석, 그리고 텍스트, 숫자, 지리적 위치, 구조화된 및 비구조화된 데이터 등 다양한 유형의 데이터를 처리할 수 있습니다.

#### 29.2. 특징  
- **풀 텍스트 검색**: 강력한 풀 텍스트 검색 기능을 지원합니다.
- **분산 시스템**: 데이터를 여러 노드에 분산하여 저장하고 처리합니다.
- **실시간 분석**: 데이터를 실시간으로 색인하고 검색할 수 있습니다.

#### 29.3. 구성요소  
- **Index**: 동일한 특성을 갖는 문서들의 집합입니다.
- **Document**: 데이터의 기본 단위로, JSON 형식으로 저장됩니다.
- **Node & Cluster**: 단일 노드 또는 여러 노드가 모여 구성된 클러스터로 데이터를 저장하고 처리합니다.

#### 29.4. 유사 프레임워크  
- Apache Solr
- Sphinx

### 30. Kibana
#### 30.1. 설명  
Kibana는 Elasticsearch 데이터를 시각화하고 탐색하는 데 사용되는 오픈 소스 데이터 시각화 플랫폼입니다. Kibana를 사용하면 데이터에 대한 인사이트를 얻기 위해 차트, 테이블, 지도 등 다양한 방식으로 데이터를 시각화할 수 있습니다.

#### 30.2. 특징  
- **다양한 시각화 옵션**: 막대 차트, 선 차트, 파이 차트, 지도 등 다양한 시각화 옵션을 제공합니다.
- **대시보드 생성**: 여러 시각화를 결합하여 대시보드를 생성할 수 있습니다.
- **데이터 탐색**: Elasticsearch에서 데이터를 탐색하고 상세 분석을 수행할 수 있습니다.

#### 30.3. 구성요소  
- **Visualizations**: 데이터를 시각화하는 다양한 방법.
- **Dashboard**: 여러 시각화를 결합한 사용자 정의 가능한 대시보드.
- **Discover**: 데이터를 탐색하고 검색할 수 있는 인터페이스.

#### 30.4. 유사 프레임워크  
- Grafana
- Tableau



