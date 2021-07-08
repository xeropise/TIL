## 트랜잭션 고립 수준(ISOLATION LEVEL)

- 락을 설정하는 목적은 적절한 수준에서 트랜잭션 동시 실행이 이루어지도록 하기 위해서이다.

- 락 설정을 생략하거나, 락 관리를 느슨하게 하면 원하지 않은 결과를 가져올 수 있다.
- 반대로 락 설정을 엄격하게 관리하면 데이터의 동시 접근성이 크게 감소하여 성능이 문제가 될 수 있음.

- 트랜잭션의 고립 수준(ISOLATION LEVEL) 에 따라 DML 의 락수준을 달리할 수 있다.

<br>

| 고립 수준        | 발생 가능 상황                                        | 락 설정                                                                                                                                           |
| ---------------- | ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| READ UNCOMMITTED | Dirty Read <br> Non-Repeatable Read <br> Phantom Read | SELECT - 락 관계없이 자유롭게 읽음 <br> UPDATE,DELETE - 독점 락 설정 및 유지                                                                      |
| READ COMMITTED   | Non-Repeatable Read <br> Phantom Read                 | SELECT - 공유 락 설정, 실행 후 즉시 해제 <br> UPDATE,DELETE - 독점 락 설정 및 유지                                                                |
| REPEATABLE READ  | Phantom Read                                          | SELECT - 공유 락 설정 및 유지 <br> UPDATE,DELETE - 독점 락 설정 및 유지                                                                           |
| SERIALIZABLE     | 없음                                                  | SELECT - 공유 락 설정 및 유지, <br> 인덱스 공유 락 설정 및 유지 (다른 트랜잭션 INSERT 문 수행 방지) <br><br> UPDATE,DELETE - 독점 락 설정 및 유지 |

<br>

```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;

SHOW VARIABLES like 'tx_isolation';
```

<br>

---

<br>

## 트랜잭션 동시성문제로 인한 발생 가능한 상황

1. Dirty Read

   - 트랜잭션이 완료되지 않은 상황에서 데이터에 접근을 허용할 경우 발생할 수 있는 데이터 불일치이다.

   - 예로 트랜잭션 A가 값을 1 -> 2로 변경하고, 커밋하지 않은 상황에서 트랜잭션 B가 같은 값을 읽는 경우 트랜잭션 B는 2가 조회 된다. 근데 이때 A가 값을 롤백하면 잘못된 값을 읽게 된다.

<br>

1. Non-Repeatable Read

   - 한 트랜잭션에서 같은 쿼리를 두번 실행했는데 데이터 불일치가 나는 것이다.

   - 트랜잭션 A가 값 1을 읽었고 한번 더 실행하려고 하는데 그 전에 트랜잭션 B가 값을 1 -> 2 로 변경하고 커밋하면 A의 두번째 실행 쿼리 결과가 달라진다.

<br>

2. Phantom Read

   - 한 트랜잭션에서 일정 범위의 레코드를 2번 이상 읽을 때 발생할 수 있는 데이터 불일치를 말한다.

   - 트랜잭션 A가 어떤 조건을 사용하여 특정 범위의 값들을 읽었고, 같은 쿼리를 실행하기 전에 트랜잭션 B가 같은 테이블에 값을 추가하거나 지운다면, 같은 쿼리를 날릴 A의 결과가 달라진다.

   - Non-Repeatable Read 는 단일 로우에 대한 것이라면, Phantom Read 는 범위에 대한 것이다.

> 참조 <br> https://goddaehee.tistory.com/167 <br> 'MySQL과 모바일 웹으로 만나는 데이터베이스의 정석'
