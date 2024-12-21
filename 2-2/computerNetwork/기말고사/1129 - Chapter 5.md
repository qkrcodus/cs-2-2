![[Pasted image 20241129121858.png]]
- link state : flooding 으로 모든 노드가 모든 정보를 안다. 한번에 전부의 내용을 받고 작업을 수행
- distance vector : 이웃한테 받은거로 전달해서 채우니까, 이웃한테 받은 거 + 내가 이웃한테 가는데 필요한 것
##### Distance vector
- 벨만 포드 

![[Pasted image 20241129121351.png]]

![[Pasted image 20241129122225.png]]
wait : 왔으면 , 다시 dv 추정값을 만든다 recompute , notify neighbors

연속적이고 비동기적 asynchronous : 작업을 블로킹 하지 않는 것 인다.

![[Pasted image 20241129122720.png]]
x : y 거쳐서 z 로 가라
y : 그대로
z : y 거쳐서 x 로 가라

Link State는 테이블이 다 다르다.
근데 
Distance Vector 는 테이블 전부 같다. 

###### 왜?? 가장 큰 차이점
LS 각 노드에서 독자적으로 수행하기에 테이블이 다 달랐다. 
DV 모든 테이블이 같은 상태를 유지




##### link cost changes 
<작게 바뀌는 경우>




##### Making routing scalable : 확장성을 고려해야한다. 

모든 라우터가 동일했고
네트워크가 flat 하다는 가정?

라우터도 계층화해야한다.  이유는 
- 1. scalability( 확장성 ) 2. 관리적 자율성

##### link state와 distance vector 차이
- link state : 모든 노드의 내용을 flood해서 알리고 시작 . 한번에 전부 받고 시작 , 기준 노드 따라서 경로가 다르다. 각 노드가 독자적으로 처리한다.
  
- distance vector : 모든 노드의 내용 알 필요 없다. 이웃한테 받은거 + 내가 이웃한테 가는데 필요한 것
  기준 노드 별 모든 테이블이 같은 상태를 갖는다.

=> key idea : 매시간마다 node들이 이웃들에게 distance vector 추정치를 알려준다. 

##### distance node의 단계 ( 이 과정이 연속적이고 asynchronous이다 )
wait : link cost가 변경되거나, 이웃에게 받는다.
recompute : 새로 만든다
notify : 알린다

- 분산 처리 가능하다. dv가 바뀌었을때 각각의 노드들이 분산 처리하며 알리기 가능하다.

![[Pasted image 20241218155922.png]]
![[Pasted image 20241218160755.png]]
link cost changes