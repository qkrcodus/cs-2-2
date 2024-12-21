##### transport layer protocol 은 다른 host의 app process간 논리적 통신을 제공한다.

애플리케이션의 프로세스로부터 수신한 메시지를 잘개 쪼개 트랜스포트 계층 세크먼트(패킷)으로 변환한다. 

각 조각에 트랜스포트 계층 헤더를 추가한다..

이후 각 헤더가 붙여진 각 세그먼트를 네트워크 층으로 보내지고 네트워크 계층 패킷(데이터그램)안에 캡슐화되어 목적지로 전달된다. 

네트워크 라우터(네트워크 층끼리 연결) 가 이를 전달하는데 데이터그램(네트워크 계층 필드)에 대해서만 동작한다. 안에 뭐가 있는진 검사 x

수신측의 네트워크 계층에서 데이터그램으로부터 캡슐화를 풀어 세그먼트를 추출하고 이를 trasport layer로 보낸다. 

이후 application layer가 이를 이용할 수 있게 수신한 segment 를 합쳐서 app layer에 보낸다.

각 socket엔 연결되는 프로토콜이 1개지만 process는 여러 socket을 만들 수 있어서 한 process는 여러 protocol을 사용할 수 있다.
	application -> 여러 process -> 각 process는 여러 packet/segment 로 구성 -> 여러 socket-protocol 생성 가능

- packet : 전송되는 데이터 단위
- socket : application 이 transport layer와 통신하기 위한 인터페이스 
- protocol : socket 과 1:1 매칭 앱 개발자가 지정

network layer - host 끼리의 연결 고리
transport layer - 다른 host 간 process 끼리 데이터 주고 받기가 가능하게 해준다.

##### 소켓을 통해 데이터는 어떤 방식으로 연결을 되는지? TCP UDP

프로세스는 여러 packet으로 분리되고 이들은 socket이라는 문을 지나 transport layer 로 내려온다. 
여기서 각 packet은 적절한 header가 붙어 network layer로 내려오고 network layer 에서 캡슐화되어 network router를 통해 다른 host의 network layer에 도달한다. 
캡슐화를 푼 packet을 transport layer에서 각 header 의 내용을 보고 어떤 socket 으로 보낼지 결정한다. 

각 segment/packet 안엔 출발지의 ip 주소와 도착지의 ip주소가 있다. (수신측에서 demultiplexing)

송신자
application layer : process 는 쪼개져서 socket으로 전달된다.
transport layer : 쪼개진 데이터에 헤더 정보를 붙여 network layer로 보낸다. (application data , 출발 port num, 도착 portnum)
transport layer : multiplexing 쪼개진 데이터에 헤더 정보를 붙여 network layer로 보낸다. (application data , 출발 port num, 도착 portnum)
network layer : 캡슐화한다. 

수신자
transport layer : demultiplexing 통해 도착 portnum 을 보고 매칭되는 각 socket에 보낸다.

socket을 생성할때, transport layer가 포트 번호를 고유하게 부여한다. 

#### connectionless UDP의 multiplexing 과 demultiplexing
host에서 UDP socket을 생성할때 transport layer에서 자동으로 포트번호를 할당한다. 아니면 우리가 할당하고 bind 해줄 수도 있다.
transport layer 에서 데이터, 출발지 portnum, 도착지 portnum을 포함한 segment을 만든다. 
network layer는 이 segment를 IP datagram으로 캡슐화하여 전달한다. 

수신된 UDP segment의 목적지 포트 번호를 확인하고 이 번호의 socket에 segment를 전달한다.

따라서 TCP와는 다르게 출발지 port number가 달라도 도착지 port num 이 같으면 같은 socket으로 데이터그램이 전송된다.

#### connection-oriented TCP demultiplexing
TCP socket은 출발 도착의 포트 번호, 출발 도착의 ip주소 모두를 이용해 socket을 식별한다. 
![[Pasted image 20241027214214.png]]
따라서 동일한 ip 주소와 포트번호로 들어오는 데이터들은 출발지의 ip주소와 포트번호 모두를 고려해 하나라도 다른게 있으면 다 다른 소켓이 뚫린다.

원래는 소켓이 뚫리면 process도 그만큼 생겨나지만, 고성능 웹서버에선 하나의 프로세스만 사용할 수도 있다. (threaded server)

#### UDP 의 최소한 노력 checksum
송신측에서 보내는 데이터들을 전부 더한다. 오버플로우는 뒤로 보내주기
이 값의 1의 보수를 취해 checksum을 만든다. 
이 값을 checksum field에 넣는다. 

수신측에서 checksum이랑 받은 데이터를 and or 연산으로 전부 0 이나 전부 1인지 확인한다. 

#### UDP 의 신뢰적 데이터 전송 원리
![[Pasted image 20241028073406.png]]
udt_send()와 rdt_rcv() 는 양방향으로 데이터를 주고 받는다.

#### rdt1.0 : 신뢰적 transfer 신뢰적 channel 

make_pkt(data) : data 커서 쪼개서 packet 만드는 거다. 
![[Pasted image 20241016172822.png]]
extract(packet,data) packet에서 data 추출한다. (packet엔 header등 다양한게 있으니)
하지만 이런 경우는 거의 없다


RECIEVER -> SENDER에게 보내는 피드백 ACK NAK 와 CHECKSUM 추가
#### rdt2.0 : 비트 오류가 있는 채널에선 checksum 으로 에러를 찾고 수신자는 송신자에게 2가지 피드백을 보낸다.
<프로토콜>
1. ACKs ( reciever 가 sender에게 패킷 오류 없어 응답) => sender 패킷 재전송 X
2. NAKs ( reciever 가 sender에게 패킷 오류 있어 응답 ) => sender가 패킷을 retransmit 한다.

###### 1.0 과 다른 점 ? 특징 2가지
=> 에러 detection을 위해 패킷을 만들때 data랑 checksum 함께 , feedback(ACK NAK)

![[Pasted image 20241016173605.png]]

##### no error 인 경우
![[Pasted image 20241016173842.png]]
##### error 인 경우
재전송 과정 => rdt_rcv()
![[Pasted image 20241016174018.png]]

2.0은 송신자의 상태가 ack 나 nak을 기다리는 상태일땐 rdt_send()를 보낼 수 없다. 
즉, ack를 받고 다시 wait for call above 상태가 되기전까진 새로운 데이터를 전달하지 않는다. 
그래서 2.0을 전송 후 대기 상태라고도 한다. 


receiver 입장에서 NAK 보냈는데 ACK로 처리 ,ACK보냈는데 NAK로 처리, ACK NAK 둘다 유실 
문제 => rdt2.1 해결 