# 8. ElastiCache

### ElastiCache

- RDS 가 관계형 데이터베이스 다루는 방식과 같음
- 관리형 Redis와 Memcached가 있음
- in-memory database의 특징
    - 빠른 성능
    - 늦은 지연
- 읽기집약적 워크로드 → DB의 부하를 최소화 할 수 있음
- 상태를 ElastiCache에 저장할 수 있음
- OS 유지보수/ 패치적용, 최적화, 모니터링, 장애복구 및 백업 등을 도와줌

### DB Cache

- Application에서 ElastiCache 먼저 뒤짐
    
    → 찾으면 : Cache hit
    
    → 못찾으면 : Cache miss → DB로부터 읽어옴 → Cache에 적음 
    
    ⇒ DB를 매번 뒤지지 않아도 됨 = 빠른 속도
    

### Session Store

- User 는 Application 서버로 접속
    
    → Application 서버는 세션 데이터를 ElastiCache에 적음
    
    → 만약 user가 다른 application 서버로 접속했다면?
    
    → 다른 application 서버도 ElastiCache에서 세션 정보 받아올 수 있음
    

### Redis VS Memcached

| Redis | Memcached |
| --- | --- |
| 다중 가용영역 & 자동 복구 | 멀티 노드 (샤딩) |
| read replica로 읽기 성능 scale & 높은 가용성 | 가용성 별로 |
| AOF 방식의 데이터 쓰기 → 매번 수정시마다 쓰기 때문에 Data Durability | 지속성 별로 |
| 복구 & 회복 간단 | 복구 회복 힘듬 |
| single threaded architecture → 한번에 한개의 명령만 실행 | multi-threaded architecture |
| 잡다한 기능이 많음 ex ) Redis Sorted sets로 게임 실시간 랭킹보드 만들기  | 잡다한 기능 없음 |

### Cache Security

- IAM authentication을 지원하지 않음!!!!
- IAM policies 로 할 수 있는 것 → API 레벨의 보안

**Redis Auth**

- 레디스 만들 때, password/token을 세팅할 수 있음
- 추가적인 레벨

**Memcached**

- SASL- based authentication을 제공
    > SASL
    >
    > Simple Authentication and Security Layer : 인증과 데이터보안을 위한 프레임워크
