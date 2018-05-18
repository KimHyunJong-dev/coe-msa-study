# 마이크로 서비스는 무엇인가?

> 본 문서는 http://microservices.io/ 의 문서를 번역한 자료입니다.

마이크로서비스(마이크로서비스 아키텍처)는 느슨하게 결함된 여러개의 서비스들로 어플리케이션을 구성하는 아키텍처이다.
마이크로서비스 아키텍처는 복잡하고 규모가 큰 어플리케이션의 지속적 딜리버리와 배포를 가능하게 한다. 또한 개발 조직의 기술 스택을 진화하도록 한다.

### 마이크로 서비스는 만능 해결책(Sliver Bullet)이 아니다
마이크로서비스 아키텍처는 만능이 아니다. 몇 가지 안좋은 점도 있다.
또한, 마이크로서비스 아키텍처를 적용하기 위해서는 많은 이슈들을 해결해야 한다.
마이크로서비스 아키텍처 패턴은 마이크로서비스 아키텍처를 적용하기위한 패턴을 모아놓은 것이며, 다음의 두 가지 목표를 갖는다.
1. 마이크로서비스를 어플리케이션에 적용하는 것이 적합한지 알 수 있다.
2. 마이크로서비스를 성공적으로 적용할 수 있게 한다.

### 시작점
전통적인 아키텍처 스타일인 모노리틱 아키텍처부터 시작한다.
모노리틱 아키텍처는 여전히 많은 어플리케이션에 적용되고 있지만, 수많은 문제점과 한계를 가지고 있다.
대규모의 복잡한 어플리케이션을 위해 마이크로서비스 아키텍처 패턴을 적용하는 것이 더 좋다.

### 마이크로서비스 어플리케이션 예제
Want to see an example? Check out Chris Richardson's Money Transfer and Kanban board examples.

[See code](http://eventuate.io/exampleapps.html)

### 마이크로서비스 사례 : 누가 마이크로서비스를 사용하고 있을까?
Many companies are either using microservices or considering using them. Read the case studies...

[Learn more](http://microservices.io/articles/whoisusingmicroservices.html)

### 학습자료 - O'Reilly: event-driven microservices

[Learn about patterns for developing, deploying and refactoring microservices.](http://shop.oreilly.com/product/0636920047551.do)

### 마이크로서비스 아키텍처 패턴

[마이크로서비스 패턴을 적용하는 방법](./how-to-apply-the-patterns.md)

핵심 패턴
- [모노리틱 아키텍처](./monolithic-architecture.md)
- [마이크로 서비스아키텍처](./microservices-architecture.md)

분해(Decomposition)
- 비즈니스 역량에 의한 분해
- 서브 도메인에 의한 분해

배포 패턴
- 각 호스트당 여러개의 서비스 인스턴스
- 각 호스트당 하나의 서비스 인스턴스
- 각 VM당 하나의 서비스 인스턴스
- 각 컨테이너당 하나의 서비스 인스턴스
- 서버리스 배포
- 서비스 배포 플랫폼

횡단 관심사 (Cross cutting concerns)
- [마이크로서비스 섀시](./microservice-chassis.md)
- [외부화된 환경설정](./externalized-configuration.md)

통신 방식
- 원격 프로시져 호출
- 메시징
- 도메인 기반 프로토콜

외부 API
- API 게이트웨이
- 프론트엔드를 위한 백엔드

서비스 탐지
- 클라이언트측 탐지
- 서버측 탐지
- 서비스 등록
- 자가 등록
- 써드 파티 등록

신뢰성
- 써킷 브레이커

데이터 관리
- 서비스에 의한 데이터베이스
- 공유되는 데이터베이스
- Saga
- API 분해
- CQRS (Command Query Responsibility Segregation)
- Event sourcing
- 트랜잭션 로그 추적
- 데이터베이스 트리거
- 어플리케이션 이벤트

보안
- Access Token

테스팅
- 서비스 컴포넌트 테스트
- 서비스 통합 컨트랙트 테스트

Observablility
- 로그 통합 수집
- Application metrics
- Audit logging
- Distributed tracing
- Exception tracking
- 헬스 체크 API
- Log deployments and changes

UI 패턴
- 서버측 페이지 프레그먼트 분해
- 클라이언트측 UI 분해
