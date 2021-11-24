## MySQL 아키텍처

- MySQL 서버는 __사람의 머리 역할을 담당하는 MySQL 엔진__, __손발 역할을 담당하는 스토리지 엔진__으로 구분 가능

  - 스토리지 엔진은 핸들러 API를 만족하면 누구든지 스토리지 엔진을 구현해서 MySQL 서버에 추가해서 사용할 수 있다.

    

<br>

***

### MySQL 엔진 아키텍처의 전체 구조

![KakaoTalk_Photo_2021-11-24-22-59-50](https://user-images.githubusercontent.com/50399804/146200242-a9478862-6ec9-4f85-aa92-b0111561aa7f.jpeg)

- MySQL 서버는 다른 DBMS에 비해 구조가 상당히 독특하다. 

  - 사용자 입장에서는 거의 차이가 느껴지지 않는다.

    -  독특한 구조 때문에 다른 DBMS가 가질 수 없는 엄청 난 혜택을 누릴 수 있으며, 반대로 DBMS에서믄 문제되지 않을 것들이 가끔 문제가 되기도 한다.

      

- 일반 상용 RDBMS와 같이 대부분의 프로그래밍 언어로부터 접근 방법을 모두 지원한다.

  - C/C++, PHP, JAVA, PEARL, PYTHON, RUBY, .NET .... 등등

    

- MySQL 서버는 위에서 말했듯, 크게 MySQL 엔진과 스토리지 엔진으로 구분할 수 있다.



<br>

***

### MySQL 엔진

- 클라이언트로부터 접속 및 쿼리 요청을 처리하는 커넥션 핸들러와 SQL 파서 및 전처리기, 쿼리의 최적화된 실행을 위한 옵티마이저가 중심을 이룬다.

  - SQL 문장을 분석하거나 최적화하는 등 DBMS의 두뇌에 해당하는 처리를 수행한다.

  

- MySQL 은 표준 SQL ([ANSI SQL](https://okky.kr/seq/160882)) 문법을 지원하기 때문에 표준 문법에 따라 작성된 쿼리는 타 DBMS 와 호환되어 실행될 수 있다.



<br>



### 스토리지 엔진

- 실제 데이터를 디스크 스토리지에 저장하거나 디스크 스토리지로부터 데이터를 읽어오는 부분을 전담한다.

  

- __MySQL 서버에서 MySQL 엔진은 하나지만, 스토리지 엔진은 여러 개를 동시에 사용할 수 있다.__

  - 테이블이 사용할 스토리지 엔진을 지정하면 이후, 해당 테이블의 모든 읽기 작업이나 변경 작업은 정의된 스토리지 엔진이 처리한다.

  ```sql
  > CREATE TABLE test_table (fd1 INT, fd2 INT) ENGINE=INNODB;
  ```

  

- 각 스토리지 엔진을 성능 향상을 위해 키 캐시(MyISAM 스토리지 엔진)나 InnoDB 버퍼 풀(InnoDB 스토리지 엔진)과 같은 기능을 내장하고 있다.



<br>



### 핸들러 API

- MySQL 엔진의 쿼리 실행기에서 데이터를 쓰거나 읽어야 할 때는 

  __각 스토리지 엔진에 쓰기 또는 읽기를 요청하는데, 이러한 요청을 핸들러(Handler) 요청이라 하고, 여기서 사용되는 API를 핸들러 API__라고 한다.

  

- InnoDB 스토리지 엔진 또한 이 핸들러 API를 이용해 MySQL 엔진과 데이터를 주고 받는다.

  

- 이 핸들러 API 를 통해 얼마나 많은 데이터(레코드) 작업이 있었는지는 다음의 명령어로 확인할 수 있다.

```sql
SHOW GLOBAL STATUS LIKE 'Handler%';
```



<br>

***

### MySQL 스레딩 구조

![KakaoTalk_Photo_2021-11-24-23-16-16](https://user-images.githubusercontent.com/50399804/146202909-52ec3c12-5723-4923-95df-37b1d2b3af5b.jpeg)



- __MySQL 서버는 프로세스 기반이 아니라 스레드 기반으로 작동__한다.

  - 크게 포그라운드(Foreground) 스레드와 백그라운드(Background) 스레드로 구분할 수 있다.

  

- MySQL 서버에서 실행 중인 스레드의 목록은 다음과 같이 performance_schema 데이터베이스의 threads 테이블을 통해 확인할 수 있다.

```sql
> SELECT thread_id, name, type, processlist_user, processlist_host
  FROM performance_schema.threads ORDER BY type, thread_id;
```



- 전체 실행중인 스레드와, 백그라운드 스레드, 포그라운드 스레드 리스트를 확인할 수 있다.
  - 'thread/sql/one_connection' 항목이 실제 사용자의 요청을 처리하는 포그라운드 스레드라고 한다. (근데 검색해보니 안나온다..)

  

- 백그라운드 스레드의 개수는 MySQL 내용에 따라 가변 적일 수 있다.

  - 동일한 이름의 스레드가 2개 이상 씩 보이는 것은 MySQL 서버의 설정 내용에 의해 여러 스레드가 동일 작업을 병렬로 처리하는 경우다.



- 작성 중....

> 참조 Real MySQL 8.0