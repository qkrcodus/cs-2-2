![[Pasted image 20241029170021.png]]
#### transport layer 원리중 multiplexing 과 demultiplexing에 대해 알아보자

##### transport layer sevice의 주역할
- 다른 ==host 에서 동작하는 process 간== logical communication (by ipaddress port#)
- 송신 : message를 packet로 쪼갬 + 알맞는 header 파일 붙여서 segment(캡슐화) =>network layer
- 수신 : segments를 합쳐 message로 => app layer
- 사용 프로토콜 tcp udp
##### network layer
- 다른 ==host간== logical communication (길 만들어주는 역할 : 보내주기 담당)
##### transport layer 
- 다른 ==host 에서 동작하는 process 간== logical communication ( 명칭은 transport지만 보내주기 담당 X )

사용 프로토콜 tcp udp
- 신뢰적 0 tcp (best effort+상호 연결,혼잡함 관리) -> 더 신뢰적
- 신뢰적 x udp (best effort) -> 데이터 빠르게 주고 받는 사이에 사용
- 따라서 지연 delay이나 속도 bandwidth unavailable

##### multiplexing 과 demultiplexing
SENDER : 여러 개의 process , 여러 개의 sockets가 1개의 렌선으로 나가야 한다. (multiplexing)
RECEIVER : 하나의 렌선으로 들어온 segments을 여러 개의 socket에 분배한다. (demultiplexing)
![[Pasted image 20241011124255.png]]

process마다 socket ㄴㄴ

![[Pasted image 20241029172610.png]]
#### connectionless UDP의 demultiplexing (==도착지 포트 번호 같으면 같은 socket으로==)
- source IP 주소가 달라도 dest port number가 같으면 같은 socket으로 demultiplexing 된다. 
host 는 UDP socket을 생성할때 IP datagram을 받고 여기엔 (출발 포트 번호 , 도착 포트 번호,)가 있다.
이를 이용해 segment 위에 출발 포트 번호 , 도착 포트 번호 올려줌
![[Pasted image 20241011124531.png]]


#### connection TCP (socket간 통신?) demux (==출발지 포트번호 다르면 다 다른 socket으로==)
- socket 에 4가지 조건이 알맞아야 (출발 ip 주소와 포트 번호 , 도착  ip 주소와 포트 번호)같은 socket으로 들어옴
- 80 port https 같은 포트번호여도 출발 ip 주소가 달라 여러 개의 socket이 생겨 여러 개의process 뚫림

=>여러 socket이 1개의 process 로 threaded server(서버 부담 감소)

![[Pasted image 20241011131101.png]]



