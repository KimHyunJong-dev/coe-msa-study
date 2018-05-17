# PART I: The Mechanics of Change
## Chapter 1: Changing Software
### 소프트웨어를 변경 하려는 4가지 이유
- 기능 추가
- 버그 수정
- 디자인 개선 (리팩토링)
- 자원 사용 최적화
### 변경점에 따른 차이
| | Adding a Feature | Fixing a Bug | Refactoring | Optimizing |
|---|---|---|---|---|
|Structure|Changes|Changes|Changes|-|
|New Functionality|Changes|-|-|-|
|Functionality|-|Changes|-|-|
|Resource Usage|-|-|-|Changes|

### 위험한 변경
- 유닛테스트의 커버가 필요하다.
