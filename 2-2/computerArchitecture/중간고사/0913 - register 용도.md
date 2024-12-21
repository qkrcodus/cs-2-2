연산, 이동 명령어

저수준 언어 : 구조에 의존적

고급 언어 -> (컴파일러) 어셈블리어 -> 기계어 (어셈블러) 1:1

ISA 가 사용하는 어셈블리어의 명령어의 피연산자는 register or memory

byte ordering : 메모리를 높/낮 주소 중 어디서부터 쓸지

RISC-V 구조 
1.연산
2.이동( load : memory => register , store: memory <= register )
3.제어 (go to, 조건문 반복문, 함수 호출)

RISC-V  register
32개의 register와 1개의 program counter 

program counter : 내가 실행할 명령어의 현재 위치 주소를 지정
memory (data , 실행할 코드)

100개의 프로그램을 돌린다 => 100개의 컴퓨터 => os는 모든걸 저장
실행이 memory?


900 높은 메모리 윗 주소 : stack top , stack pointer
stack 
10000 stack bottom : frame pointer

heap

cpu 는 32개의 register만 갖고 있기에
caller-save register
호출하는 애가 자기 정보 저장하기 위한 register
함수 a  c=a+b 함수 b
함수 b  c=a+b 

연산자는 register or memory

연산할때 register memory add a3 a0 (a1) x 
메모리 접근은 load store만  load storearchitecture

register 32개 5비트 필요함 

RISC-V 에서 데이터를 가르키는 방법=addressing 4가지 
Mem[] load 나 store에서 사용
constant
(ao)
pc+imm

register 나 상수만이 risc-v 어셈블리코드의 피연산자

imm 은 12비트만  
register 는 32비트라 signext (양수 상위 20비트 다 0 , 음수 상위 20비트 다 1로 채운다 )

sign 이라 
0 +
음수 값 2보수 +1

unsigned

상수 들어가면 다 immediate
 