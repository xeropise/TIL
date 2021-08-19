### @DataJPATest 에서 inset update flush 동작 안함

- 회사에서 JPA 테스트 코드를 작성중에 겪은 문제를 적어 본다.

- @DataJpaTest 어노테이션으로 JPA 테스트를 실시하는 경우, 몇가지 편한 자동설정과 함께 인메모리로 테스트가 가능하다.

- 조회 기능은 정상적으로 동작하는데 insert 나 update 를 테스트 해 보았다.

- ??? 근데 쿼리가 날라가지 않는다 무슨일 일까?

- 아래 링크에 따르면 @DataJpaTest 에서 생겨진 쿼리들을 플러시 하기전에 트랜잭션 롤백을 먼저 실행하기 때문에 insert나 update 가 실행되지 않는 것이다.

- 그럼 어떻게 테스트 해야할까? 스프링부트에서 다행히도 테스트용 TestEntityManager 를 제공하고 있어, 이를 통해 flush 를 강제로 호출할 수 있다.

- 이외에 다른 옵션들을 통해서 잘못설정된 JPA 엔티티들을 값을 테스트 할 수 있다. (물론 경험이 필요하겠지만..)

- 자세한 것은 링크를 참조하자.

> [관련 참고 링크](https://josefczech.cz/2020/02/02/datajpatest-testentitymanager-flush-clear/)
