# 7. Aurora

## Aurora

- AWS에만 있는 저장 장치
    
    → AWS 클라우드 최적화가 되어있고 성능이 MySQL의 5배, Postgre의 3배 정도 좋다고 함.. 
    
- 자동으로 증가
- 최대 15개의 복제본 & 복제 속도 빠름
- Failover 의 즉시 실행 ⇒ High Availability
- RDS보다 비쌈

### High Availability

- 고가용성이란? (High Availability)
    - 장애가 생겨도 빠르게 복구가 되는 것
    - ex) 다른 가용영역에 데이터 센터 위치 시, 하나의 데이터 센터가 고장나도 다른 가용영역의 데이터 센터에서 복구 가능
- **Cross-Region Replication** 가능
- Aurora의 자동 복구
    - 30초 내로 마스터를 자동 복구할 수 있음
    - peer-to-peer replication으로 자동 복구 가능
- 15개 까지 Read Replica 만들 수 있음

![Untitled](7%20Aurora%2010734/Untitled.png)

- Writer Endpoint → 마스터를 가리킴
- Reader Endpoint → LB로 연결되어 Read Replica를 가리킴

### Aurora 의 기능

1. 자동 백업
2. 백업과 회복
3. 독립성과 보안
4. 규정 준수
5. Down time없이 자동 패치
6. Advanced Monitoring
7. 정기적인 유지 보수
8. Backtrack : 백업 없이 데이터 언제든지 복원할 수 있음

### Aurora의 보안

- RDS 랑 비슷 ( 같은 엔진 사용!)
- KMS 를 이용한 rest 상태의 암호화 → replica나 스냅샷도 암호화!
- 자동 백업
- IAM 토큰으로 인증 가능
- security group으로 보호 해야 됨
- SSH 접근 X

### Aurora Serverless

- 사용양에 따라 자동으로 scaling 됨
- 간헐적으로 예측 불가능한 워크로드에 적합
- 용량 계획 필요 없음
- 초당 과금 → 비용 효율적

### Aurora Multi-master

- 대부분의 Aurora 클러스터 → 단일 마스터
- 단일 마스터 : 마스터만 모든 쓰기 작업 & 나머지는 읽기 전용
    
    → 만약 마스터가 사용 불가 상태 시 : 읽기전용 인스턴스 중 하나가 새 라이터로 승격
    
- 멀티 마스터 : 모든 DB인스턴스 쓰기 작업
    
    → 장애 조치 없음. 걍 다른 DB인스턴스가 인계해서 사용하면 됨
    
    ⇒ “**Continuous Availability** : 지속적인 가용성” 
    

### Global Aurora

- Cross Region Read Replica
    - 지역 넘어서 Read replica 만들 수 있음
    - 재난 복구에 유용
- Gloabal Database
    - 1개의 주 지역을 가짐 (읽기 쓰기 모두 가능)
    - 최대 5개의 보조 지역을 가짐 (읽기 전용)
    - 초당 최대 16개의 read-replica
    - 지연시간 단축에 도움
