# [스프링 시큐리티(Spring Security) 란?](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#introduction)

- 스프링 기반의 어플리케이션의 보안(인증과 인가)을 담당하는 프레임워크

- 스프링 시큐리티는 filter 기반으로 동작하기 때문에 Spring MVC 와 분리되어 관리되고 동작한다.

- 보안과 관련한 많은 옵션을 제공해주기 때문에 개발자 입장에서 보안 관련 로직을 전부 작성하지 않아도 된다는 장점이 있다. (물론 왜 추가하는지는 파악하고 있는게 좋다.)

- 몇가지 용어에 대한 정리가 필요하다.

  - 접근 주체(Principal) : 보호된 대상에 접근하는 사용자

  - 인증(Authentication)

    - 해당 사용자가 본인이 맞는지를 확인하는 절차

  - 인가(Authroization)
    - 인증된 사용자가요청된 자원에 접근 가능한 권한이 있는지 검사

---

## 스프링 시큐리티 의존성 추가

- 스프링 부트에서는 Spring initializer로 스타터 모듈로 여러 모듈이 합쳐진 형태로 받을 수 있다.

- 참고로 모듈의 종류는 [여기](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#modules)서 확인할 수 있다.

```java
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-test</artifactId>
            <scope>test</scope>
        </dependency>
```

- 의존성 추가 후, 스프링부트를 실행해보면 다음과 같은 로그를 통해 모듈이 작동함을 알 수 있다.

```
2021-06-28 03:16:31.925  INFO 9072 --- [  restartedMain] .s.s.UserDetailsServiceAutoConfiguration :

Using generated security password: 4957f873-9b2c-4be6-94c8-9c5c542d977f
```

<br>
<br>

---

## 스프링 부트 시큐리티 관련 자동 설정

- 다음과 같은 설정을 핵심적으로 추가해 준다. 아래 더 설명 예정

  - springSecurityFilterChain 라는 서블릿 필터 빈을 만든다. 애플리케이션에서 모든 보안에 책임이 있는 빈이다.

  - UserDetailsService 라는 빈과 함께, id가 user 이며 로그에 랜덤으로 생성되는 패스워드를 만들어 준다.

  - 서블릿 컨테이너에 모든 요청에 springSecurityFilterChain 라는 필터를 등록해 준다.

- 몇가지를 요약해서 설명하면 다음과 같다.

  - 애플리케이션과의 어떤 상호작용과도 인증된 user 임을 요구한다.

  - 기본적인 로그인 form 을 만들어 준다.

  - password storage 를 BCrypt 로 보호한다.

  - user 를 로그아웃 시켜준다.

  - CSRF 공격 방지

  - 세션 고정 방어

  - 시큐리티 관련 헤더 통합

    - 보안 요청을 위한 HTTP [Strict Transport Security](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Strict-Transport-Security)

    - X-Content-Type-Options

    - Cache Control

    - X-XSS-Protection

    - X-Frame-Options 통합 (클릭재킹 방지)

  - 서블릿 API 메서드와 통합

    - [HttpServletRequest#getRemoteUser()](<https://docs.oracle.com/javaee/6/api/javax/servlet/http/HttpServletRequest.html#getRemoteUser()>)

    - [HttpServletRequest.html#getUserPrincipal()](<https://docs.oracle.com/javaee/6/api/javax/servlet/http/HttpServletRequest.html#getUserPrincipal()>)

    - [HttpServletRequest.html#isUserInRole(java.lang.String)](<https://docs.oracle.com/javaee/6/api/javax/servlet/http/HttpServletRequest.html#isUserInRole(java.lang.String)>)

    - [HttpServletRequest.html#login(java.lang.String, java.lang.String)](<https://docs.oracle.com/javaee/6/api/javax/servlet/http/HttpServletRequest.html#login(java.lang.String,%20java.lang.String)>)

    - [HttpServletRequest.html#logout()](<https://docs.oracle.com/javaee/6/api/javax/servlet/http/HttpServletRequest.html#logout()>)

---

<br>
<br>

## 스프링 시큐리티는 필터 기반으로 동작

<br>

![filterchain](https://user-images.githubusercontent.com/50399804/123555969-d8279780-d7c3-11eb-98ec-2d3a6b4ceebc.png)

- 스프링 시큐리티의 서블릿 지원은 서블릿 필터를 기반으로 한다. 그래서 필터의 역할을 이해하는 것이 우선적으로 도움이 된다. 위 그림은 전형적으로 하나의 HTTP 요청이 들어왔을때 전형적인 필터 처리 레이어다.

- 클라이언트가 애플리케이션에 요청을 보내면, 컨테이너가 Filter 들 + Servlet 을 포함하고 있는 FilterChain 을 만든다.

- 하나 이상의 필터들은 필터들 혹은 서블릿의 downstream(뭔말인지 잘..)을 방지하고, Filter 는 HttpServletResponse 을 전형적으로 write 한다.(사용?)

- downstream 필터들과 서블릿으로 HttpServletRequest 혹은 HttpServletResponse 을 변경할 수 있다.

- downstream 이라는 말이 순서대로 동작하게 한다는 말 같다. 필터를 모두 거치고 서블릿에 도달한다는 말, 그래서 **필터 호출 순서가 매우 중요** 하다.

---

<br>
<br>

## 스프링 시큐리티 큰 그림 - 스프링 핵심 객체(1)

### [1. DelegatingFilterProxy](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-delegatingfilterproxy)

<br>

![delegatingfilterproxy](https://user-images.githubusercontent.com/50399804/123556559-dad7bc00-d7c6-11eb-921b-2692f54d84d1.png)

<br>

- 스프링은 DelegatingFilterProxy 라는 필터 구현 객체를 제공한다.
- 이 객체는 서블릿 컨테이너의 생명주기와 스프링 ApplicationContext, 즉 스프링 컨테이너와 다리 역할을 한다.

- 서블릿 컨테이너는 자신만의 표준 기준으로 필터를 등록할 수 있게 하지만, 스프링에 정의된 필터는 알지 못한다.

- DelegatingFilterProxy는 표준 서블릿 컨테이너 메카니즘으로 등록될 수 있고, 일은 전부 Filter를 구현하고 있는 스프링 빈들에게 위임한다.

- 이 객체를 통해 필터 기반으로 동작이 가능하며, 이 안에서 Spring Security 에 관련된 모든 일이 벌어진다고 생각하면 되겠다.

<br>

### [2. FilterChainProxy](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-filterchainproxy)

<br>

![filterChainproxy](https://user-images.githubusercontent.com/50399804/123556759-f98a8280-d7c7-11eb-87a6-4e02cbcf7eb6.png)

- 스프링 시큐리티의 서블릿 지원은 FilterChainProxy 에 포함되어 있다.

- FilterChainProxy 는 스프링 시큐리티에서 특별한 Filter 로 SecurityFilterChain 을 통하여 많은 Filter 인스턴스에 일을 시킨다.

- FilterChainProxy 도 역시 스프링 빈이기 때문에 DelegatingFilterProxy 에 감싸져 있다.

<br>

### [3. SecurityFilterChain](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-securityfilterchain)

<br>

![securityfilterchain](https://user-images.githubusercontent.com/50399804/123556922-e0360600-d7c8-11eb-9e9c-912259278a7f.png)

<br>

- SecurityFilterChain 은 filterChainproxy 에 의하여 어떤 스프링 시큐리티 필터가 요청에 맞게 호출되어야 하는지 결정한다.

- SecurityFilterChain 의 Security Filter 들은 전형적인 스프링 빈인데, 왜 DelegatingFilterProxy 대신에 FilterChainProxy 로 등록을 하는걸까?

  - FilterChainProxy 가 서블릿 컨테이너 혹은 DeletegatingFilterProxy 에 직접 등록하는데 많은 이점을 주기 때문이다.

  - 첫째로, 스프링시큐리티의 서블릿 지원의 모든 시작점을 제공하는데, 그런 이유로 트러블슈팅을 해보려고하는 경우 디버그 포인트를 FilterChainProxy 에 하는것이 아주 좋다.

  - 둘째로, FilterChainProxy 가 보여지지 않는 스프링 시큐리티 사용 관련 일을 수행하는 중심이다. 예를들면 SecurityContext 의 메모리 누수를 피하기 위해 이를 정리해준다. 또 특정 공격들로부터 방어조치를 위한 스프링 시큐리티 HttpFireWall 을 적용해준다.

  - 추가로, SecurityFilterChain 언제 호출되어야 하는지 유연성을 제공해 준다. 스프링 컨테이너에서 필터들은 URL 기반으로 호출되는 반면에 FilterChainProxy는 RequestMatcher 를 활용하여 HttpServletRequest 에 기반한 호출을 할 수 있다.

  <br>

  ![multi-securityfilterchain](https://user-images.githubusercontent.com/50399804/123557230-b978cf00-d7ca-11eb-95f7-4f50e5047553.png)

  <br>

  - 말하자면, FilterChainProxy 가 어떤 SecurityFilterChain 사용되어야할지 결정 할 수 있다는 말이 된다. 이 말은 SecurityFilterChain 을 여러 개 설정할 수 있다는 말이 된다.

  - 위 그림을 보면, URL에 매칭하여, 다른 SecurityFilterChain 을 부르고 있고, 각 SecurityFilterChain 마다 다른 Filter 를 가지고 있음에 주목해야 한다. 즉 URL 마다 동작하는 Filter 를 정할 수 있다는 말이 된다.

###

---

<br>
<br>

## 스프링 시큐리티 코딩 시작 - SecurityFilterChain 만들기

- FilterChainProxy에 SecurityFilterChain을 등록해 보자.

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.mvcMatcher("/api/**");
    }

}
```

- WebSecurityConfigurerAdapter 클래스는 스프링 시큐리티에 설정들을 다양하게 커스터마이징을 가능하게 하는 설정 클래스로 해당 클래스가 구현하고 있는 클래스들을 참고해서 SecurityFilterChain 생성 및 동작을 하게 한다.

- WebSecurityConfigurerAdapter 안의 소스 내용을 보면 한번 봐보자.

```java
@Order(100)
public abstract class WebSecurityConfigurerAdapter implements WebSecurityConfigurer<WebSecurity> {

	private final Log logger = LogFactory.getLog(WebSecurityConfigurerAdapter.class);

	private ApplicationContext context;

	private ContentNegotiationStrategy contentNegotiationStrategy = new HeaderContentNegotiationStrategy();

	private ObjectPostProcessor<Object> objectPostProcessor = new ObjectPostProcessor<Object>() {
		@Override
		public <T> T postProcess(T object) {
			throw new IllegalStateException(ObjectPostProcessor.class.getName()
					+ " is a required bean. Ensure you have used @EnableWebSecurity and @Configuration");
		}
	};
```

- @Order 어노테이션으로 순서가 등록되어 있음을 알 수 있다.

- 그럼 위에 그림대로 여러개의 SecurityFilterChain 을 등록하려면 어떻게 해야 할까? WebSecurityConfigurerAdapter 확장하는 설정 config 클래스를 여러개 만드는 것이다.

- 즉 말하자면, WebSecurityConfigurerAdapter를 확장하는 클래스 자체가 하나의 SecurityFilterChain이 된다.

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;
import org.springframework.core.annotation.Order;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
@Import({SecurityConfig.FooSecurityConfig.class, SecurityConfig.BarSecurityConfig.class, SecurityConfig.AllSecurityConfig.class})
public class SecurityConfig {


  @Order(100)
  static class FooSecurityConfig extends WebSecurityConfigurerAdapter {

      @Override
      protected void configure(HttpSecurity http) throws Exception {
          http.mvcMatcher("/api/**");
      }

  }

  @Order(200)
  static class BarSecurityConfig extends WebSecurityConfigurerAdapter {

      @Override
      protected void configure(HttpSecurity http) throws Exception {
          http.mvcMatcher("/get/**");
      }

  }

  @Order(300)
  static class AllSecurityConfig extends WebSecurityConfigurerAdapter {

      @Override
      protected void configure(HttpSecurity http) throws Exception {
          http.mvcMatcher("/**");
      }

  }
```

- @Order 를 같은 값으로 주면 에러가 나니 주의하자.

---

## 시큐리티 필터들

- SecurityFilterChain API와 많은 시큐리티 filter가 FilterChainProxy에 들어가 있다.

- 스프링 시큐리티 필터들의 순서를 꼭 알아야 하는 것은 아니지만 Filter들은 순서가 앞에서 말했듯 매우 중요하기 때문에 대강 알아두면 좋다.

- 대강 이렇다고 한다.

  - ChannelProcessingFilter

  - WebAsyncManagerIntegrationFilter

  - SecurityContextPersistenceFilter

  - HeaderWriterFilter

  - CorsFilter

  - CsrfFilter

  - LogoutFilter

  - OAuth2AuthorizationRequestRedirectFilter

  - Saml2WebSsoAuthenticationRequestFilter

  - X509AuthenticationFilter

  - AbstractPreAuthenticatedProcessingFilter

  - CasAuthenticationFilter

  - OAuth2LoginAuthenticationFilter

  - Saml2WebSsoAuthenticationFilter

  - UsernamePasswordAuthenticationFilter

  - OpenIDAuthenticationFilter

  - DefaultLoginPageGeneratingFilter

  - DefaultLogoutPageGeneratingFilter

  - ConcurrentSessionFilter

  - DigestAuthenticationFilter

  - BearerTokenAuthenticationFilter

  - BasicAuthenticationFilter

  - RequestCacheAwareFilter

  - SecurityContextHolderAwareRequestFilter

  - JaasApiIntegrationFilter

  - RememberMeAuthenticationFilter

  - AnonymousAuthenticationFilter

  - OAuth2AuthorizationCodeGrantFilter

  - SessionManagementFilter

  - ExceptionTranslationFilter

  - FilterSecurityInterceptor

  - SwitchUserFilter

---
