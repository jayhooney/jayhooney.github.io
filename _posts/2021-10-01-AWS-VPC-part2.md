---
title: 'AWS Cloud Part.2'
excerpt: 'AWS VPC간 통신'
toc: true
toc_sticky: true
header:
  teaser: /assets/image/aws.png
categories:
  - AWS
tags:
  - AWS
  - VPC
  - VPC peering
last_modified_at: 2021-10-01
---

## VPC간 통신
다수의 VPC를 운영하는 환경에서 VPC간 통신이 필요할 수 있다.
AWS에서는 VPC간 통신을 위해 VPC peering과 Transit gateway라는 두 개의 서비스를 제공한다.
이번 포스팅에선 위 두 가지 서비스에 대해 알아본다.

## VPC peering
[AWS 공식문서](https://docs.aws.amazon.com/ko_kr/vpc/latest/peering/what-is-vpc-peering.html)에서 정의하는 VPC peering은 아래와 같다.
>VPC 피어링 연결은 프라이빗 IPv4 주소 또는 IPv6 주소를 사용하여 두 VPC 간에 트래픽을 라우팅할 수 있도록 하기 위한 두 VPC 사이의 네트워킹 연결입니다. 동일한 네트워크에 속하는 경우와 같이 VPC의 인스턴스가 서로 통신할 수 있습니다. 사용자의 자체 VPC 또는 다른 AWS 계정의 VPC와 VPC 피어링 연결을 만들 수 있습니다. VPC는 다른 리전에 있을 수 있습니다(리전 간 VPC 피어링 연결이라고도 함).

위 정의에서도 설명됐지만, VPC peering은 **두 VPC 사이의 네트워킹 연결**이다. 단 2개의 VPC만 연결할 수 있다는것이 특징이다.

A,B,C,D,J 총 다섯 개 VPC들이 존재한다고 가정해보자. 이 중에서 VPC_J에 인증서버를 두고, VPC_A,B,C,D가 VPC_J와 통신하게끔 하려면 아래와 같이 총 네 개 VPC peering을 설정해줘야 한다.
```
VPC_A <-> VPC_J
VPC_B <-> VPC_J
VPC_C <-> VPC_J
VPC_D <-> VPC_J
```



## Transit gateway
[AWS 공식문서](https://docs.aws.amazon.com/ko_kr/vpc/latest/tgw/what-is-transit-gateway.html)에서 정의하는 Transit gateway는 아래와 같다.
>전송 게이트웨이는 가상 사설 클라우드(VPC)와 온프레미스 네트워크를 상호 연결하는 데 사용할 수 있는 네트워크 전송 허브입니다.

Transit gateway의 특징은 **네트워크 전송 허브**라는 것이다.
이번에도 A,B,C,D,E의 총 다섯 개 VPC들이 존재한다고 가정해보자. VPC_A,B,C,D,E가 서로 통신할 수 있게끔 하려면 Transit gateway를 아래와 같이 설정하면 된다.
```
VPC_A <-> Trangit_gateway_J
VPC_B <-> Trangit_gateway_J
VPC_C <-> Trangit_gateway_J
VPC_D <-> Trangit_gateway_J
VPC_E <-> Trangit_gateway_J
```


## VPC peering과 Transit gateway의 차이
**VPC peering은 단 두 개의 VPC만 연결할 수 있다는 점**과 **Transit gateway는 여러 개의 VPC를 연결할 수 있다는 점**이 차이다.
VPC들이 각각 통신하게끔 설정하려면 하나의 transit gateway에 모두 연결하여 사용하면 되고, 두 개씩 짝지어 연결하려면 VPC peering을 사용하면 된다.
하나의 VPC,Transit gateway에 각각 연결할 수 있는 VPC 개수의 차이, 요금 차이 그리고 최대 대역폭도 차이가 있다.  

