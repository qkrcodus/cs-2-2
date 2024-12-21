#### network layer - 4계층
역할 : host 간 통신

기능따라 **data plane control plane**으로 나눈다.

routing : 길 찾기, fowarding : 보내기

network layer 에 host 뿐만 아니라 router도 포함

segement 캡슐화해서 datagram 으로 네트워크로 보냄

router는 datagram의 header를 까보고 IP주소를 보고 어디로 전달할지 결정한다.

###### router의 중요 기능 fowarding ,routing ,store and foward
- fowarding : 보내는 것 
담당 : data plane ( 4장 )

local ( 내부적으로 ) per-router ( 라우터 당 ) : datagram이 router로 들어오면 어디로 내보낼지 결정 

- routing : routing algorithmn으로 길 만드는 것
담당 : control plane ( 5장 )

네트워크의 wide logic 
##### ==방법 1. tranditional routing algorithmn 2. SDN (SDNXXXX)==

##### per - routing control plane & data plane
![[Pasted image 20241108122955.png]]
라우터 별 routing algorithm 이 있고 이를 통해 fowarding table을 짠다. ( 라우터끼리 각자 역할)


###### 논리적 centralized control plane SDN은 어떻게 동작하나?
각 라우터에 CA : control agent 를 설치한다. 
Remote Controller 가 CA 정보 받아와서 routing algorithmn 역할 

control plane이 라우터 위로 빠짐 , 라우터는 이제 길 찾기에만 충실 ( 길 만들기 x )
![[Pasted image 20241108123249.png]]

##### Network service model
1. 보장된 delivery ( ?? best effort은 어디로 ?? 라우터 장비 내 guarantee다 )
2.  flow : 순서대로, 최소한의 bandwitdth , 교환 과정 처리?
뒤에 더 자세히

![[Pasted image 20241108123803.png]]
이거 그닥 자세히 x

L/N/T/D 
가 라우터에 들어오면 N/T/D 로 도착한다

경로의 결정은 Network header에 IP 주소를 확인하고 보낸다. 
Network 을 까서 확인하면 남는게 T/D
확인하고 table 보고 다시 정보 묶어서 N/T/D ( 내부적 정보 교환 )

![[Pasted image 20241108124224.png]] 
여기서! 경로 만든게 아니라 확인한거니까 - data plane

##### 들어오는 router
![[Pasted image 20241108124823.png]]

line termination
line layer protocol
queue - input port memory 에 있는 fowarding table과 header 속  IP를 보고 어디로 가 지시
        이 과정이 있으니까 queueing delay 가 있는 거고 이로 인해 loss가 있다.
         destination - based : 위 방식
         generalized fowarding : 소프트웨어적으로 길 찾기 지정됨 - SDN


##### input port memory 에 있는 fowarding table
8bit의 IP 주소
위 : 
아래 : 범위 

![[Pasted image 20241108125148.png]]
왜 안쓰냐??

그래서 longest prefix matching으로 확인

![[Pasted image 20241108125707.png]]
and 연산 하면 

![[Pasted image 20241108125730.png]]
0 link 
1 link

table 만드는 애가 routing algorithmn

tcam 안함

#### switch fabric
input buffer와 output buffer의 연결 지점

방법 3가지
1. memory
2. bus
3. crossbar

input 이 N개면 ==line도 N개== output 도 N개

- via memory ( 전통적 방식 ) => 느리다
라우터 담당 cpu에 연결된 memory가 알려준다. 
![[Pasted image 20241108130819.png]]
input port  -> memory : router cpu -> output port

- bus - channel 선로 공유 
![[Pasted image 20241108131045.png]]
bus 속도 빠를 수록  , channel 용량 클 수록 빠르다.
메모리 들르는 것보다 빠르다
input queue output queue 사이 공유하는 선로를 둔다.

- crossbar
(0,0)      좌표     
(0,1)
(1,0)
(1,1)

처리 속도는 빠르나, 
이 방법은 1:1 비싸서 bus 형태로 많이 쓴다. 비싼 값 한다.

###### 정리
network layer - control plane data plane
control plane 1. tradition 2. SDN
들어온 header IP 를 fowarding table에서 확인한다 . 그 방법으론 longest prefix 
(주로 xor 연산으로 같으면 0 다르면 1)
보내는 통로 switch fabric 방법 3가지