## Host에서 돌아가는 application과 다른점
- File System : 컨테이너는 자체 파일시스템을 갖고 호스트 시스템의 파일시스템에 접근 할 수 없다 예외적으로, 호스트의 /etc/hosts 등과 같은 파일은 컨테이너에 자동 마운트 되고 
그 외 마운트될 디렉토리를 명시적으로 지정할 수 있다. 
- Process table : 컨테이너는 자체 process table을 갖기 때문에 처음 시작시 PID 1부터 할당된다. 호스트 시스템의 process table을 볼 수 없다. 
- Network Interface : 컨테이너가 다른 컨테이너의 network interfaces를 사용할 수 있다. ??
- IPC facility : 컨테이너는 각각 자신의 IPC(inter-process communication: 프로세스 간 통신) facility를 갖는다. 호스트 시스템의 IPC facility를 컨테이너에 노출할 수 있다. 
  * IPC : 프로세스들 사이에 서로 데이터를 주고 받는 행위 또는 그에 대한 방법이나 경로. 프로세스의 특정 메모리 영역을 공유하여 읽고 쓰거나 프로세스들끼리 메시지 주고 받을 수 있다.
- Devices : 컨테이너 내부 프로세스들은 호스트 시스템의 디바이스들을 직접 볼 수 없다.(option으로 권한 줄 수 있음)

## Docker 아키텍처
![](../images/docker-architecture.svg)
- Docker demon : 도커 이미지를 만들거나 저장하고, 이 이미지를 통해 컨테이너를 생성, 실행, 모니터링한다. 
- Docker client : HTTP 통신을 통해 Docker demon에게 명령을 내린다. 도커 커멘드는 Docker API를 사용하고 클라이언트는 하나 이상의 데몬과 통신할 수 있다.
- Docker registry : 이미지를 저장하고 공유한다. Docker Hub와 Docker Cloud는 누구나 사용할 수 있는 public registry이다. 

## Docker 기반 기술
- Namespace isolation : 리눅스 커널에서 implemented. 실행되는 프로세스가 사용하는 각각의 리소스를 보는 관점(different view of the system resources)을 제한한다. 분리된 네임스페이스를 사용하는 프로세스는 다른 프로세스의 리소스에 접근할 수 없다.  
 ** linux kernel : 시스템에 존재하는 자원을 효율적으로 관리하는 자원 관리자 (프로세서 관리, 프로세스 관리, 메모리 관리, 파일 시스템 관리, 네트워크 관리 등등)
 도커 컨테이너를 실행할 떄 도커는 해당 컨테이너의 네임스페이스 셋을 생성한다. 이 네임스페이스들은 isolation을 제공하고 각각의 컨테이너는 분리된 네임스페이스에서 구동되며 다른 컨테이너의 네임스페이스로의 접근이 제한된다.
 도커엔진은 linux에 있는 다음의 네임스페이스를 사용한다. 
  - pid namespace
  - net namespace
  - ipc namespace
  - mnt namespace : filesystem mount points
  - uts namespace : isolation kernel and version identifiers
- Cgroup : 네임스페이스는 각 프로세스가 바라보는 리소스의 관점을 제공할 뿐 직접적으로 리소스의 사용을 제한하진 않는다. 
Control-group(cgroup)은 커널의 부분으로 CPU, 메모리, disk I/O와 같은 리소스들을 제한한다. 각 컨테이너들에 가능한 메모리 자원을 제한한다.  

