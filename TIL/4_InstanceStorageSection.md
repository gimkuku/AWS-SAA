
#### EBS Volume


**EBS 란?**

-   Amazon Elastic Block Store
-   EC2 는 기본적으로 인스턴스 스토어라는 스토리지 제공. 그러나 인스턴스 종료, 중지 때 불완전하다는 단점을 가지고 있음  
    (=임시스토리지)
-   데이터 보존을 위하여 EBS 필요 (=하드디스크 같은 역할) 
-   데이터 보존, 손쉬운 데이터 이동, 손쉬운 백업, 암호화 등의 기능 제공 


**EBS 의 특징**

-   한번에 하나의 인스턴스만 연결 가능 
-   **network drive :** 물리적 드라이브가 아니기 때문에, EC2와 소통 시 지연이 있음. but 다른 인스턴스로 이동이 간편
-   **AZ(Available zone)에 속해 있음 :** AZ를 다르게 연결을 할 수 없음 ( 하고싶으면 snapshot을 이용해야 함 )
-   **Provisioned capacity :** 준비된 용량만큼 가격을 냄 (용량 추가도 가능)
-   EC2 가 종료될 때 사라지게 할지 말지 옵션을 선택할 수 있음 (Delete on Termination)




#### EBS Snapshot

-   백업을 손쉽게 할 수 있도록, EBS의 특정 순간의 상태를 저장해두는 것. 
-   EBS Snapshot을 가지고 EBS 를 복제할 수 있으며, AZ를 바꿔서 EBS를 사용할 수 있음


#### AMI

-   Amazon Machine Image
-   EC2를 커스터마이징하여( 내가 원하는 대로 바꾸어서) 저장해둘 수 있음
-   snapshot과 비슷. 특정 상태의 머신 상태를 저장해두는 것
-   특정 상태의 머신 상태를 AMI로 저장한 후, 이를 이용하여 서버를 만들면 머신을 복제한 것과 같음
-   특정 지역에서 만들어지며, 지역을 넘어 복제할 수 있음


**AMI 의 종류**

-   A Public AMI: AWS가 제공하는 AMI
-   Your own AMI: 사용자가 만들어서 저장해 둔 AMI
-   An AWS Marketplace AMI: 다른 사용자가 만들어서 마켓에서 판매하고 있는 AMI 


**AMI 사용방법**

-   EC2를 배포하고, 원하는대로 customizing
-   인스턴스 중지 -> AMI 만들기
-   만든 AMI를 이용하여 새로운 인스턴스 만들기




#### EC2 instance store

-   EBS - network storage 로 성능에 한계가 있음
-   EC2 instance store : 높은 성능의 hardware disk가 필요할 때 사용


**EC2 instance store의 특징**

-   I/O 성능이 좋음
-   인스턴스 멈추면 삭제됨
-   buffer / cache/ scratch data/ temporary content 등에 적합
-   백업, 복제는 사용자의 몫.. 


#### EBS Volume Type


**SSD type**

-   gp2 / gp3 : 일반적인 목적. 적당한 가격과 성능으로 무난무난
-   io1 / io2 : 높은 성능의 SSD로 특정 목적이 있을 때 사용. 적은 지연과 높은 처리량
    -   databases workload 에 적당 ( 스토리지 성능과 consistency에 민감할 때)
    -   EBS multi-attach를 지원함


**HDD type**

-   st1 : 적은 비용. 잦은 접속이 있을 때 집약적인 처리를 해야되는 워크로드에 적당
-   sc1 : 가장 적은 비용으로 잘 접근 안하는 워크로드에 적당




#### EBS Multi Attach - io1 / io2

-   같은 AZ에 있을 때 하나의 EBS가 여러 인스턴스에 연결할 수 있음
-   각 인스턴스들은 모두 read & write 가능


**EBS Multi attach 사용하는 경우** 

-   clustered 되어있는 리눅스 시스템에서 높은 가용성을 보장하고 싶을 때
-   반드시 같이 읽고 쓰는 기능을 제공해야 할 때 




#### EBS Encryption


**암호화 된 EBS 볼륨이란?** 

-   볼륨 내부의 데이터가 암호화 되어있음
-   인스턴스와 볼륨을 오가는 데이터가 암호화 됨
-   모든 스냅샷이 암호화 됨
-   스냅샷으로 만들어진 새로운 볼륨도 암호화 됨


**EBS Encryption 의 특징**

-   사용자가 암호화와 해독을 신경쓰지 않아도 됨
-   암호화로 인한 지연이 거의 없음
-   암호화 되지 않은 snapshot도 암호화 가능  
    \-> 암호화 되지않은 EBS를 암호화 하려면 snapshot하고 암호화 한다음에 다시 EBS 생성하면 됨
-   사용하는 키 : KMS (AES-256)




#### EFS

-   Network File System 이지만 많은 AZ와 인스턴스에 연결될 수 있음


**EFS 특징**

-   높은 사용성 & 확장성
-   비쌈 ( gp2 의 3배 정도 가격)
-   사용한 만큼 돈냄 
-   content management, web serving, data sharing, Wordpress 에 적합
-   KMS 키를 이용한 데이터 암호화
-   Linux 베이스의 AMI에서만 가능!!!!


**EFS Scale**

-   몇천의 동시 NFS client 처리, 10GB/s 이상의 처리량
-   Petabyte-scale로 자동으로 커질 수 있음  


**Performance mode** (set at EFS creation time)

-   기본 : 지연이 적은 use case에서 사용(web server, CMS, etc…)
-   최대 I/O 일때 : 대기 시간 길어짐, 높은 병렬처리와 처리량 가능 (big data, media processing)


**Throughput mode**

-   Bursting (1 TB = 50MiB/s + burst of up to 100MiB/s)
-   처리량 : storage 사이즈와 관계없이 처리량이 정해져 있음 


**Storage Tiers** (lifecycle management feature – move file after N days)

-   기본 : 자주 접속하는 파일
-   Infrequent access (EFS-IA):비용 절감




#### EBS vs EFS


| **EBS** | **EFS** |
| --- | --- |
| 한번에 하나의 인스턴스만 연결 가능 | 한번에 여러개 인스턴스 연결 가능 |
| 하나의 AZ에 묶여 있음 (AZ를 옮기려면 snapshot) | 여러 AZ에서 접속 가능 |
| 비용이 고정적 | 사용한 만큼 비용 |
| EFS보다 쌈 | 비용이 비교적 비쌈(EFS-IA 로 비용 절감 가능) |
|   | 리눅스에서만 사용 가능 |
