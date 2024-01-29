# Real Time Log Processing
최초 작성일 : 2024-01-22  
마지막 수정일 : 2024-01-26
  
## 0. Overview

프로젝트를 시작하기 전, 몇 주 동안 데이터 이론과 분석 이론을 공부하고 몇 가지 프레임워크를 활용하여 데이터 파이프라인을 구축하고 분석해보았다. 하지만 데이터 엔지니어링을 더 깊게 이해하기 위해서는 분산 컴퓨팅을 통한 데이터 처리 경험이 필요하다고 판단하였고, 이를 위해 최소한 하나의 분산 컴퓨팅 책을 완벽히 이해하고 습득하겠다는 목표를 갖게 되었다. 나는 다양한 책 중에서 '실무로 배우는 빅데이터 기술'을 선택하였다. 이 책은 빅데이터 이론부터 실제 분산 환경에서 데이터 ETL, 데이터 분석, 활용까지 포괄적으로 다루고 있다.  

책에서는 스마트카 내의 수백 개의 IoT 센서로부터 자동차 상태를 모니터링하고, 수많은 차량 상태 정보를 실시간으로 수집하여 해당 데이터를 처리하고 분석하는 빅데이터 처리 파일럿 프로젝트 내용을 담고 있다. 하지만, 하루가 다르게 프레임워크는 변화하고 있기 때문에 책을 통해 공부를 하더라도 많은 시행착오를 겪을 것으로 생각한다.

<img src="./images/textbook.png" width="300" height="340" alt="Textbook">

위 교재를 공부하고, 관련된 내용을 정리하기 위해 빅데이터에 대한 이론, 프레임워크, 프로젝트 내용으로 총 4개의 내용을 나누어 markdown(.md)을 작성한다. Framework에는 교재에서 다루는 것을 중점으로 설명하되, 책 외적인 내용에 대해서도 정리할 예정이다.

|Title|Desciption|Reference|
|--|--|--|
|[Introduction](https://github.com/seminarNotes/engineering--Smartcar-real-time-log-processing/blob/main/Introduction.md)|빅데이터에 대한 내용|김강원. "실무로 배우는 빅데이터 기술." 위키북스, 2022|
|[Framework](https://github.com/seminarNotes/engineering--Smartcar-real-time-log-processing/blob/main/Framework.md)|프레임워크에 대한 내용|별도 자료 조사|
|[Pilotproject](https://github.com/seminarNotes/engineering--Smartcar-real-time-log-processing/blob/main/Pilotproject.md)|프로젝트에 대한 내용| [GitHub Link](https://github.com/wikibook/bigdata2nd), [Inflearn Link](https://www.inflearn.com/)|
|[Terminology](https://github.com/seminarNotes/engineering--Smartcar-real-time-log-processing/blob/main/Terminology.md)|용어에 대한 내용|별도 자료 조사|

