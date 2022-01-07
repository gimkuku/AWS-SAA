#### Scalability & Availability


**Scalability :** 서비스가 부하가 크게 왔을 때 조정을 하여 처리할 수 있는지

-   Vertical Scalability : 사이즈를 키우는 것 (ec2 : t2.micro -> t2.large)
    -   주로 database에서 사용 ( RDS, Elastic Cache)
    -   보통 하드웨어적인 한계가 있음
-   Horizontal Scalability (=elasticity) : 대수를 늘리는 것
    -   분산 시스템에서 주로 사용되며 현재 많이 사용하고 있는 scale up 방법
    -   cloud offering의 발전으로 Horizontal Scalability 가 굉장히 쉬워짐
    -   Auto Scaling Group, Load balancer

 
**High Availability** : 최소 2곳의 data center(AZ)를 사용




#### Load Balancing


-   여러 서버로 트래픽을 분산해주는 역할


**Load Balancer의 장점**

-   하나의 DNS 노출로 여러 서버를 사용할 수 있음
-   인스턴스가 failure 되도 빠르게 처리할 수 있음
-   주기적인 health check 를 해줌
-   SSL 보안 인증을 할 수 있음
-   쿠키를 통해 stickness 를 처리해줄 수 있음 (같은 user의 트래픽은 모두 같은 서버로 가는 것)
-   High Availability (여러 AZ를 자유롭게 사용할 수 있음)
-   public traffic 과 private traffic 을 분리할 수 있음


**그렇다면 왜 AWS Elastic Load Balancer?**

-   AWS가 관리하는 load balancer로 업그레이드, 지속 등이 보장되어있음
-   직접 load balancer를 만드는 것보다 싸고 기능이 많음
-   AWS에서 제공하는 다른 기능과 함께 사용하기 좋음




#### Health Check

-   load balancer에 연결되어있는 인스턴스들에게 주기적으로 트래픽을 보내 사용 가능한지 request 를 받는 것
-   특정 포트와 route에서 실행 됨 (보통 /health)




#### Types of Load Balancer


**Classic Load Balancer \_ CLB**

-   Old generation (2009)
-   HTTP, HTTPS (Layer 7)
-   TCP, SSL (secure TCP) (Layer 4)


**Application Load Balancer \_ ALB**


-   New generation (2016)
-   HTTP, HTTPS, WebSocket (Layer 7)
-   머신들 간에 Load balancing 가능 - target group  
    -   URL endpoint로 load balancing 가능 : /users 와 /posts
    -   hostname으로 load balancing : test.example.com 과 m.example.com
    -   Query String, Header로 load balancing : users?id=1211
-    target group : EC2 하나, EC2 instances, Lambda, IP Address
-   하나의 머신에서 다양한 application으로 load balancing 가능 - container 등
    -   microservice & container-based application 에 적합


**Network Load Balancer \_ NLB**

-   New generation (2017)
-   TCP, TLS (secure TCP), UDP (Layer 4)
-   하나의 AZ에서 하나의 고정 IP만 사용할 수 있음
-   Elastic IP 사용 가능
-   TCP, UDP 트래픽에서 굉장한 성능!! 
-   Target group으로 ALB를 설정할 수 있음
-   target group - EC2 instance, private IP, ALB) 


**Gateway Load Balancer \_ GWLB**

-   New generation (2020)
-   AWS에서 application으로 가기 전 거쳐 가야하는 기능을 제작할 때  
    ex ) 방화벽, 침입 감지 및 탐지 시스템 등
-   Operates at layer 3 (Network layer) – IP Protocol
-   Transparent Network Gateway + Load Balancer
    -   **Transparent Network gateway :** 트래픽을 하나의 입구와 출구로 거쳐 지나가게 하는 것
-   target group : EC2 instances, private IP Addresses


**Cross-Zone Load Balancing**

-   AZ에 관계없이 로드 밸런서에 붙어있는 인스턴스가 모두 동일하도록 부하를 나눔
-   az-1의 인스턴스 2개로 50, az-2의 인스턴스 10개로 50을 보내면 25,25, 5,5,5,5,5,5,5,5,5,5 로 나누어지는 게 아니라,  
    모든 인스턴스가 100/12 로 트래픽이 나누어짐
-   ALB : 반드시 Cross zone
-   NLB : default는 crosszone이 아니지만, 추가 요금을 내면 사용 가능
-   CLB :default는 crosszone이 아니지만, 추가 요금 없이 사용 가능




#### SSL Certificates

-   SSL 인증서를 통해 client 와 server 가 보안 통신을 할 수 있음
-   SSL : Secure socket layer (old version)
-   TLS : Transport Layer Security (new version)
-   ACM을 이용하여 인증서를 올리고 ALB에 설정을 해주면 ALB로 오는 보안 통신을 인증할 수 있음 


**SNI**

-   SNI : server name indicator
-   하나의 웹 서버에 (하나의 ALB를 사용하여 ) 여러개의 SSL 인증서를 올려놓을 수 있음
-   서버 네임에 따라 해당하는 SSL 사용하여 Load balance
-   ALB , NLB, Cloadfront에서만 사용 가능 




#### Connection Draining

-   서버가 unhealthy 하면 그쪽으로 connection을 보내지 않는 설정
