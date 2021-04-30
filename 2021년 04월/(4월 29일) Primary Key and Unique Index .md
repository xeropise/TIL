# PK 와 Unique Index

## Primary Key

- PK는 해당 컬럼이 그 테이블의 식별자임을 나타내는 것으로, 자신과 다른 로우가 서로 다른 인스턴스임을 확인할 수 있게 해주는 역할을 한다. 

- Null 을 허용하지 않아, 데이터에 대한 일관성을 보장해, 데이터 중복이 발생하지 않아 빠르게 쿼리를 수행 가능하다.

- InnoDB Engine 기준으로 Primary Key 는 항상 Clstered Index 이며, Primary Key가 존재하지 않을 경우 Unique Key (Unique Index) 를 선택하게 된다. 후보가 없을 경우 Auto_Increment 속성의 컬럼을 활용하게 되는데 이는 테이블에 Primary Key 를 생성하지 않으면 시스템 적으로 고유값을 찾아서 그 값을 생성하게 된다.

_[Clustered Index, NonClustered Index](https://mongyang.tistory.com/75)_


***

## Unique Key(Unique Index)

- Unique Key 는 데이터베이스 행을 고유하게 식별할 수 있는 구조는 동일하지만, Null 을 허용하게 되면 중복된 데이터를 가질 수 있게 된다. 논리적으로 보조 식별자를 구분하기 위해서 사용하게 되며, 테이블 기준으로 여러 개를 생성 가능하다.

- InnoDB Engine 기준으로 Unique Key 는 Non-Clustered Index 이다.


***

## Primary Key 와 Unique key 차이점 

|Primary key        |Unique Key         |
|-------------------|-------------------|
|테이블에 반드시 하나만 존재 | 테이블에 여러 개 존재 가능|
|NULL을 허용 하지 않음| NULL 허용 가능|
|기본 Clustered Index|기본 Non-Clustered Index|
|데이터 무결성 보장 |데이터 무결성 보장(NULL은 여러 개 존재 가능)|
|제약 조건(constraint)|제약 조건 없음|


***

### Primary Key 가 반드시 필요한가?

- 관계형 데이터베이스에서 관계를 가지기 위해서 반드시 필요, 예로 부서와 사원 테이블이 있다고 하면, 부서 테이블에 식별자 "부서 코드" 를 사원 테이블에서 "부서 코드"를 FK 로 참조 가능,
  부서 테이블에 식별자가 존재하지 않는다면 이렇게 관계가 연결될 수 없게 된다.
  
- 테이블에 Primary Key 가 존재 하지 않는다면 중복된 데이터로 인해 데이터 정제 작업이 필요하며, 각 칼럼에 값을 어떤 우선순위에 따라 결과를 추출해야 한다. 데이터 품질 향상에 도움을 줄 수 있다.

- Primary Key를 생성하게 되면 최소의 Cost 를 사용할 수 밖에 없다. InnoDB Engine (Mysql, MariaDb) 은 기본적으로 데이터를 저장할 때 인덱스를 활용하기 위해 후보가 될 수 있는 Primary Key 를 생성하게 된다. InnoDb engine 은 Unique 값을 가진 컬럼을 찾아서 보이지 않는 Clustered Index 를 만들게 된다.


### Unique Index / Non-Unique Index

- Unique: 인덱스 값이 중복되지 않는 인덱스
- Non-Unique: 인덱스 데이터가 중복되는 인덱스

#### Index read (읽기)

- Unique Index 가 빠르다고 생각할 수 있는데, 이는 전혀 사실이 아니며 유니크 인덱스는 값이 유일하므로 값을 1개만 읽어 들이면 되고, 그냥 인덱스는 여러 개 읽어들어야 하는 것이 사실이지만, 사실 상 인덱스가 한번 읽어들여져서 각각 값을 비교하는 단계에서는 이미 CPU가 처리를 담당하고 있다. 그래서 성능상 영향이 거의 없다고 볼 수 있다.

- 인덱스는 유니크한 것들보다 중복이 허용되어 처음에 LOAD 해야 할 양이 많아 느린것 뿐이지, 인덱스 속성에 자체에 의한 Delay 는 아니다.

- Unique Index 와 Index 의 읽기 성능은 거의 차이가 없다.

#### Index write (쓰기)

- Unique Index 가 적용되어 있는 컬럼에 새로운 값이 INSERT 될 때는 값의 중복을 체크해야 하는 한 단계 과정이 더 필요하므로, 일반 인덱스 보다 느리다. 

- 중요한점은 MySQL 이 중복 값을 체크할 때 read Lock 을 걸고, 쓰기를 할때는 writeLock 을 사용하는데, 여기서 DeadLock 이 아주 빈번하게 발생한다

- InnoDB Engine 을 사용하는 경우, 인덱스 키의 저장을 버퍼링 할 수 있으나, Unique Index 의 경우 중복체크로 인해 버퍼링이 불가능하다.

__[버퍼링 ??](http://cloudrain21.com/mysql-innodb-basic-performance-tunning)__

- Unique Index 는 중복체크라는 과정으로 인해 Index 보다 쓰기가 더 느리다.

### Unique Index 사용 시 주의점

- 유니크한 성질(유일성)이 필요한 데이터 컬럼이라면 Unique Index를 생성하는게 당연하다.

- 성능이 더 좋아질 것으로 생각하고 불필요하게 Unique Index 를 생성하지 말자.

- 어느 컬럼에 Unique Index 가 존재하는 경우, 해당 컬럼과 비슷한 성질의 다른 컬럼에 Index 를 만들필요는 없다.
