# PART I: The Mechanics of Change
## Chapter 2: Working with Feedback
### 유닛 테스트란?
- Common to most conceptions of unit tests is the idea that they are tests in isolation of individual components of software. 
- 사이즈가 큰 테스트의 단점
  - 실패한 경우에 실패한 지점을 찾는데 시간이 오래 걸린다.
  - 수행속도가 느리다.
  - Coverage tool을 사용하여 coverage를 정확히 측정하기 힘들다. 
### 좋은 유닛 테스트의 지표
- 빠른 수행 속도 (0.1초 이내)
  - 데이터베이스 / 네트워크 / 파일IO 등의 시간이 오래걸리는 작업을 하지 않는다.
### 레거시코드의 딜레마
- 코드를 변경하려 할 때 테스트코드가 필요하고, 테스트 코드를 작성하려면 보통 코드를 변경해야 한다.
### 레거시코드 변경 방법
1. 변경 부분을 식별하라 (Chapter 16, Chapter 17)
2. 테스트 포인트를 찾아라 (Chapter 11, Chapter 12)
3. 의존성을 끊어라 (Chapter 7, Chapter 9, Chapter 10, Chapter 22, Chapter 23)
4. 테스트를 작성하라 (Chapter 13)
5. 코드를 변경하고 리팩토링 하라 (Chapter 8, Chapter 20, Chapter 21, Chapter 22)
