# 트랜잭션 동시성 제어 정리

- 다수의 트랜잭션이 동시에 실행되는 경우, 트랜잭션의 ACID 원칙을 지키기 위해 트랜잭션 처리방식을 좀 더 고려해야 한다.

- 특정 트랜잭션을 처리하고 있고 Commit이 완료되지 않았는데 다른 트랜잭션이 같은 데이터에 접근한 경우 여러 문제가 발생할 수 있다.

<br>

---

## 트랜잭션 동시성 문제를 해결할 수 있는 방법

<br>

### **락(lock)**

- 2개 이상의 트랜잭션을 동시에 실행하는 비직렬 스케쥴(non-serial schedule)의 결과가 트랜잭션을 뒤섞지 않고 순차적으로 실행하는 직렬 스케쥴(serial schedule)의 결과와 같도록 보장하는 것이다.

- 이를 보장하는 트랜잭션 스케쥴을 '직렬화가능(serializable)' 이라고 한다.

- 데이터베이스의 데이터를 다른 사용자가 접근하지 못하도록 잠그는 것을 말한다. 락 잠금은 다수 트랜잭션의 동시 처리를 보장하는 동시성 제어 기법 중 대표적인 기법이다.

- 락을 사용하는 동시성 제어 기법에서는 트랜잭션 내에서 데이터에 SQL 명령문을 실행하기 전에 반드시 해당 데이터에 락을 설정하여 잠궈야 한다. 또, 설정된 락은 SQL 명령문 실행 직후가 아닌 트랜잭션 커밋 혹은 롤백이 실행됨과 동시에 일제히 해제해야 한다.

   <br>

### **락의 종류**

- 락에는 2가지 종류가 있다.

  - 공유 락(shared lock)

    - 데이터를 검색하는 SELECT 문을 실행하기 위한 읽기 전용 락, 읽기만 가능하고 수정은 불가능하다.

    - READ 연산 은 가능하지만, WRITE 는 불가, 여러 개의 트랜잭션이 동시에 걸기 가능

    - 공유 락이 설정된 데이터에 대해서는 추가적으로 공유 락을 설정할 수 있다.

    - 여러 트랜잭션 간에 공유 락을 같은 데이터에 설정할 수 있다.

  - 독점 락(exclusive lock)

    - INSERT, UPDATE, DELETE 문과 같은 데이터 변경을 위한 배타적 락

    - READ, WRITE 연산 둘다 가능, 한 개의 트랜잭션만 걸 수 있음

    - 특정 데이터에 대해서 오직 하나의 트랜잭션만 독점 락을 설정할 수 있다.

    - 이미 독점 락이 설정되어 있을 경우, 공유 락도 독점 락도 모두 추가적으로 설정할 수 없다.

    - 데이터에 대한 모든 공유 락과 독점 락이 해제된 이후에만 독점 락을 설정할 수 있다.

<br>

| 종류      | 공유 락                                               | 독점 락                           |
| --------- | ----------------------------------------------------- | --------------------------------- |
| 잠금 목적 | 읽기 목적                                             | 쓰기 목적                         |
| 잠금 시작 | SELECT 문 실행 시                                     | INSERT, UPDATE, DELETE 문 실행 시 |
| 잠금 해제 | SELECT 문 실행 직후 또는 트랜잭션 종료 후             | 트랜잭션 종료 후                  |
| 특성      | 다른 공유 락과는 양립하지만 독점 락과는 양립하지 않음 | 어떤 다른 락과도 양립하지 않음    |

<br>
<br>

---

### 락 설정과 해제

- 트랜잭션은 반드시 필요한 락을 먼저 설정한 이후에만 데이터에 접근할 수 있다. 트랜잭션이 커밋될 때는 모든 락을 해제해야 한다.

<br>

#### 락 단위와 락 설정

- 데이터베이스 안의 모든 데이터를 접근할 때는 먼저 락 테이블안에 해당 데이터에 대한 락 정보를 기록한다.

- 락 잠금 대상의 크기를 락 단위(lock granularity)라고 한다. 테이블 행 단위나 테이블, 데이터베이스 단위로 락을 설정할 수 있다.

- 가장 작은 락의 단위는 테이블의 행이다. 행 단위로 락을 설정하면 다른 행에 락을 설정할 수 있기 때문에 동시성은 좋아진다.

- 그러나, 많은 행을 여러 차례 반복해서 락을 설정하는 경우에는 페이지나 테이블처럼 보다 큰 범위로 락을 한 번에 설정하고 빨리 락을 해제하는 것이 더 성능을 향상시킬 수도 있다.

- 락 단위가 커지면 관리하기는 쉬워지지만 락의 충돌이 자주 발생한다. 락을 어느 단위로 설정하고 해제할지를 결정하는 것도 상황에 따라 결정해야 하는 중요한 문제이다.

<br>

#### 락 양립성

- 트랜잭션이 읽기 위해 데이터를 접근할 때는 공유 락(S-Lock)을 설정 해야 한다. 해당 데이터에 설정된 락이 없거나 공유 락만 설정되어 있다면 공유 락을 추가로 설정할 수 있다.

- 독점 락이 먼저 걸려 있다면 공유 락을 걸 수 없고 독점 락이 해제될 때까지 기다려야 한다.

- 트랜잭션이 변경하기 위해 데이터를 접근할 때는 독점 락(X-Lock)을 설정해야 한다. 해당 데이터에 공유 락이나 독점 락이 설정되어 있다면 추가로 독점 락을 걸 수 없고 모든 락이 해제될 때까지 기다려야 한다.

<br>

#### 2단계 락킹 규악과 락 해제

<br>

![lock_point](https://user-images.githubusercontent.com/50399804/124396818-e1c27980-dd46-11eb-9b0b-323b0c47bb24.png)

- 단지 락을 설정했다고 해서 동시성 제어가 완벽하게 이루어지지 않는다.

- 락을 적절히 설정했더라도 락을 너무 일찍 해제할 경우, 트랜잭션의 고립성이 손상되어 결국 데이터 일관성이 유지되지 않는다. 락 설정과 락 해제를 적절한 시점에 할 필요가 있다.

- 2단계 락킹 규악(2phased locking protocol)은 트랜잭션별로 락을 설정하는 과정과 락을 해제하는 과정 2단계로 진행함으로써 필요한 순간까지 획득한 락을 유지하도록 하는 규칙이다.

- 2단계 락킹 규악은 락을 계속 획득하면서 락을 확장하는 1단계, 반대로 락을 계속 해제하면서 락을 축소하는 2단계로 진행한다.

  - 1단계: 락 확장 단계

    - 접근하고자 하는 데이터에 대한 모든 락을 획득할 때까지 새로운 락을 지속적으로 요청하여 잠금을 설정한다.

    - 보유하고 있는 락을 해제할 수는 없고 락을 추가로 획득할 수만 있다.

  - 2단계: 락 축소 단계

    - 필요로 하는 모든 락을 획득하는 시점인 락 포인트가 되면 보유하고 있던 락을 점차적으로 해제한다.

    - 락을 계단식으로 하나씩 해제할 수도 있지만 대부분 완료(커밋 혹은 롤백) 시점에 한꺼번에 모든 락을 해제한다.

    - 일단 하나라도 락을 해제하기 시작하면 다시 새로운 락을 요청할 수는 없다.

<br>

#### 교착 상태(Dead Lock)

- 둘 이상의 트랜잭션의 락이 서로 얽혀서 영원히 풀리지 않는 상태를 말한다. 서로 필요한 데이터 중 일부에 대한 락을 획득하고 서로 상대방이 획득한 락이 풀리기를 기다리게 되면 교착 상태에 들어간다.

- 어느 한 쪽이라도 트랜잭션을 완료해야 락이 풀리고 상대 트랜잭션의 기다림 상태가 해결되는데, 두 쪽 모두가 기다림 상태로 어느 쪽도 완료할 수 없는 상태가 된다.

- 일반적인 교착 상태 회피 방법처럼 데이터를 모든 트랜잭션이 특정 순서에 따라 차례대로 락을 획득하도록 하는 것이 바람직하다.

- 또, 트랜잭셩을 구성하는 명령문들의 수를 최소화하여 트랜잭션의 처리 시간을 줄이도록 해야 한다.

- 만약 교착 상태가 발생한 경우, 트랜잭션 중 처리 시간이 가장 적게 소요된 트랜잭션을 교착 상태의 희생자로 지정하여 강제로 롤백할 필요가 있다.

<br>

- Mysql 에서 Lock은 [Select 쿼리](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking-reads.html) 를 이용하거나 [트랜잭션 고립 수준을 설정](https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-isolation-levels.html)하여 걸 수 있다.

---

<br>

> 참조 <br> 'MySQL과 모바일 웹으로 만나는 데이터베이스의 정석'
