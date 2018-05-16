# PART I: The Mechanics of Change
## Chapter 3: Sensing and Separation
### 의존성을 끊는 두가지 이유
- Sensing : 코드를 통해 접근할 수 없는 값을 감지
- Separation : 테스트 코드에서 실행 시킬수 없는 코드를 분리
### NetworkBridge 예제
```java
// NetworkBridge accepts an array of EndPoints and manages their configuration using some local
// hardware. Users of NetworkBridge can use its methods to route traffic from one endpoint to
// another. NetworkBridge does this work by changing settings on the EndPoint class. Each instance of
// the EndPoint class opens a socket and communicates across the network to a particular device.
public class NetworkBridge
{
  public NetworkBridge(EndPoint [] endpoints) {
    …
  }
  public void formRouting(String sourceID, String destID) {
    …
  }
  …
}
```
- Sensing : 메소드 호출에 대한 효과를 감지 할 수 없다. 
- Seperation : 응용 프로그램과 분리시켜 실행 할 수 없다. 
- 네트워크 페킷을 스니핑하여 확인 하거나, EndPoint에 대응하는 하드웨어를 연결하여 테스트를 위한 인스턴스를 생성할 때 멈추지 않도록 할 수 있으나 엄청난 노력이 필요하며, 변경할 코드는 EndPoint와 관계가 없을 수 도 있다.
### Faking Collaborators
![](https://user-images.githubusercontent.com/29725145/39417291-3212a2d6-4c8e-11e8-8fd8-a7a6d67579b9.png)
![](https://user-images.githubusercontent.com/29725145/39417295-330d87aa-4c8e-11e8-81a1-45f8e7d0f44f.png)
- ArtR56Display를 Display interface로 추상화하여 의존성을 끊고, 테스트시에는 FakeDisplay(FakeObject)를 만들어서 사용한다.
### Fake Object의 두면
![](https://user-images.githubusercontent.com/29725145/39417397-e5d30f2c-4c8e-11e8-8f7a-7e91cb987763.png)
```java
import junit.framework.*;
public class SaleTest extends TestCase
{
  public void testDisplayAnItem() {
    FakeDisplay display = new FakeDisplay();
    Sale sale = new Sale(display);
    sale.scan(“1”);
    assertEquals(“Milk $3.99”, display.getLastLine());
  }
}
```
- showLine : Display interface를 상속받아 구현, Sale 클래스에서 Display 객체의 메서드로서 사용한다.
- getLastLine : Sale 객체의 동작이 정확한지 확인하는데 사용한다.
### Mock Objects
```java
import junit.framework.*;
public class SaleTest extends TestCase
{
  public void testDisplayAnItem() {
    MockDisplay display = new MockDisplay();
    display.setExpectation(“showLine”, “Milk $3.99”);
    Sale sale = new Sale(display);
    sale.scan(“1”);
    display.verify();
  }
}
```
- 테스트 프레임워크의 Mock객체를 사용하여 더 쉽게 테스트 코드를 작성할 수 있다.
### 객체지향 언어가 아닌 경우
- Chapter 19
 