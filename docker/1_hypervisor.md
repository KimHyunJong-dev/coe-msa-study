# 1\_Hypervisor

## 1. 클라우드\(Cloud\)의 개념

#### SPI 모델

* IaaS\(Infrastructure as a Service\) : 서버 자원\(CPU/메모리..\) 가상화 ex\) AWS EC2
* PaaS\(Platform as a Service\) : OS + Runtime\(Java\) + Platform\(Spring, Hadoop..\) ex\) AWS memory
* SaaS\(Software as a Service\) : 서비스 일체\(Application + Data\). 사용자는 계정만 만들어서 사용 ex\) Google Drive

> 클라우드를 구축하기 위한 요소 기술에 가상화, 도커와 같은 컨테이너 기술 필요

#### 가상화\(Virtualization\)

컴퓨터 자원\(CPU, 메모리, 저장장치, 네트워크 등\)의 추상화

#### 클라우드/가상화 적용 사례

* 아마존 AWS
* MS Azure
* 구글드라이브, 드랍박스, N드라이브...
* OpenStack : IaaS스타일의 오픈소스 클라우드 구축 플랫폼

## 2. 가상화기술의 개념\(하이퍼바이저, 전가상화/반가상화\)

### 하이퍼바이저 기반 가상화의 특징

#### 하이퍼바이저 주요 개념

* 하이퍼바이저 : 호스트시스템에서 다수의 게스트 OS를 돌리기 위한 플랫폼 ex\) VMWare
* 호스트OS : 메인시스템에 설치된 OS
* 게스트OS : 가상머신/컨테이너 위에 설치된 OS

#### 하이퍼바이저 유형

* Type 1 : 하이퍼바이저와 호스트OS가 결합된 형태.

  ```text
       하이퍼바이저가 하드웨어 위에서 바로 실행.
       Hardware - Hypervisor(+hostOS) - OS
  ```

* Type 2 : 호스트OS 위에 하이퍼바이저 실행

  ```text
       Hardware - hostOS - Hypervisor - guestOS
       ex) Win-VMWare-Linux
  ```

#### 하이퍼바이저기반 가상화의 특징

* 장점 : 다양한 게스트OS 실행 가능
* 단점 : 물리시스템과 게스트OS간의 가상화 기능으로 인한 성능 저하
  * 약 10~15% 오버헤드
  * 호스트OS와 게스트OS간의 기능중복

### 전가상화와 반가상화

#### 전가상화

* 하드웨어 완전히 가상화, OS제약 없이 사용 가능
* 게스트OS는 자신이 가상머신 위에서 동작하고 있다는 것 인식 불가
* 게스트OS에서 물리자원 직접 접근 불가. 반드시 하이퍼바이저를 통해 변환 후 접근

#### 반가상화

* 게스트OS가 자신이 가상머신 위에서 동작하고 있다는 것을 인식
* HyperCalls를 통해 게스트OS에서 물리자원 직접 접근 가능\(성능 개선\)

  -&gt; 커널을 수정해 변환과정 없이 물리자원 직접 접근 가능하나 커널을 수정해야 하므로 현재 리눅스만 가능

#### 하이퍼바이저 종류

* VMWare
  * ESXi \(Type 1\), Workstation, Workstation Player \(Type 2\)
* MS Hyper-V : 윈도우서버에 통합
* Ctrix Xen\(오픈소스\) : 최초의 반가상화 하이퍼바이저
* KVM\(오픈소스\)
* Parallels : OSX기반
* Oracle VirtualBox : GPL\(Open License\) 기반

#### 하이퍼바이저 자동화 기술

Vagrant

* 스크립트를 이용한 Box 설치
* VM 위에 스크립트를 통해 자동으로 OS설치
* VirtualBox\(무료\), VMWare, 도커 지원

호스트OS에서 vagrant 설치 후,

```text
$ vagrant init ubuntu/trusty64    --vagrantfile 생성
-> 생성 된 VagrantFile에서 IP주소나 이미지 명 확인
$ vagrant up     -- 박스 다운로드 및 생성
-> 설치 완료 후 Virtual Box에서 생성된 가상머신 확인
$ vagrant ssh
-> 가상머신 ssh 접속
```

