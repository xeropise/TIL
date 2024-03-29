### SQL 문 수행 절차(쿼리 실행 구조)

![image](https://user-images.githubusercontent.com/50399804/145686384-9e4dd6af-3f24-4877-adaf-82fd67c37bbc.png)

1. 쿼리 캐시 (Query Cache)

   - 5.7 버전부터 Deprecated 되었고, 8.0 에서는 삭제되었다.

     - 쿼리 캐시는 테이블의 데이터가 변경되면 캐시에 저장된 결과 중에서 변경된 테이블과 관련된 것들을 모두 삭제(Invalidate)해야하므로 심각한 동시 처리 성능 저하를 유발했다.

       

     - MySQL 서버가 발전하면서 성능이 개선되는 과정에서 쿼리 캐시는 계속된 동시 처리 성능 저하와 많은 버그의 원인이 되기도 했다.

       

   - Key, Value 구조의 Map

     

2) 파서 (Parser)

   - MySQL Engine에 포함되는 오브젝트로, 사용자가 요청한 SQL 문을 쪼개 최소 단위로 분리하고 트리를 만든다.

     

   - 트리를 만들면서 문법 검사를 수행한다. 쿼리 문장의 기본 문법 오류는 이 과정에서 발견되고, 사용자에게 오류 메시지를 전달한다.

     

3) 전처리기 (Preprocessor)

   - MySQL Engine에 포함되는 오브젝트로, 파서에서 생성한 트리를 토대로 SQL 문에 구조적인 문제가 없는지 파악한다.

     

   - SQL 문에 작성된 테이블, 열, 함수, 뷰와 같은 오브젝트가 실질적으로 이미 생성된 오브젝트인지, 접근 권한은 부여되어 있는지 확인하는 역할을 한다.

     

4) 옵티마이저 (Optimizer)

   - MySQL 의 핵심 엔진 중 하나로, 파서 트리를 토대로 필요하지 않은 조건은 제거하거나 연산 과정을 단순화 한다.

     

   - 어떤 순서로 테이블에 접근할지, 인덱스를 사용할지, 사용한다면 어떤 것을 사용할지, 정렬할 때 인덱스를 사용할지 아니면 임시 테이블을 사용할지와 같은 

     실행 계획(Execution Plan) 을 수립한다.

     

   - 실행 계획으로 도출할 수 있는 경우의 수가 지나치게 많을 때는 실행 계획을 수립하고 비용 산적하여 최적의 실행 계획을 선택하기까지 시간이 오래 걸리는 만큼 모든 실행 계획을 판단하지는 않는다.

     - 이는 옵티마이저가 선택한 최적의 실행 계획이 최상의 실행 계획이 아닐 가능성도 있다는 걸 의미한다. (쿼리 튜닝을 해야 하는 이유)

       

   - 실행 계획을 수립하는 작업 자체만으로도 사용자의 대기 시간과 하드웨어 리소스를 점유하므로, 시간과 리소스에 제한을 두고 실행 계획을 선정해야 한다.

     

5) 엔진 실행기, 실행 엔진 (Engine executor)

   - MySQL Engine 과 Storage Engine 영역 모두에 걸치는 오브젝트

     

   - 옵티마이저에서 수립한 실행 계획을 참고하여 스토리지 엔진에서 데이터를 가져온다.

     - 만들어진 계획대로 각 핸들러에게 요청해서 받은 결과를 또 다른 핸ㄷ들러 요청의 입력으로 연결하는 역할을 수행한다.

     

   - 이후, MySQL Engine 에서는 읽어온 데이터를 정렬하거나 조인하고, 불필요한 데이터는 필터링 처리하는 추가 작업을 한다.

     
   
   - MySQL Engine의 부하를 줄이려면 스토리지 엔진에서 가져오는 데이터양을 줄이는게 매우 중요하다.
   
     
   
6. 핸들러 (스토리지 엔진)

   - MySQL 서버의 가장 밑단에서 MySQL 실행 엔진의 요청에 따라 데이터를 디스크로 저장하고 디스크로부터 읽어 오는 역할을 담당한다.

     

   - 핸들러는 결국 스토리지 엔진을 의미하며, MyISAM 테이블을 조작하는 경우에는 핸들러가 MyISAM 스토리지 엔진이 되고, InnoDB 테이블을 조작하는 경우에는 핸들러가 InnoDB 스토리지 엔진이 된다.



> 참조 Real MySQL 8.0