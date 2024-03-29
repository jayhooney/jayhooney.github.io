---
title: 'AWS Cloud Part.1'
excerpt: 'AWS VPC 기초개념'
toc: true
toc_sticky: true
header:
  teaser: /assets/image/aws_vpc.png
categories:
  - AWS
tags:
  - AWS
  - VPC
  - Subnet
  - CIDR 
  - NAT gateway
last_modified_at: 2021-10-01
---

## 으아, 큰일났어요(?)
현재 재직 중인 회사에 입사했을 때 AWS를 이용해 서비스가 제공되고 있었다. 그리고 먼저 입사하신 시니어 서버 개발자분이 문제점을 파악해 개선하셨고 그 상태로 서비스가 운영되고 있다.

문제는 지금부터인데, 나의 AWS 지식은 EC2 인스턴스를 생성해 보안 그룹을 설정하는 것이 전부였다. 하지만 시니어 개발자분이 퇴사를 하게 됨에 따라 인수인계를 받게 됐다. 아, 정말 큰일 났다(서버 개발자 절망 편).
Node.js를 사용해 처음 서버를 개발할 때도, Spring Boot를 처음 접했을 때도 막연함보다 '공부하자!'라는 생각이 먼저 들었는데 Infra는 워낙 진입장벽이 높다는 소리를 많이 들었던 탓인지 겁이 많이 났다.

그래도 정말 다행인 건, 시니어 개발자분께서 AWS 관련 양질의 콘텐츠와 함께 스터디를 진행해 주셨다. 덕분에 뜬구름같이 뭉텅뭉텅 알고 있던 AWS 리소스 개념들이 정리됐고 이를 정리해 보고자 한다.
또한, 이 글이 나처럼 AWS에 입문하시는 분들께 기초 지식 쌓기 좋은 가이드가 됐으면 좋겠다.

## VPC, Vitual Private Cloud
EC2 인스턴스를 생성해 서버를 올린다고 가정해 보자. 이때, 생성한 EC2 인스턴스를 __어디에__ 위치시킬 것인가 중요한데 이때 등장하는 개념이 VPC이다.

AWS에서 VPC를 제공하기 전에는 EC2를 생성하면 위치가 무작위로 지정됐다고 한다.
어차피 SSH 접속을 하던, HTTP Request를 보내던 인스턴스 IP만 알고 있다면 위치는 굳이 알 필요 없지만 문제는 내가 생성한 EC2가 다른 AWS 사용자의 EC2와 같은 공간에 있다는 것이다.

위에 언급한 것과 달리 다른 AWS 사용자들과 __논리적으로 격리된 공간을__ 만들기 위해 VPC를 사용한다. 즉, VPC는 AWS 계정 전용 가상 네트워크이다.

단순히 이게 전부라면 좋겠지만 세상은 호락호락하지 않다. VPC를 생성하고 난 이후에 설정해야 할 것들이 첩첩산중이다. 크게 아래와 같다.  
1. IP주소 범위를 정한다.
2. Subnet 단위로 나눈다.
3. 라우트 테이블을 설정한다.
4. 방화벽을 설정한다.

아래부터는 각 단계별로 알아보도록 한다. 

## CIDR, Classless Inter-Domain Routing
VPC를 생성했으니, 사용할 IP 범위를 정해줘야 한다. 싸이더라고도 불리는 이것은 직독직해하면 클래스 없는 도메인 간 라우팅이란 기법인데 AWS에서는 CIDR를 사용한다. 아래 표현식을 한 번 살펴보자. 

```
172.0.0.0/8
```
위 IP 표현식에서 /8이 의미하는 것은, IP 앞 8비트는 항상 고정하고 뒤에 24비트는 변경 가능하다는 의미이다. 위와 같이 정의했을 경우 사용 가능한 IP 개수는 2^24개이다. 

AWS에서는 RFC1918 권고안에 명시된 IP 주소를 private 하게 사용할 것을 권장한다. 간단하게 RFC 1918을 살펴보면 아래와 같다.
```
10.0.0.0 - 10.255.255.255 (=10.0.0.0/8)
172.16.0.0 - 172.31.255.255 (=172.16.0.0/12)
192.168.0.0 - 192.168.255.255 (=192.168.0.0/16) 
```

## Subnet
IP 범위를 정했다면 Subnet(부분망)이라는 VPC 안의 작은 논리 공간을 만들어야 한다. 서비스를 제공할 Infra를 디자인할 때 외부에 노출시킬 것과 철저히 숨겨놓을 것을 나눈다.
WEB 서비스를 제공한다고 가정해 보자. 그렇다면 WEB 페이지는 모든 IP에 대해 접근이 가능해야 하며, WEB에서 호출하는 API 서버는 숨겨져 있어야 한다. 이것을 가능케 하는 게 Subnet이다.

AWS에 가용 영역이라는 게 존재한다. 아시아 태평양(서울)/미국 서부(캘리포니아)/유럽(파리) region이라고 불리는 것들이 가용 영역이다. Subnet은 사용할 가용 영역마다 만들어 줘야 한다.   

AWS에서는 아래와 같이 권고한다.
```
/16 VPC (64K address)
/24 Subnets (251 address) > 0,255는 기본적으로 제외되며 1,2,3,254는 AWS에서 관리목적으로 사용한다.
One subnet per Availty Zone
```

## Internet Gateway
VPC를 덩그러니 생성해놓기만 하면, 인터넷을 사용할 수 없다. 그래서 Internet Gateway를 만들어 VPC와 매핑해 인터넷을 사용할 수 있도록 설정해야 한다. 즉, 인터넷 개통이다.
필수는 아니며, VPC 자체를 폐쇄적으로 운영하려면 굳이 Internet Gateway를 설정해 줄 필요가 없다. 추후 작성하겠지만 VPC끼리 통신이 가능하게끔 설정할 수 있다.

## Route Table 
VPC와 Subnet을 만들고 인터넷을 개통하면, 인터넷 경로를 만들어 줘야 하는데 이것을 Route Table이라고 한다. 쉽게 말해서 패킷 교통정리. 패킷 별로 VPC 내부로 들어가도 될 것과, 안될 것들이 있는데 이 흐름 자체를 정의한다.

VPC를 만들면 기본 Route Table이 존재하지만 Subnet은 다른 Route Table을 적용할 수 있다. 이런 특징을 사용해 어떤 Subnet은 인터넷과 완전히 격리시켜 private 하게, 어떤 Subnet은 인터넷에 연결하여 public 하게 사용할 수 있다.  

## Security 
AWS는 기본적으로 서비스 사용자가 허용하지 않는 한 모든 것을 불허한다.
아무리 VPC를 만들고, Subnet으로 쪼개고, 인터넷을 개통해서 EC2의 서버를 80포트로 띄워도, 80포트를 개방하지 않으면 외부에서의 접근이 절대 불가능하다.
AWS에서 제공해 주는 방화벽 서비스는 아래 두 가지가 있다. 

### 1.Network ACLs(Access Control List) : stateless firewalls
__외부 요청에__ 대한 보안정책을 정하는 것.
Subnet 단위로 적용 가능하며, 하나의 Subent은 하나의 Network ACL을 지정할 수 있다.
만약 하나의 Subnet에 1,000개 인스턴스가 존재한다면 모두 같은 보안 정책을 따르게 된다.

### 2.Security Group: stateful firewalls
__내부 요청에__ 대한 보안정책을 정하는 것.
Web Server는 모든 외부 접근에 대해서 개방돼야 하고, API Server는 Web Server의 요청만 허용하고 싶다.
이럴 때 사용하는 것이 Security Group이다.
인스턴스 단위로 적용 가능하며 Source(허용할 트래픽)는 IP, PORT를 직접 입력해 지정할 수도 있지만 다른 Security Group 자체를 포함 시킬 수도 있다.
Web Server들을 하나의 보안 그룹으로 묶어 놓으면 API Server Security Group에 Web Server마다 IP를 하나하나 입력할 필요가 없다는 의미이다.

### 3. state less ? ful?
Network ACL와 Security Group은 각각 stateless, stateful 차이가 있다. 그럼 두 개의 차이는 무엇일까?
stateful 방화벽은 연결이 발생할 경우 그 연결을 기억하고 있다. 따라서, 들어온 요청(inbound)에 대해 응답(outbound)을 별도로 제한하지 않는다.
반면 stateless 방화벽은 반대다.
연결을 기억하지 않으며 들어온 요청에 대해 응답하려면 별도 설정을 해줘야 한다.

## NAT gateway (Netwrok Address Translation)
Public subnet에는 서비스 제공을 위해 0.0.0.0/0 대역으로 방화벽 오픈이 됐을테고, Private subnet은 서버,DB등을 배포 및 설치하여 사용할 것이므로 Public subnet의 요청만 수락하도록 설정됐을 것이다.
하지만 서버나 DB는 종종 코드 배포나 마이그레이션 작업이 필요한데, Private Subnet은 Public Subnet과의 통신만을 허용해 놨으므로 불가능하다.
이런 경우에 사용하는 것이 NAT gateway이다.

Private subnet에 NAT gateway를 적용하면 Private subnet에서 외부로 패킷을 보내는 시도가 발생할 때 AWS에서 NAT gateway로 보낸다.
NAT gateway는 해당 패킷을 인터넷으로 보내고, 커넥션을 기억하고 있다가 응답이 오면 Private Subnet으로 전달해준다.
즉, NAT gateway는 Private subnet의 outbound만을 위한 특수 설정이다. 