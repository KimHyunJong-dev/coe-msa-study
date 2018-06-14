##### uuid 사용. id generate를 위해 blocking 금지 
##### ubiquotous 언어 사용. ex) Leg. originLocation/destinationLocationId
##### domain 용어를 정확히 사용했다면 모델링만 봐도 추가 설명 없이 이해할 수 있음

##### 모델(uml.개념)과 코드는 1:1를 일치하지 않음. 모델(zoom-out) <-> 코드(zoom-in) Generate/Reverse하면서 계속 확장시킴


#### 서비스 간 세션 정보 공유 :  context 공유할 수 있는 tenant service 필요
#### 서비스 간 트랜젝션 처리 : rollback은 없고 보상트랜젝션 처리 필요. transaction이 중요한다면 하나의 서비스로 묶어서 서비스 내 처리. Nosql일 경우 document내에서 트랜잭션 처리 가능
