- 테스트 코드 실제 일하는 곳에서는 처음 작성했는데 매우 어색하였다.

- 어떻게 작성해야 할지, 특히 어떤 영역에 대해서는 어떤 테스트를 해야할지 매우 헷갈렸다.

- 말로서는 설명이 잘 어려운거 같다. 처음에 테스트 코드는 이런식으로 작성하였다.

```java

@SpringBootTest
public class userServiceTest {

    @Autowired
    private userRepository UserRepository;

    @Test
    @Transational
    public void 유저 생성해보기() throws Exception {

       User user = new User();
       user.set ....

       User userSaved = userRepository.save()
        (로직)
       ...

       assertEquals(user.getId(), userSaved().getId());
       ...
    }

}
```

- 테스트가 문제 없이 통과하지만, 사실 이 테스트에는 정말 많은 문제가 많았다.

  - 나는 분명 Service Layer를 테스트하려는 건데 왜 전부 테스트 하고 있을까

  - @SpringBootTest 를 사용하고 있기때문에 컨텍스트를 모두 가져와야 하므로 속도가 느리다.

  - 테스트가 Repository에 의존하고 있어, 단위 테스트라고 보기 어렵다.

<br>

- 클린 아키텍처(clean architecture) 책의 유명한 저자인 마틴 엉클밥에 따르면 유닛 테스트는 [FIRST](https://dzone.com/articles/writing-your-first-unit-tests) 라는 원칙에 따라 단위 테스트 되어야 한다고 한다.

  - **Fast**

    - 단위 테스트는 가능하면 빠르게 실행 되어야 한다.

    - 여러개의 멀티 컴포넌트를 한번에 테스트하는 통합 테스트는 하지 않도록 해서 가능하다면 1초 이하로 실행 할 수 있어야 한다.

    - 위의 경우, @SpringBootTest 를 이용한 통합 테스트를 하고 있으므로, 테스트하려는 Layer에 따라서 적절한 방법을 통하여 테스트 해야 한다. (테스트 어노테이션은 [여기](https://github.com/xeropise/TIL/blob/main/2021%EB%85%84%2006%EC%9B%94/%EC%8A%A4%ED%94%84%EB%A7%81%20%EB%B6%80%ED%8A%B8%20%ED%85%8C%EC%8A%A4%ED%8A%B8%ED%95%98%EA%B8%B0.md))를 참조해 보자.

  - **Independent**

    - 단위 테스트는 이전 테스트에 의한 영향이나 다른 객체나 목 메서드에 의해 영향 받아서는 안 된다.

    - 단위 테스트의 순서를 바꿔도 성공적으로 실행 되어야 한다.

  - **Repeatable**

    - 단위 테스트는 반복 가능해야하며, DB에 의해 의존하게되어 여러번 실행 시 실패하면 안 된다.

    - 스프링 부트의 경우, @Transactional 을 통하여, DB를 롤백시키거나, @After 을 이용하여, 데이터를 삭제하거나, 혹은 In-memory DB인 H2를 통하여, 순간적으로 필요한 데이터를 넣는 방법도 있다.

  - **Self-validating**

    - 단위 테스트는 자체 검증이 가능 해야 하며, 테스트를 개발자가 직접 수동으로 확인할 필요 없이 Assert 문으로 성공을 확인할 수 있어야 한다.

    - Junit의 Assert 를 사용했는데 회사 팀장님께서 AssertJ 를 추천해주셨다. 이에 대해서는 나중에 테스트는 하는 방법에 대해 따로 정리해보겠다.

  - **Timely**

    - 테스트를 통과하는 코드가 작성되기 전에 단위 테스트를 작성해야 한다.

    - 위의 항목의 경우, TDD로 개발 하고 있다면, 적용할만 하지만 반드시 지켜야할 필요는 없다.

- 위 기준에 근거해보니, 내가 하고 있는 테스트는 통합테스트였다. 위 원칙을 계속 생각하면서 테스트 코드를 짜보려고 노력해야 겠다는 생각이 들었다.

- Layer 별 단위 테스트를 위해 코드를 어떤 식으로 작성해야 할지는 다른 문서에서 정의해 보겠다.
