# Project #3: Stock Server

#### Goal
> 여러 client들의 동시 접속 및 서비스를 위한 Concurrent stock server 구축
- Server:
	주식 정보 저장
	여러 client들과 통신
	주식 정보 List 판매, 구매
- Client:
	Server에 주식 사기, 팔기, 가격과 재고 조회 요창

`Plus alpha:`
	memory에 올릴 때 binary search tree로 만들게 되어있음
	concurrent하게 사고파는 요청이 온다.
	Event 기반:
		process 하나가 조건문을 돌면서 실행	
	Thread 기반:
		Pthread 여러개를 띄워서 connection마다 thread를 띄울 수도 있고 Thread pool을 만들어서 필요한 thread를 꺼내 쓸 수도 있고

##### Background
- 강의자료 4, 5, 6

##### 주식 `stock.txt`
```
ID  잔여 주식  주식 단가
1   7         10000
5   3         3700
:
// Client request - 잔여 주식 이상의 주식 요구: `"Not enough left stocks"` 출력
```
- 변동 되는 Data: `잔여 주식`
- Data structure: Binary Tree
```c
// Example
struct item {
	int ID;
	int left_stock;
	int price;
	int readcnt;
	sem_t mutex; // lock 변수
}
// ID는 [1 ~ ID_MAX] 사이에서 random unique id 발급
```
- Problem, Readers-writers:
	client a가 item_i 주식을 읽을 때 다른 client b가 item_i를 업데이트 하게되면 올바르게 동작하지 않는다. `Read and write의 순서 문제`
	-> Solution, fine-grained locking `노드 단위의 관리`

##### Multiclient 실행 파일 `multiclient.c`
- 수정 가능 변수
```
#define MAX_CLIENT 100
#define ORDER_PER_CLIENT 10
#define STOCK_NUM 10
#define BUT_SELL_MAX 10
```
- 사용법
```
' ./stockserver 1119 먼저 실행 (stockclient와 달리 port번호 달라짐)
' Task1, Task2에서는 client# <= 4
' Zombie 또는 Orphan이 발생하지 않도록 주의하자.

~$ ./multiclient <host> <port> <client#>
```

##### Response Format
```
~$ [buy] success           // buy request 정상적으로 처리
~$ [sell] success          // sell request 정상적으로 처리
~$ Not enough left stocks  // but request에서 잔여 주식이 부족한 경우우
```

#### Task1
> Event-driven Approach `using select()`

-> client: trigger `fd`
-> server: `select()`에 의해 동작 시작
	table(`stock.txt`)의 내용을 읽어 메모리에 적재 후 요청을 처리
-> server: 동작 종료
	업데이트 된 변경사항을 table(`stock.txt`)에 정해진 format으로 저장

#### Task2
> Thread-based Approach `using pthread library`
- Pthread: Process-based Server와 비슷하나 Process 대신 Thread를 쓴다.

#### Task3
> 성능 평가 및 분석
- Task1과 Task2d의 elapse time 측정 및 분석 후 보고서 기재
- Task3에 한정해서 4개 초과의 client process를 띄우는 것을 허용
- 분석 포인트:
	- 확장성: Client 개수 변화에 따른 동시 처리율 변화 분석
	- Workload: Client 요청 타입(buy, show, sell..)에 따른 동시 처리율 변화 분석
	- 수업과 비교: Branch 방법의 성능 또는 다양한 관점에서 비교 분석
	- 기타

#### 제출
- Contents:
	format: `prj3_20221623.tar.gz`
	![300](../../Pasted%20image%2020240522185140.png)
- Docs:
	format: `pdf`
	내용: `document.docx` 기반
- Date:
	~ 6/3 (지각 허용)

#### Caution
- Process Management:
	`ps -aux` 혹은 `top`를 통해 Background Process를 수시로 확인할 것

#### Tip
- 먼저 command string을 server가 concurrency하게 판독할 수 있도록 구현
	`./echoservert 1119` 를 만들자
- Responce 출력은 `multiclient.c`말고 `stockclient.c`를 실행시켜 command를 하나씩 입력하고 제대로 처리하는지 확인
- 채점은 stock.txt에 업데이트 내용이 제대로 반영되었는지 확인함으로써 진행한다.
