- 프론트에서 input type='file' 에 대해 공부했고, 미리보기 기능도 살펴보았으니 이제 서버로 보내보도록 하자. 방법에는 Form 속성을 직접 사용하는 방법과 Ajax 요청을 이용하는 방법이 있을 수 있다.
  Ajax 요청으로 정보를 보내보면 많이 보던 오류를 마주칠 수 있다.

![image](https://user-images.githubusercontent.com/50399804/118124389-7936da80-b430-11eb-9073-8c02d64163ea.png)

> Origin이 같으면 발생하지 않음

**Access to XMLHttpRequest at '주소A' from origin '주소B' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**

- 해당 오류는 동일 출처 정책([same-origin-policy](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy)) 으로 인한 것으로, 어떤 출처([Origin](https://developer.mozilla.org/ko/docs/Glossary/Origin))에서
  불러온 문서나 스크립트가 다른 출처에서 가져온 리소스와 상호작용하는 것을 제안하는 보안 정책이다.

- 웹 브라우저 보안을 위해 프로토콜, 호스트, 포트가 동일한 서버로만 ajax 요청을 주고 받을 수 있도록 한 정책이다.

![스크린샷 2021-05-13 오후 9 25 33](https://user-images.githubusercontent.com/50399804/118125389-dbdca600-b431-11eb-981a-dc1507aaa372.png)

> Javascript에서 location 을 검색, origin, protocol, host 을 확인해보자.

- origin 이란 쉽게 말하자면 요청을 보낸 도메인을 말하는데, 예를들어 3090 포트에서 8080 포트로 실행한 후, 클라이언트에서 서버로 Ajax 요청을 보내는 경우, 위와 같은 에러를 똑같이 마주할 수 있다.
  강남 학원에서 강사님께서 "편하지? 근데 이게 서버쪽에서 보면 해킹인거야, 그래서 막았다" 라고 하셨던게 기억이 난다.

<img width="1004" alt="스크린샷 2021-05-13 오후 9 30 52" src="https://user-images.githubusercontent.com/50399804/118125878-85bc3280-b432-11eb-8ee5-7f9ca12e7ad7.png">

> origin 이 같은 경우, 다른 경우.. Host, Protocol, Port가 하나라도 다를 경우에는 동일한 출처로 보지 않는다! 다른 경우를 Cross Origin 이라고 한다.

- 보안을 위해 걸어두었다고는 하지만, 서버를 나혼자 사용하는 것도 아니고 다른 사람도 같이 사용하게 되면 이 오류는 반드시 마주치는 오류다. 그럼 이를 어떻게 해결해야 할까? 답은 2가지가 있다.

## **[Cross-Origin Resource Sharing(CORS)](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS) - 서버 해결 방법**

- 교차 출처 리소스 공유 (CORS) 는 추가적인 HTTP Header 를 사용하여, 한 출처에서 실행 중인 웹 애플리케이션이 다른 출처에서 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 것이다. (넌 ok야!) 웹 애플리케이션은 리소스가 자신의 출처와 다를 때 교차 출처(Cross origin) HTTP 요청을 실행한다.

![CORS_principle](https://user-images.githubusercontent.com/50399804/118364016-beded900-b5d1-11eb-819b-9fec51d0f666.png)

- CORS 체제는 브라우저와 서버 간의 안전한 교차 출처 요청 및 데이터 전송을 지원하고, 최신 브라우저의 같은 경우 XMLHttpRequest 또는 Fetch 와 같은 API 에서 CORS 를 사용하여, 교차 출처 HTTP 요청의 위험을 완화한다.

- 그럼 어떤 요청들이 CORS 를 사용해야할까?

  1. 위에서 말한 [XMLHttpRequest](https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest) 와 [Fetch API](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API)

  2. 웹폰트 (CSS 내 @font-face 에서 교차 도메인 폰트 사용하는 경우)

  > 엥? font는 font-family 를 지정하지 않나? 사실 font-family 도 잘 모른다 ㅋㅋ  
  >  이에 대한 정리가 잘되어있는 [이 블로그](https://webclub.tistory.com/261)를 참조하자.

  3. WebGL(웹 기반의 그래픽 라이브러리) Texture

  4. 이미지로부터 추출하는 CSS Shapes

<br>

- **그럼 대체 CORS 는 어떻게 동작을 하는 것일까?** 위에서 언급했듯 웹 애플리케이션이 다른 출처에서 리소르르 요청할 때는 HTTP 프로토콜을 사용하여 요청을 보내게 되는데, 이때 브라우저의 Request Header 에 Origin 이라는 값을 담아 출처를 보낸다.

- 요청을 받은 서버는 응답을 할 때, **[Access-Control-Allow-Origin](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Access-Control-Allow-Origin)** 이라는 응답 헤더에 이 리소스에 접근가능한 출처를 보내주고, 응답을 받은 브라우저가 자신이 보냈던 Origin 값과 비교하여, 동작이 가능한지 체크하는 것이다.

- 잉? 그러면 요청을 보낼 때, 서버에서 보냈던 요청이 한번 다 동작하고, 비교를 한다는 것일까? 그럼 문제가 되지 않나? 이런생각이 들었는데, 요청을 보낼 때, 이를 확인하는 절차가 있었다.

![sop-cors-preflight](https://user-images.githubusercontent.com/50399804/118365125-65c57400-b5d6-11eb-9d22-0715833cbb1a.jpg)

- 전에 언급한 것처럼, 서버에 side Effect 를 일으킬 수 있는 HTTP 요청 메서드 (GET 을 제외한 HTTP 메서드)에 대해서, CORS 명세에서는 브라우저가 요청을 [OPTIONS](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/OPTIONS) 메서드로 "PREFLIGHT" (사전에.. 라고 알아두면 되겠다) 하여 지원하는 메서드를 확인하고, 서버의 허가가 떨어지면 실제 요청을 보내는 것이다. 서버쪽에서는 클라이언트쪽에 요청해 인증정보 (쿠키, HTTP 인증) 을 함께 보내라 라고 할 수도 있다.

- 다시 돌아와 Access-Control-Allow-Origin 은 같은 출처(Same Origin) 외의 다른 출처(Cross Origin) 에 대해 Same Origin Policy 정책을 통과해도 좋다고 말할 수 있는 값인 것이다. 사용법은 이렇다

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Origin: <origin>
Access-Control-Allow-Origin: null
```

- 참고로, \* 의 경우 [crendential](https://developer.mozilla.org/ko/docs/Web/API/Request/credentials) 이 없는 요청들에 와일드카드로 모든 요청을 허용한다는 뜻이지만 보안이 그만큼 취약해진 다는 것을 명시하자. 쿠키를 사용한다면 [withCredentials](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/withCredentials) 을 명시하여야 한다. 클라이언트, 서버쪽 모두 명시하여야 제대로 동작한다. 서버쪽의 경우 [Access-Control-Allow-Credentials](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Access-Control-Allow-Credentials): true 로 해줘야 한다.

- Mozila 에서 감사하게도 자바쪽에서 어떤식으로 코드를 짜야하는지 보여주고 있다. SERVLET Filter 예시이다.

```java
import java.io.IOException;
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;


@Component
public class SimpleCORSFilter implements Filter {

private final Logger log = LoggerFactory.getLogger(SimpleCORSFilter.class);

public SimpleCORSFilter() {
    log.info("SimpleCORSFilter init");
}

@Override
public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException {

    HttpServletRequest request = (HttpServletRequest) req;
    HttpServletResponse response = (HttpServletResponse) res;
    response.setHeader("Access-Control-Allow-Origin", request.getHeader("Origin"));
    response.setHeader("Access-Control-Allow-Credentials", "true");
    response.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS, DELETE");
    response.setHeader("Access-Control-Max-Age", "3600");
    response.setHeader("Access-Control-Allow-Headers", "Content-Type, Accept, X-Requested-With, remember-me");
    chain.doFilter(req, res);
}

@Override
public void init(FilterConfig filterConfig) {
}

@Override
public void destroy() {
}

}
```

- 스프링에서는 이에 대한 글을 다루고 다루고 있다.

  1.  [스프링에서 컨트롤 메서드에 cors 설정하는 방법](https://spring.io/guides/gs/rest-service-cors/#controller-method-cors-configuration), @CrossOrigin 이라는 Annotation 을 사용하거나, WebMvcConfigure 에서 Cors 값을 등록해주는 것이다.

- 그럼 이 CORS와 관련된 헤더에는 뭐가 있을까?

- **HTTP Response Header 에는 다음과 같은 것이 있다.**

  **1. Access-Control-Allow-Origin**
  : 위에서 언급한 header 로, 단일 출처를 지정하여 브라우저가 해당 출처가 리소스에 접근하도록 허용하는 헤더이다.

  **2. Access-Control-Expose-Headers**
  : 브라우저가 접근할 수 있는 header 를 서버의 화이트리스트에 추가할 수 있다.

  ```
  Access-Control-Expose-Headers: X-My-Custom-Header, X-Another-Custom-Header
  ```

  **3. Access-Control-Max-Age**
  : preflight request 요청 결과를 캐시할 수 있는 시간을 나타내는 헤더이다.

  ```
  Access-Control-Max-Age: <delta-seconds>
  ```

  **4. Access-Control-Allow-Credentials**
  : crendentials 플래그가 true 일 경우, 요청에 대한 응답을 표시할 수 있는지를 나타낸다. GET request 는 preflight request 를 보내지 않으므로 credentials이 있는 리소스를 요청하면, 이 header 의 리소스가 함께 반환되지 않는다. 이 header 가 없으면 브라우저에서 응답을 무시하고 웹 컨텐츠로 반환되지 않으므로 주의하자.

  ```
  Access-Control-Allow-Credentials: true
  ```

  **5. Access-Control-Allow-Methods**
  : 리소스에 접근할 때 허용되는 메서드를 지정한다. preflight request 에 대한 응답으로 사용된다.

  ```
  Access-Control-Allow-Methods: <method>[, <method>]*
  ```

  **6. Access-Control-Allow-Headers**
  : preflight request 에 대한 응답으로 실제 요청시 사용할 수 있는 HTTP header 를 나타낸다.

  ```
  Access-Control-Allow-Headers: <header-name>[, <header-name>]*
  ```

- **HTTP Request Header 에는 다음과 같은 것들이 있다.**

  **1. Origin**
  : cross site 접근 요청 또는 preflight request 의 출처를 나타낸다. null 값 혹은 URI가 올 수 있다. 접근 제어 요청에는 항상 Origin header 가 전송된다.

  **2. Access-Control-Request-Method**
  : 실제 요청에서 어떤 HTTP 메서드를 사용할지 서버에게 알려 주기 위해, preflight request 할 때 사용된다.

  ```
  Access-Control-Request-Method: <method>
  ```

  **3. Access-Control-Request-Headers**
  : 실제 요청에서 어떤 HTTP 헤더를 사용할지 서버에게 알려 주기 위해 사용된다.

  ```
  Access-Control-Request-Headers: <field-name>[, <field-name>]*
  ```

- 조금 더 자세한 것은 W3C 의 CORS 에 대한 [설명](https://fetch.spec.whatwg.org/) 을 참조하자.

## **[프록시 서버(Proxy Server)]() - 클라이언트 쪽 해결 방법**

> 프록시 서버란?

- 클라이언트가 자신을 통해서 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해 주는 컴퓨터 시스템이나 응용 프로그램을 가리킨다. 서버와 클라이언트 사이에 중계기로서 대리로 통신을 수행하는 것을 가리켜 프록시, 그 중계 기능을 하는 것을 프록시 서버라고 한다. (위키 백과)

- 말하자면 "대리응답" 을 하는 서버로, 클라이언트가 서버에 직접 접근해서 요청의 내용을 가져와야 하지만, 프록시 서버가 대신 서버에 요청하고 클라이언트에게 가져와 주는 것이다.

![다운로드](https://user-images.githubusercontent.com/50399804/118398576-35dca600-b694-11eb-9f9d-2c9bbb967795.png)

- 프록시 서버를 사용하는 이유는 CORS 뿐만아니라 다음의 이유도 있다.

  1. 개인정보를 보호  
     : 프록시 서버 없이 클라이언트가 네이버 서버에 요청을 할 때 나의 IP 주소가 전달이 되는데 프록시 서버를 사용하면 내 IP가 아닌 프록시 서버의 IP가 전달된다.

  2. 캐시 사용 - 속도 향상
     : 프록시 서버가 웹페이지를 가져올 때 자신이 정적 이미지 등을 저장시켜 놓다가 서버까지 가기 전에 프록시 서버를 통해 빠르게 접근할 수 있다.

  3. 성인 컨텐츠 차단  
     : 부적절한 사이트의 접근을 강제로 거부할 수 있다.

  4. 문서 접근 제어  
     : 많은 웹 서버들과 우베 리소스엔 대한 단일한 접근 제어 전략을 구현하고 감사 추적을 하기 위해 사용할 수 있다. 각기 다른 조직에서 관리되는 다양한 종류의 수많은 웹 서버들에 대한 접근 제어를 수시로 갱신할 필요 없이, 중앙 프록시 서버에서 접근 제어가 가능하다. 본 서버에 고정적으로 프록시 서버로부터의 요청만 받아들이록 설정 함으로써, 고의적으로 제어 프록시를 피해 가능 경우도 방어 가능하다.

  5. 콘텐츠 라우터
     : 인터넷 트래픽 조건과 콘텐츠의 종류에 따라 요청을 특정 웹 서버로 유도하는 콘텐츠 라우터로 동작할 수 있다.

  6. 트랜스 코더
     : 콘텐츠를 클라이언트에게 전달하기 전에 본문 포맷을 수정할 수 있다. 데이터의 표현 방식을 자연스럽게 변환하는 것을 말한다.

  7. 익명화 프록시
     : HTTP 메시지에서 신원을 식별할 수 있는 특성들 (IP, Referer, Cookie, From) 등을 제거함으로써 개인 정보 보호와 익명성 보장에 기여 한다.

- 프록시 서버에는 2가지 종류가 있다

  1. Forward Proxy
     : 일반적인 프록시 서버로 Client 와 Server 사이에 중간에 위치하여, 요청을 중계하는 서버로, 서버에게 클라이언트가 누구인지 감추는 역할을 해 준다.

  ![다운로드](https://user-images.githubusercontent.com/50399804/118399330-a46f3300-b697-11eb-8ff3-39811203597b.jpg)

  2. Reverse Proxy
     : 포워드 프록시와 반대 방향으로 흐르는 서버로, 클라이언트가 서버를 호출할 때, 리버스 프록시를 호출하게 되고, 프록시 서버가 요청하여 받은 응답을 클라이언트에게 전달하는 방식이다. 리버스 프록시는 서버가 누구인지 감추는 역할을 해 준다.

  ![다운로드 (1)](https://user-images.githubusercontent.com/50399804/118399356-c668b580-b697-11eb-9bde-57cdefc98a11.jpg)

- 프록시 서버를 자바 혹은 프론트에서 설정하는 방법은 지금 이미지 업로드를 하기 위해서는 당장 필요하지 않으므로, 다음에 다뤄보도록 하자.
