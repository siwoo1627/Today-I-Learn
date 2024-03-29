### 제1장 인프라 아키텍처를 살펴보자

***

#### 집약형 아키텍처

: 하나의 컴퓨터로 모든 처리

* 장점: 구성이 간단, 리소스 관리나 이중화에 의해 안정성이 높고 고성능
* 단점: 도입 비용 및 유지 비용이 큰 경향이 있음, 확장성의 한계

#### 분할형 아키텍처

: 여러 대의 컴퓨터를 조합해서 하나의 시스템을 구축하는 구조(오픈 시스템, 분산 시스템)

* 장점: 낮은 비용으로 시스템 구축 가능, 서버 대수를 늘릴 수 있어 확장성이 높음
* 단점: 대수가 늘어나면 관리 구조가 복잡해짐, 한 대가 망가지면 영향 범위를 최소화하기 위한 구조를 검토해야함

#### 수직 분할형 아키텍처

: 서버 별로 다른 역할을 함

##### 클라이언트-서버 아키텍처(C/S)

* 장점: 클라이언트 측에서 많은 처리를 실행할 수 있어서 소수의 서버로 다수의 클라이언트를 처리할 수 있음
* 단점: 클라이언트 측의 소프트웨어 정기 업데이트가 필요, 서버 확장성에 한계가 발생할 수 있음

##### 3계층형 아키텍처

#### 수평 분할형 아키텍처

: 클라이언트-서버형의 발전형태, 프레젠테이션 계층-애플리케이션계층-데이터 계층의 3층 구조

`프레젠테이션 계층`: 사용자 입력을 받음, 웹 브라우저 화면을 표시

`애플리케이션 계층`: 사용자 요청(Request)에 따라 업무를 처리

`데이터 계층`: 애플리케이션 계층의 요청에 따라 데이터 입출력을 함

* 장점: 서버 부하 집중 개선, 클라이언트 단말의 정기 업데이트가 불필요, 처리 반환에 의한 서버 부하 저감
* 단점: 구조가 클라이언트-서버 구성보다 복잡

#### 수직 분할형 아키텍처

: 용도가 같은 서버를 늘려나가는 방식

##### 단순 수평 분할형 아키텍처

: 같은 기능을 가진 복수의 시스템으로 단순분할

* 장점: 수평으로 서버를 늘리기 때문에 확장성이 향상, 분할한 시스템이 독립적으로 운영되므로 서로 영향을 주지 않음
* 단점: 데이터를 일원화해서 볼 수 없음, 업데이트는 양쪽으로 해야함, 처리량이 균등하게 분할돼 있지 않으면 서버별 처리량에 치우침이 생김

##### 공유형 아키텍처

: 데이터 계층을 상호 접속함

* 장점: 확장성이 향상, 서로 다른 시스템의 데이터를 참조 가능
* 단점: 서버 간 독립성이 낮아짐, 공유한 계층의 확장성이 낮아짐

#### 지리 분할형 아키텍처

: 업무 연속성 및 시스템 가용성을 높이기 위한 방식으로 지리적으로 분할하는 아키텍처

##### 스텐바이형 아키텍처

: 물리 서버를 최소 두 대를 준비하여 한 대가 고장나면, 가동중인 소프트웨어를 다른 한 대로 옮겨서 운영하는 방식

##### 재해 대책형 아키텍처

: 특정 데이터센터에 있는 상용 환경에 고장이 발생하면 다른 사이트에 있는 재해 대책 환경에서 업무 처리를 재개하는 것

***

### 제2장 서버를 열어 보자

CPU(Center Processing Unit): 명령을 받아서 연산을 실행하고 반환한다.

메모리: 기억 영역, CPU에 전달하는 내용이나 데이터를 저장하거나 처리 결과를 받는다. 영구성이 없다.

I/O 장치

* 하드 디스크 드라이브(HDD): 자기 원반에 저장, 물리 법칙에 좌우됨
* SSD(Solid State Disk 반도체 디스크): 물리적인 회전 요소를 사용하지 않는 디스크

버스(Bus): 서버 내부에 있는 컴포넌트들을 서로 연결시키는 회선

* 대역: 데이터 전송 능력

***

### 제3장 3계층형 시스템을 살펴보자

#### 프로세스와 스레드

: 프로그램 실행 파일 자체가 아니라 OS상에서 실행돼서 어느 정도 독립성을 가지고 동작하는 것

* 프로세스: 전용의 메모리 공간을 이용해서 동작
* 스레드: 메모리 공간을 공유하고 있는 운명공동체

프로세스와 스레드의 목적은 같지만 어떤 것을 이용할 지는 애플리케이션 개발자가 정한다.

프로세스는 독자 메모리 공간을 가지기 때문에 생성 시 CPU 부하가 스레드와 비교해 높아진다. 때문에 멀티 프로세스 애플리케이션에서는 프로세스 생성 부담을 낮추기 위해 미리 프로세스를 시작시켜둔다(연결 풀링 Pooling)

|      | 프로세스                | 스레드                                                       |
| ---- | ----------------------- | ------------------------------------------------------------ |
| 장점 | 개별 처리 독립성이 높다 | 생성 시 부하가 낮다.                                         |
| 단점 | 생성 시 CPU 부하가 높다 | 메모리 공간을 공유하기 때문에 의도하지 않은 데이터 읽기/쓰기가 발생할 수 있다. |

#### OS커널

: 뒤에서 무슨 일이 벌어지는지 은폐하면서도 편리한 인터페이스를 제공하는 것

1. 시스템 콜 인터페이스: 프로세스/스레드에서 커널로 연결되는 인터페이스

2. 프로세스 관리: 가동되고 있는 프로세스 관리와 CPU 이용 우선순위 등을 '스케줄'한다.
3. 메모리 관리: 서버상의 메모리를 단위크기의 블록으로 분할해서 프로세스에 할당한다.
4. 네트워크 스택: 네트워크를 관리한다.
5. 파일 시스템 괸리: 디렉터리(폴더) 구조 제공, 엑세스 관리, 고속화, 안정성 향상 등
6. 장치 드라이버: 디스크나 NIC 등의 물리 장치용 인터페이스 제공

#### 웹 데이터 흐름

* 프로세스나 스레드가 요청을 받는다.
* 도착한 요청을 파악해서 필요에 따라 별도 서버로 요청을 받는다
* 도착한 요청에 대해 응답한다.

##### 클라이언트 PC부터 웹서버까지

1. 웹 브라우저가 요청을 발행한다.
2. 이름을 해석한다
3. 웹서버가 요청을 접수한다
4. 웹 서버가 정적 컨텐츠인지 동적 컨텐츠인지 판단한다
5. 필요한 경로로 데이터에 엑세스한다.
   * 동적 컨텐츠는 AP서버가 HTML파일을 동적으로 생성한다. 웹 서버는 동적 컨텐츠에 대한 요청을 AP서버에게 던지고 결과를 기다린다.

##### 웹서버부터 AP서버까지

1. 웹 서버로부터 요청이 도착한다.
2. 스레드가 요청을 받으면 자신이 계산할 수 있는지, 아니면 DB접속이 필요한지를 판단한다
3. DB접속이 필요하다면 연결 풀에 엑세스한다.
4. DB서버에 요청을 던진다.

##### AP서버부터 DB서버까지

1. AP서버로부터 요청이 도착한다.
2. 프로세스가 요청을 접수하고 캐시가 존재하는지 확인한다
3. 캐시에 없으면 디스크에 엑세스한다
4. 디스크가 데이터를 반환한다
5. 데이터를 캐시 형태로 저장한다
6. 결과를 API 서버에 반환한다

##### AP서버부터 웹 서버 까지

1. DB 서버로부터 데이터가 도착한다
2. 스레드가 데이터를 가지고 계산 등을 한 후에 파일 데이터를 생성한다
3. 결과를 웹 서버로 반환한다.

##### 웹서버부터 클라이언트 PC까지

1. AP 서버로부터 데이터가 도착한다.
2. 프로세스는 받은 데이터를 그대로 반환한다
3. 결과가 웹 브라우저로 반환되고 화면에 표시된다.

#### 가상화

: 컴퓨터 시스템에서 물리 리소스를 추상화하는 것

##### 호스트OS형

: 윈도우즈나 리눅스 등의 호스트 OS상에 가상화 소프트웨어를 설치해서 이용

* 소프트웨어를 에뮬레이터하는 것으로 성능면에서 제한이 있음

##### 하이퍼바이저형

: 하드웨어상에서 직접 가상화 소프트웨어를 실행하고 그 위에 가상 머신을 동작하는 기술

* 호스트OS를 거치지 않으므로 호스트형보다 성능이 우수

![image](https://github.com/siwoo1627/Today-I-Learn/assets/114638386/1de2fc24-77df-4833-9821-9cd12d64baf0)

##### 컨테이너

: 리소스가 격리된 프로세스, 하나의 OS상에서 여러 개를 동시에 가동할 수 있으며 각각 독립된 루트 파일 시스템, CPU/메모리, 프로세스 공간 등을 사용할 수 있다.

##### 도커

* 컨테이너는 호스트 OS와 OS커널을 공유하므로 컨테이너 실행이나 정지 속도가 빠르다
* 호스트 OS의 커널을 공유하므로 VM만 사용하는 경우와 비교해 한 대의 호스트 머신상에서 훨씬 많은 컨테이너를 실행할 수 있다. 이를 통해 리소스를 한 곳에서 쉽게 관리할 수 있다.
* 도커는 라이브러리나 프레임워크 등을 도커 이미지로 묶어서 공유할 수 있는 것으로, 특정 환경에서는 재현되지만 자신의 개발 환경에서는 재현되지 않는 문제가 발생하기 어렵다. 따라서 버그를 효율적으로 수정할 수 있다.

![image](https://github.com/siwoo1627/Today-I-Learn/assets/114638386/e4d91179-3337-46c4-9a8e-05c19c9527af)

***

### 제4장 인프라를 지탱하는 기본 이론

분담할 수 있는 처리는 CPU 코어를 늘리면 빨라진다(병렬)

분담할 수 없는 처리는 CPU 코어를 늘려도 빨라지지 않는다 -> CPU 클럭 주파수를 올린다

#### 직렬/병렬

* 직렬 처리로 속도를 올리는 데는 한계가 있다.
* 병렬화를 통해 속도는 빨라지지만, 단위 시간당 처리량을 늘릴 수 있다.
  * 병렬 처리에서는 합류점, 직렬화 구간, 분기점이 병목 지점이 되기 쉽다.
  * 병렬화할 때는 일을 분담해서 처리를 한 후 다시 집약할 때 오버헤드가 걸린다. 그러므로 이 오버헤드를 감안하더라도 효과가 있을 경우에 병렬화 한다.

##### 웹서버와 AP서버에서의 병렬화

웹 서버: 복수의 프로세스가 분담해서 병렬처리

AP서버: JVM 프로세스가 하나지만 복수의 스레드가 병렬 처리

##### DB서버에서의 병렬화

|      | 장점                                                         | 단점                                                         |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 직렬 | 구조가 간단해서 설계나 구현 난이도가 낮다                    | 복수의 리소스를 유용하게 이용할 수 없다                      |
| 병렬 | 복수의 리소스를 유용하게 이용할 수 있으며, 직렬에 비해 동일 시간당 처리할 수 있는 양이 증가한다. 또한, 일부가 고장 나더라도 처리를 계속할 수 있다 | 처리 분기나 합류를 위한 오버헤드가 발생한다. 배타적 제어 등을 고려해야하고 구조가 복잡해서 설계나 구현 난이도가 높다 |

#### 동기/비동기

|            | 장점                                                         | 단점                                                         |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 동기       | 의뢰한 처리가 끝났는지 여부를 쉽게 확인할 수 있어서 구조가 간단하고 구현 난이도도 낮다 | 의뢰한 처리가 끝나기까지 기다려야 하기 때문에 대기 시간을 활용할 수 없다 |
| 비동기[^1] | 의뢰한 처리가 진행되고 있는 동안 시간을 효율적으로 사용해서 병렬 처리를 할 수 있다. | 의뢰한 처리가 끝났는지 확인하지 않으면 모르기 때문에 불필요한 확인 처리가 늘어난다. 구조가 복잡해서 구현 난이도가 높다 |

[^1]: AJAX

#### 큐

: 대기행렬, FIFO(선입선출)

* 런큐: CPU를 기다리고 있는 프로세스 행렬
* 메세지큐: 이메일, 발신자는 상대 상황을 개의치 않고 메일을 전송할 수 있기 때문에 수신자의 상황에 끌려다니지 않음

#### 배타적 제어

: 다른 것을 배제하는 제어, 여러 사람이 공유하는 경우 누군가 사용 중이면 다른 사람이 사용할 수 없다.

* 복수의 처리가 공유 자원(CPU, 메모리, 디스크 등)에 동시에 엑세스하면 불일치가 발생할 수 있기 때문에 배타적 제어로 보호해주어야 함
* 배타적 제어에서는 특정 처리가 공유 자원을 이용하고 있는 동안 다른 처리가 이용할 수 없게 해서 불일치가 발생하지 않도록 한다
* 병목 현상이 발생하기 쉽다.

|                                  | 장점                             | 단점                             |
| -------------------------------- | -------------------------------- | -------------------------------- |
| 배타적 제어를 사용하는 경우      | 공유 데이터의 일관성을 유지 가능 | 병렬처리가 안됨                  |
| 배타적 제어를 사용하지 않는 경우 | 병렬로 빠르게 처리 가능          | 데이터의 불일치가 발생할 수 있음 |

#### 상태 저장(Stateful)/상태 비저장(Stateless)

상태

* 과거에 부여한 정보를 저장해서 계속 활용할 수 있다
* 세분화된 제어가 가능, ssh

비상태

* 과거 정보를 알 수 없다
* 고기능은 아니지만 간단, http

#### 가변 길이(Variable-length)/고정 길이(Fixed-length)

* 가변 길이는 공간을 유용하게 활용할 수 있지만 성능 면에서 불안정하다
* 고정 길이는 쓸데없는 공간이 생기지만 성능 면에서는 안정적이고 관리가 수월하다

#### 데이터 구조(배열과 연결 리스트)

: 데이터를 순차적으로 처리하는 구조

##### 배열

* 데이터를 빈틈없이 순서대로 나열한 데이터 구조
* 탐색이 빠름
* 데이터 추가, 삭제가 느림

##### 연결 리스트

* 데이터를 선으로 연결한 데이터 구조
* 탐색이 느림
* 데이터 추가, 삭제가 빠름

배열에 SQL을 고정길이 해시 값으로 저장 하고 이 해시 값 배열로부터 연결 리스트를 찾아가면 빠르게 탐색할 수 있다.

#### 탐색 알고리즘(해시/트리 등)

풀 스캔(Full Scan): 인덱스가 없는 경우 테이블의 모든 블록을 처음부터 순서대로 읽어 가는 것

인덱스가 있는 경우 검색이 빨라지지만 데이터 추가, 갱신, 삭제 시에 테이블뿐만 아니라 인덱스 데이터도 갱신해야 한다. 

인덱스 갱신 때문에 불필요한 오버헤드가 발생할 수 있다.

##### 인덱스의 구조 - B 트리 인덱스

![image](https://github.com/siwoo1627/Today-I-Learn/assets/114638386/e03425da-f47c-4085-848e-f495b2a2105c)

①루트 블록②브랜치 블록③리프 블록

DBMS에서 자주 사용되는 이유는 트리 구조 계층이 깊어지지 않도록 디스크 I/O를 최소한으로 제어하기 때문이다.

##### 해시 테이블

: 키, 값 조합으로 표를 구성한 데이터 구성이다. 키는 해시 함수를 통해 해시 값으로 변환된다. 해시 값은 고정 길이 데이터이기 때문에 조합 표의 데이터 구조가 간단해서 검색이 빠르다는 장점이 있다.

***

### 제5장 인프라를 지탱하는 응용 이론

#### 캐시(cache)

: 사용 빈도가 높은 데이터를 고속으로 엑세스할 수 있는 위치에 두는 것(임시 저장소)

* 일부 데이터를 데이터 출력 위치와 가까운 지점에 일시적으로 저장한다.
* 데이터 재사용을 전제로 한다

* 실제 데이터에 대한 엑세스 부하를 줄일 수 있다

#### 끼어들기

: 끼어들기는 급한 일이 생겨서 지금 진행 중인 일을 중단하고 급한 일을 끝낸 후에 다시 원래 일을 진행하는 것이다.

* PC에서 애플리케이션이 처리를 하는 동안에도 키보드를 누르면 문자가 화면에 곧바로 표시되는 것(하드웨어 끼어들기)

#### 폴링

: 정기적으로 질의하는 것, 정기적으로 질의함으로써 상대가 어떤 상태인지, 어떤 요구를 가지고 있는지 알 수 있다

* 집배원이 우체통안에 편지가 있든 없든 우체통을 정기적으로 확인하는 것
* 질의 방향이 단방향이다
* 질의는 일정 간격을 따라 정기적으로 발생한다
* 반복만 하면 되기에 프로그래밍이 쉽다
* 상대가 응답하는지 확인할 수 있다
* 모아서 일괄적으로 처리할 수 있다

> 전화는 끼어들기이고, 폴링은 이메일이다.

#### I/O 크기

: 데이터를 주고 받을 때 사용되는 I/O의 크기

> 크기가 너무 크면 비용이 많이들고 쓸데없는 공간도 많고, 작으면 여러 번 옮겨야 하고 시간도 많이 듬

* 물건을 운반할 때는 상자에 넣으면 효율적으로 관리할 수 있다
* 운반하는 양에 따라 상자 크기를 선택하면 효율적으로 운반할 수 있다

#### 저널링(Journaling)

: 저널(Journal)은 트랜잭션이나 매일 갱신되는 데이터 이력이다

* 데이터 자체가 아닌 처리(트랜젝션) 내용을 기록한다
* 데이터 일관성이나 일치성이 확보되면 필요 없어진다
* 데이터 복구 시 롤백(실제 데이터 정보를 과거로 되돌리는 처리), 롤포워드(저널을 읽어서 실제 데이터 정보를 앞으로 진행시키는 처리)에 이용된다

장점

* 시스템 장애 시 복구가 빠르다
* 데이터 복제보다도 적은 리소스를 소비해서 데이터를 보호할 수 있다

단점

* 기록 빈도가 많으면 오버헤드도 높아짐

#### 복제(Reflection)

: 각종 재해에 대비해서 멀리 떨어진 장소에 예비 시스템에 복제, 대량의 사용자 접속에 대비해 동일 데이터를 여러 서버에 복제해서 부하분산

* 장애 시 데이터 손실을 예방할 수 있다
* 복제를 이용한 부하 분산이 가능하다
* 사용자가 데이터에 엑세스할 때 복제한 것이라는 것을 의식할 필요가 없다
* 복제 데이터를 캐시처럼 사용할 수 있다

#### 마스터-워커(Master-Worker)

: 주종관계, 한 명이 관리자가 되어 모든 것을 제어

>  단일 리소스를 관리할 때 한 사람이 관리하는 것이 효율적이다

장점

* 관리자가 한 명이기 때문에 구현이 쉽다
* 워커 간 처리를 동기화할 필요가 없기 때문에 통신량이 줄어든다

단점

* 마스터가 없어지면 관리를 할 수 없다(작업 인계 구조가 필요)
* 마스터의 부하가 높아진다

**피어-투-피어(Peer-to-Peer, P2P)**

: 마스터-워커 반대, 관리대상 리소스를 공유하지 않으면 효율적이다

#### 압축

: 정보의 낭비[^2]를 막으면 압축할 수 있다

디지털 데이터 압축의 기본은 '중복 패턴 인식'과 그것을 '변경'하는 것이다.

zip 파일로 압축할 때 어떤 파일은 크기가 작아지지만 어떤 파일은 그대로인 것을 보면 같은 패턴이 많을수록 압축률이 높아진다(파일 크기가 작아짐)

* 장점: 데이터 크기를 줄임

* 단점: 처리 시간이 걸림

* 압축한 데이터를 원래대로 복원할 수 있는 가역 압축과 이미지나 음성 데이터 등에 있는 사람이 인식할 수 없는 부분을 생략하는 비가역 압축이 있다

[^2]: 자신에게 필요 없는 정보, 이미 알고 있는 정보

#### 오류 검출

: 데이터의 파손 여부를 확인하는 기법

1. 통신 중에 데이터 파손(전기적 잡음 등)

2. 칩(chip)에서의 데이터 파손(메모리나 CPU등의 물리적 파손)

***

### 제6장 시스템을 연결하는 네트워크 구조

#### 계층 구조

계층구조에서는 데이터나 기능 호출 흐름에 따라 계층 간 역할이 나누어진다는 특징이 있다. 역할이 나누어져 있기 때문에 각 층은 자신이 담당하는 일만 책임지며, 다른 일은 다른계층이 책임진다. 상호 연결돼 있는 계층들에서는 교환방법 즉 `인터페이스`만 정해 두면 된다.

* OSI 7계층 모델

| 애플리케이션 계층 | 애플리케이션 처리             |
| ----------------- | ----------------------------- |
| 프레젠테이션 계층 | 데이터 표현 방법              |
| 세션 계층         | 통신 시작과 종료 순서         |
| 전송 계층         | 네트워크의 통신 관리          |
| 네트워크 계층     | 네트워크 통신 경로 선택       |
| 데이터 링크 계층  | 직접 접속돼 있는 기기 간 처리 |
| 물리 계층         | 전기적인 접속                 |

![image](https://github.com/siwoo1627/Today-I-Learn/assets/114638386/5803061a-7eac-43f3-81a0-051b80eec9f7)

#### 프로토콜

: 컴퓨터가 서로 소통하기 위한 통신 규약

TCP/IP 프로토콜, USB 프로토콜, CPU 내부 프로토콜 등

#### TCP/IP 4계층 모델

실제 현장에서는 OSI 참조모델의 7계층 방식으로 부르는 경우가 많다.

링크 계층을 레이어2나 L2, IP계층을 레이어 3이나 L3, 전송계층(TCP계층)을 레이어4나 L4, 애플리케이션 계층은 애플리케이션 계층이나 L7이라고 한다

> L5,L6,L7을 모아서 애플리케이션 계층으로 취급

##### 애플리케이션 계층의 프로토콜 - HTTP

클라이언트 PC와 웹 서버 사이 요청과 응답은 HTTP를 통해서 주고 받는다.

시스템 콜을 통해 통신을 의뢰받은 커널은 소켓을 만들고 IP 주소와 TCP 포트의 정보로 접속 대상의 서버도 연결되고 접속 대상 서버도 시스템 콜로 소켓이 생성된다.

* 소켓에 기록된 데이터가 TCP/IP를 통해 상대 서버까지 전달된다.

##### 전송 계층 프로토콜 - TCP(Transmission Control Protocol)

: 애플리케이션이 보낸 데이터를 그 형태 그대로 상대방에게 확실하게 전달하는 것

TCP가 담당하는 것은 서버가 송신할 때와 서버가 수신한 후 애플리케이션에게 전달할 때로, 상대 서버까지 전송하는 부분은 하위 계층인 IP에 위임한다.

> IP는 데이터가  상대방에게 확실히 전달됐는지 확인하는 기능이나 도착한 순서를 확인하는 기능이 없다

* 포트 번호를 이용해서 데이터 전송 - 80번 등
* 연결 생성 - 가상경로(버추얼 서킷)
* 데이터 보증과 재전송 제어 - ACK
* 흐름 제어와 폭주 제어 - ACK

##### 네트워크 계층의 프로토콜 - IP(Internet Protocol)

: 지정한 대상 서버까지 전달받은 데이터를 전해 주는 것

* IP 주소를 이용해서 최종 목적지에 데이터 전송

IP계층에 전달된 TCP 세그먼트는 최종 목적지가 적힌 IP헤더를 추가하여 IP패킷으로 생성되고 최종 목적지 서버까지 복수의 네트워크를 경유해서 데이터가 전송된다.

* 라우팅(Routing)

다른 네트워크에 있을 경우 최종 목적지에 도착할 때까지 목적지를 알고 있는 라우터에 전송을 부탁한다.

IP 패킷을 받은 라우터는 해당 IP패킷의 헤더에서 목적지를 확인해서 어디로 보내야 할지를 확인한다.

외부와 접속하는 네트워크는 보통 기본 게이트웨이라는 라우터가 설치되어 있고, 기본 게이트웨이는 외부 네트워크에 접속되어 있어 외부 세계에 패킷을 보낼 수 있다.

![image](https://github.com/siwoo1627/Today-I-Learn/assets/114638386/cc009ec1-5288-4390-a088-3245b4eaf27d)

##### 데이터 링크 계층의 프로토콜 - 이더넷(Ethernet)

: 동일 네트워크 내의 네트워크 장비까지 전달받은 데이터를 운반한다(물리 계층), 이더넷 프레임

IP는 IP주소를 사용해서 여러 네트워크를 거쳐 데이터를 전송할 수 있지만, 이더넷은 동일 네트워크 내, 즉 자신이 포함된 링크 내에서만 데이터를 전송할 수 있다. 이때 사용되는 주소가 MAC(맥) 주소다.

* MAC 주소는 네트워크 통신을 하는 하드웨어에 할당된 주소로, 원칙적으로는 세상에 있는 모든 장비가 고유한 물리 주소를 가지고 있다

***

### 제7장 무정지를 위한 인프라 구조

#### 안정성 및 이중화

안정성(고가용성): 시스템 서비스가 가능한 한 멈추지 않도록 하는 것

* 고장, 장애에 의한 정지가 발생하지 않을 것(컴포넌트 이중화)
* 고장, 장애가 발생해도 복구할 수 있을 것(컴포넌트 이중화)
* 고장, 장애가 발생한 것을 검출할 수 있을 것(컴포넌트 감시)
* 고장, 장애가 발생해도 데이터가 보호될 것(데이터 백업)

이중화: 하나의 기능을 병렬로 여러 개 나열해서 하나에 장애가 발생해도 다른 것을 이용해서 서비스를 계속할 수 있는 것

* 부하 분산
* 내부적 생존 감시
* 마스터 결정
* 페일오버(교체): 액티브 측에 문제가 생기면 스탠바이 장비로 교체되어 액티브로 변경된다.

####  서버 내 이중화

* 전원, 장치 등의 이중화
* 네트워크 인터페이스 이중화

#### 저장소 이중화

* SAN을 이용한 HDD 이중화

* RAID는 여러 HDD를 묶어서 그룹을 만들고 이것을 논리적인 HDD로 인식한느 기술이다.
  * 안정성확보, 성능향상, 용량확장

#### 웹 서버 이중화

* 웹 서버를 여러 개 가동시켜 두어서 프로세스/스레드 중 하나에 장애가 발생해도 다른 프로세스/스레드가 가동되고 있기 때문에 웹서버의 서비스 전체가 정지되는 일은 없다.

* DNS를 이용해서 하나의 호스트명에 대해 복수의 IP주소를 반환한다.(Round Robin)
  * 부하 분산 장치(로드 밸런서)를 이용: 이전에 어느 웹서버에 요청을 할당했는지를 쿠키에 저장해서 HTTP 프로토콜의 메시지 헤더에 포함한다.
    * 클라이언트 측에서 사용을 허가하면 두 번째 이후 접속부터 HTTP 요청 헤더에 쿠키를 저장해서 접속한다. 부하분산 장치는 이 쿠키를 읽어서 같은 서버에 요청을 할당하는 것이다.

| 알고리즘                    | 내용                                                         |
| --------------------------- | ------------------------------------------------------------ |
| 라운드 로빈(Round Robin)    | 서버의 IP 주소에 순서대로 요청을 할당한다                    |
| 최소 연결(Least Connection) | 현재 활성 세션 수보다 세션 수가 가장 적은 서버의 IP주소에 요청을 할당한다 |
| 응답 시간(Response Time)    | 서버의 CPU 사용률이나 응답 시간 등을 고려해서 가장 부하가 적은 서버의 IP 주소에 요청을 할당한다 |

#### AP 서버 이중화

* 부하분산 장치 이용(위와 같음)
* 세션 정보 이중화
  * 세션 정보란 애플리케이션의 상태를 가리킨다, 쿠키 정보를 가지고 보조 세션 정보에 접속해서 세션이 계속 유지된다
* DB연결 이중화 - 연결 풀링(Connection Pooling)

#### DB 서버 이중화

액티브-스탠바이형의 클러스터(Cluster) 구성(=HA 고가용성 구성이라고도 부른다)

클러스터 소프트웨어는 등록된 서비스가 정상 동작하고  있는지 정기적으로 확인한다. 이상이 발생하면 서비스를 정지하고 대기하고 있던 스탠바이 측 서비스를 시작해서 서비스를 유지시킨다.

#### 네트워크 장비 이중화

* L2 스위치 이중화
* L3 스위치 이중화
* 네트워크 토폴로지

#### 사이트 이중화

데이터 관련 사이트 이중화는 복제(Replication) 예에서 본 것처럼 MySQL 복제나 저장소 측 복제를 이용한다.

사이트가 떨어져 있을 때는 비동기로 해서 어느 정도의 데이터 손실을 감수하고 있는 구성이 일반적이다

#### 감시

: 서비스를 안전하게 지속시키기 위해

* 생존감시
* 로그(에러) 감시
* 성능 감시

* 감시전용 프로토콜 - SNMP
  * 네트워크 장비나 서버 가동 상태
  * 서비스 가동 상태
  * 시스템 리소스(시스템 성능)
  * 네트워크 트래픽
* 콘텐츠 감시(웹 화면)

#### 백업

* RTO(Recovery Time Object): 복구 목표 시간 - 복구에 어느 정도 시간이 걸리나
  * RTO가 짧을 수록 설계 난이도가 높고 백업 시스템 가격도 비싸진다.
* RPO(Recovery Point Object): 복구 기준 시점 - 어느 시점으로 복구할 것인가

시스템에서 백업 해야하는 대상: 시스템 백업(OS나 미들웨어 등의 백업), 데이터 백업(데이터베이스나 사용자 파일)

##### 시스템 백업

가동 중인 서비스는 백업할 수 없어서 계획된 시스템 일정에 따라 실시

- 시점

  * 초기 구축 후

  * 일괄 처리 적용 시

  * 대규모 구성 변경 시

- 취득 방법

  * OS 명령(tar, dump 등)

  * 백업 소프트웨어

##### 데이터 백업

매일 변경되는 데이터가 손실되지 않도록 하는 것으로, 취득 빈도가 높다

***

### 제8장 성능 향상을 위한 인프라 구조

#### 응답[^3]과 처리량[^4]

응답시간은 웹 브라우저의 처리 속도, AP 서버의 처리 시간, DB서버의 처리 시간 중간의 통신 시간  등 모든 응답 시간에는 반드시 물리적 제약이 존재한다.

* 데이터라는 정보가 물리적으로 이동하는 이상은 어쩔 수 없으며, 이것의 개선에 한계가 보일 때는 처리량 개선을 통해서 시스템 사용률을 개선하는 것이 일반적이다.

대량의 데이터를 교환하고 싶은데 영역이 부족한 경우에 처리량 문제가 발생한다. 물리적으로 데이터를 통과시킬 수 없을 때 처리량 관점의 병목 현상이 발생하게 된다.

> 2차선 도로에서는 세 대가 동시에 통과하기 힘들다.

[^3]: 처리 하나당 소요 시간(사용자 관점)
[^4]: 단위 시간당 처리하는 양(서비스 제공자 관점)

#### 병목 현상(BottleNeck)이란?

: 처리량을 제한하고 있는 요인

* CPU 사용률 높음으로 인해 AP서버 병목지점이 된다

각 서버의 처리량이나 응답 상황 로그를 취득해서 어느 서버가 병목 지점이 되는지 찾아낸다.

* 병목위치를 찾아내서 해결하거나 이용자 수를 제한한다.

#### 3계층형 시스템 그림을 통해 본 병목 현상

* CPU 병목 현상
  * CPU를 이용하는 처리가 많아서 대기 행렬이 발생함 -> 처리량을 늘림(점원, 계산대를 늘림)
  * CPU 응답이 느림 -> 처리 능력 향상(스케일 업), 병렬 처리 - 멀티 프로세스화
* 메모리 병목 현상
  * 영역 부족 
    * 가상 메모리: 페이징 또는 스와핑 처리로 빈 메모리 확보
  * 동일 영역의 경합: 다수의 프로세스가 동시에 같은 영역에 엑세스하면, 이 메모리 영역을 확보하기 위해 대기 시간이 발생한다.
    * 래치(Latch) 구조 이용: 참조나 갱신을 하고 싶은 프로세스가 서로 경합해서 빠른 쪽이 그 영역을 독점한다
    * 경합이 발생하지 않도록 복수의 프로세스나 스레드가 같은 메모리 영역을 참조하지 않도록 만듬
* 디스크 I/O 병목 현상
  * 하드 디스크 등의 저장 장치에 대한 I/O 병목
* 네트워크 I/O 병목 현상
  * 통신 프로세스의 병목
    * 병렬화 - 멀티 코어화
    * 압축을 이용해 전송량을 줄임
  * 네트워크 경로의 병목: 기본 게이트웨이에 큰 트랜젝션들이 모두 게이트웨이인 특정 라우터를 경유하여 처리 한계에 다름
    * 전용 네트워크 개설, 장시간 지연되는 세션 강제 종료 등
* 애플리케이션 병목 현상
  * 알고리즘 문제
  * 특정 데이터에 의존하는 처리로 인한 병목
    * 값의 캐시화
    * 병목 지점 분할
