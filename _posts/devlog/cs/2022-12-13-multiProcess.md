---
layout: post
title: Multi Process & Thread 정리
image:
subtitle: Multi Process & Thread의 차이를 알아보고 정리하자
category: devlog
tags: cs
accent_image: 
  background: url('/assets/img/web_dev/webdev_sidebar.jpeg') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  Multi Process & Thread를 정리하고 차이를 상세히 알아보자
invert_sidebar: true
sitemap: false
comments: true
---

# Multi Process & Thread
CS 지식에서 제일 많이 물어보는 부분이 Multi Process와 Thread가 아닐까 한다.

학부생 시절부터 정리했던 부분이지만 다시 한 번 더 정리하고 정의하고자 한다.

* toc
{:toc .large-only}

## Process 란?
> Process란 하나 혹은 그 이상의 Thread로 실행되는 컴퓨터 프로그램의 instance이다. <br />
> Process는 Program Code와 Activity를 포함한다. <br />
> - Wikipedia

> 프로세스는 가상 메모리 공간, 코드, 데이터, 시스템 자원의 집합이다. <br />
> - Microsoft

프로세스는 운영체제가 프로그램을 실행하기 위해 필요한 가장 작은 단위의 쓰레드, 메모리, 소스코드들의 집합이며 프로그램 동작 그 자체를 의미한다. <br />
운영체제는 프로세스를 작업의 단위로 보고 자원들을 프로세스들에 적절하게 배분한다. <br />

쉽게 말하자면 프로세스란 실행중인 프로그램이라고 볼 수도 있다.

### Process의 상태 (States)
![ProcessStates](https://iingang.github.io/assets/img/posts/process_state.png)
[출처](https://iingang.github.io/posts/OS-process/)

위 그림을 프로세스의 5 가지 상태(Five States)라고 한다. <br />
9 가지 상태로 상세히 표현하는 방식도 있지만 이 글에서는 5 가지 상태를 다룰 것이다. <br />

 - New : 프로세스가 생성 되었지만, 운영체제에 의해 수행 가능한 프로세스 풀로의 진입이 허용되지 않은 상태
 - Ready : 자원이 할당되면 수행할 준비가 되어 있는 상태
 - Running : 현재 수행 중인 프로세스
 - Blocked : 입출력 연산(I/O)와 같은 작업 중 완료가 될 때까지 수행될 수 없는 프로세스
 - Exit(Terminated) : 프로세스 수행이 중지되거나, 어떤 이유로 중단되었기 때문에 프로세스 풀에서 방출된 프로세스, 종료된 상태

#### Process 상태 변화 요인
##### New -> Ready(Admitted)
New 상태에서 OS의 승인을 받아서 프로세스가 생성이 되면, 해당 프로세스의 PCB가 OS커넗의 Ready Queue에 올라온다. <br />
##### Ready -> Running(Scheduler dispatch)
Ready Queue에 있는 프로세스들 중에서 스케줄링 알고리즘에 의해서 선택된 프로세스가 CPU를 할당 받는다. <br />
##### Running -> Blocked(Wait)
현재 CPU의 명령을 받아서 명령어를 수행중인 프로세스가 I/O와 같은 특정 작업을 해야하는 경우 CPU를 반납하고 해당 장치 큐에 들어가게 된다. <br />
##### Blocked -> Ready(I/O Completion or interrupt)
작업을 위해 장치 큐에 있던 프로세스가 디스크 컨트롤러에 의해 서비스를 받아 일을 하고, <br />
디스크 컨트롤러가 인터럽트를 발생하여 프로세스가 한 일을(로컬 버퍼에 저장된 데이터) 메모리에 올려놓고 프로세스는 다시 Ready Queue에 들어가게 된다. <br />
##### Running -> Exit
프로세스 실행이 완료되어 자원을 반납한 상태 <br />

### Process 메모리
운영체제는 프로세스 마다 고유의 가상 메모리 공간을 제공한다. 이러한 메모리 공간은 Data, Text, Stack/Heap Section으로 나뉜다. <br />

![ProcessMemory](https://images.velog.io/images/curiosity806/post/200822c8-b98a-4c78-affa-999ae1d34e26/image.png)
[출처](https://velog.io/@curiosity806/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EB%9E%80)

 - Stack : 
   - 매개변수, 지역변수, return 주소 등과 같은 데이터를 저장하는 영역이다.
   - 컴파일러에 의해 Run Time 도중 크기가 결정되며, 함수가 호출&종료 되는 시점에서 생성&제거 된다.
 - Heap : 
   - new, delete, malloc, free 등을 호출하여 데이터를 저장, 관리하는 영역
   - Run Time에 크기가 결정
 - Data Section : 
   - 사전에 선언된 데이터가 저장되는 영역이다. (global, staic, variables, etc)
   - Compile Time에 크기가 결정된다.
   - 내부에서 DATA & BBS 영역으로 구분된다.
     - DATA : 초기화된 전역변수가 저장되는 영역이다. (Initialized data section)
     - BBS : 초기화되지 않은 전역변수가 저장되는 영역이다. (Uninitialized data section)
 - Instruction(Text Section) : 
   - 컴파일된 기계어가 저장되는 영역이다.
   - Compile Time에 크기가 결정된다.

### PCB(Process Controll Block)
Process 마다 현재 상태를 하나의 데이터 구조에 저장하여 관리하는데, 이를 Process Controll Block이라고 한다. <br />
PCB는 다른 프로세스들이 쉽게 접근할 수 없고, Kernel 영역에 저장된다. <br />

운영체제가 프로세스를 관리하기 위해 필요한 정보를 담고 있는 자료구조이며, <br />
주요 역할은 수행 프로세스를 인터럽트한 후 나중에 그 인터럽트가 발생되지 않은 것처럼 프로세스 수행을 재개하도록 충분한 정보를 유지하는 것이다. <br />

PCB는 프로세스 식별자, 프로세서(CPU) 상태 정보, 프로세스 제어 정보를 담고 있다. <br />
![PCB](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FtIPDr%2FbtqUnKRlmuB%2FDJIs4kFAwQE5ySaiJz25Kk%2Fimg.png)
[출처](https://yoongrammer.tistory.com/52)

![PCB](https://velog.velcdn.com/images/curiosity806/post/06d141f2-ff5a-4855-a064-b90f218bc0e2/image.png)

 - 포인터 : 프로세스의 현재 위치를 저장하는 포인터 정보
 - 프로세스 상태 : 프로세스의 각 상태를 저장
 - 프로세스 번호 : 모든 프로세스에는 프로세스 식별자를 저장하는 프로세스 ID 또는 PID라는 고유한 ID가 할당
 - 프로그램 카운터 : 프로세스를 위해 실행될 다음 명령어의 주소를 포함하는 카운터를 저장
 - 레지스터 : 누산기, 베이스, 레지스터 및 범용레지스터를 포함하는 CPU 레지스터에 있는 정보
 - 메모리 제한 : 운영체제에서 사용하는 메모리 관리 시스템에 대한 정보
   - 페이지 테이블 : 페이징 프로세스의 메모리 주소를 관리할 때 프로세스의 페이지 정보를 저장하고 있는 테이블
   - 세그먼트 테이블 : 프로세스를 논리적으로 잘라 메모리에 배치하는 방식을 세그멘테이션이라고 한다. 세그먼트 테이블은 이 세그먼트들의 실제 물리적 메모리 주소의 정보를 담고 있다.
 - 열린 파일 목록 : 프로세스를 위해 열린 파일 목록
 - Accountin 정보 : Process를 실행한 유저 정보
 - I/O 상태 정보 : Process에 할당된 물리적 장치 및 프로세스가 읽고 있는 파일에 관한 정보

### Process 생성
프로세스 생성은 부모 프로세스가 연산을 통해 자식 프로세스를 만들어낸다. 생성된 자식 프로세스 또한 새로운 자식 프로세스를 만들 수 있으며, 이를 구별하기 위해 모든 프로세스는 각자 고유의 PID를 가지고 있다. <br />
이렇게 생성된 프로세스 간의 관계는 하나의 큰 트리구조가 된다. <br />

생성된 자식 프로세스는 각자 고유의 PID, 메모리, CPU 등 새 PCB가 할당되며 고유의 자원을 획득하게 된다. <br />
이로 인하여 부모 프로세스의 자원 접근에 제한이 생기며 특수한 방법을 통해 공유할 수 있게 된다. <br />

프로세스를 생성한 후 부모 프로세스는 다음과 같이 2가지 행동을 할 수 있다.

> 자식 프로세스가 끝날 때까지 기다린다 ( -> waiting queue ) <br />
> 자식 프로세스와 함께 동작 (멀티 프로세싱 환경) <br />


> 자식 프로세스는 다음 중 하나의 프로세스가 된다. <br />
> - 부모 프로세스와 동일한 새로운 프로세스 : 이 경우 부모 프로세스의 프로그램, 데이터가 완전 복사
> - 새로운 프로그램 실행 : 새로운 프로그램을 메모리에 load 하고 이를 실행

#### fork()
Linux/UNIX 환경에서 새로운 프로세스를 만드는 시스템 콜 함수

생성된 자식 프로세스는 부모 프로세스의 데이터와 프로그램이 완전 복사가 되어 똑같은 프로그램을 수행하는 프로세스가 된다. <br />
멀티 프로세싱을 통해 부모, 자식 프로세스는 함께 동작한다. <br />
fork() 함수는 부모 프로세스에서 자식의 PID를 반환하고, 자식 프로세스에서는 0을 반환하여 구분할 수 있도록 해준다. <br />
![fork](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FezlnDL%2FbtqZP26N311%2FMSSflKfXCsHUtY8GNID4P0%2Fimg.png)

#### exec()
Linux/UNIX 환경에서 프로세스를 새로운 프로그램을 실행하는 프로세스로 대체하는 시스템 콜 함수

fork()와 다르게 자식 자식 프로세스를 생성하는 것이 아닌 현재 프로세스의 프로그램 코드를 새로운 프로그램 코드로 바꿔준다. <br />
이로 인하여 프로그램 코드, 메모리, 파일 등 프로세스 자원이 새로 바뀌게 된다. <br />
exec() 함수는 현재 프로세스가 완전히 새로운 프로그램을 실행하는 프로세스로 대체되므로 반환 값이 없다. <br />
![exec](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Ft9CUW%2FbtqZP2scV43%2F7EkUh5kfj4ATB64Aknm451%2Fimg.png)

보통 동작하는 방식은 fork()를 통해 자식 프로세스를 생성하고 자식 프로세스에서 exec()를 통해 새로운 프로그램을 돌리게 된다. <br />
이때 부로 프로세스가 자식 프로세스가 끝나기를 기다려야 한다면 wait() 시스템 콜 함수를 이용하여 기다릴 수 있다. <br />

### Multi Process
두개 이상, 다수의 프로세서(CPU)가 협력적으로 하나 이상의 작업(Task)을 동시에 처리하는 것이다. (병렬처리) <br />
**각 프로세스 간 메모리 구분이 필요하거나 독립된 주소 공간을 가져야 할 경우** 사용한다. <br />

#### 장점
 - 독립된 구조로 안전성이 높은 장점이 있다.
 - 프로세스 중 하나에 문제가 생겨도 다른 프로세스에 영향을 주지 않아, 작업속도가 느려지는 손해정도는 생기지만 정지되거나 하는 문제는 발생하지 않는다.
 - 여러개의 프로세스가 처리되어야 할 때 동일한 데이터를 사용하고, 이러한 데이터를 하나의 디스크에 두고 모든 프로세서(CPU)가 이를 공유하면 비용적으로 저렴하다.

#### 문제점
 - 독립된 메모리 영역이기 때문에 작업량이 많을수록( Context Switching이 자주 일어나서 주소 공간의 공유가 잦을 경우) 오버헤드가 발생하여 성능저하가 발생 할 수 있다.
 - Context Switching 과정에서 캐시 메모리 초기화 등 무거운 작업이 진행되고 시간이 소모되는 등 오버헤드가 발생한다.

#### Context Switching
CPU는 한번에 하나의 프로세스만 실행 가능하다. <br />
때문에 CPU에서 여러 프로세스를 돌아가면서 작업을 처리하는 데 이 과정을 Context Switching라 한다. <br />
구체적으로, 동작 중인 프로세스가 대기를 하면서 해당 프로세스의 상태(Context)를 보관하고, 대기하고 있던 다음 순서의 프로세스가 동작하면서 이전에 보관했던 프로세스의 상태를 복구하는 작업을 말한다. <br />


### Process간 통신 방식 (IPC : Inter Process Communication)
프로세스 간 통신이란 프로세스가 서로 데이터를 주고받는 방법, 경로 등을 의미한다. <br />
커널의 디자인에 따라 마이크로 커널, 나노 커널 등 통신이 많이 일어나는 디자인의 경우 IPC 방식이 성능을 크게 좌지우지할 수 있다. <br />

#### Shared memory
운영체제의 도움을 받아 일부 영역의 메모리를 여러 프로세스가 동시에 접근할 수 있도록 권한을 받는다. <br />
프로세스는 공유 메모리를 읽고 쓰면서 프로세스 간 통신을 하게 된다. <br />
같은 메모리를 사용하는 환경에서 작동하므로 메모리에 접근하여 값을 변경하면 그 즉시 변경된 값이 반영되어 다른 프로세스들이 접근 시 변경된 값을 얻을 수 있다. <br />
처음 메모리에 여러 프로세스가 접근 권한을 부여하는 작업에서만 커널 작업이 필요하고 이후엔 커널 동작이 필요 없다. <br />

##### 동작 과정
공유 메모리를 사용할 프로세스들 중 하나의 프로세스가 자신이 부여받은 메모리 영역 중 일부분을 선택 <br />
다른 프로세스 들은 공유할 메모리의 주소를 받아 이를 자신의 메모리 영역에 붙인다. <br />
운영체제가 해당 프로세스들 간의 메모리 접근 제한을 풀어준다. <br />
이후 프로세스들은 공유 메모리를 통해 통신 <br />
* 운영체제는 더 이상 관여할 필요 없으며 프로세스가 종료 시 메모리를 반환하면 공유 메모리 또한 같이 반환하게 된다. <br />

#### Message Passing
데이터가 하나의 메시지가 되어 프로세스 간 통신을 하게 된다.  <br />
충돌 위험이 없기 때문에 소량의 데이터 전송에 유리  <br />
명령만을 주고받는 분산 시스템, 마이크로/나노 커널 방식에 유리  <br />
공유 메모리 방식과 다르게 같은 환경에 있지 않아도 인터넷을 통해서 메시지를 주고받을 수 있다. (소켓)  <br />
생산자-소비자 모델의 공유 메모리 방식을 이용하면 동기화 작업이 필요 없지만 메시지 전달 방식은 동기화 작업이 필요한 경우가 많습니다. 비동기, 동기 방식에 따라 결정한다.  <br />

##### Send, Receive 2가지 명령에 의해 동작
 - Direct communication : send, receive 동작이 특정 프로세스를 지정하며 메시지를 직접 주고 받는다.
 - Indirect communication : 프로세스들 가운데 하나의 시스템, 네트워크 등을 두고 메시지를 중재하며 동작한다. 직접 전달 방식보다 더 유연하게 동작할 수 있으며 중재 시스템의 구현에 따라 보안성, 성능 등 다양한 처리가 가능해진다.
 
##### 동기, 비동기
메시지 전달 방식은 동기화 작업이 필요한데, 동기방식과 비동기 방식이 있다. <br />

> 동기(Blocking, Synchronous) <br />
> 동기 Send : 메시지를 보내고 나면 받는 프로세스에게 메시지가 도착할 때까지 다른 작업을 할 수 없다. <br />
> 동기 Receive : 메시지를 받기 전까지 해당 프로세스는 다른 작업을 하지 않고 멈춰 있어야 한다. <br />


> 비동기(Nonblocking, Asynchronous) <br />
> 비동기 Send : 메시지를 보내고 프로세스는 멈추지 않고 다음 작업을 진행한다. <br />
> 비동기 Receive : 메시지를 받으면 해당 메시지가 유효한 내용인지 검사를 하고 처리 <br />

##### 버퍼 형태
 - Unbounded buffer : 메모리 버퍼의 크기에 제한이 없으며 소비자 프로세스는 새 자원이 만들어질 때까지 기다려야 하며 생산자 프로세스는 항상 새로운 자원을 만들 수 있다. 이때 생산자 역할의 프로세스는 공유한 메모리 자원의 크기를 지정하여 이를 함께 소비자 프로세스에 알려주어야 한다. 
 - Bounded buffer : 메모리 버퍼 크기에 제한이 있으며 소비자 프로세스는 버퍼가 빈 상황이면 새로운 자원이 채워질 때까지 기다리며 생산자 프로세스는 버퍼가 꽉 차 있으면 공간이 남을 때까지 기다려야 한다.

#### File 사용
텍스트 파일txt(혹은 다른 포멧의 파일)을 통해 데이터를 주고 받는 것도 IPC 기법 중 하나이다. <br />

물론 이 방식은 문제가 많다. 이 방식은 실시간으로 직접 원하는 프로세스에 데이터 전달하는게 어렵기 때문이다. <br />
디스크에서 데이터 파일을 읽고, 프로세스에 적재load되는 과정에서 컨텍스트 스위칭Context-switching, 인터럽트Interrupt 등 여러 일을 처리해야 하기 때문이다. <br />

#### 파이프(Pipe)
단방향 통신, 즉 부모 프로세스 → 자식 프로세스에게 일방적으로 통신하는 기법으로, fork()를 통해 자식 프로세스를 만들고 나서 부모의 데이터를 자식에게 보낸다.
![pipe](https://velog.velcdn.com/images%2Fredgem92%2Fpost%2F3cadf7e9-cf1c-4800-bc5d-03d325e03654%2Fimage.png)

#### 소켓(Socket)
많이 쓰이는 기법이자, 본래 목적이 IPC로 활용하기 위한 것은 아니지만, 충분히 활용이 가능하다. <br />

본래 소켓은 네트워크 통신을 위한 기술이다. 기본적으로 클라이언트와 서버 등 두 개의 다른 컴퓨터 간의 네트워크 기반 통신을 위한 기술로, 네트워크 디바이스를 사용할 수 있는 시스템 콜이기도 하다. <br />
소켓은 이렇게 클라이언트와 서버 뿐만 아니라, 하나의 컴퓨터 안에서 두 개의 프로세스 간의 통신 기법으로 활용하는 경우도 더러 있다. <br />

![ipcsocket](https://velog.velcdn.com/images%2Fredgem92%2Fpost%2F93b6e638-147a-4a25-b0b2-4fe01b1aef04%2Fimage.png)

소켓을 사용하면 로컬 컴퓨터간의 통신 시 이렇게 계층을 타고 내려가면서 송신을 하고, 아래 계층부터 위로 올라가서 대상 프로세스가 수신을 하는 방식을 취하게 된다. <br />

---

## Thread 란?
사실 프로세스가 단일의 실행 쓰레드를 실행하는 프로그램이다. 진짜로 일하는 것은 쓰레드라고 보면 된다.


### Multi Thread
하나의 프로세스에 여러 스레드로 자원을 공유하며 작업을 나누어 수행하는 것이다. <br />
![multiThread](https://user-images.githubusercontent.com/58318041/91327464-82c7d600-e800-11ea-818e-faf42b7ff162.png)

#### 장점
 - 시스템 자원소모 감소 (자원의 효율성 증대)
 - 프로세스를 생성하여 자원을 할당하는 시스템 콜이 줄어 자원을 효율적으로 관리할 수 있다.
 - 시스템 처리율 향상 (처리비용 감소)
 - 스레드 간 데이터를 주고 받는 것이 간단해지고 시스템 자원 소모가 줄어든다.
 - 스레드 사이 작업량이 작아 Context Switching이 빠르다. (캐시 메모리를 비울 필요가 없다.)
 - 간단한 통신 방법으로 프로그램 응답시간 단축
 - 스레드는 프로세스 내 스택영역을 제외한 메모리 영역을 공유하기에 통신 비용이 적다.
 - 힙 영역을 공유하므로 데이터를 주고 받을 수 있다.

#### 문제점
 - 자원을 공유하기에 동기화 문제가 발생할 수 있다. (병목현상, 데드락 등)
 - 주의 깊은 설계가 필요하고 디버깅이 어렵다. (불필요 부분까지 동기화하면, 대기시간으로 인해 성능저하 발생)
 - 하나의 스레드에 문제가 생기면 전체 프로세스가 영향을 받는다.
 - 단일 프로세스 시스템의 경우 효과를 기대하기 어렵다.

---

## Process와 Thread의 차이
 - 프로세스는 현재 실행되고 있는 프로그램으로 메모리에 올라와서 독립적인 메모리 공간을 가진다.
 - 쓰레드는 프로세스 내에서 실행되는 흐름으로 프로세스 자원을 공유한다.

### 멀티 스레드 vs 멀티 프로세스
 - 멀티 스레드는 멀티 프로세스보다 적은 메모리 공간을 차지하고 Context Switching이 빠른 장점이 있지만, 동기화 문제와 하나의 스레드 장애로 전체 스레드가 종료 될 위험을 갖고 있다.
 - 멀티 프로세스는 하나의 프로세스가 죽더라도 다른 프로세스에 영향을 주지 않아 안정성이 높지만, 멀티 스레드보다 많은 메모리공간과 CPU 시간을 차지하는 단점이 있다.
 - 두 방법은 동시에 여러 작업을 수행하는 점에서 동일하지만, 각각의 장단이 있으므로 적용하는 시스템에 따라 적합한 동작 방식을 선택하고 적용해야 한다.

### 왜 멀티 스레드로 나눠가며 할까?
 - 운영체제가 시스템 자원을 효율적으로 관리하기 위해 스레드를 사용한다.
 - 멀티 프로세스로 실행되는 작업을 멀티 스레드로 실행할 경우, 프로세스를 생성하여 자원을 할당하는 시스템 콜이 줄어들어 자원을 효율적으로 관리할 수 있다.
 - 또한, 프로세스 간의 통신보다 스레드 간의 통신 비용이 적으므로 작업들 간 통신의 부담이 줄어든다. (처리비용 감소. 프로세스는 독립구조이기 때문)

#### 그렇다면 무조건 멀티 스레드가 좋은가?
스레드를 활용하면 자원의 효율성이 증가하기도 하지만, 스레드 간의 자원 공유는 전역 변수를 이용하므로 동기화 문제가 발생 할 수 있으므로 프로그래머의 주의가 필요하다. <br />

---

## 좀비 & 고아 프로세스
### 좀비 프로세스
자식 프로세스가 부모 프로세스보다 먼저 죽는 경우 부모 프로세스가 종료 상태를 회수하기 위해 커널이 자식 프로세스의 최소한의 정보(PID, 종료 상태 등, 리눅스의 경우 커널에서 사용하는 구조체)를 남겨 둔다. <br />
부모 프로세스는 wait 함수를 호출하여 이 상태를 회수하면 남은 모든 정보가 제거되어 자식 프로세스는 완전히 소멸하게 된다. <br />

위와 같은 진행상황에서 부모 프로세스가 wait 함수를 호출하지 않아 최소한의 정보가 메모리에 남아 있는 경우를 좀비 프로세스라고 한다. <br />
좀비 프로세스는 최소한의 정보만을 가지고 있어 큰 성능 저하를 야기하지 않지만, 운영체제는 한정된 PID를 가지고 있으므로 좀비 프로세스가 PID를 차지하며 다른 프로세스 실행을 방해하게 된다. <br />
따라서 부모 프로세스는 좀비 프로세스 생성을 방지하기 위해 wait 함수를 호출하여 상태를 회수하여야 한다. <br />

> 커널 입장에서 좀비 프로세스는 성능 저하를 일으킨다고 볼 수 있다. <br />
> 프로세스 스케줄링에 있어서 queue에 걸려있는 프로세스의 양이 증가하고 커널 구조체를 유지하기 위한 비용 또한 무시할 수 없다. <br />

좀비 프로세스를 다음과 같은 방법으로 관리할 수 있다. <br />
 - wait : 간단하게 부모 프로세스에서 wait 함수를 호출하여 좀비 프로세스를 없앨 수 있다.
 - signal, wait : wait 함수는 블록 모드로 동작하는 함수이므로 부모 프로세스는 wait함수를 호출한 즉시 동작을 멈추게 된다. 이를 방지하기 위해 시그널 도구를 활용하여 자식 프로세스가 종료될 경우 발생하는 SIGCHLD 시그널에 해당하는 핸들러를 만들고 해당 핸들러에서 wait 함수를 호출하면 된다.


### 고아 프로세스(Orphan)
부모 프로세스가 자식 프로세스보다 먼저 종료되는 경우 부모 프로세스가 없는 자식 프로세스를 말한다. <br />
운영체제는 이러한 고아 프로세스를 허용하지 않으며 부모 프로세스가 먼저 종료되면 자식 프로세스의 새로운 부모 프로세스로 init(PID = 1)가 설정된다. <br />

init 프로세스는 자식 프로세스가 종료될 때까지 기다린 후 wait 함수를 호출하여 고아 프로세스의 종료 상태를 회수하여 좀비 프로세스가 되는 것을 방지한다. <br />
고아 프로세스는 프로세스 자신이 시스템의 자원을 낭비할 수 있고, 시스템이 프로세스가 종료될 때까지 추적을 해야 하기 때문에 성능 저하의 원인이 된다. <br />

고아 프로세스는 init 프로세스가 관리를 해 주지만 성능 저하를 방지하기 위해 부모 프로세스가 종료되기 전에 모든 자식 프로세스를 wait 해 주는 것이 좋다. <br />