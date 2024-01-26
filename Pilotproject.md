
프로젝트의 요구 사항
- 요구사항1 : 차량의 다양한 장치로부터 발생하는 로그 파일을 수집해서 기능별 상태를 점검한다.
- 요구사항2 : 운전자의 운행 정보를 담긴 로그를 실시간으로 수집해서 주행패턴을 분석한다.



1. 수집 요구사항의 구체화

|Specific Requirements|Analysis and Solution Approach|
|--|--|
|스마트카로부터 로그 파일들을 주기적으로 발생|플롬을 이용해 대용량 배치 파일 및 실시간 로그 파일을 수집|
|스마트카의 배치 로그 파일 이벤트를 감지|플럼의 Source 컴포넌트 중, SpoolDir을 이용해서 주기적인 로그 파일 발생 이벤트 감지|
|스마트카의 실시간 로그 발생 이벤트를 감지|플럼의 Source 컴포넌트 중, Exec-Tail을 이용해 특정 로그 파일에서 로그 생성 이벤트를 감지|
|스마트카가 만들어내는 로그 데이터 중 가비지 데이터가  있을 가능성|플럼의 Interceptor를 이용해서 정상 패턴의 데이터만 필터링|
|수집 도중 장애가 발생해도 데이터를 안전하게 보관 및 재처리|플럼의 메모리 중 Channel 및 카프카 Broker 활용으로 로컬 디스크의 파일시스템에 수집 데이터 임시 저장|
|스마트카의 실시간 로그 파일은 비동기 처리로 빠른 수집 처리|플럼에서 수집한 데이터를 카프카 Sink 컴포넌트를 이용해 카프카 Topic에 비동기 전송|

플럼을 통해 데이터를 수집하기 위해 플럼 에이전트를 생성하고 설정해야 한다.
먼저, Agent 이름을 설정해야 하는데, 구성 파일 내 객체를 구분하는 이름이기 때문에 구성 파일(.conf)와 동일하게 한다면, Agent 이름은 큰 제약은 없다. 여기 Agent를 SeminarNotes_Agent라고 가정하고, 구성 파일을 작성해보겠다. 구성 파일은 Agent가 어떤 파일과 데이터를 읽어, 어떤 동작을 하는지 명세하는 파일이며, 각 구성 정보는 다음과 같은 의미를 갖는다.

``` conf
SeminarNotes_Agent.sources = SmartCarInfo_SpoolSource
SeminarNotes_Agent.channels - SmartCarInfo_Channel
SeminarNotes_Agent.sinks = SmartCarInfo_LoggerSink
```



















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
