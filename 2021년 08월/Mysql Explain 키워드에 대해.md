```sql 
EXLPAIN tbl_name 

or

EXPLAIN [EXTENDED] SELECT select_options
```



- EXPLAIN 을 사용함으로써, 테이블의 어느 곳에 인덱스를 추가해야만 SELECT 가 보다 빠르게 되는지 알 수 있다.

  

- 옵티마이저가 최적의 순서로 테이블을 조인할 수 있는지 여부도 검사할 수 있다.

  

- SELECT 문에 명명한 테이블의 순서와 똑같이 조인 순서를 사용하도록 옵티마이저를 만들려면, SELECT 문 대신에 SELECT STRATIGH_JOIN 을 사용해서 시작하면 된다.

  

- EXPLAIN 은 SELECT 명령문에서 사용된 각 테이블 정보 열을 리턴한다.

  

- EXTENDED 키워드가 사용되면,  EXPLAIN 명령문 다음에 SHOW WARNINGS 명령문을 입력해서 볼 수 있는 기타 정보를 리턴한다.

  - 옵티마이저가 SELECT 명령문에 있는 컬럼 이름과 테이블을 얼마나 많이 검증하였는지, 최적화 과정에 관한 어플리케이션 재 작성과 최적화 규칙, 다른 가능한 노트를 보여준다.

    

- EXPLAIN을 통해서 나오는 정보는 테이블에 대한 정보이므로 다음과 같은 정보를 가지고 있다.

![JyxfO](https://user-images.githubusercontent.com/50399804/130393454-ab320b7d-372b-4e6f-ab58-139f455d35ea.png)

- id

  - SELECT 식별자, 순차적인 번호

  

- select_type

  - SELECT에 대한 타입으로, 다음 종류 중 하나이다.

  | TYPE               | DESCRIPTION                                                  |
  | ------------------ | ------------------------------------------------------------ |
  | SIMPLE             | Simple __SELECT__ (not using __UNION__ or subqueries)        |
  | PRIMARY            | Outermost(가장 바깥쪽) __SELECT__                            |
  | UNION              | Second or later __SELECT__ statement in a __UNION__          |
  | DEPENDENT UNION    | Second or later __SELECT__ statement in a __UNION__, dependent on outer query |
  | UNION RESULT       | Result of a __UNION__                                        |
  | SUBQUERY           | First __SELECT__ in subquery                                 |
  | DEPENDENT SUBQUERY | First __SELECT__ in subquery, dependent on outer query       |
  | DERIVED            | Derived table __SELECT__ (subquery in __FROM__ clause)       |

  

- table

  - 행과 열이 참조하는 테이블

  

- type

  - 조인 타입으로 나열순은 가장 좋은 것부터 나쁜 것의 순서로 나열해보겠다.

    - system

      - 테이블의 하나의 열만 가지고 있는 경우로, const 조인 타입 중 특별한 경우

        

    - const

      - 테이블은 적어도 하나의 매칭 테이블을 가지고 있는데, 쿼리가 시작되는 시점에서 이 테이블을 읽게 된다.

        

      - 하나의 열만이 존재하기 때문에, 이 열에 있는 컬럼에서 얻는 값은 옵티마이저에 의해 상수로 인식될 수 있다.

        

      - 한번 밖에 읽혀지지 않기 때문에 매우 빠르다.

        

      - PRIMARY KEY 또는 UNIQUE 인덱스의 모든 부분을 상수 값과 비교를 할 때 사용한다.

      ```sql
      SELECT * FROM tbl_name WHERE primary_key=1;
      
      SELECT * FROM tbl_name
      WHERE pirmary_key_part1=1 AND primary_key_part2=2;
      ```

      

    - eq_ref

      - 이전 테이블로부터 각 열의 조합하기 위해, 테이블의 열을 하나읽는다.

        

      - system 및 const 타입과 달리, 가장 최선의 가능 조인 타입으로 조인에 의해 인덱스의 모든 부분이 사용될 때 쓰이게 된다.

        

      - 이때 인덱스가 PRIMARY KEY 또는 UNIQUE 인덱스가 된다.

        

      - = 연산자를 사용해서 비교되는 인덱스된 컬럼용으로 사용될 수 있다. 

        

      - 비교 값은 이 테이블 전에 읽었던 테이블에서 컬럼을 사용한 수식 또는 상수(contant) 가 될 수 있다.

      ```sql
      SELECT * FROM ref_table, other_table
      WHERE ref_Table.key_column = other_table.column;
      
      SELECT * FROm ref_table, other_table
      WHERE ref_Table.key_column_part1 = other_table.column
        AND ref_Table.key_column_part2 = 1;
      ```

      

    - ref

      - 이전 테이블에서 읽어온 각각의 열을 조합하기 위해 테이블에서 매칭 되는 인덱스 값을 가진 모든열을 읽어온다.

        

      - 만일 조인이 키의 좌측 끝 접두사만을 사용하거나 또는 키 값이 PK 또는 UNIQUE 인덱스가 아니라면 ref 가 사용된다.

        (조인이 키 값을 기반으로 한 단일 열을 선택하지 않는다면)

        

      - 만약 사용된 키가 적은 수의 열에 대해서만 매치가 된다면, 좋은 조입 타입인 것이다.

        

      - = 또는 <=> 연산자를 사용해서 비교되는 인덱스된 컬럼에 대해 사용될 수 있다.

      ```sql
      SELECT * FROM ref_table WHERE key_column=expr,
      
      SELECT * FROM ref_Table, other_table
       WHERE ref_Table.key_column = other_Table.column;
       
       SELECT * FROM ref_Table, other_table
        WHERE ref_table.key_column_part1 = other_table.column
          AND ref_table.key_coluimn_part2 = 1;
      ```

      

    - ref_or_null

      - ref와 유사하지만, null 값을 가지고 있는 열에 대해서도 검색을 한다는 점에서 차이가 있다. 이 조인 타입 최적화는 서브 쿼리를 해석할 때 자주 사용된다.

      ```sql
      SELECT * FROM ref_Table
        WHERE key_column=expr OR key_column IS NULL;
      ```

      

    - index_merge

      - 인덱스 병합 최적화가 사용되었음을 나타낸다. 행과 열에 있는 key 컬럼은 사용된 인덱스 리스트를 가지고 있고, key_len 은 사용된 인덱스에 대해서 가장 긴 키 부분의 리스트를 가지고 있다.

        

    - unique_subquery

      - IN 서브 쿼리에 대해서 ref 를 대체한다.
      - unique_subquery 는 효율성을 위해 서브 쿼리를 대체하는 인덱스 룩업 함수 이다.

    ```sql
    value IN (SELECT primary_key FROm single_table WHERE some_expr)
    ```

    

    - index_subquery
      - unique_subquery 와 유사한 조인 타입으로, IN 서브 쿼리를 대체하지만 논-유니크 인덱스에 대해서도 동작한다.

    ```sql
    value IN (SELECT key_column FROm single_table WHERE some_expr)
    ```

    

    - range

      - 주어진 범위에 들어 있는 열만을 추출하며, 열 선택은 인덱스를 사용한다. 결과 열에 있는 key 컬럼은 어떤 인덱스가 사용되었는지를 가리킨다.

        

      - key_len 은 사용된 키에서 가장 긴 부분을 가진다.  ref 컬럼은 이 타입에 대해서는 NULL 값이 된다.

        

      - range 는 키 컬럼이 =, <>, >, >=, <, <=, IS NULL, <=>, BETWEEN 또는 IN 연산자를 사용하는 상수와 비교할 때 사용할 수 있다.

    ```sql
    SELECT * FROM tbl_name
     WHERE key_column = 10;
    
    SELECT * FROM tbl_name
     WHERE key_column BETWEEN 10 and 20;
    
    SELECT * FROM tbl_name
     WHERE key_column IN (10, 20, 30);
    
    SELECT * FROM tbl_name
     WHERE key_part1 = 10 AND key_part2 IN (10, 20, 30);
    ```

    

    - Index

      - 이 조인 타입은 ALL과 동일하지만, 인덱스 트리(index tree) 만을 스캔한다는 점에서 다르다.

        

      - 일반적으로, 보통의 인덱스 파일이 데이터 파일보다 작기 떄문에, ALL 보다는 빠르게 동작한다.

        

      - 쿼리가 단일 인덱스의 일부분인 컬럼만을 사용할 때, 이 조인 타입을 사용한다.

        

    - ALL

      - 이전 테이블에서 읽어온 각각의 열을 조합하기 위해, 전체 테이블 스캔(full table scan)을 실행한다. 테이블이 const가 표시되지 않은 첫 번쨰 테이블이고, 다른 모든 경우에 있어서 매우 좋지 않은 경우라면, 이것은 그리 좋읜 경우가 아니다.

        

      - 일반적인 경우에는, 이전 테이블에서 가져온 상수 값 또는 컬럼 값을 사용해서 테이블 열을 추출하는 인덱스를 추가하면 ALL을 피할 수 있다.

        

        

  - possible_keys

    - 이 테이블에서 열을 찾기 위해 MySQL 이 선택한 인덱스를 가리킨다.

      

    - 이 테이블은 EXPLAIN 결과에서 나타나는 테이블 순서와는 전적으로 별개의 순서가 된다.

      

    - possible_key 에 있는 키 중에 어떤 것들은 테이블 순서를 만드는 과정에서 사용되지 않을 수도 있음을 의미한다.

       

    - 만일 이 컬럼 값이 NULL이라면, 연관된 인덱스가 존재하지 않게 된다. 이런 경우, WHERE 구문을 검사해서, 이 구문이 인덱스 하기에 적당한 컬럼을 참조하고 있는지 여부를 알아 봄으로써 쿼리 속도를 개선 시킬 수가  있게 된다.

      - 그런 경우, 적절한 인덱스를 하나 생성한 후에, EXPLAIN 을 다시 사용해서 쿼리를 검사한다.

        

    - 테이블이 어떤 인덱스를 가지고 있는지 보기 위해서는 SHOW INDEX FROM tbl_name 을 사용한다.

      

  - key

    - key 컬럼은 MySQL 이 실제로 사용할 예정인 키(인덱스) 를 가리킨다.

      

    - 만일 아무런 인덱스도 선택되지 않았다면 NULL 이 된다.

      

    - MySQL로 하여금 possible_keys 컬럼에 있는 인덱스를 사용하거나 또는 무시하도록 만들기 위해, FORCE INDEX, USE INDEX 또는 IGNORE INDEX 를 쿼리에서 사용하도록 한다.

      

    - MyISAM 및 BDB 테이블의 경우에는, ANALYZE TABLE 를 구동시키면 옵티마이저가 보다 좋은 인덱스를 선택하도록 도움을 줄 수 가 있다.

      

  - key_len

    - MySQL이 사용하기로 결정한 키의 길이를 나타내는 컬럼으로, key 컬럼이 NULL 이라면 이 값도 NULL 이 된다.

      

    - 다중-부분(multiple-part) 키 중에 얼마나 많은 부분을 MySQL 이 실제로 사용하는지 알 수 있도록 해 준다.

      

  - ref

    - 테이블에서 열을 선택하기 위해 key 컬럼 안에 명명되어 있는 인덱스를 어떤 칼럼 또는 상수와 비교하는지를 보여준다.

    

  - rows

    - MySQL 이 쿼리를 실행하기 위해 조사해야 하는 열의 숫자를 가리킨다.

    

  - Extra

    - 이 컬럼은 MySQL 이 쿼리를 어떻게 해석하는지에 관한 추가적인 정보를 제공한다. 가질 수 있는 값은 다음과 같다.

      - Distinct

        - 명확한 값을 찾게 되며, 매칭되는 열을 찾게되면 더 이상의 열에 대해서는 검색을 중단한다.

          

      - Not exists

        - LEFT JOIN 최적화를 실행했으며, 이 최적화와 매치되는 열을 찾은 후에는 더 이상 이 테이블에서 이전 열 조합 검색을 하지 않게 된다.

        ```sql
        SELECT * FROM t1 LEFT JOIN t2 ON t1.id = t2.id
         WHERE t2.id IS NULL;
        ```

        > t2.id를 NOT NULL 로 정의했다면, t1을 스캔하고 t1.id 값을 사용해서 t2에 있는 열을 검색한다.
        >
        > t2에서 매칭되는 열을 발견하면, t2.id 가 결코 NULL 이 아님을 알게되어, 동일한 id 값을 가지고 있는 t2에서는 더 이상 열을 스캔하지 않게 된다.

        

      - range checked for each record (index map: N)

        - 사용하기에 좋은 인덱스를 찾지 못했으나, 이전 테이블에서 컬럼 값을 찾고 난 후에는 사용할 수도 있을 법한 인덱스는 알아냈다.

          

        - 이전 테이블에 있는 각 열 조합에 대해서는, 그 조합이 열을 추출하기 위해 range 또는 index_merge 접근 방식을 사용할 수 있는지를 검사한다.

          

        - 이 방법은 그리 빠른 방법은 아니지만, 인덱스를 전혀 사용하지 않는 것보다는 빠르게 진행한다.

          

      - Using filesort

        - 저장된 순서에 따라서 열을 추출하는 방법을 찾기 위해 기타 과정을 진행한다.

          

        - 정렬은 조인 타입과 정렬 키 및 WHERE 구문과 매치가 되는 모든 열에 대한 열 포인터를 사용해서 모든 열에 걸쳐 진행 된다. 그 키는 저장이 되고 열을 저장 순서에 따라서 추츨된다.

          

      - Using index

        - 인덱스 트리에 있는 정보만을 가지고 테이블에서 컬럼 정보를 추출한다. 쿼리가 딘일 인덱스의 일부 컬럼만을 사요하는 경우에, 이러한 전략을 사용할 수 있다.

          

        - 테이블에는 접근하지 않고 인덱스에서만 접근해서 쿼리를 해결하는 것을 의미한다. **커버링 인덱스로 처리됨 index only scan이라고도 부른다**

          

      - Using temporary

        - 쿼리를 해석하기 위해, 결과를 저장할 임시 테이블을 하나 생성해야 한다. 만일 쿼리가 컬럼을 서로 다르게 목록화 하는 GROUP BY 및 ORDER BY 구문을 가지고 있는 경우에 이런 것이 일어난다.

          

      - Using where

        - WHERE 구문은 다음 테이블에 대한 열 매치 또는 클라이언트에 보내지는 열을 제한하기 위해 사용된다.

          

        - 테이블에서 모든 열을 조사하거나 불러올 의도가 특별히 없다면, Extra 값이 Using Where 가 아니고, 테이블 조인 타입이 ALL 또는 index 일 경우에는 쿼리에 문제가 생길 수도 있다.

          

        - 가능한 한 빠른 쿼리를 만들고 싶다면, Extra Using filesort 및 Using temporary 값을 조사하도록 한다.

        

      - Using sort_union(...), Using union(...), Using intersect(...)

        - 인덱스 스캔이 어떻게 index_merge 조인 타입과 병합(merge)이 되는지를 나타낸다.

          

        - 자세한 것은 [인덱스 병합 최적화](http://www.mysqlkorea.com/sub.html?mcode=manual&scode=01&m_no=21451&cat1=7&cat2=217&cat3=232&lang=k)를 참조하자.

        

      - Using index for group-by

        - 테이블 접근에 대한 Using index 방식과 유사한 Using index for group-by 방식은 MySQL 이 실제 테이블을 추가적으로 검색을 하지 않고서도, GROUP BY 또는 DISTINCT 쿼리의 모든 컬럼을 추출하기 위해 사용될 수 있는 인덱스를 찾음을 가리킨다.

          

        - 그 인덱스가 각 그룹에 대해 가장 효과적인 방식으로 사용되기 때문에, 적은 수의 인덱스 엔트리만이 읽혀지게 된다.

          

      - Using where with pushed condition

        - [NDB Cluster](https://www.google.com/search?rlz=1C5CHFA_enKR963KR963&hl=ko&sxsrf=ALeKk03xTtl2N_4_t4Mv3E84ZKml4z-yRw:1629699424116&q=MySQL+NDB+Cluster&sa=X&ved=2ahUKEwjl-Y66v8byAhXb4mEKHfI_BfgQ1QIwEXoECBYQAQ&biw=1920&bih=814) 테이블에만 적용된다.

          

        - MySQL 클러스터가 인덱스가 되지 않은 컬럼 (non-indexed column) 과 상수(constant) 간의 직접 비교(direct comparison (=))의 효율성을 개선하기 위해 조건문을 푸시 다운(condition pushdown) 하는 중이라는 의미를 갖는다.

          

        - 이런 경우, 조건문은 동시에 값이 검사되는 클러스터의 모든 데이터 노드로 "푸시 다운" 이 된다. 매치되지 않는 열을 네트워크 전체에 보낼 필요성을 없애 주며, 조건문 푸시 다운을 하지 않는 경우에 비해서 5~10배의 속도 향상을 얻을 수 있다.

        ```sql
        -- 1. 아래와 같은 클러스터 테이블을 가지고 있다고 가정하자.
        CREATE TABLE t1 (
        	a INT, 
          b INT,
          KEY(a)
        ) ENGINE=NDBCLUSTER:
        
        -- 2. 조건문 푸시 다운은 아래와 같은 쿼리에 함께 사용될 수 있다.
        SELECT a,b FROM t1 WHERE b = 10;
        
        -- 3. EXPLAIN SELECT 로 결과를 보면 다음과 같다.
        /*
                   id: 1
          select_type: SIMPLE
                table: t1
                 type: ALL
        possible_keys: NULL
                  key: NULL
              key_len: NULL
                  ref: NULL
                 rows: 10
                Extra: Using where with pushed condition
        */
        
        -- 4. 다음의 경우에는 조건문 푸시 다운을 적용할 수 없다.
        SELECT a,b FROM t1 WHERE a = 10; -- 인덱스가 컬럼 a에 존재
        
        SELECT a,b FROM t1 WHERE b + 1 = 10; -- 인덱스가 되지 않은 컬럼 b를 포함하는 비교문이 간접적이기 때문  ( b + 1이 간접적, b = 9 로 바꾸면 푸시 다운 사용 가능)
        
        -- > 또는 < 연산자를 사용해서 인덱스된 컬럼을 상수와 비교를 하는 경우에는 조건문 푸시 다운을 사용할 수가 있다.
        EXPLAIN SELECT a,b FROM t1 WHERE a<2/G
        
        /*
                   id: 1
          select_type: SIMPLE
                table: t1
                 type: range
        possible_keys: a
                  key: a
              key_len: 5
                  ref: NULL
                 rows: 2
                Extra: Using where with pushed condition
        */
        
        -- 5. 조건문 푸시 다운에 관해서는 다음 사항을 기억해야 한다. 
        
        /*
        	- 조건문 푸시 다운은 MySQL 클러스터에만 관련이 있고, 다른 스토리지 엔진을 사용하고 있는 테이블에 대해서 쿼리를 실행할 경우 발생하지 않는다.
        	
        	- 조건문 푸시 다운 기능은 디폴트로 사용되지 않는다. 기능을 활성화 시키기 위해서는 mysqlid 를 --engine-condition-pushdown 옵션과 함꼐 시작하거나, 또는 다음 명령문을 실행한다
        	SET engine_condition_pushdown=On;
        */
        ```

        

      

      
