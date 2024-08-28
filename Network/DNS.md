# 1. 도메인(Domain)
## 1-1. 도메인이란
>네트워크에서 도메인이란 www.example.com 과 같은 형태로, **특정 웹사이트나 리소스를 식별하기 위해 사용되는 고유한 이름**이다.

우리는 주소 창에 example.com이라는 도메인을 검색하면 그 도메인과 연결된 특정 웹사이트나 리소스에 연결되는 경험을 할 수 있다.

전화번호부를 생각해보자 010-0000-0000이라는 숫자를 모두 외우고 다니면 불편하기 때문에 우리는 전화번호부에 전화번호(숫자)를 의미 있는 문자열로 저장해 놓는다.

네트워크에서는 **IP주소를 의미 있는 도메인으로 매핑**해 저장해 놓는다.

## 1-2. 필요성
### A. IP주소는 기억 및 관리하기 어렵다.
도메인을 통해 어떤 서버와 연결되는 것은 서로 다른 PC간에 통신하는 것으로 볼 수 있다.
- 서로 다른 PC가 네트워크 상에서 통신할 때는 소켓 통신을 사용한다.
- 그리고 소켓 통신은 IP와 Port#를 통해 이루어진다.

특정 서버에 접속하기 위해 매번 123.123.123.123과 같은 IP주소를 외우고 다닌다고 생각해보자.
심지어 IPv6는 2001:0db8:85a3:0000:0000:8a2e:0370:7334와 같은 훨씬 더 복잡한 주소를 사용한다.


### B. 도메인은 유연하게 관리할 수 있다.
도메인을 사용하면 도메인이 가리키는 IP주소가 바뀌어도 사용자는 원래 사용하던 도메인을 통해 바뀐 서버에 연결될 수 있다.
- 웹사이트가 서버를 변경하거나 여러 서버를 사용해 서비스를 제공하는 경우 IP주소가 변경될 수 있다.


또한 하나의 도메인을 통한 연결 요청을 여러 IP(서버)로 분산시키거나 보안 서버를 경유하도록 하는 등 네트워크 환경을 유연하게 구성할 수 있다.

---

# 2. DNS(Domain Name System)

- DNS는 도메인 네임을 IP주소로 변환시켜주는 시스템이다.

  - 도메인과 IP주소를 매핑해 저장하고, 네트워크를 통신 과정에서 도메인 네임을( www.amazon.com)을 IP 주소(192.0.2.44)로 변환시켜 준다.

- DNS는 여러개의 DNS가 계층적(hierarchical)으로 연결된 분산형(distributed) 데이터베이스다.
  - 여러개의 루트 DNS와 그 서브 도메인들로 이루어져 있다.
  - 전세계의 IP주소는 너무 많다. 이를 효율적으로 관리하려면 계층형 아키텍처로 구성될 필요가 있다.

![](https://velog.velcdn.com/images/jjbin/post/75e23db8-b53e-4b66-a45c-050d79535084/image.png)




## 2-1. DNS 구성 요소

### A. DNS Resolver(Local Name Server, DNS Recursor)
DNS Resolver란 클라이언트(웹 브라우저, 응용 프로그램)로부터 DNS 쿼리를 받아 IP주소를 찾기 위해 **다양한 도메인 네임 서버와 통신하는 주체**이다.
- 클라이언트와 도메인 네임 서버 사이의 중개자
- 루트 네임 서버부터 단계적으로 접근하며 클라이언트가 요청한 도메인의 IP주소를 가지고 있는 네임 서버를 찾아 나간다.


<br>
도메인의 IP주소를 갖고 있는 DNS 네임 서버로부터 IP를 전달받아 캐시하고, 클라이언트에게 전달한다.

- 캐시의 역할도 수행
- 요청받은 도메인이 캐시되어 있는 경우 다른 DNS 네임 서버에 접근하지 않고 바로 결과 반환

<br>

대부분의 경우 DHCP(Dynamic Host Configuration Protocl)에 의해 ISP(Internet Service Provider)가 제공하는 DNS Resolver를 사용하지만 설정을 통해 특정 resolver를 사용할 수도 있습니다.
- cloudflare : 1.1.1.1
- google : 8.8.8.8
- 특정 resolver를 사용할 경우 관련 서비스 속도는 빨라지지만 다른 서비스 속도가 느려질 수 있음
<br>

### B. Root Name Server
루트 네임 서버는 DNS 계층 구조의 최상위에 위치하며 전 세계에 13개의 루트 서버 집합이 존재한다.

DNS Resolver가 특정 도메인의 IP주소를 찾을 때, 도메인 확장자(.com, .net, .org, etc..)에 따라 그 최상위 도메인(TLD)에 대한 정보를 제공한다.

- 전 세계에 13개 유형의 루트 서버로 구성되며 12개의 기관이 관리
  - 개수가 13개가 아니라 그 유형이 13개
  - 실제 인스턴스는 수백개 이상

<br>

### C. TLD(Top-Level Domain) Name Server
TLD 네임 서버는 .com, .net, .kr과 같은 url의 마지막 점 뒤에 오는 도메인 확장자 별로 하위 도메인 정보들을 가지고 있는 서버다.

TLD 네임 서버는 일반 최상위 도메인과 국가 코드 최상위 도메인으로 구분된다.
- 일반 최상위 도메인 : (.com, .net, .org, .edu, ...)
- 국가 코드 최상위 도메인 : (.kr, .us, .uk, .jp, ...)

![](https://velog.velcdn.com/images/jjbin/post/723d856d-99a0-4f8c-8cbf-851a3469f34d/image.png)
<br>

### D. Authoritative Name Server
권한있는 네임 서버는 특정 도메인에 대한 최종적인 정보를 제공하는 서버다.
- 일반적으로 IP주소를 확인하는 마지막 단계에 해당한다.
- 국내 서비스로는 가비아, 해외 서비스로는 Amazon Route 53 등이 해당한다.
- 도메인에 해당하는 A레코드(IPv4주소)나 AAAA레코드(IPv6주소), CNAME레코드(별칭) 등의 정보를 반환한다.

Authoritative Name Server에는 도메인의 서브 도메인에 대한 IP주소도 모두 저장된다.
- google.com, www.google.com, apis.google.com, play.google.com 등 관련 서브 도메인 IP주소 모두 저장

![](https://velog.velcdn.com/images/jjbin/post/75e23db8-b53e-4b66-a45c-050d79535084/image.png)

## 2-2. DNS 동작 과정


![](https://velog.velcdn.com/images/jjbin/post/fb94042a-4092-4909-8cf2-a39dc19668fe/image.png)

1) 클라이언트가 주소 창에 도메인을 입력한다.

2) 클라이언트가 입력한 도메인이 DNS Resolver에게 전달된다.
- DNS Resolver에 해당 도메인이 캐시되어 있다면 즉시 결과를 반환한다.

3) DNS Resolver는 Root Name Server로 요청을 전달하고 TLD Name Server 주소를 전달받는다.
- example.com을 보고 .com에 해당하는 TLD 서버 주소를 전달한다.

4) DNS Resolver가 TLD Name Server로 요청을 전달하고 Authoritative Name Server 주소를 전달받는다.

5, 6) DNS Reslover가 Authoritative name Server로 요청을 전달하고 도메인의 IP 주소를 전달받는다.
- 레코드를 조회하여 도메인의 주소를 전달한다.

7) DNS Resolver가 도메인의 IP주소를 클라이언트에게 전달한다.

8, 9) 클라이언트는 도메인에 해당하는 IP주소를 통해 서버에 접근하고 필요한 자원을 전달받는다.

<br>

## 2-3. DNS Round Robin
DNS Round Robin은 Authoritative Name Server에 의해 수행되는 부하 분산(Load Balancing)기술이다.
- 별도 장비 없이 DNS(Authoritative Name Server)에서 레코드 정보를 조회하는 시점에서 트래픽을 분산한다.


- 하나의 도메인 이름에 여러 IP 주소를 연결하고, DNS 서버가 이 IP 주소들을 순차적(Round Robin)으로 반환한다.

<br>
아래 이미지는 nslookup 명령어를 통해 naver.com을 조회한 결과 4개의 ip주소가 반환된 것이다.

이처럼 하나의 도메인에 여러 ip주소가 연결된 경우 DNS Round Robin을 적용하여 서버의 부담을 줄이고 가용성을 확보할 수 있다.

![](https://velog.velcdn.com/images/jjbin/post/21069479-43ab-44f5-af65-fd434dabc5d8/image.png)



# References.
>
[AWS, what is dns?](https://aws.amazon.com/ko/route53/what-is-dns/)

[CloudFlare, DNS란 무엇인가?|DNS 작동 방식](https://www.cloudflare.com/learning/dns/what-is-dns/)

[CloudFlare, DNS 서버 유형](https://www.cloudflare.com/ko-kr/learning/dns/dns-server-types/)

[CloudFlare, DNS 루트 서버](https://www.cloudflare.com/ko-kr/learning/dns/glossary/dns-root-server/)

[팬도라, 라운드로빈 DNS (Round-Robin DNS), tistory](https://judo0179.tistory.com/entry/%EB%9D%BC%EC%9A%B4%EB%93%9C%EB%A1%9C%EB%B9%88-DNS-Round-Robin-DNS)

[널널한 개발자 TV, 전세계 인터넷을 멈추는 방법과 DNS, Youtube](https://www.youtube.com/watch?v=XXzxetbAIfA)
[KISA 한국인터넷진흥원, 국가DNS 운영](https://www.kisa.or.kr/1041103)
