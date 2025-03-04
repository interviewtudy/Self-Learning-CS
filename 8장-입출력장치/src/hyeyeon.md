# 장치 컨트롤러와 장치 드라이버

## 장치 컨트롤러

입출력 장치는 다루기 까다롭다

1. 입출력 장치에는 종류가 많음

- 입출력장치와 정보를 주고받는 방식을 규격화하기 어려움

2. 일반적으로 CPU와 메모리의 데이터 전송률은 높지만 입출력장치의 데이터 전송률은 낮다

- 전송률 : 데이터를 얼마나 빨리 교환할 수 있는지 나타냄
- 전송률에서 차이는 장치간의 통신을 어렵게 함
- 그래서 바로 컴퓨터에 직접 연결하지 않고 장치 컨트롤러 하드웨어를 통해 연결함
  - 장치 컨트롤러는 입출력 제어기, 입출력 모듈 등으로 다양하게 불림
  - 다음과 같은 문제를 해결함
    1. CPU와 입출력 장치 간의 통신 중개
    2. 오류 검출
    3. 데이터 버퍼링
    - 버퍼링이란 전송률이 높은 장치와 낮은 장치 사이에 주고 받는 데이터를 버퍼라는 임시 저장 공간에 저장하여 전송률을 비슷하게 맞추는 방법

### 내부

- 데이터 레지스터: CPU와 입출력 장치 사이에 주고받을 데이터가 담기는 레지스터, 버퍼 역할을 함
- 상태 레지스터: 입출력 장치가 입출력 작업을 할 준비가 되었는지, 입출력 작업이 완료되었는지, 입출력 장치에 오류는 없는 등의 상태 정보가 저장됨
- 제어 레지스터: 입출력 장치가 수행할 내용에 대한 제어 정보와 명령을 저장함

## 장치 드라이버

새로운 장치를 컴퓨터에 연결하려면 장치 드라이버를 설치해야한다

- 장치 컨트롤러의 동작을 감지하고 제어함으로써 장치 컨트롤러가 컴퓨터 내부와 정보를 주고받을 수 있게 하는 프로그램
- 프로그램이기에 당연히 실행 과정에서 메모리에 저장됨
- 장치 컨트롤러가 입출력장치를 연결하기 위한 하드웨어작인 통로라면, 장치 드라이버는 입출력장치를 연결하기 위한 소프트웨어적인 통로임

# 다양한 입출력 방법

## 프로그램 입출력

기본적으로 프로그램 속 명령어로 입출력 장치를 제어하는 방법
CPu가 프로그램 속 명령어를 실행하는 과정에서 입출력 명령어를 만나면 CPU는 입출력 장치에 연결된 장치 컨트롤러와 상호작용하며 입출력 작업을 수행한다
프로그램 입출력 방식에서의 입출력 작업은 CPU가 장치 컨트롤러의 레지스터 값을 읽고 씀으로써 이루어진다.

CPU는 입출력 장치들의 주소를 어떻게 아는 걸까?
CPU 내부에 있는 레지스터들과 달리 CPU는 여러 장치 컨트롤러 속 레지스터를 모두 알고 있기란 어려움

명령어들은 어떻게 명령어로 표현되고, 메모리에 어떻게 저장되어 있을까

### 메모리 맵 입출력

메모리에 접근하기 위한 주소 공간과 입출력장치에 접근하기 위한 주소 공간을 하나의 주소 공간으로 간주하는 방법

### 고립형 입출력

메모리를 위한 주소공간과 입출력 장치를 위한 주소공간을 분리하는 방법

## 인터럽트 기반 입출력

입출력 장치가 아닌 장치 컨트롤러에 의해 발생함, CPU는 장치 컨트롤러에 입툴력 작업을 명령하고, 장티 컨트롤러가 입출력 장티를 제어하면 입출력을 수행하는 동안 CPU는 다른 일을 할 수 있다
장치 컨트롤러가 입출력 작업을 끝낸 뒤 CPU에게 인터럽트 요청 신호를 보내면 CPU는 하던 일을 잠시 백업하고 인터럽트 서비스 루틴을 실행한다
이렇게 인터럽트 기반으로 하는 입출력을 입터럽트 기반 입출력이라고 한다

플래그 레지스터 속 인터럽트 비트가 활성화되어 있는 경우 혹은 인터럽트 비트를 활성화해도 무시할 수 없는 인터업트인 NMI가 발생한 경우 CPU는 이렇게 우선순위가 높은 인터럽트부터 처리합니다

## DMA 입출력

입출력 장치와 메모리 간의 데이터 이동은 CPU가 주도하고, 이동하는 데이터도 반드시, CPU를 거친다는 점
입출력 장치와 메모리가 CPU를 거치지 않고도 상호작용을 할 수 있는 입출력 방식인 DMA가 등장함 DMA는 이름 그대로 직접 메모리에 접근할 수 있는 입출력 기능임, DMA 입출력을 하기 위해서 시스템 버스에 연결된 DMA 컨트롤러라는 하드웨어가 필요하다
