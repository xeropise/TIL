## Stateless, Stateful 빈

- 오늘 면접 중에 유일하게 답변 하지 못한 질문인데 추후 답변을 위해 정리해 본다.

- 질문은 이랬다. "스프링의 싱글턴 빈이 Stateful 하면 어떤 문제가 있을까요?"

- Stateful? 상태가 꽉차있다? 바로 자유롭지 못하다는 생각이 들었고 나는 Stateful 한 빈은 강한 결속력을 가지고 있다.
  (애매 모호하게) 인터페이스처럼 다형성 확보가 되지 못한다. 라고 대답했다.

- 틀린 말은 아니라고 하셨지만, 내가 제대로 못한 이유는 분명할 것이다.

- **내가 스프링 Bean에서의 Stateful, Stateless 하다는 말을 이해를 못한것이다. 대체 뭘까?**

  <br>

## Stateful?

- 이 단어가 어디서 언급이 될까 하고 찾아봤는데 무려 스프링 공홈에서 언급이 되었다.

- 그것도 주제는 Bean Scope 로..

<br>

```
4.4.2 The prototype scope
The non-singleton, prototype scope of bean deployment results in the creation of a new bean instance every time a request for that specific bean is made (that is, it is injected into another bean or it is requested via a programmatic getBean() method call on the container). As a rule of thumb,

>> you should use the prototype scope for all beans that are stateful <<

, while the singleton scope should be used for stateless beans.

The following diagram illustrates the Spring prototype scope. Please note that a DAO would not typically be configured as a prototype, since a typical DAO would not hold any conversational state; it was just easier for this author to reuse the core of the singleton diagram.

```

> protoTypeBean 은 stateFul Bean 들을 위해 사용해라?

<br>

- 사실 실무에서 protoType Bean 을 사용하는 경우는 단 한번도 없다. 대체 protoTypeBean 이 어떤 역할을 하길래 stateFul 하다는 걸까?

- 참고로 prototype Bean 은 스프링 컨테이너가 생명주기를 전부 관리하지 않는다. 객체 생성 후 의존성 주입 한 후, protoType bean 의 모든 관리 및 소멸은 사용자에게 맡긴다.

- prototype Bean 은 ApplicationContext getBean 메서드로 해당 빈을 가져올 때마다 매번 새로운 인스턴스가 생성된다.
