 퀴즈 문제
 
 1주차
 CNCF : cloud native computing foundation 
클라우드 최대 점유율 3사 :  aws azure google cloud

2주차
- cloud 기본 특징 5가지
![[Pasted image 20241019170548.png]]
- 유용성 4가지
ex) cloud 기본 속성 특징 중 틀린 것은 ? 틀린 것을 고쳐 써라
![[Pasted image 20241019170832.png]]
- edge-computing 
![[Pasted image 20241019171031.png]]
![[Pasted image 20241019171047.png]]

- ai ml 클라우드 기반의 트렌드 aiops mlops라는 용어가 생겨났다. 
- 용어 정리 (o,x 문제임) (3-16pg)
![[Pasted image 20241019171316.png]]
![[Pasted image 20241019171340.png]]
![[Pasted image 20241019171435.png]]
![[Pasted image 20241019171447.png]]
![[Pasted image 20241019171525.png]]


![[Pasted image 20241019171609.png]]
![[Pasted image 20241019171938.png]]
![[Pasted image 20241019171958.png]]


![[Pasted image 20241019172015.png]]


public private
- iaas pass sass (정의와 개념을 확실하게 익혀야 한다 ) 
ex ) paas 정의 서술하라
![[Pasted image 20241019172434.png]]
![[Pasted image 20241019172620.png]]

- faas cass 안씀 
기본적으론 casspass 같이 가져간다.
![[Pasted image 20241019172913.png]]
![[Pasted image 20241019172955.png]]


- multi cloud hybrid cloud  public cloud private cloud 차이점과 장점
![[Pasted image 20241019173244.png]]
![[Pasted image 20241019173347.png]]
![[Pasted image 20241019173355.png]]
- public cloud : aws ,microsoft azure ,google cloud
- private cloud : openstack
![[Pasted image 20241019173601.png]]
- hybrid multi cloud 장점 구분
![[Pasted image 20241019174540.png]]
- iaas란? 나올수도 
![[Pasted image 20241019174636.png]]
==특징 : 물리적 리소스를 가상화하여 유연한 infrasturcture 제공==
===iaas 가상화 유형=
![[Pasted image 20241019175449.png]]
hyprervisor 위에 guest os 
하이퍼바이저 x 대신 container engine guest os 가 없다.
==- container의 장점 hyperviser 장점만 보자 단점 보지 말자==![[Pasted image 20241019175711.png]]
- pass 
pass 중요한 듯 특징도 중요한 듯
![[Pasted image 20241019180013.png]]
![[Pasted image 20241019180558.png]]
![[Pasted image 20241019180711.png]]

![[Pasted image 20241019180751.png]]
![[Pasted image 20241019180910.png]]

![[Pasted image 20241019181025.png]]
![[Pasted image 20241019181045.png]]

- sass
![[Pasted image 20241019181125.png]]

성숙된 saas 는 환경 설정 맞춤화, 다중 사용자 지원, 확장성 조건 충족
![[Pasted image 20241019181550.png]]

이것에 대한 saas 성숙도 모델 중 레벨 2까진 서버 공통적 특징
레벨 3부터가 실질적 saas
3,4 차이는 부하 분산 , tennant가  
![[Pasted image 20241019181838.png]]


3주차
퀴즈 잘 봐라

##### cloud native application : 클라우드로부터 선천적으로 만들어짐
주요 특징 4가지
![[Pasted image 20241019182207.png]]

- 아키텍쳐가 cncf 와 매칭은 된다 자세히 몰라도 됨
- cloud native application 핵심은 서비스
- cloud native application 개발 원칙 4가지 무상태(종속성 없다), 느슨한 결함, 독립적/자율적(db 가 독립적이다), cicd
![[Pasted image 20241019184053.png]]

##### 전통적 vs 클라우드 네이티브 어플리케이션
개발 방법, 팀 구성 ,배포 주기 잘 보기
- 전통적 : 서버 중심+강한 단일+길고 간헐적 배포
- 클라우드 네이티브 : 컨테이너 중심+느슨한 분산 api 기반 +짧고 지속적
![[Pasted image 20241022153134.png]]
![[Pasted image 20241022153122.png]]

msa를 api gateway로 각 컨테이너가 연결된다.

##### monloitic miscro service 구조 차이
 CNA에선 데이터베이스가 독립적 db를 가질 수 없어 물리적으로 1개를 두고 논리적으로 이를 연결해서 사용한다. 무결성이 깨져서 논리적 db사용
![[Pasted image 20241022154617.png]]
api 
##### iass 가상화 3가지 내용 잘보기
![[Pasted image 20241022160757.png]]
![[Pasted image 20241022160830.png]]

api란 파트 충분히 잘 보기
soap rest api (ms opensource가 만듬) 유형, 기능, 데이터포멧, 대역폭, soap vs rest 표 잘 보기 차이 잘 보기 
기능 프로토콜 아키텍쳐
기능위주 데이터위주
xml xmljsonhtml 다양
ws-security ssl sslhttps
많은 리소스 대역폭 저근리소소 갑벼
캐시 불가 가능 
soap rest
![[Pasted image 20241025150127.png]]
표 다음장 보지마 ==

- JSON YAML
rest url로request paratmeter / response로 해당 file 원하는 형태로 가장 대표적 json 
xml json 많이 사용하는데 클라우드에선 yaml로 사용한다. 


##### 12 factors - 객관식 ox문제 이해하자 왜 log 가 필요? 왜 의존성이 없어져야하나? 
==피피티 보기-------------------------==
##### MSA란
단독 실행, 독립적 배치, 작은 모듈로 기능을 분해하여 서비스하는 아키텍쳐
- 장점
![[Pasted image 20241022154858.png]]
- 단점
![[Pasted image 20241022155014.png]]


##### Container란

컨테이너 특징
![[Pasted image 20241022155346.png]]![[Pasted image 20241022155449.png]]
##### Devops란 
운영과 개발 협업 과정을 자동화 개발과 개선 속도 빨라짐
Devops를 위한 필수 방법론 1. 애자일 2. CICD
CICD continus integration, delivery (통합과 운영자와 사용자에게 제공)
==개방형 클라우드 플랫폼은 정부에서 만들었다. => pass를 위한 클라우드 플렛폼이고 이건 쿠버네티스 기반으로 만들어졌다
iassXXX
기대효과xxx
핵심아키텍쳐xxx

==개발 방법론 퀴즈 봐라==
#### session clustering , sticky session , redis , load balancing
- 왜 필요? 
대용량 트래픽 장애 없이 처리하기 위해 여러 대의 서버에 적절히 트래픽을 분배한다. => 로드밸런싱
- 세션 클러스터링 => 여러 서버에 걸쳐 세션 정보를 공유하는 방식

- 스티키 세션? 
특정 서버에 세션을 고정한다.
이는 서버 과부하와 서버 다운시 세션이 소실될 위험이 있다.

=> 이를 해결하기 위해 세션 서버를 분리하고, Redis 와 같은 외부 오픈소스 스토리지를 이용해 세션을 저장한다.

=>Redis는 주기억장치은 보조기억장치보다 빠르게 접근 가능하기에 빠르게 접근할 수 있는 인 메모리 데이터 스토어 방식을 이용한다.

로드밸런싱 . 스티키세션 . 세션 클러스터링 . redis 인메모리 

##### 포트 fowarding (터널링)
포트 fowarding (터널링) 과 그 내부의 기술 명칭들이 몇개 잇엇다
왜 필요?
==firewall 이라는 방화벽에 막혀서 그 내부에 들어가기 위해서 포트포워딩이랑 터널링이 필요하다. 
![[Pasted image 20241025145918.png]]
![[Pasted image 20241025145905.png]]
6주차
개방형 클라우드 플랫폼 kpass . 쿠버네티스 컨테이너 플랫폼

vmware 회사에서 나온제품이 유명하다

지원환경 iaas aws azure openstack

###### 컨테이너 플렛폼이란? 쿠버네티스 단독배포, 서비스형 배포, edge배포 가능한 서비스

###### 배포 방식 3가지가잇다
1.단독배포 - 쿠버네티스 플랫폼 단독 배포 독립적 환경 제공
2.  edge 배포 - edge computing 기반, 단독배포와 함께 edge 환경에 단독 베포
3.서비스형 배포 - 어플리케이션 플랫폼에서 서빗 형태로 배포
어플리케이션과 같이 쓰느-서비스형 배포

###### 컨테이너란
![[Pasted image 20241025142333.png]]
![[Pasted image 20241025143415.png]]

container engine와 contianer runtime(oci를 중시한다 oci가 뭔지 알자)
os위에서 oci 그림
podman이 만들든 어떤 회사가 만들든 oci를 중시하면 다 사용가능
컨테이너를 실행하고 만드는 podman그리고 docker 가 있다.
docker podman 차이  : podman은 daemonless다


##### 쿠버네티스란
컨테이너화된 어플리케이션을 관리하기 위한 오픈소스 시스템이다.
클러스터안엔 마스터노드와 워커노드가 있다.
쿠버네티스는 다수의 컨테이너를 관리한다.
클러스터는 여러개의 네임스페이스로 분리되고 각 네임스페이스는 
배포의 기본 단위인 pod (고유 ip 주소)
여러 pod를 묶은 replicaset
그 위에 deployment
그 위에 namespace로 구성
![[Pasted image 20241025143443.png]]
클러스터 구성요소 클라우드 이루는 집합인 클러스터
![[Pasted image 20241025143535.png]]

master = 용어 1가지 
worker= 용어 2가지 slave, agent, compute

![[Pasted image 20241025144227.png]]

namespace 논리적으로 분리 되는것 그리고 각각에서 개발함
worker 내부에서 namespace deplyment replicset pod
pod여러개 동일한 형태로 배포해서 관리 가능한 형태 replicasest
replicatset 여러개가 deployment
![[Pasted image 20241025144544.png]]
![[Pasted image 20241025144602.png]]

##### 퀴즈

아래 영역에 해당하는 오픈소스 혹은 상용소프트웨어의 명칭을 3개씩 작성하시오.

1. IaaS - aws, microsoft azure, google cloud platfrom

2. PaaS - ====d

대표적인 CSP(Cloud Service Provider)기업 6개를 작성하시오.(국내외 무관)

microsoft azure ,aws ,google cloud platform, oracle cloud,alibabacloud , ibm cloud

컨테이너, MSA, CI-CD, DevOps 각각에서 사용되는 

오픈소스를 2개 이상 찾아서 그 오픈소스 이름을 작성하세요.
DockerKubernetes
springbootconsul
jetkinsgitlabcici
ansibleterrafrom


cloud native application 주요 특징 4가지 [mdcc] 
saas/paas/iaas 정의 서술하라. 

iaas 물리적 시스템 가상화된 서비스로 제공 =>1.2.
paas 컨테이너 기반 sw 서비스 제공 , 플랫폼 구축 필요 없다 , devops 적용 굳

3. 대표적인 CSP(Cloud Service Provider)기업 6개를 작성하시오.(국내외 무관)

아래 영역에 해당하는 오픈소스 혹은 상용소프트웨어의 명칭을 3개씩 작성하시오.

1. IaaS - 

2. PaaS - 

4.컨테이너, MSA, CI-CD, DevOps 각각에서 사용되는 

오픈소스를 2개 이상 찾아서 그 오픈소스 이름을 작성하세요.

6.아래 3가지 배포 방법에 대해 간략히 설명하시오. [rolling update, greenbluedeployment, canaray release]
새 버전으로 점진적으로 업데이트하는 방식. 새 버전을 순차적 배포하며 점차 하나씩 제거한다
소수의 사용자나 특정 트래픽에 제한적으로 적용하는 방식 문제 없으면 점차 확대 
 K3s가 무엇인지 서술 쿠버네티스의 기능을 축소해서 핵심 컴포넌트만 포함시켜 리소스가 제한된 환경에서도 클라우드 네이티브 어플리케이션 사용가능하게 함

5.msa mfa차이점
작은 서비스들로 분리. 이들은 자체 데이터베이스 이용 가능. 각 각 배포 가능해 전체 시스템 중단하지 않음 서비스별 기술스택 선정 가능
fe-여러개 service
mfa
단순 배포
상호의존성
확장성 제한
단일 코드베이스 fe-1service

전통적 어플리케이션과 클라우드 네이티브 어플리케이션 차이

monolithic vs microservice

iaas 가상화 스토리지 종류 3가지

방화벽에 막혀서 내부에 들어가기 위해 뭐가 필요한가?







