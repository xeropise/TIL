### NoSQL의 개념?

- 인터넷 및 모바일을 통해 접하게 되는 데이터의 양이 상상할 수 없을 만큼 기하적으로 늘어남

- 기존 RDBMS에서 최근 환경을 감당할 수 없는 다양한 문제점들이 도출됨

- 이에 맞는 새로운 데이터 저장 관리 기술이 등장, 이를 NoSQL 이라고 함

- 기존의 RDBMS가 클라이언트/서버 플랫폼을 기반으로 한다면 NoSQL은 클라우드 컴퓨팅과 클라이언트/서버 플랫폼 모두를 기반으로 한다는 점이 차이점

- MongoDB, Redis, Cassandra, Neo4j 등과 같은 제품들이 있음

- 동일한 데이터 저장 구조는 아님 ( 데이터의 논리적, 물리적 구조가 다름. 설계 및 구축 운영, 관리 방법도 다름 )

- 이 중, Redis 는 대표적인 key-value DB 유형의 제품, MongoDB 는 도큐먼트 DB 유형의 제품

<br>

### NoSQL 제품을 언제... 어떤 유형의 제품을 선택해야 하는가?

1. 초당 5만 건 이상의 데이터가 발생하는가?

   - 초당 데이터 발생량이 5만~10만건 이상이 발생한다면, 기존 RDBMS로는 처리할 수가 없으므로 빅데이터 솔루션을 선택해야 한다.

   - RDBMS가 개발된 1970~1980년 대에는 이와 같은 데이터가 발생하지 않아, 이를 고려하여 개발되지 않았다.

2. 트랜잭션 제어가 필요한가?

   - RDBMS 에서는 COMMIT 과 ROLLBACK 명령어로 사용자가 직접 트랜잭션을 제어하여, 이를 통해 데이터의 입력, 수정, 삭제가 이루어 진다.

   - 예로 Hadoop Eco-System은 파일 시스템을 기반으로 하기 때문에 사용자가 직접 트랜잭션을 제어할 수 없으며 데이터를 공유할 수 없다.

   - 트랜잭션 제어가 필요한 비즈니스 환경이라면 NoSQL 제품을 선택해야 한다.

3. 데이터 무결성이 요구되는가?

   - RDBMS의 경우, Primary Key, Foreign Key, Not Null, Check, Unique 와 같은 제약 조건을 통해 원치 않는 데이터가 저장되는 것을 방지하기 위해 무결성 보장을 위한 설계를 함

   - 장점도 있지만, 매번 데이터를 저장할 때마다 무결성을 검증함으로써 성능 지연문제를 유발 시키게 됨.

   - 위와 같은 기능이 필요하지 않다면, key-value 혹은 Column-Family DB를 선택하는 것이 바람직 함.
     - 단 위와 같은 제품들은 무결성을 보장해 줄 수 있는 기능들이 없기 때문에 도큐먼트 DB가 대안이 될 수 있음. (도큐먼트 DB가 모두 무결성을 보장하는 것은 아님)

4. 수평적 DataSets 인가?

   - RDBMS의 경우 테이블 구조를 일반적으로 2차원 수평적 데이터 구조라고 표현하는데 여러 개의 컬럼으로 구성되는 열과 다수의 행이 교차되는 구조를 의미함

     - 여러 개의 컬럼으로 구성되는 열과 다수의 행이 교차되는 구조

   - 이러한 수평적 DataSets 구조로 저장 관리해야 한다면 도큐먼트 DB를 선택해야 함

5. 계층형 데이터 인가?

   - 계층형 데이터 구조는 오래 전부터 기업 환경에서 보편적으로 발생하고 있는 데이터 유형 중에 하나

   - 최근에는 그래프 DB를 통해 저장 관리할 수 있음

   - 기본적인 계층형 구조는 도큐먼트 DB로도 충분히 저장 관리 가능, 계층 Depth가 10~20레벨 이상인 경우, 선택적으로 그래프 DB 사용을 권장
