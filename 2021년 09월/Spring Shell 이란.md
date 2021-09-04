https://docs.spring.io/spring-shell/docs/2.0.0.RELEASE/reference/htmlsingle/

(추후 정리)

- 스프링 쉘은 쉽게 말하자면 스프링 부트의 특성은 가저가면서 커맨드로 스프링 메서드를 실행 가능케 하는 스프링 프로젝트 중 하나이다.

- nodeJs의 npm 을 생각해 보면 쉽다.

- 설치는 다음과 같이 spring-shell-starter 를 의존성에 추가하여 시작할 수 있다.

```maven
<dependency>
    <groupId>org.springframework.shell</groupId>
    <artifactId>spring-shell-starter</artifactId>
    <version>2.0.0.RELEASE</version>
</dependency>
```

- 튜토리얼을 한번 해보자. @ShellComponent 어노테이션을 클래스에 선언함으로써, 특정 클래스를 커맨드에 의해 동작하게 지정할 수 있다.

```java
@ShellComponent
public class MyCommands {

    @ShellMethod("Add two integers together.")
    public int add(int a, int b) {
        return a + b;
    }
}
```

- 어플리케이션을 빌드하고 JAR 파일을 다음과 같이 실행해 보자.

```
.mvnw clean install -DskipTests
[...]

java -jar target/demo....(jar 파일명)
```

- 스프링 부트 실행과 동시에 다음과 같이 shell 커맨드 창이 뜬다

```java
shell:>
```
