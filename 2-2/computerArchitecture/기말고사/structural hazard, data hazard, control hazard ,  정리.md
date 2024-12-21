##### structural hazard 정리
돈 투자하면 된다
##### data hazard 정리
read after write 을 해결하기 위해서
###### 1. stall
3cycle 쉰다
###### 2. register file
하드웨어적 consistency는 유지하되, perfomance는 향상
register file rising 일 때 쓰고 falling 일 때 읽기
2cycle 쉰다
###### 3.foward
하드웨어 추가 datapath 랑 그 path control 할 combinational logic 추가
alu 의 결과를 WB단계에서 register에 쓰기 전에 다음 명령어의 alu 입력에 넣어준다.
(ex/mem 파이프라인 레지스터에서 데이터를 전달한다.)
###### 4. compiler로 순서 바꾸기

- 하지만, fowarding으로 해결 못하는 경우가 load 의 destination이 그 다음 명령어의 source인 경우다. => 1 stall 

lw $t0, 0($s0)    # $t0에 메모리에서 값 로드
add $t1, $t0, $s1 # $t0 값 필요

이 경우엔 레지스터의 담기는 값은 MEM 단계를 거쳐야 유효해지므로, 이 값을 레지스터에 fowarding하는 것 불가. 
##### control hazard
data hazard보다 빈도는 적지만 퍼포먼스 측면 성능 저하 크다.
분기문 : 분기 여부에 대한 결정이 ==mem 단계==에서 결정된다. 
if id ex mem wb 에서 decoding하고 3 cycle 뒤에 다음 pc값이 정해진다. 
3cycle stall은 너무 오래 걸린다. 
###### 1. 분기 not taken 이라고 가정
만약 mem 단계에서 분기가 taken이면 if id ex 단계에 있는 명령어들을 버린다. flush
###### 2. branch prediction buffer로 최근 분기한 pc저장
###### 3. reduce branch penalty
###### 4. dynamic branch prediction 1 bit 2bit prediction

ex) 분기가 일어나지 않는다고 예측, 분기가 일어난다고 예측, 동적 예측의 세가지 분기 예측 방법을 생각해보자
이 기법이 제대로 예측할 경우 손실은 0 예측 실패한 경우 손실이 2cycle이라 가정하자.
동적 예측기 평균 정확도가 90%라면, 어떤 예측기가 가장 좋은 선택인가?
1. 5% 비율로 분기가 일어나는 조건부 분기
2. 95%비율로 분기가 일어나는 조건부 분기
3. 70% 비율로 분기가 일어나는 조건부 분기
![[Pasted image 20241210145056.png]]

Taken을 예측했는데 Not Taken이면 왜 1stall 이지?
read write 권한
7단원 마지막

