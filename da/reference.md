국민은행 차세대 계정계

https://img.etnews.com/photonews/2101/1373070_20210107160825_713_0001.jpg

여신 & 수신 업무 MSA

고객APP - 고객DB (자연인 & 법인 & ATM & 지로 ...)
계좌APP - 계좌DB
고객-계좌APP - 1:N 구조 고객계좌DB
거래APP - 거래내역DB & 거래이벤트
거래이벤트처리APP (입금 & 출금)


이체요청APP큐 - kafka (event hub)

이체요청처리APP - kafka에서 꺼내서 타행 거래 요청 큐에 넣고 성공 응답을 받으면, 이체요청을 이체내역DB에 넣고, 
               timeout이나, 실패면, 거래중지 내역을 이체내역DB에 기록, 
이체내역DB 






---------------------

azure big data warehouse

https://learn.microsoft.com/ko-kr/azure/architecture/solution-ideas/media/enterprise-data-warehouse.svg


on-premise hadoop

https://learn.microsoft.com/en-us/azure/hdinsight/hadoop/media/apache-hadoop-introduction/hdinsight-architecture-hybrid.png

