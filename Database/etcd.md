Etcd는 분산 시스템의 상태를 조정하고 관리하는 데 필요한 중요한 데이터를 저장하고 검색하는 데 사용되는 분산 키-값 저장소.

클러스터의 여러 노드에 데이터를 복제 저장하여 일부 노드에 장애가 발생하더라도 시스템 전체가 계속 작동할 수 있도록 보장. 이러한 복제는 일부 데이터의 손실조차 심각한 문제를 일으킬 수 있는 시스템에서 매우 중요.

etcd의 데이터는 컴퓨터의 경로 내 파일처럼 계층적 방식으로 저장된다. 이 구조는 수 많은 키와 값이 필요한 복잡한 시스템을 다룰 때 체계적인 데이터 관리와 효율적인 검색을 가능하게 한다.

분산된 특성에서 일관성을 보장하기 위해 Raft 합의 알고리즘을 사용. Raft는 시간의 텀을 두고 각 텀마다 로그 복제를 관리하는 단일 리더를 선출. 리더가 실패하면 새로운 리더가 선출되어 지속적인 가용성과 일관성을 보장한다.

### 분산 키-값 저장소인 redis와 비교

- 핵심 목적 및 디자인 철학에서의 차이점
  - redis는 주로 데이터베이스, 캐시, 메시지 브로커로 사용되는 인메모리 데이터 구조 저장소로 설계. 성능을 강조하며 캐싱, 세션 관리 또는 실시간 분석과 같이 빠른 데이터 접근이 필요한 시나리오에 적합
  - etcd는 공유 구성 및 서비스 검색을 위해 설계. 분산 시스템의 안정성과 일관성을 위해 최적화되어 있으며, Raft 합의 알고리즘을 사용하여 데이터가 노드 간에 올바르게 복제되도록 보장.
- 데이터 지속성
  - redis는 속도를 위해 주로 메모리에서 동작하지만, 데이터 손실을 방지하기 위해 디스크에 대한 지속성 옵션을 제공. 하지만 지속성 메커니즘(스냅샷 및 추가 전용 파일)은 분산 시스템 전반에서 데이터 일관성에 대한 보장이 아니라 재시작 시 복구에 초점이 맞춰져 설계.
  - etcd는 일관성과 내결함성에 중점을 두고 데이터를 디스크에 즉시 보존하도록 설계. 네트워크 파티션 혹은 노드 장애가 발생하더라도 데이터 무결성과 가용성이 중요한 사례에 더 적합
- 사용 사례
  - redis는 자주 접근하는 데이터 캐싱, 세션 저장, 실시간 분석, 메시지 브로커링을 위한 큐로 사용
  - etcd는 분산 시스템 조정 및 상태 관리에 필요한 중요한 시스템 데이터를 저장하는 데 사용(대표적으로 쿠버네티스 상태 저장)