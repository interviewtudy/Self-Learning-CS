# 프로세스 개요

## 프로세스 직접 확인하기

- 윈도우 - 작업 관리자의 프로세스 탭
- 유닉스 체계의 운영체제 - `ps` 명령어
- 포그라운드 프로세스: 사용자가 보는 앞에서 실행되는 프로세스
- 백그라운드 프로세스: 사용자가 보지 못하는 뒤에서 실행되는 프로세스
  - 사용자와 상호작용하지 않고 정해진 일만 수행하는 백그라운드 프로세스를 유닉스 체계의 운영체제에서는 데몬(daemon), 윈도우에서는 서비스(service) 라고 부름

<br/><br/>

## 프로세스 제어 블록

- 타이머 인터럽트(타임아웃 인터럽트): 클럭 신호를 발생시키는 장치에 의해 주기적으로 발생하는 하드웨어 인터럽트
- 모든 프로세스는 차례대로 돌아가며 한정된 시간만큼 CPU를 이용하고 시간이 끝났음을 알리는 타이머 인터럽트가 발생하면 차례를 양보하고 다음 차례를 기다림
- 프로세스 제어 블록(PCB, Process Control Block): 프로세스와 관련된 정보를 저장하는 자료 구조
  - 커널 영역에 생성
  - 프로세스 생성 시 만들어지고 실행이 끝나면 폐기됨

### PCB 정보

- 프로세스 ID(PID, Process ID): 프로세스를 식별하기 위한 고유 번호
  - 특정 프로세스를 식별하기 위해 부여하는 고유한 번호
  - 같은 일을 수행하는 프로그램이라도 두 번 실행하면 PID 가 다른 두 개의 프로세스가 생성됨
- 레지스터 값
  - 프로세스가 자신의 실행 차례가 돌아오면 이전까지 사용했던 레지스터의 중간값을 복원하기 위해 레지스터 값을 저장해둠
- 프로세스 상태
  - 입출력장치 사용 대기 상태인지, CPU 사용 대기 상태인지 등
- CPU 스케줄링 정보
  - 프로세스가 언제, 어떤 순서로 CPU 를 할당 받을지에 대한 정보
- 메모리 관련 정보
  - 프로세스가 어느 주소에 저장되어 있는지에 대한 정보
  - 베이스 레지스터, 한계 레지스터 값과 같은 정보가 담김
  - 페이지 테이블 정보 또한 PCB 에 들어감
- 사용한 파일과 입출력장치 목록
  - 어떤 입출력 장치가 이 프로세스에 할당 되었는지, 어떤 파일을 열었는지에 대한 정보

<br/><br/>

## 문맥 교환

- 문맥(context): 하나의 프로세스 수행을 재개하기 위해 기억해야 할 정보
- 문맥 교환(context switching): 기존 프로세스의 문맥을 PCB 에 백업하고 새로운 프로세스를 실행하기 위해 문맥을 PCB 로 복구하여 새로운 프로세스를 실행하는 것
- 문맥 교환이 자주 일어나면 프로세스가 그만큼 빨리 번갈아 가며 수행되기 때문에 프로세스들이 동시에 실행 되는 것처럼 보임

<br/><br/>

## 프로세스의 메모리 영역

하나의 프로세스는 사용자 영역에 크게 코드 영역, 데이터 영역, 힙 영역, 스택 영역으로 나뉘어 저장됨

### 프로세스의 사용자 영역

- 코드 영역(텍스트 영역): 실행할 수 있는 코드, 즉 기계어로 이루어진 명령어 저장
  - 쓰기가 불가능한 읽기 전용 공간
  - 정적 할당 영역
- 데이터 영역: 프로그램이 실행되는 동안 유지할 데이터가 저장되는 공간
  - ex. 전역 변수
  - 정적 할당 영역
- 힙 영역: 프로그래머가 직접 할당할 수 있는 공간
  - 힙 영역에 메모리 공간을 할당했다면 언젠간 해당 공간을 반환해야 함
  - 메모리 누수: 메모리 공간을 반환하지 메모리 낭비를 초래하는 것
  - 동적 할당 영역
  - 낮은 주소 -> 높은 주소로 할당
- 스택 영역: 데이터를 일시적으로 저장하는 공간
  - ex. 매개변수, 지역 변수
  - PUSH 를 통해 저장, POP 을 통해 제거
  - 동적 할당 영역
  - 높은 주소 -> 낮은 주소로 할당

<br/><br/><br/><br/>

# 프로세스 상태와 계층 구조

## 프로세스 상태

- 생성 상태(new): 프로세스를 생성 중인 상태. 막 메모리에 적재되어 PCB를 할당 받은 상태
- 준비 상태(ready): 실행할 준비가 완료되어 CPU 할당을 기다리는 상태
- 실행 상태(running): CPU를 할당받아 실행중인 상태. 할당 시간을 모두 사용하면 다시 준비 상태가 되고, 입출력장치의 작업이 끝날 때까지 기다려야 하면 대기 상태가 됨
- 대기 상태(blocked): 특정 이벤트의 발생을 기다리는 상태(대표적으로 입출력장치의 작업이 끝날 때) 기다리는 상태. 이벤트 발생 완료 시 다시 준비 상태가 됨
- 종료 상태(terminated): 프로세스가 종료된 상태. PCB와 메모리를 정리함

<br/><br/>

## 프로세스 계층 구조

프로세스가 자식 프로세스를 생성하는 등의 과정을 도표로 그려 나타낸 것

- 프로세스는 실행 도중 시스템 호출을 통해 다른 프로세스를 생성할 수 있음
- 부모 프로세스: 새 프로세스를 생성한 프로세스
- 자식 프로세스: 부모 프로세스에 의해 생성된 프로세스
- 부모 프로세스와 자식 프로세스도 다른 프로세스이므로 다른 PID 를 가짐
- 일부 운영체제에서는 자식 프로세스에서 부모 프로세스의 PID인 PPID 를 기록하기도 함
- 최초의 프로세스
  - 유닉스 운영체제: init
  - 리눅스 운영체제: systemed
  - macOS: launchd
- 최초의 프로세스의 PID 는 항상 1번임

<br/><br/>

## 프로세스 생성 기법

- fork: 부모 프로세스가 자신의 복사본을 자식 프로세스로 생성하는 시스템 콜
  - fork 로 생성된 자식 프로세스는 부모 프로세스의 자원(메모리 내용, 열린 파일 목록 등)을 상속 받음
  - 단 PID나 저장된 메모리 위치는 다름
- exec: 자식 프로세스가 자신의 메모리 공간을 다른 프로그램으로 교체하는 시스템 콜
  - 코드 영역과 데이터 영역의 내용이 실행할 프로그램 내용으로 바뀌고 나머지 영역은 초기화 됨

<br/><br/><br/><br/>

# 스레드

## 프로세스와 스레드

- 스레드 구성: 스레드ID, 프로그램 카운터 값을 비롯한 레지스터 값, 스택
- 스레드 마다 다른 레지스터 값, 스택을 갖기 때문에 스레드마다 다른 코드를 실행할 수 있음
- 프로세스의 스레드는 실행에 필요한 최소한의 정보만을 유지한 채 프로세스의 자원을 공유하며 실행됨
- 최근의 많은 운영체제는 CPU에 처리할 작업을 프로세스 단위가 아닌 스레드 단위로 전달하기도 함
- 많은 운영체제가 프로세스와 스레드를 구분하지만 리눅스 같이 명확하게 구분하지 않는 운영체제도 있음. 프로세스와 스레드 대신 테스크(task) 라는 이름으로 통일하여 명명하고 프로세스와 스레드 모두 실행의 문맥이라는 점에서 동등하게 간주함

## 멀티프로세스와 멀티스레드

- 멀티프로세스: 여러 프로세스를 동시에 실행하는 것
- 멀티스레드: 여러 스레드로 프로세스를 동시에 실행하는 것
- 스레드는 서로 자원을 공유하기 때문에 같은 프로그램에 대하여 여러 프로세스를 병행 실행하는 것보다 메모리를 효율적으로 사용할 수 있음
- 프로세스는 기본적으로 독립적으로 실행되지만 스레드는 자원을 공유하기 때문에 협력과 통신에 유리함
- 멀티 프로세스 환경에서는 하나의 프로세스에 문제가 생겨도 문제가 없지만 멀티 스레드 환경에서는 하나의 스레드에 문제가 생기면 프로세스 전체에 문제가 생길 수 있어 다른 스레드들이 영향을 받을 수 있음
- 프로세스 간 통신(IPC, Inter-Process Communication): 한 파일에 대해 작업을 하는 프로세스들이 있다면 이를 파일을 통한 프로세스 간 통신으로 볼 수 있음
- 공유 메모리: 프로세스들 사이 공유하는 메모리 영역을 두어 데이터를 주고 받을 수 있는데 이 영역을 가리킴
- 이외에도 프로세스들은 소켓, 파이프를 통해 통신할 수 있음
