##### POP3 protocol
authorization 단계, transaction 단계

transaction 단계 속 pop 이라 삭제 dele 있다.
![[Pasted image 20241004122555.png]]

###### IMAP 
메시지를 한 큰 곳에 ; 서버에 저장
##### DNS(domain name system) 는 도메인 네임을 IP주소로 바꿔주는 역할을 한다.
회사 - 고유 IP (static)
data 식별 위해 IP(32bit) 주소가 필요. 이 긴 걸 매번 알기 힘드니
도메인 서버에 1000.1000.1000.1000 은 네이버 서버야 라고 저장함. (클라이언트 저장 X)

DNS는 많은 이름 서버(name servers)의 계층 구조로 구현된 분산 데이터베이스

DNS는 application-layer protocol : 쿼리하면 IP를 던져줌 (5단계)
1. 사용자가 url 입력
2. 브라우저가 로컬 dns 서버에 도메인 이름을 ip 주소로 바꿔달라고 요청
3. 위 요청을 처리하기 위해 로컬 서버는 캐시 확인 후 없으면 루트 서버에게 쿼리를 보냄. 최상위 도메인 서버의 주소 -> TLD
4. TLD 서버 로컬 dns 서버는 tld에 접근하여 해당 도메인의 권한이 있는 서버 주소를 받는다.
5. 로컬 dns 서버는 권한이 있는 dns 서버에 접근하여 최종적으로 요청한 도메인 이름의 ip주소를 받아 사용자에게 전달한다.

역할 
	1. 사용자가 요청한 문자로 된 도메인 네임을 IP 주소로 바꿔줌 
	2. host aliasing (canonical :정식 (naver.com) , alias names : 별칭(네이버))
	3. mail server aliasing (@naver.com 도메인 = 메일 서버 IP)
	4. load distribution 회사가 여러 IP 가짐 (한 도메인이 여러 IP를 가질 수 있다.) -카카오톡 화재처럼 한 서버가 꺼져도 돌아갈 수 있게 

##### 왜 DNS 분산? ROOT DNS 서버의 부하를 막자
트래픽,어디에 둘 건데?,유지 보수,실패 위험 나눔 부담

해답=> DNS를 계층화하자.
![[Pasted image 20241004125532.png]]
전 세계에 13개 root name server 존재한다.

ROOT DNS server - TLD (top-level domain) server - authoritative server
##### local dns server는 계층에 포함된건 아니지만, host 요청 제일 처음 받는 서버임
###### iterated qurey(연속적 방식) 서버 : 나 dns.poly.edu는 모르는데 딴 서버한테 가봐
![[Pasted image 20241004130434.png]]
###### recursive query (재귀적)
- 이 방법이 iterated query 보다 덜 효율적이다. 
- root DNS 에 부화는 여전
- ![[Pasted image 20241028235546.png]]
###### DNS 에도 cache 있다. 
cf) http에도 일 부담 덜기 위해 cache 있었음.
cache 가 out-of-date일지도? 근데 알빠노임
###### DNS records(이 정도만 알자)
type-A: name-hostname , value-ip 주소
type-CNAME : name 에 canonical name 있음

![[Pasted image 20241029000306.png]]





