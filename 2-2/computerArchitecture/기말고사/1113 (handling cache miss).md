- average memory access time = L1 hit time +L1 miss penalty

L1 : instruction , data cache 구분
CACHE 구분 안 됨 : unified cache
Main Memory : instruction data 구분 안됨

- 계층적 메모리 사용 이유? average memory access 시간을 줄이기 위해서

CPU는 항상 주소를 요청한다. 
메모리에 들어있는 명령어는 전부 짝수다. 

#### Instruction 의 사이클 
fetch decode execution memory writeback 여기서 메모리에 접근하는건
- fetch => instruction memeory
CPU가 pc 값을 주면서 pc에 있는 instruction을 요청한다
무조건 read request =>pc

- memory => data memory
load 나 store 명령어 일 때
read(load) 일 수도 => 레지스터에 저장된 주소 값 +offset
write(store)일수도  => 레지스터에 저장된 주소 값 +offset
	ex) LOAD R1, 100(R2)  STORE R1, 100(R2)
#### CACHE MISS (Instruction miss, Data miss)

1. CPU 는 메모리 주소를 BTYE 단위로 요청한다. 
2. 만약 그 메모리 주소가 Cache에 담겨 있다면 Hit, 없다면 Loss 
3. Loss 발생하면 pipline에 영향
4. Instruction miss, Data miss 났는지 따라 대응 방법이 다르다

LOSS 발생하면 L1 아래 L2,... 있는지 몰라 그냥 lower level memory에 주소를 요청한다. 

1. **명령어 미스 (Instruction Miss)**
- 현재 PC- 4를 메인 메모리에 보낸다
- 메인 메모리에서 명령어를 읽는다.
- 읽은 결과를 캐시 entry에 저장한다.
- 결과를 cpu 에 보내고 명령어를 다시 시작한다. 

2. **데이터 미스 (Data Miss)**
- 파이프라인 중지
- 메인 메모리에서 데이터 읽음

##### CPU 속 cache를 어떻게 만들까?
고려할 점 3가지
1. data placement
2. data replacement
3. write policy

1. data placement
memory 에서 data 가져와서 cache 속 어디에 적을까?  

2.  data replacement
cache 에서 같은 위치로 매핑 된 memory를 어떻게 다룰까? 어떻게 쫓아낼까?

3. write policy
write 요청을 받았을 때, cpu 는 memory에 쓰겠다는거다. 
cache가 모았다가 쓰는 방법, 요청 올 때마다 쓰는 방법이 있다.

##### 1. data placement
1. direct mapped
2. fully associative
3. n-way set associative  ( 요즘 )

##### a )Direct Mapped Cache






