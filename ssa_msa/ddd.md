##### uuid 사용. id generate를 위해 blocking 금지 
##### ubiquotous 언어 사용. ex) Leg. originLocation/destinationLocationId
##### domain 용어를 정확히 사용했다면 모델링만 봐도 추가 설명 없이 이해할 수 있음

##### 모델(uml.개념)과 코드는 1:1를 일치하지 않음. 모델(zoom-out) <-> 코드(zoom-in) Generate/Reverse하면서 계속 확장시킴


#### 서비스 간 세션 정보 공유 :  context 공유할 수 있는 tenant service 필요
#### 서비스 간 트랜젝션 처리 : rollback은 없고 보상트랜젝션 처리 필요. transaction이 중요한다면 하나의 서비스로 묶어서 서비스 내 처리. Nosql일 경우 document내에서 트랜잭션 처리 가능

#### Message Queue를 이용한 사례 : DB(ESB or EAI)x -> broker를 이용한 느슨한 결합. 실시간 분산은 zookeeper가. kafka는 stream processing에 집중. kafka 자원이 많이 필요



내부 개발 방법

하나의 화면에서 여러 서비스 호출해서 데이터 aggregate할 경우. key mapping해주거나 여러번 호출하는 경우 생김
join이 없기때문에 코드레벨에서 처리함. 
-> 
그래서 immutable데이터인 경우 메세지 전달해서 들고있게 함. 아니면 json 데이터를 한벌로 저장해놓고 처리. 
DB가 아니라 서비스간 호출로 처리하는 코드레벨 확인 필요


content 권한 관리. 
custom annotation + aop 

cqrs 
update는 자주 일어나지 않고
read 많은 경우. read 영역만 scale-out

command area
1. update REST
2. query area에서 데이터 가져와서 변경 된 값으로 바꿔서 
3. mq produce
4. query area에서 subscribe 후 다시 만들기

command area block이 없음. 

event를 통한 snapshot생성
event store - log 
domain store - snapshot

-> lock을 안 걸기 때문에 변경 중인 상태에서 다시 업데이트 할 경우 reject 필요
-> entity version으로 관리(업데이트할 때 버전을 가지고 가서 source랑 비교. 업데이트된 버전에 대해서 업데이트)
command에서 modify service에 version id param 

youtube. building ms with event sourcing and CQRS
