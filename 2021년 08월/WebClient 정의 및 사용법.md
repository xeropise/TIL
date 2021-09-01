- API 통신을 위해 흔히 쓰는 모듈로는 HttpClient가 있다.

- Java (Spring) 에서 가장 많이 쓰는 HttpClient 로는 RestTemplate 이 있다.

- WebClient 또한 API 를 호출하기 위한 HttpClient 모듈 중 하나이다.

- Spring 5.0 버전 부터는 RestTemplate은 deperecated 될 예정으로, WebClient 을 사용을 적극적으로 권장하고 있다.

```
- Non-blocking I/O
- Reactive Streams back pressure
- High concurrency with fewer hardware resources
- Functional-style, fluent API that takes advantage of Java 8 lambdas
- Synchronous and asynchronous interactions
- Streaming up to or streaming down from a server
```

- 사용법은 [여기](https://binux.tistory.com/56)를 참조하자. 원문 번역 해 놓은 곳이라 내가 작성하는 것과 비교가 안됨
