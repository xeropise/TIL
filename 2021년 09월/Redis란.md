> [레디스 공홈 주소](https://redis.io/)

- Redis는 오픈 소스로 데이터베이스, 캐시, 메시지 브로커로 사용되는 인메모리 데이터 구조의 저장소이다. 

  

- Redis는 strings, hashes, lists, sets, sorted sets 등의 자료구조를 range queries, bitmaps, hyperloglogs, geospatial indexes 그리고 streams 와 함께 제공한다.

  

- Redis는 내장 replication, Lua scripting, LRU eviction, transactions 과 다른 레벨의 on-disk persistence 를 가지고 있고, Redis Sentinel 을  통해 높은 유효성 및 Redis Cluster로 자동 파티셔닝을 지원한다.

  

- strings을 더하거나, hash 안에 값을 증가시키거나, list에 element 를 더하고, computing set intersection, union 그리고 difference 혹은 sorted set에서 높은 순위등을  얻는등의 원자 조작을 할 수 있다.

  

- 높은 퍼포먼스를 이루기 위해, Redis는 인 메모리 데이터셋으로 동작하며, 사용 케이스에 따라서 디스크에 데이터셋을 정기적으로 저장한다거나 로그 커맨드를 남기는 등 다양한 작업을 할 수 있다.

  

- Redis는 또한 비동기 replication을 지원해서 매우 빠른 논블럭킹 동기화와 auto-reconnection with partial resynchronization on net split의 작업을 할 수 있다.
  

- 이외에도 다음과 같은 작업들을 지원한다.

  - 트랜잭션
  - Pub/Sub
  - Lua scripting
  - Keys with a limited time-to-live
  - LRU eviction of keys
  - Automatic failover

