
sample RTT : 실제 들어온 데이터 -> 병목 현상 등 외부 요인으로 편차가 워낙 크다 보니까 대체적으로 어느 정도대의 대역에서 들어오는지 estimated RTT를 측정해서 timeout interval을 구했다.
언제 만나?
![[Pasted image 20241101121807.png]]

estimated RTT : 평균값을 찾기 위해 기존 estimated RTT에 가중치를 더 잡고 구한다. 
![[Pasted image 20241101121611.png]]


TCP ACK generation 생략
#### TCP fast retransmit (timeout 걸리기 이전에 재전송)
재전송하는 이유? 
timeout 생기면 loss를 확인하고 재전송한다.

![[Pasted image 20241101121745.png]]
예상보다 늦게 들어오는 경우

일반적 go back N : N부터 그 이후 패킷 다 다시 전송한다. 계속 ack N-1 받는다. 이를 해결하기 위해서
timeout 걸리기 이전에 재전송하는 방법?
- duplicate ACK -> detect loss segment 안다.
- 만약 sender 3개 duplicate ACK 받으면 unacked 한 가장 작은 seq # 가진 segment 재전송한다.
![[Pasted image 20241101122343.png]]

timeout 쓰이는 경우도 물론 있다.

loss 가 일어나서 재전송하는게 아니라, timeout 으로 재전송하는거라면? 
1. flow control 2. congestion control

#### TCP flow control - receiver가 ( congestion control - sender가 )

receiver의 버퍼가 overflow되지 않게 receiver 가 rwnd를 보낸다. 
이는 수신 버퍼에서 application 이 필요해서 데이터를 지우고 가져갈 때 업데이트 된다.
sender가 보내는 데이터의 양 inflight( sender가 보내서 네트워크 상 있지만 receiver의 ack를 받지 못한 데이터) 이랑 속도를 제한한다. 

어떻게?
![[Pasted image 20241101123526.png]]

rwnd(남은 공간) : receiver 사용 가능한 공간 , receiver 가 rwnd를 TCP 세그먼트 헤더에 포함시켜 sender에게 전달해서 sender가 그에 맞는 데이터를 보낼 수 있게 한다.
###### 가져가는 놈  application
data 는 TCP 소켓 버퍼에 있다. application이 필요할 때 버퍼에서 data지우고 data 가져온다. => like pop

![[Pasted image 20241101141050.png]]
민트 - header 노란 - 데이터

##### TCP congestion control을 보기 전에 connection management 더 알아보자

connection - TCP handshake ( listen , connect , accept )
![[Pasted image 20241101124509.png]]

##### 왜 3 way handshake 인가?
2 way handshake 안되는 이유
![[Pasted image 20241101124918.png]]

##### 2 handway 오류 시나리오
오류 시나리오 1 
서버에 들어온 연결 요청이 시간 지연으로 온 것이라면 TCP 통신은 이전에 이미 끝난거라 연결을 확립해주면 안된다! 
![[Pasted image 20241101141857.png]]

오류 시나리오 2 
![[Pasted image 20241101144313.png]]
##### 3 way handshake 는 오류 시나리오 다 피해간다.
면담 할래
그래 몇시에 와
네 

synbit 연결 돼?
syn 에 대한 ack를 보내줌
ack로 데이터 보낼게 응답 

syn , synack , ack

![[Pasted image 20241101130001.png]]

tcp 니까 create new socket 한다. 소켓에 추가적인 정보 담는다. 1:1로 

![[Pasted image 20241101125942.png]]
##### TCP close connection : 소켓 없애기 없애기 전엔 data 주고 받을 수 있다.

 syn 처럼 3번이 아니라 2번 
종료 요청하는 놈은 client 다. ack 와 함께 FIN 전송

서버 나 종료 준비 끝 ( 종료 5분 남았다 ) => FINbit =1 보내고 no longer send data
client ACKbit=1 보내는 건 아무것도 안 하는 것
마무리 5분 시간 동안 컴퓨터 사용 가능 TIMED WAIT
![[Pasted image 20241101130439.png]]




