# Hadoop Ecosystem
최초 작성일 : 2024-01-23  
마지막 수정일 : 2024-01-23
  
## 0. Overview
하둡 에코 시스템(Hadoop Ecosystem)은 대규모 데이터를 저장, 처리 및 분석하기 위한 Apache Hadoop 프레임워크의 여러 컴포넌트 및 관련 기술들의 모음을 의미한다. 이 에코 시스템은 다양한 작업을 수행하고 다양한 비즈니스 및 데이터 분석 요구 사항을 해결하기 위해 다양한 툴과 기술로 구성되어 있다. 이 글에서는 하둡 에코시스템을 구성하는 프레임워크의 설명과 간단한 사용 예제에 대해 알아본다.

<img src="./images/logo_hadoop.png" width="360" height="120" alt="Textbook">


하둡(Hadoop)은 대규모 데이터 집합을 분산하여 처리하고 저장할 수 있는 오픈 소스 분산 컴퓨팅 프레임워크이다. 아파치 소프트웨어 재단에서 개발하고 관리하며, 빅데이터 처리와 저장을 위한 핵심 도구 중 하나이며, 하둡은 크게 두 가지 핵심 구성 요소로 구성되어 있습니다.

- Hadoop Distributed File System (HDFS)  
  HDFS는 대용량 데이터를 분산하여 저장하기 위한 파일 시스템이며, 이 파일 시스템은 데이터를 여러 노드에 나누어 저장하고, 데이터 손실을 방지하기 위한 복제 기능을 제공한다. 이를 통해 빅데이터를 안정적으로 저장하고 관리하는 데 중요한 역할을 수행한다.

- MapReduce
  MapReduce는 데이터를 처리하고 분석하는 데 사용되는 프로그래밍 모델과 프레임워크이다. MapReduce 작업은 두 단계로 나누어지는데, "맵" 단계에서 데이터를 분할하고 처리하며, "리듀스" 단계에서 중간 결과를 집계하고 최종 결과를 생성한다. 이 과정을 통해 병렬 처리와 분산 데이터 처리를 수행한다.

하둡은 하드웨어 클러스터를 활용하여 대용량 데이터를 효율적으로 처리하고 저장할 수 있는 기능을 제공한다. 이러한 특징으로 하둡은 대규모 데이터 처리, 데이터 분석, 머신러닝, 그리고 데이터 웨어하우스 등 다양한 빅데이터 작업에 사용된다. 또한 하둡 생태계는 다양한 관련 프로젝트와 도구를 포함하고 있어, 다양한 빅데이터 요구 사항에 맞게 확장하여 사용할 수 있다.

아래에서 설명하는 server01과 server02는 직접 linux 서버를 구성한 것을 기준으로 설명한 것으로, 각 프레임워크는 cloudera를 통해 구축하였으며, host의 구성은 아래와 같다.  

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

## Table of Contents

1. [Hadoop Distributed File System](#1.-Hadoop-Distributed-File-System)


## 1. Hadoop Distributed File System  

Hadoop Distributed File System (HDFS)는 아파치 하둡(Hadoop) 프레임워크의 핵심 구성 요소 중 하나로, 대용량 데이터를 저장하고 관리하기 위한 분산 파일 시스템이다. HDFS는 주로 빅데이터 처리를 위한 분산 스토리지 시스템으로 사용되며, 하둡 클러스터의 여러 노드에 데이터를 블록(일반적으로 128MB 또는 256MB 크기) 단위로 나누어 분산 저장하여 고가용성과 데이터 병렬 처리를 할 수 있도록 한다. 

아래에서는 HDFS를 사용하기에 자주 사용되는 명령어에 대해서 알아본다. 먼저, HDFS 명령어에는 자주 사용되는 접두사가 있는데, 각 명령어 요소는 아래와 같은 의미이다.

|Prefix|Description|
|--|--|
|hdfs|Hadoop Distributed File System에 접근하기 위한 Hadoop 명령어|
|dfs|Distributed File System의 약어로, Hadoop의 분산 파일 시스템|
|fsck|file system check의 약어로, 파일 시스템 체크를 수행하는 옵션|

이 외 사용되는 명령어는 UNIX 또는 LINUX 명령어와 유사하다.

### 1-1. HDFS에 파일 저장
먼저 Server02에 SSH 접속을 하고, 업로드한 샘플 파일이 있는 위치로 이동해서 HDFS의 put 명령을 실행한다.

PuTTy 프로그램 실행
Server02에 root 계정으로 SSH 접속
$ cd /home/bigdata
$ hdfs dfs -put Sample.txt /tmp

위 명령어를 통해 Sample.txt 파일이 HDFS의 /tmp 디렉터리에 저장


2. HDFS에 저장한 파일 확인
$ hdfs dfs -ls /tmp
앞서 /tmp 디렉터리에 저장한 "Sample.txt" 파일의 목록을 저장

3. HDFS에 저장한 파일 내용 보기
$ hdfs dfs -cat /tmp/Sample.txt

4. HDFS에 저장한 파일 상태 확인
$ hdfs dfs -stat '%b %o %u %n' /tmp/Sample.txt
파일크기(%b), 파일 블록 크기(%o), 복제 수(%r), 소유자명(%u), 파일명(%n) 정보를 출력한다.

5. HDFS에 저장한 파일의 이름 바꾸기
$ hdfs dfs -mv /tmp/Sample.txt /tmp/Sample2.txt

6. HDFS의 파일 시스템 상태 검사
$ hdfs fsck /
전체 크기, 디렉터리 수, 파일 수, 노드 수 등 파일 시스템의 전체 상태를 출력한다

$ hdfs dfsadmin -report
하둡 파일시스템의 기본 정보 및 통계를 출력한다

7. HDFS에 저장된 파일을 로컬 파일 시스템으로 가져오기
$ hdfs dfs -get /tmp/Sample2.txt

8. HDFS의 저장한 파일 삭제(휴지통)
$ hdfs dfs -rm /tmp/Sample2.txt

삭제 명령을 실해앟면 우선 휴지통으로 임시 삭제되며, 복구가 가능하다.
휴지통으로 임시 삭제된 파일을 특정 시간이 지나면 자동으로 완전 삭제가 된다.
휴지통에 임시 삭제가 불필요할 때는 -skipTrash 옵션을 사용한다.

추가적으로 HDFS 파일의 비정상적인 상태 식별되는 경우, CORRUPT CLOCK, MISSING BLOCK, MISSING SIZE, CORRUPT BLOCKS 등의 항목에 숫자가 표기된다. 이와 같은 상황이 지속되면 하둡과 연결된 HBase, Hive에 이슈가 발생할 수 있다. 비정상적인 파일 블록이 식별된 경우, 다른 노드에 복구하려고 시도하여, 다음과 같이 사용자가 강제 삭제/이동 명령을 하여 이슈 사항을 조치할 수 있다.

Safe Mode는 HDFS에서 데이터의 안정성을 유지하고 클러스터의 상태를 관리하기 위한 모드로, 일반적으로 클러스터의 유지 보수 또는 복구 작업 중에 활성화된다. 안전 모드 상태로 전환되었다면, 강제 안전 모드를 해제함으로써, "Safe Mode"를 빠져나와야 한다.

$ hdfs dfsamin -safemode leave

손상된 파일(MISSING BLOCK, CORRUPT CLOCK)에 대비해 하둡은 Replication 기술을 존재한다. Replication은 주 파일(main file)과 서브 파일(sub file) 사이의 복제 및 교체 프로세스를 의미한다. 하둡은 파일 저장 시, Server01과 Server02에 각각 파일을 저장하여, 한쪽 서버의 파일을 손상 될 경우, 나머지 한쪽 서버에서 파일 복제하여, 손상된 파일 대체하는 시스템이 존재한다. 하지만, 양쪽 서버의 동일한 파일이 손상될 경우, 복제/복구 작업이 불가능하기 때문에 이 경우, 사용자에 의해 파일을 강제 삭제를 해야한다.

$ hdfs fsck / -delete

위 명령어는 root 디렉토리에 존재하는 파일 중, 손상된 파일 모두 삭제하는 명령어이다. 하지만, 손상된 파일 중, 시스템에 의해 복구가 진행 중인 파일도 존재하기 때문에 손상된 파일을 /lost + found 디렉터리로 옮기는 명령어를 사용하는데 

$ hdfs fsck / -move

위 명령어는 손상된 파일/블록을 옮기는데 사용이 된다.


