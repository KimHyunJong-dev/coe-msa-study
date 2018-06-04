# Chapter 4: The Seam Model

## Chapter 4: The Seam Model

### Seam ?

* 수정하지 않고 프로그램의 수행 동작을 바꿀 수 있는 부분.

  **예제코드**

  ```cpp
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
      PostReceiveError(SOCKETCALLBACK, SSL_F AILURE);
    }
    CreateLibrary(m_hSslDll1,“syncesel1.dll”);
    CreateLibrary(m_hSslDll2,“syncesel2.dll”);
    m_hSslDll1->Init();
    m_hSslDll2->Init();
    return true;
  }
  ```

  ```cpp
  PostReceiveError(SOCKETCALLBACK, SSL_FAILURE);
  ```

* 예제 코드 중 위 코드는 다른 서브시스템과 통신하는 전역 함수로 테스트를 진행하는데 방해가 되는 부분이다.

  **회피**

  ```c++

  // header file

  class CAsyncSslRec

  {

   …

   virtual void PostReceiveError\(UINT type, UINT errorcode\);

   …

  };

  // source file void CAsyncSslRec::PostReceiveError\(UINT type, UINT errorcode\) { ::PostReceiveError\(type, errorcode\); }

  // test file class TestingAsyncSslRec : public CAsyncSslRec { virtual void PostReceiveError\(UINT type, UINT errorcode\) { }}};

  ```

* 전역 함수와 동일한 이름의 멤버 함수를 생성하고, 새로 생성한 함수에서 전역 함수를 호출한다. 테스트코드에서는 해당 멤버 함수를 구현하여 전역 함수 호출을 회피한다.
* 이렇게 테스트 대상 함수의 원래 동작을 대체하여 의존성을 끊을수있는 부분을 Seam\(이음새\)라고 합니다.

  **Seam Types**

* [Preprocessing Seams](../chapter-4-the-seam-model-types-1.-preprocessing-seams.md)
* [Link Seams](../chapter-4-the-seam-model-types-2.-link-seams.md)
* [Object Seams](../chapter-4-the-seam-model-types-3.-object-seams.md)

  **올바른 Seams Type 선택**

* 위 예제코드는 다양한 종류의 Seams를 적용 할 수 있다.
  * Link : PostReceiveError는 전역 함수로서 Stub function으로 된 라이브러리를 생성하여 Link단계에서 행동을 변경 할 수 있다.
  * Preprocessing : PostReceiveError함수를 대체하는 define macro가 작성된 헤더 파일을 include하여 행동을 변경할 수 있다.
  * Object : 위 예제에서 설명한 방법이 Object Seam으로 행동을 변경 하는 것 이다.
* 일반적으로 객체지향 언어의 경우 Object Seams가 최선의 선택 이다. Preprocessing seams와 Link seams는 유용해 보이지만 Object seams에 비해 명확하지 않다.
* 태스트코드는 유지보수 하기 쉬워야 하므로 Preprocessing seams와 Link seams는 불가피한 경우에만 사용한다.
* Seams에 익숙해지면 테스트 코드를 작성하기 쉬워지고, 테스트를 작성하기 쉬운 코드를 작성하게 된다.

