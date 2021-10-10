#### [RestTemplate](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/client/RestTemplate.html) 이란?

![다운로드](https://user-images.githubusercontent.com/50399804/138808092-5f49c575-03f9-437d-849a-94040e6cf1d5.png)

- HTTP Request 를 수행하는 동기 Client로 JDK의 HttpURLConnection, Apache HttpComponents 등, 같은 HTTP client 라이브러리 밑에서 동작한다.

  - [여기](https://sjh836.tistory.com/141) 를 참조해서 다른 라이브러리들의 특징을 확인하자.

  

- ClientHttpRequestFactory를 구현한 HTTP client 기반으로, 통신을 위한 편리한 메소드들을 제공한다.

  - 사용법은 [여기](https://advenoh.tistory.com/46) 를 확인하자.

    

- 동작 순서를 정리하면 다음과 같다.

  1. 어플리케이션이 RestTemplate를 생성하고, URI, HTTP메소드 등의 헤더를 담아 요청한다.

     

  2. RestTemplate 는 HttpMessageConverter 를 사용하여 requestEntity 를 요청메세지로 변환한다.

     

  3. RestTemplate 는 ClientHttpRequestFactory 로 부터 ClientHttpRequest 를 가져와서 요청을 보낸다.

     

  4. ClientHttpRequest 는 요청메세지를 만들어 HTTP 프로토콜을 통해 서버와 통신한다.

     

  5. RestTemplate 는 ResponseErrorHandler 로 오류를 확인하고 있다면 처리로직을 태운다.

     

  6. ResponseErrorHandler 는 오류가 있다면 ClientHttpResponse 에서 응답데이터를 가져와서 처리한다.

     

  7. RestTemplate 는 HttpMessageConverter 를 이용해서 응답메세지를 java object(Class responseType) 로 변환한다.

  어플리케이션에 반환된다.

  

- RestTemplate은 HttpMethod로 동작하는 보통의 시나리오 템플릿들을 제공하고, 흔하지 않은 케이스에 대한 exchange, excute의 사용을 일반화 시켰다.

  

- Spring 5.0 이후로는 WebClient의 사용을 추천하고 있다.



<br>



***

#### [WebClient](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/reactive/function/client/WebClient.html) 란?

![0_XhceqwwJ9LyB6h6R](https://user-images.githubusercontent.com/50399804/138810486-01d873dd-2a8e-4c0d-b460-984c6675813c.png)

- Reactor Netty와 같은 HTTP client 아래에서 작동하는 비동기, 반응형 Client 이다.

  

- create(), create(String), builder() 와 같은 메소드가 필요한데, 사용을 위해서는 기본적으로 [Project Reactor](https://projectreactor.io/docs/core/release/reference/#getting-started-introducing-reactor) 에 대한 이해가 필요하다.

  

- 동작 원리는 [여기](https://alwayspr.tistory.com/44) 를 참조하자.



<br>



***

#### RestTemplate vs WebClient?

![EAYYkB5XUAELWpm](https://user-images.githubusercontent.com/50399804/138810391-d05bc490-db49-4de8-81a6-68000018f20a.jpeg)



<br>



- RestTemplate 은 MultiThread 와 Blocking 방식으로 동작한다.

  

  ![다운로드 (1)](https://user-images.githubusercontent.com/50399804/138811303-360cf84a-05ee-4e84-8221-9481e94e3c33.png)

  

- Request가 Queue 에 들어와 처리 가능한 경우, 스레드가 할당된다. 1요청 당 1스레드가 할당되면서 동기 처리되므로 모든 스레드가 처리중일 경우, 추가적인 요청을 처리할 수가 없다. 



<br>



#### 그에 반해



<br>



- WebClient 는 싱글 스레드, 비동기 방식을 사용한다.

![TRM-10-FINAL](https://user-images.githubusercontent.com/50399804/138811527-38513f84-24d9-4ff7-9552-b4e4d044f0de.png)



- 많은 요청들이 이벤트 루프내의 작업으로 등록되고, 작업자들이 결과를 기다리지 않고 다른 작업을 처리한다.

  

- 다른 작업자들이 작업을 마치면 콜백으로 요청자에게 결과를 제공하도록 작업을 하므로 쓰레드를 효율적으로 사용할 수 있다.



<br>



#### 성능을 비교

![다운로드 (2)](https://user-images.githubusercontent.com/50399804/138812106-b2e6e2b8-0dc7-47eb-9736-cdef1f31bac1.png)

- 하자면, 사용자 규모에 따라 저구간에서는 비슷한 성능을 보이나, 사용자가 높아질수록 처리속도가 급격하게 느려지는 것으로 보인다.

  

- 그럼, 무조건 RestTemplate 사용을 지양해야할까? 라고하면 아니다. 동기식으로 코드를 짜기때문에 디버깅 및 코드 이해 등에 대한 장점이 반드시 있고, WebClient 는 잘못된 지식으로 사용할 경우,  동기식보다 처리 효율이 더 떨어지게 되므로 학습 그래프가 꽤 높다.

  

<br>



> https://happycloud-lee.tistory.com/220
>
> https://alwayspr.tistory.com/44
>
> https://timewizhan.tistory.com/entry/%EB%B2%88%EC%97%AD-Concurrency-in-Spring-WebFlux