- 테스트 코드 실제 일하는 곳에서는 처음 작성해 보는데 작성하면서 생각했던점이나 고려해야 했던 점을 적어보려 한다.

- Repository 영역은 DB와 직접 상호작용하는 곳이므로, 데이터가 제대로 저장되었는지? 혹은 제대로 조회되는지.. 말하자면 CRUD 작업이 제대로 작동하는지 확인하면 된다.

- DDD에 따라 코드 영역을 잘 나누어놨다면, Service Layer에서는 트랜잭션이나 도메인 간의 로직이 제대로 작동하는지 테스트 하면된다.

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

- 테스트가 문제 없이 통과하지만, 사실 이 테스트에는 정말 많은 문제가 있다.

  - 나는 분명 Service Layer를 테스트하려는 건데 왜 전부 테스트 하고 있을까
  
  - @SpringBootTest 를 사용하고 있기때문에 컨텍스트를 모두 가져와야 하므로 속도가 느리다.
  
  - 테스트가 Repository에 의존하고 있어, 단위 테스트라고 보기 어렵다.

<br>

- 클린 아키텍처(clean architecture) 책의 유명한 저자인 마틴 엉클밥에 따르면 유닛 테스트는 [FIRST](https://dzone.com/articles/writing-your-first-unit-tests) 라는 원칙에 따라 테스트 되어야 한다고 한다.

  - __Fast__
