
3장에서 transport layer에 대해 다뤘다. 호스트 사이의 통신 서비스 아래 무엇이 있고, 무엇을 동작하게 할까?

4장에선 network layer에 대해 다룬다. network layer는 호스트 사이의 통신 서비스를 어떻게 제공할까?
라우터들마다, 호스트마다 네트워크 계층의 일부가 존재한다는 사실도 알게 될 것이다. 

![[Pasted image 20241217205636.png]]

#### network layer - 3계층
역할
1. <근본> 수신과 송신 host 사이의 packet을 전달
2. segment를 캡슐화해서 datagram으로 만든 후 network으로 보낸다. 라우터는 datagram의 header를 까보고 IP 주소를 보고 어디로 전달할지 결정한다. 
3. 수신측이 datagram에서 header를 제거하고 transport layer로 segment을 보낸다.
4. network protocol은 router, 송신 수신 host에 전부 존재한다. IP 프로토콜이 대표적 network protocol이다. 
5. router 는 모든 datagram의 header를 검사해 ip 주소를 보고 어디로 보낼지 결정한다. 
6. routing : 길 찾기, fowarding : 보내기

#### ROUTER 의 중요 기능 fowarding, routing (network layer functions)

router에도 network layer protocol 이 기능을 한다.
###### - fowarding : 보내기 ( 라우터 내부의 입력 port -> 출력 port) [data plane]
router는 우선 받은 datagram을 입력 포트로 이동시킨다. 
datagram header를 까서 IP 주소를 확인한다. router 별로 있는 fowarding table을 참고하여 매칭된 출력 port로 보낸다.
###### -routing : 길 찾기 ( 라우터간 협력 네트워크 전체 ) [control plane]

fowarding : local , per-router
routing : network wide logic 

#### control plane 의 ROUTING 알고리즘 2가지 방법

1. traditional routing algorithmns : router 별 라우팅 알고리즘이 있다.
라우터별로 routing algorithmns 가 있고 이를 이용해 fowarding table을 만든다.

2. sdn : remote server에 라우팅 알고리즘
라우터 속 control plane이 중앙 컨트롤러로 이동한다. 라우터는 routing 계산 x , fowarding 만 집중한다. 라우터 속에 control agent 를 설치하고 외부 remote controller와 통신한다.
#### Network 계층에서 제공되는 service
1. 개별 datagram에 대한 service
보장된 delivery
지연 제약이 있는 보장된 delivery

2.  datagram flow에 대한 service
송신자가 보낸 순서대로 수신자에게 도착 보장
최소 대역폭 보장
datagram 사이 균일 시간 간격 보장
#### Router 구성 요소 ==4가지==

![[Pasted image 20241216164739.png]]
- 스위칭 패브릭 : input -> output 을 fowarding table을 보고 전달해주는 역할
- ==라우팅 프로세서 : routing table을 보고 전체적인 경로를 계산==
- input port 
- output port
##### router의 패킷 처리 과정
![[Pasted image 20241217130214.png]]

1. link layer를 통해 전달 받은 data packet (link header/network header/transport header/ application data)에서 ==link header를 제거==한다.
2. 라우터는 network header 속 ip 주소를 읽고 fowarding table을 이용하여 출력 port를 확인한다.( 라우터 프로세서는 라우팅 알고리즘으로 fowarding table을 만들어둔다)
3. 도착지 ip 주소를 ARP(address resolution protocol)을 이용하여 MAC주소로 바꾸고 출발 MAC 주소 도착 MAC 주소를 ==새로운 link layer header에 넣어서== 출력 port를 통해 다음 hop으로 데이터 패킷을 전달한다. 

##### input port function
![[Pasted image 20241217130214.png]]
1. link termination : physical layer에서 수신한 신호 해석
2. link layor protocol : 네트워크로 데이터를 전달하기 위해 처리
3. queue - input port memory에 있는 fowarding table을 보고 출력 port를 결정한다. input port에 많은 datagram이 온다면 queueing delay, delay loss 방식
여기서 fowarding table은 SDN ( remote server ) 나 router 속 traditional 방식인, control plane이 결정한다.

![[Pasted image 20241217211923.png]]

##### input port memory 에 있는 fowarding table => 전부 data plane 꺼
1. 방법 : destination based
ip 주소 범위 내에 정확히 일치하는 경로를 찾는다.
2. 방법 : longest prefix match 
ip 주소 범위 내 가장 길게 일치하는 범위로 찾는다.
##### ==router의 switch fabric 은 3가지 방법이 있다==
1. =='memory'==
2. bus
3. crossbar
![[Pasted image 20241217134324.png]]
memory : router cpu 에 저장된 memory로 input port ouput port 연결 , 느리다

bus : 공유 채널을 통해 memory 보다 빠르게 연결 (input queue output queue  사이 공유하는 채널 둔다)

crossbar : 1:1 연결이라 아주 빠르지만, 비싸서 bus 많이 사용한다. 

##### Input queueing - 특히 switch fabric 방법 중 bus에서 많이 발생한다.
switch fabric 속도 < input queue 나가는 속도
패킷이 input queue 에 대기해야하는 queueing delay 가 발생한다. 이로 인한 packet loss 발생

- head of line (HOL) blocking : 나가야 하는 패킷이 delay로 못나가면 queue 속 다른 패킷들이 나가질 못한다.
-  output port connection 
  ![[Pasted image 20241217140316.png]]

##### output port function 3가지 
![[Pasted image 20241217130214.png]]
![[Pasted image 20241217212700.png]]
- buffering 발생한다
switch fabric 으로 들어오는 속도가 out put buffer에서 처리하는 속도보다 빠르면 datagram packet들이 쌓인다. (buffer 크기는 유한이다)
loss 나 error 발생한다

- buffer에 대기 중인 datagram packet들을 무슨 순서로 내보낼지 중요하다 => scheduling mechanism
1. buffer로 데이터 패킷 처리
2. link layer header 붙여 datagram을 frame으로 만들기
3. link layer를 통해 송출하기 위해 신호 변환 과정

<권장 버퍼 크기>
RTT X Link Capacity  ( N은 FLOW 다)
![[Pasted image 20241217141420.png]]
10Gps ( 10 Gigabits per second ) = 1 초당 10^9 bits 전송 가능하다.

EX) C= 10Gps , RTT=250msec
0.25 s x 10Gbits/s= 2.5G bits 크기의 버퍼가 필요하다.

##### ==output buffer에서 Scheduling mechanisms 엔 3가지 방법== prwfq
1. priority scheduling
![[Pasted image 20241217143107.png]]
중요도 적은 초록색이 계속 밀려날 수도 있다. => 기아 발생

2. round robin scheduling
   여러 class 의 queue가 있다. 각 클래스의 큐를 순차적으로 방문하며 해당 큐의 첫번째 패킷을 가져온다.  
![[Pasted image 20241217143348.png]]
priority 없어서 문제 => weighted fair queueing 

3==. Weighted Fair Queueing ( WFQ)==
각 클래스에 가중치를 부여한다.
한 사이클에 모든 클래스를 방문하며 가중치에 비례하는 양의 서비스를 할당한다. 넌 패킷 4개 보내 , 넌 패킷 2개 보내, 넌 1개 보내

싼데 문제 없는 방법으로 Round Robin 많이 사용

#### IP : internet protocol

##### network layer protocol
1. ICMP ( internet connect messgage protocol) : 문제 있을 때
2. IP ( datagram을 전달하는 , fowarding table, data plane)
3. routing protocols (control plane, routing table)

그중 IP 에 대해! 
##### IP datagram의 구성
**IPv4** 헤더의 기본 크기 = **20 bytes**
MTU (Maximum Transfer Unit) : 링크에서 **한 번에 전송할 수 있는 최대 패킷 크기** 기본적 = 1500bytes

헤더 포함한 전체 패킷 크기가 MTU를 초과하면 **패킷을 분할(Fragmentation)** 해야 한다.

**TTL(Time To Live)**: IP 헤더에 포함된 **수명 값**
패킷이 라우터를 한 번 통과할 때마다 **TTL 값이 1씩 감소**

패킷이 MTU 크기를 초과하면 , 패킷을 분할한다. 
분할된 각 패킷에는 **독립적인 IP 헤더**가 추가

**라우터**는 패킷을 분할하거나 전달만 할 뿐, 재조립은 하지 않습니다.

==계산 문제!! 
=> 패드에==
![[Pasted image 20241217160616.png]]
##### IP 주소
32bit 
- network - 같은 네트워크 => IP의 앞자리 유사
- host(식별하기 위해 IP 주소 사용)- **최소 1개의 네트워크에 포함**되어 있어야 통신하고 식별에 의미가 있다.

IP의 앞자리만 보고 네트워크 같은지 확인 가능

- 만약 subnet mask /16이면?
앞 16bit를 네트워크 주소로 뒤 16bit 호스트 주소

###### CLASS 란? (고정된 네트워크와 호스트 부분)
한 네트워크에서 쓸 수 있는 IP 주소를 정하자에서 나온 개념

CLASS A 8 / 24 256개만 네트워크 주소  (it 인프라 좋은 나라)  
CLASS B 16/16 
CLASS C 24/8 256개만 호스트 주소 (it 인프라 안 좋은 나라) 
... 

이렇게 동작하는 방식을 classful 이라고 한다.  (개념만)

##### IP란? 
네트워크 상 장치와 인터페이스들을 식별하기 위해 사용되는 주소
호스트와 라우터의 인터페이스를 구별하기 위해 사용
ex) 호스트 : 유선 이더넷, 무선 네트워크 등의 인터페이스
라우터 :  수많은 인터페이스
##### subnet이란?
동일한 네트워크 주소를 공유하는 장치들의 그룹
라우터 없어도 통신 가능


![[Pasted image 20241217171820.png]]

![[Pasted image 20241217172132.png]]
라우터의 목적은 라우터끼리의 통신이다. 

##### subnet mask 을 유동적으로 설정할 수 있는 classless을 이용한다.
한 서브넷에서 호스트로 사용 가능한 ip 주소 개수가 늘어난다. 

##### 네트워크를 합치면?
classless interdomain routing CIDR을 사용하여 subnet mask를 유동적으로 사용하여 네트워크로 사용 가능한 ip 주소를 적게 설정해둔다면, 호스트로 사용 가능한 주소들이 늘어나고 불필요하게 네트워크 주소랑 브로드캐스트 주소를 설정해둘 필요가 없다.

##### 네트워크에서 식별자 역할을 하는 ip 주소를 어떻게 받을까?
DHCP 서버로부터 네트워크에 연결할 때 동적으로 IP 주소를 획득한다. 
그 과정으로 discover, offer, request, ack

특징 
- dhcp 서버가 동적으로 ip 주소를 할당한다. 일정 시간동안 임대하는 개념이기에 만료되기 전에 갱신 가능하다
- 반환된  ip 주소를 네트워크에 접속한 다른 기기에 할당 즉, 재사용 가능하다
- 계속 사용할거면 hold도 가능하다
- 모바일 사용자 지원

새 클라이언트가 네트워크에 접근한다
이때 두가지의 ip 1. network 주소 2. broadcast 주소 는 사용하면 안된다.

discover : is there DHCP (Dynamic Host Configuration Protocol) server?
offer : wanna use this ip?
request : can i use ip?
ack : sure
##### discover
네트워크에 연결된 클라이언트는 내 ip 주소를 모르니 
src : 0.0.0.0 , 68
브로드캐스트로 일단 모든 네트워크 상의 장치에게 보냄
dest : 255.255.255.255 , 67 
##### offer
src : 223.1.2.5 ,67  DHCP 서버 IP 주소, 67
dest : 255.255.255.255 , 68  -> 클라이언트는 아직 ip 주소가 없어 브로드캐스트로 보낸다.
##### request 
src : 0.0.0.0 , 68 -> 아직 ip 주소 없어
dest : 225,225.225.225 , 67 -> 브로드캐스트로 요청( 난 아직 ip 없어서 특정 지어서 못보낸다. )
##### ACK
src : 223.1.2.5 , 67
dest : 255.255.255.255 ,68 -> 브로드캐스트로 요청 ( 아직 ip 주소 안 받았우니까)

다만! 여기서 제약 조건이 dhcp 서버는 subnet 주소 내 , 브로드캐스트, 네트워크 주소 제외한 ip주소를 할당해야한다

##### DHCP의 다른 역할
1. 네트워크에 접근한 장치에게 IP 주소 할당
2. dns 서버 IP 주소 알려줌
3. SUBNET MASK 알려줌
4. 다음 네트워크와 통신할 때 router 거쳐야함 . router IP 알려줌

##### DHCP는 IP 주소를 모르는 상태로 통신하기에 TCP 대신 UDP를 사용한다

##### route aggregation , more specific route
isp 처럼 더 상위의 네트워크가 ip 주소를 광고할때 계층 별로 있는 subnet들을 하나의 큰 네트워크로 묶는다.
만약 isp 아래 새로운 isp가 들어오고 조직중 1개를 광고하고 싶다면, 조직 1의 longest prefix match를 광고하는 주소에 추가해 조직 1을 라우팅한다

##### NAT (network address translation)
다수의 디바이스의 사설 ip를 외부 내트워크의 공인 ip로 변환하는 역할을 수행한다. 이로 인해 다수의 디바이스가 하나의 공인 ip로 통신할 수 있다.

내부 -> 외부
![[Pasted image 20241218005029.png]]

외부 -> 내부
![[Pasted image 20241218005105.png]]
![[Pasted image 20241218005221.png]]


##### NAT 논란
여러 디바이스가 하나의 공인 ip 주소를 이용 가능하다. 하지만, 이는 논란거리다
라우터는 3계층만을 처리한다. 근데 NAT는 4계층의 UDP TCP정보를 이용한다. 
NAT는 사설 IP 주소들과 포트번호들을 공인 IP 주소와 포트번호로 바꿔준다.
포트번호 기반으로 매핑을 하기에 4계층의 정보를 봐야만한다!

사실 NAT가 나온 이유는 IPV4가 32bit의 주소를 갖는데 이게 부족해서였다. 공인 ip 주소들을 재사용하기 위해 nat가 필요했던 것이다

만약 IPV6 128 bit 주소 체계를 사용한다면  NAT 필요 없어지겠지만 ..

END TO END 네트워크는 데이터를 변경하지 말아야한다는 원칙을 위배

이를 해결하기 위해 NAT TRAVERSAL 기술이 필요.  ( 종단간의 통신을 위해)

##### IPV6 (과정을 알자)
IPV4는 기본 헤더는 20bit 지만, 옵션 필드가 추가되면 늘어날 수 있다. 유동적이다. 
근데 IPV6의 헤더는 40bit로 고정이다.

기기 종류가 늘어나는데 32 bit 주소로 공인  ip 부족 문제가 나타났다.
과거엔 텍스트 기반 -> 영상 음성 등 대용량 데이터 전송이 일반적. 
qos : 우선순위 정해 중요한 데이터부터 빠르게 처리
ipv4는 라우터별로 3계층 checksum 이 있었는데 이를 없애고 end to end 4계층에서 오류 검사

IPV4에서 IPV6으로 바로 호환되지 않는다 .공존을 해야하기 때문에 생긴 TUNNELING 이라는 기법
##### TUNNELING
ipv6은 ipv4 없는 것 처럼 생각
IPv6 패킷을 IPv4 패킷의 **데이터 부분**에 캡슐화하여 IPv4 네트워크를 통해 전달.


