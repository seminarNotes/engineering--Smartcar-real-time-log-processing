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
