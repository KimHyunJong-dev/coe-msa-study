# PART I: The Mechanics of Change
## Chapter 4: The Seam Model Types - 3. Object Seams
### Object Seams
```c++
bool CAsyncSslRec::Init()
{
  if (m_bSslInitialized) {
    return true;
  }
  m_smutex.Unlock();
  m_nSslRefCount++;
  m_bSslInitialized = true;
  FreeLibrary(m_hSslDll1);
  m_hSslDll1=0;
  FreeLibrary(m_hSslDll2);
  m_hSslDll2=0;
  if (!m_bFailureSent) {
    m_bFailureSent=TRUE;
    PostReceiveError(SOCKETCALLBACK, SSL_FAILURE);
  }
  CreateLibrary(m_hSslDll1,“syncesel1.dll”);
  CreateLibrary(m_hSslDll2,“syncesel2.dll”);
  m_hSslDll1->Init();
  m_hSslDll2->Init();
  return true;
}
```
- PostReceiveError 함수는 전역 함수로 의존성이 있다.
```c++
// header file
class CAsyncSslRec
{
   …
   virtual void PostReceiveError(UINT type, UINT errorcode);
   …
};

// source file
void CAsyncSslRec::PostReceiveError(UINT type, UINT errorcode)
{
  ::PostReceiveError(type, errorcode);
}

// test file
class TestingAsyncSslRec : public CAsyncSslRec
{
  virtual void PostReceiveError(UINT type, UINT errorcode)
  {
  }
};
```
- 전역 함수와 동일한 이름의 멤버 함수를 생성하고, 새로 생성한 함수에서 전역 함수를 호출한다. 테스트코드에서는 해당 멤버 함수를 구현하여 전역 함수 호출을 회피한다.
### Object Seams 가 아닌경우 1
```java
public class CustomSpreadsheet extends Spreadsheet
{
  public Spreadsheet buildMartSheet() {
    …
    Cell cell = new FormulaCell(this, “A1”, “=A2+A3”);
    …
    cell.Recalculate();
    …
  }
  …
}
```
- 위 예제는 코드에서 Cell의 타입을 지정하기 때문에(new FormulaCell ...) Seam이 될 수 없다.
```java
public class CustomSpreadsheet extends Spreadsheet
{
  public Spreadsheet buildMartSheet(Cell cell) {
    …
    cell.Recalculate();
    …
  }
  …
}
```
- 매개변수를 통해 받도록 개선하면 Seam이 된다.
### Object Semas가 아닌 경우 2
```java
public class CustomSpreadsheet extends Spreadsheet
{
  public Spreadsheet buildMartSheet(Cell cell) {
    …
    Recalculate(cell);
    …
  }
  private static void Recalculate(Cell cell) {
    …
  }
  …
}
```
- private static 메소드의 경우.
```java
public class CustomSpreadsheet extends Spreadsheet
{
  public Spreadsheet buildMartSheet(Cell cell) {
    …
    Recalculate(cell);
    …
  }
  protected void Recalculate(Cell cell) {
    …
  }
  …
}
```
- protected 메소드로 변경 하여 override를 통해 변경이 가능하다.

[Chapter 4로 이동](https://github.com/SDSACT/microservice-coe-refactoring/wiki/%5BWELC%5D-Chapter-4:-The-Seam-Model)