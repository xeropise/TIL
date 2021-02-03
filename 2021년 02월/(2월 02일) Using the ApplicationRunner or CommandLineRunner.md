[관련 주소는 여기](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-command-line-runner)

> 회사 배치 프로그램이 이렇게 도는건 아는데 이건 어디에 쓰는 걸까? 공홈에서 찾아봤다.

- 스프링 어플리케이션이 시작하면서 특정 코드를 작동하게 하고싶으면, ApplicationRunner 혹은 CommandLIneRunner 인터페이스를 구현하면 된다.  
  두 메서드는 같은 방법으로 동작하면 run 이라는 단일 메서드를 제공한다. ( SpringApplication.run(..) 이 완료되기 전에 호출 됨 )
  
- CommandLineRunner 인터페이스는 string 배열로 application arguments ( 어플리케이션 매개변수 ) 에 접근을 가능하게 하는 반면,   
  ApplicationRunner 는 ApplicationArguments 라는 인터페이스를 사용한다. (discussed earlier 라고 적혀있는데, 사전에 설정한걸 말하는거 같다.)   
  다음 예는 CommandLIneRunner와 run 메소드의 예이다.
  
```Java
import org.springframework.boot.*;
import org.springframework.stereotype*;

@Component
public class MyBean implements CommandLineRunner {
    
    public void run(String... args) {
        // Do something...
    }
}
```

- 만약 CommandLineRunner 혹은 ApplicationRunner의 몇몇 bean들이 반드시 특정한 순서로 동작해야 한다면, org.springframework.core.Ordered 인터페이스 혹은  
  어노테이션을 사용하여 순서를 지정할 수 있다.
  
  
```Java
private void callRunners(ApplicationContext context, ApplicationArguments args) {
    List<Object> runners = new ArrayList<>();
    runners.addAll(context.getBeansOfType(ApplicationRunner.class).values());//ApplicationRunner먼저
    runners.addAll(context.getBeansOfType(CommandLineRunner.class).values());//CommandLineRunner를 나중에
    AnnotationAwareOrderComparator.sort(runners);//@Order를 기준으로 정렬 한 번 하고

    for (Object runner : new LinkedHashSet<>(runners)) { 
        if (runner instanceof ApplicationRunner) { 
              callRunner((ApplicationRunner) runner, args); 
        } 

        if (runner instanceof CommandLineRunner) { 
              callRunner((CommandLineRunner) runner, args); 
         }

    }
  }
```
> 내부에선 이렇게 동작

__그런데...__

![20210203_140413](https://user-images.githubusercontent.com/50399804/106700578-77d36600-6628-11eb-86ed-7a50073e46a9.png)

__엄연히 Spring Batch 라는 Batch Application 을 Spring 에서는 지원하는데? 그럼 위의 인터페이스들은 언제 사용할까__
  
  
그에 대한 해답은 [스택 오버 플로우](https://stackoverflow.com/questions/59328583/when-and-why-do-we-need-applicationrunner-and-runner-interface)에서 찾을 수 있다.  
  
  
  
