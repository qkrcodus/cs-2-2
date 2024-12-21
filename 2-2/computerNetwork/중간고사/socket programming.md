##### 소켓, 네트워크를 통해 입 출력을 위한 요소 5가지
프로토콜
출발지 IP 주소
출발지 PORTNUM
도착지 IP 주소
도착지 PORTNUM

##### 소켓이란?
네트워크를 통해 입 출력하기 위한 OS controlled interface( 서로 다른 프로그램을 연결하는 관문)
- soket 의 위치는?
application - socket - transport
![[Pasted image 20241026165841.png]]

![[Pasted image 20241026172100.png]]
process = application  ( 카톡, lms, 인스타)
##### 포트번호란?
<과정>
	개발자가 application을 만들고 포트번호를 부여한다. application은 socket을 생성하고 소켓을 통해 os 에 데이터를 전송한다. 
	os는 1 포트 번호 = 1 프로세스로 인식하기에 bind 하는 과정에서 포트 번호가 겹치면 문제가 생긴다.
	
통신에서 사용되는 프로그램들의 이름표
어떤 process에서 데이터 나가는지 알아볼 수 있게 process를 구분 짓기 위한 번호

ex) 웹은 포트 번호가 80 이다. (well known port )

##### 데이터 저장 방식
![[Pasted image 20241026172912.png]]

우리 CPU 호스트 데이터 저장 방식은 리틀 인디언이다. 

네트워크 저장 방식은 빅 인디언이다.

변환이 자동적으로 되지만, 구현 방법에 따라 추가적 변환이 필요할 수 있다. 


##### 프로토콜이란? => 통신의 방식
TRANSPORT LAYER에서 사용하는 프로토콜 2가지
1. TCP [연결지향형]
   연결이 파랗게 먼저 이루어지고 ( 파이프라인 ) 확인이 되고
   메세지 경계가 나뉘고
   순서대로 수신되며
   에러가 생기면 다시 보내달라고 요청한다.
   ![[Pasted image 20241026174232.png]]
##### c언어 tcp
프로세스에서 socket을 만들고, os에서 이를 이용할 수 있게 bind한다.
클라이언트 연결될때까지 listen

#### java tcp
serversocket 만들 때 bind 포함 : listen 하는 소켓이다. => 여러 클라이언트에서 오는 거 기다리는 용도
커넥션 소켓이 : 클라이언트와 송수신하는 용도의 소켓

listen -> connect -> accept ( 3 way hand shaking)


2. UDP [비연결 지향형]
![[Pasted image 20241026232512.png]]



코드를 짜보자.
![[Pasted image 20241026233050.png]]
