##### network layer 목적은 host 간 통신, router, host 전부 여기서 중ㅇ ==
control plane 은 길 찾기 역할을 한다.
routing protocol엔 두 가지 방법이 있다. 
1. link state - 다익스트라
2. distance vector - 벨만포드

##### link state - 다익스트라 가장 빠르게 가장 싸게
topology, node, link, link cost
flooding ; ㅣlink state broadcast 으로 모든 topology가 link cost을 안다. 
![[Pasted image 20241218153957.png]]
모든 노드 기준으로 계산한다. 

##### 계층적 라우팅의 한 방법으로 AS를 설정한다.
네트워크는 커질수록 모든 라우터가 모든 정보를 알 필요가 없기에 , 네트워크를 계층화한다.
- LINK STATE 문제 => 모든 라우터가 모든 정보를 알아야하기에  FLOODING으로 시작, 하지만 이 방법은 노드가 많아질수록 복잡해짐. 굳이 모든 노드의 정보를 알 필요 없기에 범위를 설정하는 것이 중요
- DISTANCE VECTOR 문제 => 노드간 거리를 업데이트하는 방법이다. 이 방법은 COUNT TO INFINITY문제가 생길 수도 있다. 

독립된 그룹이나 네트워크를 AS라고 하며, AS는 자체적인 프로토콜과 라우팅 정책을 사용한다. DOMAIN
- INTRA : 내부적 ( 나가는 통로는 GATEWAY 라우터), OSPF RIP 같은 프로토콜 사용
- INTER : 서로 다른 AS간의 통신 , BGP(border gateway protocol)사용