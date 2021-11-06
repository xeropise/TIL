- 인덱스는 특정 칼럼 값의 로우를 찾기위해 사용되어진다. 일반적으로 인덱스 없이 어떤 로우를 찾으려고한다면 첫 줄부터 찾으려고하는 Full table Scan을 실행하게 된다.

  

- 테이블이 크고 비용이 클수록, 인덱스가 모든 데이터를 볼 필요 없이 빠르게 데이터를 찾아낸다.

  

- MySql Index (Primary key, Unique Index, FullText) 는 B-Tree 로 저장되고, 공간적인 데이터 타입의 인덱스는 R-Tree 로 저장된다.

  

- MySQL 은 Index 를 다음과 같은 동작에 실행한다.

  - Where 절에 맞는 로우를 빨리 찾기 위함

    

  - 고려사항에서 로우를 빨리 제거하기 위해, 다양한 Index 중에서 선택한다면, 보통 Mysql은 가장 적은 로우를 찾아내는 인덱스를 사용한다.

    (중복도가 낮은 것이 결과가 가장 적은 로우이다. 여러 컬럼으로 인덱스를 잡는다면 중복도가 낮은 것 ~ 높은 것으로 잡아야 한다.)

    > 중복도가 낮으면 카디날리티가 높다. 

    

  - 만약 테이블이 복합 인덱스를 가지고 있다면, 인덱스의 제일 왼쪽 접두사가 옵티마이저에 의해 사용된다.

    - 예를들면 3개의 칼럼 인덱스(col1, col2, col3) 가 있다면 (col1), (col1, col2), (col1, col2, col3) 로 찾을 수 있게 색인할 수 있다.

      

    - 조회 조건에 첫번째 인덱스가 포함되어 있지 않는다면 조회 쿼리 사용 시 인덱스를 태우지 않는다. 반드시 첫 번쨰 인덱스를 포함할 것

      
      
    - **복합 인덱스를 사용한다면 제일 좌측의 인덱스는 카디날리티가 높은 것으로 사용하는 것이 빠르다고 한다.**

      

  - Join을 통하여 다른 테이블로부터 로우를 검색하는 경우, Column 들이 같은 타입과 같은 사이즈로 선언되어 있는 경우, 인덱스를 더 효율적으로 사용할 수 있다. 

    - VARCHAR 와 CHAR 는 같은 사이즈라면 같은 것으로 간주된다.

      

    - 비교를 위해서 Nonbinary string 컬럼의 경우, 같은 character set 을 사용해야 한다.

      

    - 유사하지 않은 칼럼(string 과 temporal 또는 numeric) 끼리의 비교는 conversion 이 제대로 이루어지지 않았을 경우, index의 사용을 막을 수 있다.

      - 예로 numeric 1의 경우 string 의 '1', ' 1', '00001', '01.e1' 과 동등하다고 비교할 수도 있다.

        

  - 특정 칼럼의 MIN(), MAX() 를 찾는 경우, 특정 칼럼의 인덱스가 동작하기 전에 'where column = 상수' 방식으로 사용하고 있는지 전처리기가 확인함으로써 최적화된다. 

    

  - 사용가능한 인덱스의 제일 왼쪽 접두사로 테이블을 정렬하고 그룹핑이 가능하다. (ORDER BY key_part1, key_part2) 다만, DESC 로 key_part가 동작한다면, 거꾸로 동작한다. (descending index 의 경우, ascending 순서로 바뀐다.)

    

  - 쿼리 자체를 최적화할 수도 있다.(인덱스가 [커버링 인덱스](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_covering_index) 라고 불리우는 쿼리에 필요한 모든 결과를 제공한다.) 만약 쿼리가 몇 인덱스에 포함된 컬럼만 사용하는 경우, 매우 빠르게 검색해 올 수 있다.

    
  
  - 참고로 조회 검색 시 인덱스 순서와 조회 순서를 반드시 지킬 필요는 없다.  옵티마이저가 조회 조건의 컬럼을 인덱스 순서에 맞춰 재배열하지만 거의 차이가 없다. 하지만,  group by로 정렬하는 경우, 인덱스 순서를 맞춰주는 것이 좋다.
  
    

> https://dev.mysql.com/doc/refman/8.0/en/mysql-indexes.html
>
> https://jojoldu.tistory.com/243
>
> https://dev.mysql.com/doc/refman/8.0/en/multiple-column-indexes.html