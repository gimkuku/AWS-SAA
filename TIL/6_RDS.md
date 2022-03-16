# 6. RDS

### RDS

- RDS : Relational Database Service
- 클라우드 상에 관계형 데이터베이스를 만들 수 있음
    - ex) Postgres, MySQL, Maria DB, Oracle, Microsoft SQL Server, Aurora

**왜 RDS 를 쓰면 좋을까?** 

- 자동으로 용량을 준비해주고 OS 를 patching해줌
- 백업과 특정 시간으로 복구가 쉬움
- 모니터링 대시보드를 제공
- Read Replica
    - 읽기 전용으로 복사본을 만들 수 있음
    - 사용 시간은 길지만 읽기만 하면 될 때 유용 ( 예, DB 사용 보고서 작성 등)
- Multi AZ setup
    - 하나의 가용영역(AZ - Available zone)이 재난이 났을 때 다른 지역에서 복구가 가능하도록, 
    여러 가용영역으로 복사본을 만들어 두는 것
- 사이즈 키우는 게 쉬움
- EBS로 복구해둘 수 없음

**백업의 종류**

1. 자동 백업
    - 매일 전체 데이터 백업해둠
    - 5분마다 트랜잭션 로그 백업
        
        → 언제든지 트랜잭션 로그를 이용하여 복구할 수 있음!
        
    - 기본 7일 보존 (35일까지 늘릴 수 있음)
2. DB 스냅샷
    - 사용자가 원하는 특정 시점을 저장하기 위해 스냅샷을 찍어둠
    - 계속 보존되고, 언제든지 스냅샷을 이용하여 특정 시점으로 복구 가능

**Auto Scaling**

- RDS 가 용량이 부족하다고 감지하면 자동으로 확장이 됨
- 수동으로 확장 X
- 확장의 제한을 걸어두어야 함!!! → “Maximum Storage Threshold”
- 다음과 같은 경우 자동으로 확장이 일어남
    - 10% 아래로 Free storage가 남았을 때
    - 최소 5분 Low-storage 가 유지될 때
    - 최종 수정 후 6시간이 지났을 때
- 워크로드를 예측할 수 없는 애플리케이션에서 유용하게 사용~.~
- 모든 RDS DB를 지원함

**Read Replicas**

- 읽기 작업을 위해서 읽기만 가능한 복제본을 복사해두는 것
- 5개까지 가능
- 같은 가용영역, 다른 가용영역, 다른 리전까지 가능
- ***비동기*** ***ASYNC* : 연속적으로 복사가 계속 되는 것이 아님**
    
    **Network Cost**
    
    - 같은 지역 Read Replica 만들기 : 무료
    - 다른 지역 Read Replica 만들기 : **유료**
    

**Multi AZ**

- ***동기*** ***SYNC*** : 연속적으로 동기화 되어서 복제 됨
- availabiltiy 증가!
- 사고가 나서 특정 AZ의 데이터베이스가 고장났을 때 복구가 가능함!!
- 무료>~<

**Snapshot**

- 다른 지역으로 RDS 를 옮기고 싶다면?
    - snapshot 찍고
    - 새 AZ에서 복구한다
        
        → 동기화 된 두개의 데이터 베이스 생성! 
        
- DB를 암호화하고 싶다면?
    - snapshot찍고
    - snapshot을 암호화 하여 복제하고
    - 암호화된 snapshot으로 DB 복구!
        
        → 원래 DB가 암호화 되어있지 않다면, 다른 read replica도 암호화 할 수 없음
        
    - SSL 로 암호화 하는 것도 가능
        - PostgreSQL : rds.force_ssl
        - MySQL : GRANT USAGE ON **.** TO ‘mysqluser’@’%’ REQUIRE SSL;
    
     
    

**보안 - Network & IAM** 

- **Network Security**
    - RDS 는 항상 Private Subnet에!!
    - security group을 설정해서 다른 곳과 소통하게 해야 함
- **Access Management**
    - IAM Policy 들로 설정을 할 수 있음
    - db 로그인 시 : username & password 사용
    - MySQL & PostgreSQL : IAM based authentication 사용 가능
        - 토큰으로 RDS 접근 가능
        - SSL 로 암호화 되게 네트워크 접근 가능
        - IAM 에서 유저를 관리 해줌
