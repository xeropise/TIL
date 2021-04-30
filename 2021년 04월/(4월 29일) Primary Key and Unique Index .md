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
