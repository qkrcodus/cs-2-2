![[Pasted image 20241215121246.png]]

4.5장에서 네트워크 계층이 네트워크 통신 서비스를 제공하는 것을 봤다.

6장은 network layer 아래 link layer에 대해 다룬다. 
##### link layer 가 제공하는 서비스
1. 두 호스트 사이 datagram은 header 와 trailer가 붙어 encapsulation되어 frame이 된다. 출발지 호스트에서 시작해 유무선 link 타고 나가며, 패킷 스위치들 (switch , router) 거친 뒤에 목적지 호스트에 도착하게 해준다
   
2. frame header는 MAC 주소를 이용한다. 이는 source destination 구별하게 해준다(ip주소와 다름)
   
3. adjacent node간 reliable delivery 가능하게 한다. 베이스는 RDT
굳이 low bit error link 에선 사용 안하지만, 무선 link엔 error 많이 발생해서 사용한다.

4. 수신 송신 node 간 수신 노드 버퍼에 남은 데이터 크기를 고려하고, delay loss를 방지하기 위해 송신에게 신호를 보내고 flow control을 한다
   
5. **error detection : 받는이가 (수신자)가  CRC나 checksum으로 에러를 감지**
   
6. **error correction : 수신자가 에러를 감지하고 수정한다**
   
7. half duplex : 무전기 (동시 불가능), full duplex : 전화기 ( 동시 데이터 전송 가능) 방식을 결정한다
##### transport layer에서도 하는 error detect랑 correction은 link layer 에서 제공하는 두가지 서비스다.
< 오류 감지 기본틀 >
data에 EDC (error detection correction) 비트를 붙여 송신 측으로 보낸다. 
송신 측은 받은 data, EDC 를 보고 data에 이상이 없나 확인하고 link layer 위의 network layer로 패킷을 보낸다.
하지만, 이는 100% 신뢰할 수 없다. EDC 필드가 더 길어야지 에러가 덜 발생한다. 

##### 물리적으로 이웃한 두 노드 사이로 전송되는 link layer의 data packet ; frame의 비트 오류를 확인하는 방법 3가지
1. bit parity (single bit parity- detect만 가능 , two demension bit parity - dtect, correct가능)
2. checksum (transport layer 에서 주로 사용, 소프트웨어로 구현되기에 빠른 연산인 chekcsum을 이용, 오류 발생 가능성 높음)
3. crc(network layer에서 자주 사용, 하드웨어에서 처리하기에 crc 연산 빠르게 처리 가능 )

##### CRC 비트 오류 검출 방법 
- data bit, divider bit는 주어진다

![[Pasted image 20241215160042.png]]
이후 modulo 2 연산( xor )
![[Pasted image 20241215160049.png]]

##### 링크 계층은 2 종류, pointtopoint link이랑, broadcast link이 있다.
- point to point 는 링크 양 끝에 스위치나 호스트 1:1
=> PPP(p2p protocol)로 동작 

- broad cast 는 한 노드가 frame을 전송하면 수많은 수신 노드들이 frame 복제본을 받음 
=> 그래서 중요한게 multiple access protocol 
< broadcast link 예시 > 유선 HFC, 무선 LAN ,RF , shared wire

##### link layer의 데이터 전송의 총체적 규칙을 맡는 프로토콜인 MAC protocol, 그 하위 개념 더 세부적 규칙을 맡는 multiple access protocol
##### multiple access protocol 은 각 노드가 한다. 별도의 관리자 없음
노드들이 frame을 동시에 여러 개 보낸다면 송신 노드는 두 개 이상의 frame을 받는다. interference가 발생한다. 
네트워크의 각 노드는 각자의 프로토콜이 구현되어 있다. 이 프로토콜( 두 개 이상의 노드가 통신하기 위해 따르는 규칙, 절차) 이 각 노드에서 분산 알고리즘을 실행한다. 
##### multiple access protocol은 어떤 방법으로 broadcast link의 단점을 극복하는가?
###### 1. 채널 분리 channel partioning - 충돌 자체를 방지하자 ==(3가지)==
###### TDMA - 안 쓰면 idle로 냅두자
broadcast channel 이 round 단위로 고정 길이의 slot이 모든 사용자에게 할당
slot을 한 frame이 독점 사용
안 쓰이는 idle slot있을 수도 ( round robin은 안 쓰이는 slot을 분배한다.)
![[Pasted image 20241220082043.png]]
###### FDMA
broadcast channel을 참여하고 있는 노드 개수만큼 주파수 별로 분리되어 각 노드에게 할당
사용하지 않는 주파수 대역은 goes idle
각 대역 간의 간섭 방지를 위해 guard band라는 빈 공간이 있는데 이로 인해 사용 가능한 주파수 자원이 감소함 => TDMA에 비해 효율 떨어짐 

자원 낭비 심하니까 라디오에서만 사용

일반적으로  TDMA
###### CDMA
할당 받으면 노드는 이를 독점적으로 사용 -> 충돌 x
###### 2. random access - 충돌 발생하면 이를 처리하자 ==(5가지)==
at full channel data rate 로 노드가 통신하니, 무조건 충돌이 난다. 쓸 땐 너 혼자 써

충돌 탐지랑 해결? slotted ALOHA, CSMA, CSMA/CD, CSMA/CA, ALOHA
##### PURE ALOHA ( 충돌 해결 방법 : 재전송)
전송할 프레임이 생기면 바로 전송 
최대한으로 RTT + @ 동안 ACK를 기다림 
충돌 나면 재전송
BACKOFF LIMIT (미리 정해둔 시간) 까지 기다리다 재전송 반복되면 전송 포기함

비동기식이라 효율 떨어진다. 노드 들 간 동기화 x

==어떻게 해결 ?== slotted ALOHA
##### slotted ALOHA ( 충돌 해결 방법 : 확률 기반 재전송)
frame 크기 통일해. 
각 슬롯의 길이는 **1개의 프레임을 전송하는 시간**
노드 들 간 동기화 0

==동시에 frame 보낸다면 이를 어떻게 해결==? => 슬록에서 확률 기반으로 재전송을 시도함, 성공할 때까지 반복함
![[Pasted image 20241216174439.png]]
내가 독점적으로 슬롯을 쓰거나, collide 거나, empty거나

**장점**
고도화 분산 시스템 ( 중앙 제어 x) , 슬롯 독점 자치 가능
**단점**
3개의 프레임을 보내기 위해 9개의 slot을 보냄. slot 낭비. 전송 효율이 33.3%으로 낮음

#####  CSMA(carrier sense multiple access)
이 선로 아무도 안 쓰는지 확인하고 frame 전송
propagation delay로 선로 확인할 때 오차가 있을 수 있어서 충돌 발생할 수 있다 . 하지만 이 방법은 충돌이 발생해도 중단하지 않는다. 따라서 낭비되는 시간이 크다.

==어떻게 해결?== => CSMA/CD, CSMA/CA
#####  CSMA/CD- collision detect , 유선
감지를 빠르게 한다면, 안의 구성원이 그 내용을 전부 알려준다.
보내려는 프레임을 취소하고 broadcast channel 을 idle로 만든다.

LAN 은 일정 구역에 묶인 subnet이다. 
유선에선  CD 감지 빠르다. 
무선에선 CD 감지 어렵다

![[Pasted image 20241216183003.png]]

<알고리즘>
![[Pasted image 20241216183037.png]]
기다렸다가. 충돌나면 전송을 중단하고 jam 신호 . NIC가 binary BACKOFF 방식 따라 기다림. (랜덤하게 기다리는 시간 프레임에게 부여). 이후 프레임들은 IDLE 상태인지 감시함.

NIC가 datagram 을 받고 frame을 만든다.
송신 준비
채널 감시
송신하다가 collision나면 중단 후 NIC가 binary BACKOFF로 jam 신호 보낸다. 

cf ) slotted ALOHA 는 각각 노드별로 즉 프레임별로 랜덤한 확률 따라서 전송하지만 , CSMA/CD 에선 각 프레임 별로 랜덤 시간만큼 기다리고
##### CSMA/CA- collision avoidance , 무선
왜 필요한가? 
무선 LAN에서 충돌 자체를 회피하자.

유선망 컴퓨터를 키면, DHCP 통해 IP를 받아온다( 컴퓨터가 해당 네트워크에 속한다.)
근데 무선망에선 셀별로 구역이 있고, AP가 하나씩 있다. 무선 LAN에선 어떤 호스트가 어떤 네트워크에 속하는지 아는 것 자체부터 복잡하고 인지 불가능하다.

==> 충돌 자체를 회피하자

<알고리즘>
IDLE 상태면, IFS (INTERFRAME SPACE)동안 기다림 (전파 지연까지 고려했는데도 아무도 안 쓰네), STILL IDLE일 때 데이터 전송함

만약 기다는 시간(IFS) 동안 다른 프레임이 들어오면?
만약 아닐 때 BACKOFF TIME 대기 ( 이 값은 충돌 발생할 때마다 늘어난다 )
![[Pasted image 20241216184502.png]]

##### 3. take turns (얘가 최고)
1. POLLING 
사전에 우리 네트워크는 한 턴에 얼마 만큼의 데이터와 얼마 만큼의 시간을 쓸 수 있는지 정해둠
- overhead
- 지연
- master 노드가 죽으면 다음 순서 노드가 역할을 못 한다. 
  
2. TOKEN PASSING
- 토큰을 가진 노드가 죽으면 나머지는 하염 없이 기다림

##### 정리
CHANNEL PARTITIONING - FDMA 라디오 , TDMA
TAKING TURNS - 블루투스
RANDOM ACCESS - 일반적으로 사용하는 기기
![[Pasted image 20241216205003.png]]
##### 차이점
channel partitioning : fdma- 전송 대역 폭이 전체 폭/ 통신하는 노드수보다 적음 든 tdma - 전송률이  전체 전송률/통신하는 노드수 
	이런 식으로 채널 전체를 이용하지 못한다. 충돌은 나지 않는다
random access : 채널 전체를 사용하는 방법이다. 충돌은 난다.
	 aloha - 프레임이 오면 바로 전송한다. rtt+@시간 만큼 기다렸다가 ACK 안오면 재전송. backofftime까지 기다렸다가 그래도 안 오면 포기한다. 
	 slotted aloha - frame 크기랑 slot이 정해지고, 보내는 시간도 정해진다. 충돌 나면 랜덤 확률에 맞게 다시 보낸다
	 csma - 프레임을 보내기 전에 미리 채널 확인 , 충돌나면 정지 한다. 그치만 이 방법도 propagation delay때문에 충돌이 날 수 있다. 해결을 위해서 
	 csma-cd 충돌이 나면 중지하고, 랜덤한 시간만큼 기다렸다가 재전송한다
	 csma-ca 채널 감지하고, ifs 만큼 더 기다렸다가 유선에서 충돌나면 중지하고, 재전송한다.
take turns : 만약 n개의 노드가 활성화했을때 n/전체 전송률 만큼이 가능하게 하는 방법. ildle slot 제거
	  polling 마스터 노드가 자식 노드에게 순서대로 보낼 수 있는 프레임 개수를 정해주고 자식이 그만큼을 순서대로 보내주는 방식. master 죽으면 끝, polling 지연 있음
	  token tossing 정해진 노드 순서대로 토큰을 전달한다. 보낼 프레임이 없으면 토큰을 전달, 있다면 토큰을 킵한다. 지연 있고 토큰 받은 노드가 죽으면 끝

##### MAC 주소랑 IP 주소
-  IP 주소 32bit
네트워크간 라우팅에 사용
3계층 network layer 

- MAC 주소 48bit
LAN(LOCAL AREA NETWORK) 내의 로컬 장치 간 데이터 전송에 이용
2계층인  link layer에서 사용
NIC (NETWORK INTERFACE CARD)에 네트워크 구성하는 모든 장치에 고유하게 할당

컴퓨터 5계층 장비 
라우터 3계층 장비
스위치 2계층 장비

실제 컴퓨터들은 스위치에 연결되어 가기 때문에 IP 가 없는 스위치에서 MAC 주소를 사용하여 IP 역할을 하며 데이터를 전달한다.

**CF) IP 주소의 역할**
3계층에서 장치를 식별하고 데이터를 목적지까지 전달한다. 라우터는 IP 주소 기반으로 경로를 설정한다. 

![[Pasted image 20241216204248.png]]
앞에 3 바이트는 제조사를 식별하는 값 , 뒤에 3 바이트는 제조사 맘대로

MAC 주소는 NIC의 ROM에 저장된다( READ ONLY MEMORY)

SWITCH 는 IP 주소가 없다. FRAME 엔 송신지 MAC 주소, 수신지 MAC 주소가 있는데 SWITCH 는 FRAME을 받고 이 MAC 주소를 읽고 다음 스위치나 라우터에 보낸다.

네트워크를 이루는 장치들은 NIC를 갖고 있다. NIC엔 고유한 MAC 주소가 부여된다. 

MAC 주소는 네트워크 내에서 **장치를 식 별**하는 데 사용

- SWITCH는 LOCAL AREA NETWORK (LAN)에서 사용된다. 한정된 지역 안에서 연결된 장치들은 데이터를 주고 받기 위해 ROUTER 상에선 IP주소를 이용하고, SWITCH에선 MAC 주소를 이용한다. 

네트워크를 이루는 장치들은 NIC를 갖고 있다. NIC엔 고유한 MAC 주소가 부여되기에 LAN에 연결된 각 네트워크 어댑터(NIC)에도 고유 MAC 주소가 있다.

IP 주소를 LINK LAYER주소로 바꿔주는 ARP 방식에 대해서도 알아보자.
#### ARP 
IP 는 겹칠 수도 있다. Mac는 무조건 유일
DHCP로 ip 받으면, 3계층 이용한다는 것, 처음엔 mac 주소 모르는데 이걸 어떻게 알지? 

상대의 ip -> 상대 mac
dhcp 와 다르게 mac주소를 묻는 broadcast를 던진다? 

ARP 테이블은 각각의 HOST가 갖는다. 계속 그 MAC 주소로 통신해야한다. 알아낸 정보를 각각 노드가 ARP 테이블에 둔다. 더 빨리 통신 가능

IP - MAC 매핑 + 내 LAN SUBNET 안의 애들 것 만 저장해둠
IP MAC TTL (어느 정도 이후에 잊어버릴까 일반적 20분) 

DHCP는 사용하지 않는 IP는 반납한다. 

< 과정 >
- A : B의 IP 주소를 포함해서 BROADCAST
IP B IP
MAC FF.FF.FF.FF 로 요청
- B 가 받으면 내 MAC 주소를 담아 보냄
- A 내의 ARP TABLE에 저장 IP MAC TTL (TIME TO LIVE)

PLUG AND PLAY : 바로 쓰기 가능

##### 같은 SUBNET이 아니라 실제로 외부로 나가는 통신이 많은데 이 경우엔???
라우터 R을 거쳐서 다른 네트워크로 브로드캐스트 못 보냄
A는 나에게 붙는 라우터 인터페이스 맥 주소 붙임
![[Pasted image 20241218165312.png]]

라우터도 ARP 테이블이 있으니까 

![[Pasted image 20241218165448.png]]
IP 주소는 안 바뀌네?



ttL 어디서 나왔더라? => 라우터에서 패킷은 TTL 값이 0이 되면 라우터는 해당 패킷을 **폐기(drop)**하고, 송신자에게 ICMP(Time Exceeded) 메시지

SLOW START , THRES만나면 1개씩 증가(CONGESTION AVOIDANCE), RECOVERY(soft timeout, hard timeout)
1문제 버리기 14문제
3.5->arp
과제

##### 정리

송신자가 내 트래픽을 보낸다. loss 인지 duplicated 인지  timeout인지 
additional increase multiple decrese

방법 나올듯!

하나씩 늘려나가니까 처음 시작하는 사람이면 힘들다. 비효율적
slow start ; 보낼 수 있는걸 2배씩 막힐때까지 
1. threshold에서 막히면 ai로 중요! 
slow start 는 aimd 입각 16, 20이면 32까지 못보내고 멈춤 20부터 ai
hard time out 1부터 
soft time out 반절 threshod 부터

위에 그래프!
data plane ; ip 
input port function 
fowarding table을 보고 동작. 범위를 적어두면 힘들다. longest prefix match. 
switching 방식 3가지 보통 bus나 crossbar , memory 
==hol blocking : 나가는 포트에 동시에 여러 패킷이 접근하면 HEAD OF LINE==
스케쥴링 FIFO ,우선순위, RR, WFQ (RR베이스)

IP FRAGMENATION문제 MTU 일반적 1500. 헤더에 20 이라 실제 보내는 데이터는 그보다 작다. 계산 FRAGFLAG 데이터 끝나면 0 , OFFSET 1480/8 왜 8이지?? BIT=>BYTE로 

CIDR :  중요한가보다 

NETWORK 주소 : 내가 어떤 네트워크에 속해? => 동일하면 여기 속한거임
HOST : 그 안 몇번

192.168.0.1
192.168.0.1 같은 네트워크에 속하는지 판단하려면 subnetmask를 알아야한다. 

 classless CIDR SUBNET이 유동적이게 
 DHCP 
 NAT 내가 갖고있는 공인 IP한정적, 내부에 많은 애들을 거느려야하는데 .. 외부로 나갈땐 공인 IP 외부 내부 들어올때도 IP
 뒤에 포트번호도 결정!
 NAT TABLE에 넣어뒀다.

IPV6(폰) IPV4(컴)양이 적어서 나온개념, 패킷을 보낼때 스트림 형태 FLOW .QOS, 다룬다
혼용되니까 터널링 IPV6라우터 IPV4라우터 서로 각자끼리 통신가능하게 


안에 데이터패킷 뭐가담기는지 

5장
LINK STATE -다익 , FLOOD (각자 테이블)
DV - 벨만 소형 네트워크 위주(1개 테이블) 둘의 차이?

다익스트라 표! 실제로 어떻게 동작?
벨만 표!

만약 LINKCOST변경? 작을땐 ㄱㅊ 커질때? 죽어버림
=> 방지하는 방식 posion reverse 
버리쟝!

ospf link state 기반 방식
- 계층화된 네트워크를 사용한다. 실제로
rip dv 방식
- 


bgp쪽에서 밖으론 처리, intra에선 최소 비용따짐

6장 
crc 계산 ,1껴있음 오류
채널 분배 (2가지 배움 ) - 차이랑 특징
많이 쓰이는 random access (5가지) csma 알로하 비효율적이여서 나옴 
뭐를 해결하기위해 나와는지 cd는 충돌나면 멈춰서 ㅓloss 시간 줄임
ca - topology명확 x , ifs 주기 기다림. 2배씩 늘어난다 backkof time

polling tokenpassing 
ARP 방식
