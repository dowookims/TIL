## 목차
    1. 컴퓨터 시스템과 운영체제 개요 ( 1 ~ 2장)
        프로세스 중심으로 CS 전반적인 요소를 다루며 OS 개념과 역할, 기능 등을 정리

    2. 프로세스 관리 ( 3 ~ 6장)
        프로세스를 주제로 프로세스 상태와 변화 관련 기술과 스레드, 병행 프로세스, 교착 상태 등

    3. 메모리 관리 ( 7 ~ 8장)
        메모리 관리 전략, 메모리 할당 방법, 가상 메모리 개념과 요구페이징, 페이지 대치 알고리즘

    4. 장치 관리 및 파일 관리 ( 9 ~ 10장)
        입출력 관리와 주변장치 공간 할당, 디스크 스케줄링, 파일 관리 시스템 구성, 디렉터리, 보조기억장치

    5. 분산 및 보호 ( 11 ~ 12장 )
        분산 운영체제와 다중 처리, C/s, 클러스터, 보안과 보호, 신뢰 시스템

    6. UNIX OS ( 13장)
        unix 설계 목표

***
## 컴퓨터 시스템의 소개
    1. 컴퓨터 하드웨어의 구성
    2. 컴퓨터 시스템의 동작

    *   HW를 구성하는 프로세서, 메모리, 주변장치, 시스템 버스
    *   메모리의 역할과 계층 구조
    *   컴퓨터 시스템의 동작 원리
    *   프로그램의 기본 단위인 명령어의 구성과 실행 주기
    *   인터럽트 개념과 처리 과정
***
### 1. 컴퓨터 하드웨어의 구성
    Computer System : HW(데이터 처리하는 물리적 기계 장치) + SW(작업을 지시하는 명령어로 작성된 프로그램)

    OS : HW를 관리하는 소프트웨어

    HW는 크게 프로세서, 메모리(기억장치), 주변장치로 구성되고, 시스템 버스로 연결한다.


![ComputerHW](https://t1.daumcdn.net/cfile/tistory/99B94A505B552F3834)

***
### 1. 프로세서 : HW에 부착한 모든 장치의 동작을 제어, 명령, 실행. CPU
프로세서는 ( 연산장치 + 제어장치 + 레지스터 ) 내부 버스  
레지스터와 연산장치에서 데이터의 흐름이 양방향으로 발생하고, 제어 장치는 이를 
통제한다.

레지스터 :  

    프로세서에 위치한 고속 메모리 
    극히 소량의 데이터나 처리중인 중간 결과와도 같은 프로세서가 바로 사용할 수 있는 데이터를 담고 있는 영역
    컴퓨터 구조에 따라 크기와 종류가 다양하다

    용도 => 전용레지스터 | 범용 레지스터  
    변경가능 => 사용자 가시 레지스터 | 사용자 불가시 레지스터  
    저장 정보의 종류 => 데이터 레지스터 | 주소 레지스터 | 상태 레지스터

    사용자 가시 레지스터

    데이터 레지스터(DR, Data Register) : 함수 연산에 필요한 데이터값 저장. 연산 결과로 플래그 값을 저장.

    주소 레지스터(AR, Address Register) : 주소나 유효 주소를 계산하는 데 필요한 주소의 일부분 저장.

    *   기준 주소 레지스터 : 프로그램 실행할 때 사용하는 기준 주소 값 저장
    *   인덱스 주소 레지스터 : 유효 주소를 계산하는 데 사용하는 주소 정보 저장
    *   스택 포인터 레지스터 : 메모리에 프로세서 스택 구현시 사용. 
        프로세서와 레지스터를 스택과 큐로 사용한다. 보통 반환주소, 프로세서 상태 정보, 서브루틴의 임시 변수 저장.

    사용자 불가시 레지스터
    *   프로그램 카운터(PC) : 다음에 실행할 명령어의 주소를 보관하는 레지스터
    *   명령어 레지스터(IR, Instruction Register) : 현재 실행하는 명령어를 보관하는 레지스터
    *   누산기(ACCumulator) : 데이터를 일시적으로 저장하는 레지스터
    *   메모리 주소 레지스터(MAR, Memory Address Register) : 프로세서가 참조하려는 데이터의 주소를 명시하여
        메모리에 접근하는 버퍼 레지스터
    *   메모리 버퍼 레지스터(MBR, Memory Buffer Register) : 프로세서가 메모리에서 읽거나 저장할 데이터 자체
        를 보관하는 버퍼레지스터. 메모리 데이터 레지스터(MDR)
***
### 2. 메모리
![Memory Hierarchy Structure](https://t1.daumcdn.net/cfile/tistory/23721C4A590C5A7425)

메모리 계층 구조는 프로그램 실행 또는 참조시 모두 메인 메모리에 올리면 비효율적인데 이를 해결하기 위해 나온 방법.

불필요한 프로그램이나 데이터는 보조기억장치에 저장했다가 실행, 참조할 때만 메인 메모리로 옮기는 방법. 비용, 속도, 크기(용량)가 다른 메모리를 효과적으로 사용함으로써 시스템의 성능 향상.

    메인 메모리 : 프로세서 외부에 있으며, 프로세서에서 즉각적으로 수행할 프로그램과 데이터를 저장하거나 프로세
            서에서 처리한 결과를 메인 메모리에 저장한다. I/O Devices도 메인 메모리에서 데이터 받거나 저장.
            주기억장치 또는 1차 기억장치. DRAM을 많이 사용.
            다수의 셀(Cell)로 구성되었으며, 셀은 주소로 참조한다.

    물리적 주소 : 컴퓨터에 주어진 주소
    논리적 주소 : 컴파일러가 프로그램을 기계 명령어로 변환할 때 변수와 명령어에 주소를 할당
    매핑(메모리 맵) : 컴파일로 논리적 주소를 물리적 주소로 변환

    메모리 속도 : 메모리 접근시간과 메모리 사이클 시간으로 표현

    메모리 접근 시간 : 명령이 발생한 후 목표 주소를 검색하여 데이터 쓰기(읽기)를 시작할 때 까지 걸린 시간
    ex) 읽기 제어 신호를 가한후 데이터를 메모리 버퍼 레지스터리에 저장할 때까지 걸린 시간

    메모리 사이클 시간 : 두 번의 연속적인 메모리 동작 사이에 필요한 최소 지연 시간
    ex) 읽기 제어 신호를 가한 후 다음 읽기 제어 신호를 가할 수 있을 때 까지 필요한 시간

    메인메모리는 프로세서와 보조기억장치 사이에 있으며, 발생하는 디스크 입출력 병목 현상을 해결하는 역할도 한다.
    프로세서와 메인 메모리 간 속도 차이가 나면서 메인 메모리의 부담을 줄이려고 프로세서 내부나 외부에 캐시를 구현하기도 한다.
***
### 3. 캐시
프로세서 내부나 외부에 있으며 처리 속도가 빠른 프로세서와 상대적으로 느린 메모리의 속도 차이를 보완하는 고속 버퍼

    버퍼 : 한 곳에서 다른 곳으로 데이터를 이동할 때 임시적으로 그 데이터를 저장하기 위해 사용되는 물리적인 메모리 저장소의 영역
***
    메인 메모리에서 데이터를 블록 단위로 가져와 프로세서에 워드 단위로 전달하여 속도를 높인다.  
    데이터가 이동하는 통로(대역폭)를 확대하여 프로세서와 메모리 속도 차이를 줄인다.
    캐시는 주소 영역을 한 번 읽어 들일 수 있는 크기로 나눈 후 각 블록에 번호를 부여하여 이 번호를 태그로 저장

![캐시의 기본 동작](https://t1.daumcdn.net/cfile/tistory/276414365645CAF704)
    
    프로세서는 메모리에 접근하기 전에 캐시에 해당 주소의 자료가 있는지 먼저 확인

    캐시의 성능은 작은 용량의 캐시에 프로세서가 이후 참조할 정보가 얼마나 들어 있느냐로 좌우된다.

    캐시 적중 : 프로세서가 참조하려는 정보가 있을 때
    캐시 실패(cache miss) : 없을 때

    블록의 크기는 캐시의 성능으로 좌우되는데 참조한 메모리에 대한 공간적 지역성(국부성)과 시간적 지역성(국부성)이 있기 때문에.

    공간적 지역성(spatial locality) : 프로그램이 참조한 주소와 인접한 주소의 내용을 다시 참조하는 특성
    시간적 지역성(temporal locality) : 한번 참조한 주소를 곧 다시 참조

    이는
        프로그램이 명령어를 순차적으로 실행하는 경향에 의해 명령어가 특정 지역 메모리에 인접해 있고
        순환 때문에 프로그램을 반복하더라도 메모리는 일부 영역만 참조하기 때문에 발생하게 된다.

    지역성은 블록이 크면 cache hit rate이 올라갈 수 있지만, 블록이 커지면 이에 따른 전송 부담과 cache data 교체 작업이 자주 일어나므로 블록 크기를 과하게 늘릴수 없다.

보조장치 : 주변장치 중 프로그램과 데이터를 저장하는 하드웨어, 2차 기억장치 또는 외부기억장치.
ex) 자기디스크, 광디스크, 자기테이프

### 3. 시스템 버스(System Bus) : HW를 물리적으로 연결하여 DATA를 주고받을 수 있게 하는 통로
컴퓨터 내부의 다양한 신호(I/O, Process State, Interupt, clock)을 시스템 버스로 전달한다.
시스템 버스는 기능에 따라 Data bus, Address Bus, Control bus로 구분된다.

![시스템버스](https://upload.wikimedia.org/wikipedia/commons/thumb/6/68/Computer_system_bus.svg/350px-Computer_system_bus.svg.png)

    데이터버스 : Processor와 메모리, 주변 장치 사이에서 데이터 전송.
                데이터 버스를 구성하는 배선 수는 프로세서가 한 번에 전송할 수 있는 비트 수를 결정하는데,
                이를 워드라고 함
    주소 버스 : 프로세서가 시스템 구성 요소를 식별하는 주소 정보를 전송. 주소 버스를 구성하는 배선 수는 
                프로세서와 접속할 수 있는 메인 메모리의 최대 용량 결정
    제어 버스 : 프로세서가 시스템의 구성 요소를 제어하는데 사용. 연산 종류와 메모리 읽기 쓰기 동작을 결정

***
### 4. 주변 장치 : 프로세서와 메인 메모리를 제외한 나머지 하드웨어 구성 요소
    입력장치
    출력장치
    저장장치