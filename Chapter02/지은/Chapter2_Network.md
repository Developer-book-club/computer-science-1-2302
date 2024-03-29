#  2 네트워크 

# 2.1 네트워크의 기초 

네트워크 
- 노드(node)와 링크(link)가 서로 연결되어 있으며 리소스를 공유하는 집합
    - 노드(node): 서버, 라우터, 스위치등 네트워크 장치
    - 링크(link): 유선 또는 무선

## 2.1.1 처리량과 지연시간
- 네트워크를 구축할 때는 '좋은' 네트워크를 만드는 것이 중요함.
- 좋은 네트워크: 많은 처리량, 짧은 지연시간, 적은 장애빈도, 좋은 보안

처리량(= throughput, 단위(bps, 초당 전송 또는 수신되는 비트 수))
- 링크 내에서 성공적으로 전달된 데이터의 양
- 얼만큼의 트래픽을 처리했는지를 나타냄. '많은 트래픽을 처리' = '많은 처리량'
- 트래픽의 크기, 네트워크 장치간의 대여폭, 네트워크 중의 에러, 장치의 하드웨어 스펙이 영향을 받음

트래픽
- 특정 시점에 링크 내에 '흐르는' 데이터의 양 
- ex. 서버에 저장된 파일(문서, 이미지, 동영상 등)을 클라이언트(사용자)가 다운로드할 때 발생되는 데이터의 누적량

트래픽과 처리량의 차이
1. 트래픽이 많아졌다 = 흐르는 데이터가 많아졌다
2. 처리량이 많아졌다 = 처리되는 트래픽이 많아졌따 

대역폭
- 주어진 시간 동안 네트워크 연결을 통해 흐를 수 있는 최대 비트 수

지연 시간(latency)
- 요청이 처리되는 시간
- 어떤 메시지가 두 장치 사이를 왕복하는데 걸린 시간
- 매체 타입(무선, 유선), 패킷 크기, 라우터의 패킷 처리 시간에 영향을 받음

## 2.1.2 네트워크 토폴로지와 병목 현상

네트워크 토폴로지(network topology)
- 네트워크를 설계할 때 고려
- 노드와 링크가 어떻게 배치되어 있는지에 대한 방식, 연결 형태

트리 토폴로지 = 계층형 토폴로지
- 트리 형태로 배치한 네트워크 구성
- 노드의 추가, 삭제가 쉬움
- 특정 노드에 트래픽이 집중될 때 하위노드에 영향을 끼칠 수 있다

버스 토폴로지
- 중앙 통신 회선 하나에 여러 개의 노드가 연결되어 공유하는 네트워크 구성
- 근거리 통신망(LAN)에서 사용
- 장점: 설치 비용이 적음. 신뢰성 우수, 중앙 통신 회선에 노드를 추가하거나 삭제하기 쉬움
- 단점: 스푸핑이 가능



스푸핑
- 스위칭 기능을 마비 시키거나 속여서 특정 노드에 해당 패킷이 오도록 처리 
- 스위칭: LAN상에서 송신부의 패킷을 송신과 관련없는 다른 호스트에 가지 않도록 하는 기능
<img width="432" alt="스크린샷 2023-03-18 오후 9 24 18" src="https://user-images.githubusercontent.com/122012413/226105878-9a313bec-22bc-4716-8006-8a94bc01882d.png">
- 올바르게 수신부로 가야 할 패킷이 악이적인 노드에 전달되게 됨

스타 토폴로지
<img width="335" alt="스크린샷 2023-03-18 오후 9 30 22" src="https://user-images.githubusercontent.com/122012413/226106190-e26f6e90-fdce-4d3e-9952-e3c63602a77a.png">

- 중앙에 있는 노드에 모두 연결된 네트워크 구성
- 장점
    1. 노드를 추가하거나 에러를 탐지하기 쉬움. 패킷의 충돌 발생 가능성이 적다.
    2. 어떤 노드에 장애가 발생해도 쉽게 에러 발견 가능
- 단점
    1. 중앙 노드에 장애 발생 시, 전체 네트워크를 사용할 수 없음
    2. 고가의 설치비용

링형 토폴로지
<img width="369" alt="스크린샷 2023-03-18 오후 9 30 39" src="https://user-images.githubusercontent.com/122012413/226106207-3fa9983f-6556-4a31-ad1e-6508ad6fa90d.png">
- 각각의 노드가 양 옆의 두 노드와 연결하여 전체적으로 하나의 연속된 길을 통해 통신을 하는 망

메시 토폴로지(=망형 토폴로지)
- 그물망처럼 연결되어 있는 구조 
- 장점
    1. 한 단말 장치에 장애가 발생 시 여러 개의 경로가 존재하여 네트워크를 계속 사용할 수 있음
    2. 분산 처리 트래픽이 가능
- 단점
    1. 노드의 추가가 어려움
    2. 고가의 구축/운용 비용

병목 현상(bottleneck)
- 전체 시스템의 성능이나 용량이 하나의 구성 요소로 인해 제한을 받는 현상
- 병목 현상을 찾기 위해 **토폴로지**는 중요한 기준
- 네트워크가 어떤 토폴리지를 갖는지, 어떠한 경로로 이루어져 있는지를 알아야 병목 현상을 해결할 수 있음


## 2.1.3 네트워크 분류
- 규모 기반으로 분류
- LAN: 사무실, 개인적으로 소유 가능한 규모
- MAN: 서울시 등 시 정도의 규모
- WAN: 세계 규모 
    
LAN(= 근거리 통신망)
- 같은 건물이나 캠퍼스 같은 좁은 공간에서 운영
- 전송 속도가 빠르고 혼잡하지 않음

MAN(= 대도시 지역 네트워크)
- 도시 같은 넓은 지역에서 운영
- 평균의 전송 속도
- LAN 보다는 혼잡

WAN(= 광역 네트워크)
- 국가 또는 대륙의 넓은 지역에서 운영
- 낮은 전송 속도. MAN 보다 혼잡하다.

## 2.1.4 네트워크 성능 분석 명령어
- 애플리케이션 코드상의 문제는 없으나, 사용자가 서비스로부터 데이터를 가져오지 못하는 상황의 경우 -> 네트워크 병목 현상의 가능성이 있음

네트워크 병목현상의 주된 원인
- 네트워크 대역폭
- 네트워크 토폴로지
- 서버 CPU, 메모리 사용량
- 비효율적인 네트워크 구성

ping(Packer INternet Groper)
- 네트워크 상태를 확인하려는 대상 노드를 향해 일정 크기의 패킷을 전송하는 명령어
- 해당 노드의 패킷 수신 상태, 도달 하기까지의 시간, 네트워크의 연결 상태를 확인 가능

netstat
- 접속되어 있는 서비스들의 네트워크 상태를 표시하는데 사용
- 서비스의 포트가 열려있는지 확인할 때 사용
- 네트워크 접속, 라우팅 테이블, 네트워크 프로토콜 리스트를 보여줌

nslookup
- DNS에 관련된 내용을 확인하기 위해 사용
- 특정 도메인에 매핑된 IP 확인 위해 사용

tracert
- 윈도우에서 사용. / 리눅스에서는 traceout 
- 목적지 노드까지 네트워크 경로를 확인할 때 사용 
- 목적지 노트까지 구간들 중 어느 구간에서 응답 시간이 느려지는지 등 확인 


## 2.1.5 네트워크 프로토콜 표준화
- 다른 장치들끼리 데이터를 주고 받기 위해 설정된 공통된 인터페이스
- IEEE 또는 IETF 표준화 단체가 정함


# 2.2 TCP/IP 4계층 모델

인터넷 프로토콜 스위트(internet protocol suite)
- 인터넷에서 컴퓨터들이 서로 정보를 주고 받는데 쓰이는 프로토콜 집합
-

## 2.2.1 계층 구조

<img width="874" alt="스크린샷 2023-03-18 오후 9 50 06" src="https://user-images.githubusercontent.com/122012413/226107077-a716fd13-e08b-4937-b55f-3a02cbd300e6.png">

OSI 계층
- 애플리케이션 계층을 세개로 쪼갬
- 링크 계층 -> 데이터 링크 계층 + 물리 계층
- 인터넷 계층 -> 네트워크 계층

- 특정 계층이 변경되었을 때 다른 계층이 영향을 받지 않도록 설계
- ex. 전송 계층에서 TCP를 UDP로 변경했다고 해서 인터넷 웹 브라우저를 다시 설치해야 하는 것이 아니듯 유연하게 설계

1. 애플리케이션 계층
- FTP, HTTP, SSH, SMTP, DNS 등 응용 프로그램이 사용되는 프로토콜
- 웹 서비스, 이메일 등 서비스를 실질적으로 사람들에게 제공하는 층

2. 전송 계층 
- 송신자와 수신자를 연결하는 통신 서비스 제공
- ex. TCP, UDP가 있음
TCP
- 패킷 사이의 순서를 보장
- 연결지향 프로토콜 사용 (신뢰성 구축)
- 수신 여부를 확인하여 '가상회선 패킷 교환 방식' 사용

UDP
- 순서/수신여부 보장하지 않음
- 단순히 데이터만 주는 '데이터그램 패킷 교환 방식' 사용


가산회선 패킷 교환 방식
- 패킷 당 가상 회선 식별자가 포함되어 있음
- 모든 패킷 전송 시 가상 회선 해제, 패킷들은 전송 순서대로 도착하는 방식

데이터그램 패킷 교환 방식
- 하나의 메시지에서 분할된 여러 패킷은 서로 다른 경로로 전송될 수 있고, 도착한 순서가 다를 수 있는 방식

TCP 연결 성립 과정
- TCP는 신뢰성 확보 시 '3-way handshake' 작업 진행
- UDP는 이 과정이 없어서 신뢰서이 없는 계층이라고 함 
<img width="780" alt="스크린샷 2023-03-18 오후 9 56 20" src="https://user-images.githubusercontent.com/122012413/226107668-4ddeec02-6ff1-462b-ae13-9b649e505bc8.png">

1. SYN 단계: 클라이언트는 서버에 클라이언트의 ISN을 담아 SYN을 보냄. 
- ISN: 새로운 TCP 연결의 첫번째 패킷에 할당된 임의의 시퀀스 번호. 장치마다 다를 수 있음
2. SYN + ACK 단계: 서버는 클라이언트의 SYN을 수신. 서버의 ISN을 보내며 승인번호로 클라이언트의 ISN + 1을 보냄
3. ACK 단계: 클라이언트는 서버의 ISN+1 한 값인 승인번호를 담아 ACK를 서버에 보냄


4-way handshake: TCP 해제 과정
<img width="590" alt="스크린샷 2023-03-18 오후 9 59 52" src="https://user-images.githubusercontent.com/122012413/226107821-f5ed5bdc-2986-4e4e-9617-10b8ce620481.png">
1. C->S 연결 종료요청 FIN FLAG 전송
2. S->C 종료 수락 메시지 ACK 전송
- 일시적 TIME_OUT (데이터를 모두 보낼 때 까지)
3. S->C 연결 종료 FIN FLAG 전송
4. C->S 확인 메시지 ACK 전송
- 서버 소켓 연결 close(), 클라이언트 일정 시간 동안 TIME_WAIT (잉여패킷 대기)
- 일정 시간 동안 TIME_WAIT의 이유
  1. 지연 패킷이 발생할 경우 대비
  2. 두 장치가 연결되어 있는지의 확인을 위함

인터넷 계층
- 장치로부터 받은 네트워크 패킷을 IP주소로 지정된 목적지로 전송하기 위해 사용되는 계층
- IP(Internet Protocol) : 비신뢰성, 비연결지향 데이터그램 프로토콜
- ARP(Address Resolution Protocol) : 주소변환 프로토콜. IP주소를 MAC주소로 변환하는 프로토콜
- RARP(Reverse ARP) : MAC주소로 IP주소를 찾는 프로토콜
- ICMP(Internet Control Message Protocol) : 상태 진단 메시지 프로토콜. ex. ping
- IGMP(Internet Group Message Protocol) : 멀티캐스트용 프로토콜

링크 계층 (a.k.a 네트워크 접근 계층)
- 전선, 광섬유, 무선 등으로 실질적으로 데이터를 전달
- 장치 간에 신호를 주고 받는 '규칙'을 정하는 계층 

계층 간 데이터 송수신 과정

캡슐화 과정
<img width="633" alt="스크린샷 2023-03-18 오후 10 05 53" src="https://user-images.githubusercontent.com/122012413/226108119-759f023a-02b1-4bff-9174-4790dc1b40b4.png">

- 애플리케이션 계층의 데이터가 전송 계층으로 전달되면서 '세그먼트' 또는 '데이터그램'화 되며 TCP(L4) 헤더가 붙여짐
- 인터넷 계층으로 가면서 IP(L3) 헤더가 붙여지게 되며 '패킷'화가 된다.
- 링크 계층으로 전달되면서 프레임 헤더와 프레임트레일러가 붙어 '프레임'화가 된다.

비캡슐화 과정
<img width="613" alt="스크린샷 2023-03-18 오후 10 07 02" src="https://user-images.githubusercontent.com/122012413/226108169-5a172e8d-1ed4-4409-b406-34ef7912e171.png">

- 하위 계층에서 상위 계층으로 가며 각 계층의 헤더 부분을 제거하는 과정


## 2.2.2 PDU(Protocol Data Unit)
계층 간 데이터 전달 시 한 덩어리의 단위
제어 관련 정보들이 포함된 '헤더', 데이터를 의미하는 '페이로드'로 구성
- 애플리케이션 계층: 메시지
- 전송 계층: 세그먼트(TCP), 데이터그램(UDP)
- 인터넷 계층: 패킷
- 링크 계층: 프레임(데이터 링크 계층), 비트(물리 계층)

# 2.3 네트워크 기기
여러 개의 네트워크 기기 기반으로 네트워크 구축

## 2.3.1 네트워크 기기의 처리 범위
- 계층별로 처리 범위가 나뉨
- 애플리케이션 계층: L7 스위치 
- 인터넷 계층: 라우터, L3 스위치
- 데이터 링크 계층: L2 스위치, 브리지
- 물리 계층: NIC, 리피터, AP 

## 2.3.2 애플리케이션 계층을 처리하는 기기
L7 스위치 
- 로드 밸런서라고도 하며, 서버의 부하를 분산하는 기기 
- 클라이언트로부터 오는 요청들을 뒤쪽의 여러 서버로 나누는 역할
- 시스템이 처리할 수 있는 트래픽 증가를 목표

## 2.3.3 인터넷 계층을 처리하는 기기
라우터
- 여러 개의 네트워크를 연결, 분할, 구분시켜주는 역할

L3 스위치
- L2 스위치의 기능과 라우팅 기능을 갖춘 장비 
<img width="390" alt="스크린샷 2023-03-18 오후 10 14 06" src="https://user-images.githubusercontent.com/122012413/226108502-774f6e1c-15e5-449d-b5c6-73f10a8c97ab.png">

## 2.3.4 데이터 링크 계층을 처리하는 기기
L2 스위치
- 장치들의 MAC 주소를 MAC 주소 테이블을 통해 관리
- 연결된 장치로부터 패킷이 왔을 때 패킷 전송 담당

브리지
- 두 개의 근거리 통신망(LAN)을 상호 접속할 수 있도록 하는 통신망 연결 장치
- 포트와 포트 사이의 다리 역할
- 장치에서 받아온 MAC 주소를 MAC주소 테이블로 관리

## 2.3.5 물리 계층을 처리하는 기기
NIC(= LAN 카드)
- 2대 이상의 컴퓨터 네트워크를 구성하는데 사용
- 네트워크와 빠른 속도로 데이터를 송수신 할 수 있도록 컴퓨터 내에 설치하는 확장 카드
- 각 LAN 카드에는 주민등록번호처럼 각각을 구분하기 위한 고유 식별번호(MAC 주소)가 있음

리피터
- 들어오는 약해진 신호 정도를 증폭하여 다른 쪽으로 전달하는 장치

AP
- 패킷 복사 기기 

# 2.4 IP 주소

## 2.4.1 ARP
- IP 주소로부터 MAC 주소를 구하는 IP와 MAC 주소의 다리 역할을 하는 프로토콜

## 2.4.2 홉바이홉 통신
- IP 주소를 통해 통신하는 과정
- hop(홉): 통신망에서 각 패킷이 여러 개의 라우터를 건너가는 모습을 비유적으로 표현.

라우팅 테이블 
- 송신지에서 수신지까지 도달하기 위해 사용
- 라우터에 들어가 있는 목적지 정보, 그 목적지로 가기 위한 방법이 들어 있는 리스트
- 게이트웨이와 모든 목적지에 대해 목적지로 도달하기 위해 거쳐야 할 다음 라우터의 정보를 가지고 있음

게이트웨이
- 서로 다른 통신망, 프로토콜을 사용하는 네트워크 간 통신을 가능하게 하는 관문 역할

## 2.4.3 IP 주소 체계
IPv4: 32비트를 8비트 단위로 점을 찍어 표기
IPv6: 64비트를 16비트 단위로 점을 찍어 표기

클래스 기반 할당 방식
- IP 주소 체계 초기에 사용
- A,B,C,D,E 다섯 개의 클래스로 구분
- 앞에 있는 부분을 네트워크 주소, 뒷 부분을 컴퓨터에 부여하는 주소인 호스트 주소로 놓아서 사용

DHCP
- IP 주소 및 기타 통신 매개변수를 자동으로 할당하기 위한 네트워크 관리 프로토콜
- 네트워크 장치의 IP 주소를 수동으로 설정할 필요 없이 인터넷에 접속할 때마다 자동으로 할당 

NAT
- 패킷이 라우팅 장치를 통해 전송되는 동안 패킷의 IP 주소 정보를 수정하여 IP주소를 다른 주소로 매핑하는 방법

## 2.4.4 IP 주소를 이용한 위치 정보
- IP주소는 인터넷에서 사용하는 네트워크 주소이기 때문에 동 또는 구까지의 위치 추적이 가능

# 2.5 HTTP

## 2.5.1 HTTP/1.0
- 한 연결당 하나의 요청을 처리하도록 설계
- RTT 증가를 불러움
- RTT: 패킷이 목적지에 도달하고 나서 다시 출발지로 돌아오기까지 걸리는 시간. 패킷 왕복 시간

RTT 증가를 해결하기 위한 방법
- 이미지 스플리팅: 많은 이미지가 합쳐 있는 하나의 이미지를 다운받고 position을 이용하여 이미지 표기
- 코드 압축: 코드를 압축하여 개행 문자, 빈칸을 없애서 코드의 크기를 최소화 하는 방법
- 이미지 Base64 인코딩: 이미지 파일을 64진법으로 이루어진 문자열로 인코딩 하는 방법


## 2.5.2 HTTP/1.1
- HTTP/1.0에서 발전된 설계 방식
- 한번 TCP 초기화를 한 후 keep-alive 옵션으로 여러 개의 파일을 송수신 할 수 있게 바뀜 
- keep-alive는 이 버전 부터 표준화 되고, 기본 옵션으로 설정

HOL Blocking(Head Of Line Blocking)
- 네트워크에서 같은 큐에 있는 패킷이 첫번째 패킷에 의해 지연될 때 발생하는 성능 저하 현상

무거운 헤더 구조
- HTTP/1.1에서는 쿠키 등 많은 메타 데이터가 들어 있고, 압축이 되지 않아 무거웠다.

## 2.5.3 HTTP/2
- HTTP/1.x 보다 지연 시간을 줄이고 응답 시간이 더 빠름
- 멀티플렉싱, 헤더 압축, 서버 푸시, 요청의 우선순위 처리를 지원하는 프로토콜

멀티플렉싱
- 여러 개의 스트림을 사용하여 송수신
- 패킷이 손실되어도 해당 스트림에만 영향을 끼침
- 스트림: 시간이 지남에 따라 사용할 수 있게 되는 일련의 데이터 요소를 가리키는 데이터 흐름


헤더 압축
- HTTP/1.x 의 큰 헤더 방식 문제를 해결 위해 HTTP/2에서 헤더 압축 적용
- 허프만 코딩 압축 알고리즘(HPACK) 사용

허프만코딩
- 전체 데이터의 표현에 필요한 비트양을 줄이는 원리.
- 문자열을 문자 단위로 쪼개 빈도수를 세어 빈도가 높은 정보는 적은 비트, 빈도가 낮은 정보는 비트 수를 많이 사용. 

## 2.5.4 HTTPS
- HTTP/2는 HTTPS 위에서 동작
- 애플리케이션 계층과 전송 계층 사이에 신뢰 계층인 SSL/TLS 계층을 넣은 **신뢰할 수 있는 HTTP 요청** 
- '통신을 암호화'

SSL/TLS
- 전송 계층에서 전송 계층에서 보안을 제공하는 프로토콜 
- 클라이언트와 서버가 통신할 때 제 3자가 메시지를 도청하거나 변조하지 못하도록 함
- 보안 세션을 기반으로 데이터를 암호화하며 보안 세션이 만들어 질 때 인증 메커니즘, 키 교환 알고리즘, 해싱 알고리즘 사용

보안 세션
- 보안이 시작되고 끝나는 동안 유지되는 세션
- SSL/TLS는 핸드 쉐이크를 통해 보안 세션을 생성 상태 정보등을 공유 

세션
- 운영체제가 어떠한 사용자로부터 자신의 자산 이용을 허락하는 일정한 기간
- 사용자는 일정 시간 동안 응용 프로그램, 자원등을 사용할 수 있음


## 2.5.5 HTTP/3
- HTTP/1.1, 2 와 함께 WWW에서 정보를 교환하는데 사용
- QUIC 계층, UDP 기반
- 멀티플렉싱을 가지고 있음. 초기 연결 시 지연시간 감소

QUIC
- 순방향 오류 메커니즘(FEC)이 적용됨 -> 전송한 패킷 손실 시 수신 측에서 에러를 검출 및 수정 
- 열악한 네트워크 환경에서 낮은 패킷 손실률





















