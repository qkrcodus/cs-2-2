##### network layer - 4계층
역할 : host 간 통신

기능따라 **data plane control plane**으로 나눈다.

- routing : 길 찾기, fowarding : 패킷 들어온 걸 보내기

network layer 에 host 뿐만 아니라 router도 포함

segement 캡슐화해서 datagram 으로 네트워크로 보냄

router는 datagram의 header를 까보고 IP주소를 보고 어디로 전달할지 결정한다.

###### router의 중요 기능 fowarding ,routing ,store and foward
- fowarding : 보내는 것 
담당 : data plane ( 4장 )

local ( 내부적으로 ) per-router ( 라우터 당 ) : datagram이 router로 들어오면 어디로 내보낼지 결정 

- routing : routing algorithmn으로 길 만드는 것
담당 : control plane ( 5장 )

#####  control plane
1. traditional routing algorithms
2.  Internet Control Message Protocol
3.  network management

###### traditional (per router)
###### logically (중앙이 관리)

###### routing protocol  목표 : 적은 돈으로 빠르고 최저 혼잡도로 경로 찾자
싸고 좋아서 ethernet 쓰는데 router에서도 시간, 속도라는 비용 고려한다.

보내고 받는==host 사이 여러 router 들이 상호작용아래와 같은 형태가 topology==


![[Pasted image 20241127165040.png]]

cost 는 유동적임 이 routing algorithmn으론 뭘 하지???

[글로벌] topology : 라우터 구성 형태 완벽하고 , link cost 정보 있다.
==link state algorithms==

[탈 중앙화] 전체 정보는 몰라도 이웃 정보는 안다 
==distance vector algorithm==

[static , dynamic] 고정,유동

- static : 관리자가 직접 설정해줘야해서 change slowly over time
- dynamic : 알아서 할거니까 빠르다


##### link state algorithms
- 라우터 구성 형태 완벽하고 , link cost 정보 있다.
- 근데 이걸 어떻게 알고 있는데? 
FLOODING 방식의 프로토콜이 존재한다. broadcast 하면 나랑 연결된 모든 애들에게 내 정보를 준다. 
모든 라우터가 flooding 하면 고르게 정보 전달

C (x,y) : cost
D (v) : destination
P (v) : 이전 값

- 가장 대표적으로 다익스트라 알고리즘 
routing algorithmn => fowarding table 만든다. 
출발지 정하고 나머지 노드까지 경로 전부 계산 

![[Pasted image 20241129120737.png]]

K 개 노드 => K 번 동작해서 ==K개의 라우터마다 fowarding table 갖는다.==

시간 0(n^2)~0(nlogn)