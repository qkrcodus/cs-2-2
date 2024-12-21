
- error detection : 추가적 방법 하나
- correction까지
- 외부 isp업체 kt 나 skt에 빌려서 사용 중인데, broadcast channel을 공유 multiple access 해서 쓰는데 어떻게 그게 가능하냐?
- 맥주소 : layer2 의 주소
- LAN(local area network) 중 ethernet (현재 사용하는 거) 이랑 VLAN (virtualization 가상화 LAN)

node : 호스트와 라우터
node끼리 잇는게 link

1. wired link 
2. wireless link
3. LAN (뭉탱이)

message
segment
datagram
frame이라 불리는 패킷이 layer 2의 맥 주소 존재

##### layer 2 패킷 이름, frame 의 역할
1. datagram에 header와 trailer를 붙여 캡슐화
2. channel : 공유하는 선로를 어떤 매체로 ? 
3. MAC 주소 frame header에 붙어 source dest 를 구별하게 해줌. IP 와는 다름
4. 인접 노드 신뢰적 통신 - base : RDT 보조 역할이 layer에
애초에 데이터 변화할 확률 적은 low bit error link엔 굳이 X
무선 링크엔 에러 많이 남 사용 o
5. 상대의 버퍼에 남은 데이터 크기, delay loss를 방지하게 수신 측에서 야 그만보내 메시지 보내면 송신 그만 보냄 - flow control 
6. error 감지- checksum crc 프리티? 
7. error correction - 그동안 에러 났으니 재전송해줘. 근데 이러면 재전송 시간 추가됨
8. half duplex : 무전기 full deplex : 전화기  (나 줄 때 기다리지 않음) 선택 가능

##### error correction
EDC : 에러 확인하는 애 
오차 있음
##### parity checking 하고 error detect & correct
찾을 수 있음
![[Pasted image 20241211165628.png]]

##### checksum - error detect

##### CRC  - error detect 나누기 주의하셈
더 강력 
G - 에러 검출 생성자 + D로 에러 검출 코드
송수신에서 서로 만들고 공유하고 사용

G : 4자리라면 이건 r+1인거임 r=3
D 뒤에 r 즉, 3개 0 붙임

xor : 같으면 0 다르면 1 
modulo 2 로 결과 가 0이면 에러 없는 것
![[Pasted image 20241211170841.png]]
이후에 여기 R는 r이랑 다른거임
![[Pasted image 20241211171044.png]] 
shift 연산 기능 
n 이 몫임 
D + R 에 modulo 2 == 다시 xor 해라 
D+R는 
![[Pasted image 20241211170920.png]]
![[Pasted image 20241211170927.png]]
굳이 식 안 외워도 됨. 계산 방법만 알자

##### multiple access links protocols
1. ptop : 1 대 1 
PPP(p2p protocol)로 동작 
ethernet switch - host 나
host - host 

2. broadcast 공유 (더 중요)
예전에 사용 - 하나의 버스 망 여러 개로 빠져나가는 형태 
shared Radio Frequency - 주파수 공유
유선 - HFC
무선 - LAN

- 공유망 아무나 막 사용 가능하냐?
노드가 2 개 이상 signal 받으면 충돌

해결 해주는 애가 multiple access protocol by 
##### 분산 알고리즘 
protocol 자체가 함 누가 해주지 않음 왜 이렇게? 더 싸서

- 관리자도 없고
- clock slot 동기도 안해줌

방법 3가지
1. 채널 쪼개기
a) 일정 시간 별로 TDMA -일반적
round 별로 사용 가능한 시간 설정
쓰이지 않는 slot은 다른 애한테 주지 않고 냅둠 (round robin이랑 다른 점)

왜 이렇게 함?
	 2 턴 됐을 때 나중에 어차피 충돌남

b) 주파수 FDMA
쪼개진 주파수 : band 이를 분배
안 쓰는 애들 있어도 쪼개진 주파수 변함 X
못쓰는 주파수 있음 : 모호 대역(가드 밴드) , 이거 빼고 나눠서 TDMA 비해 효율 떨어짐

c) 코드별로 
할당 받으면 노드는 이를 독점적으로 사용 -> 충돌 x

2. 랜덤 접속
분리 x 충돌도 허용하는데 이를 복구함

3. take turns0




