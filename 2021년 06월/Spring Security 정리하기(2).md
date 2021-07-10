## 인증(Authentication) - 스프링 핵심 객체(2)

<br>

![ss](https://user-images.githubusercontent.com/50399804/123566088-ca8d0480-d7f9-11eb-9053-54431bf48242.png)

<br>

- 스프링 시큐리티의 서블릿 인증에서 사용되어지는 구조적인 컴포넌트에 대해 알아보자.

### [1. SecurityContextHolder](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-authentication-securitycontextholder)

<br>

![securitycontextholder](https://user-images.githubusercontent.com/50399804/123564510-b266b680-d7f4-11eb-99ac-e19d5e57e513.png)

- SecurityContext의 홀더

- 누가 인증되었는지 상세정보들(Authentication) 을 저장하는 곳으로 스프링 시큐리티 인증 모델의 중심

- Authentication을 가지고 있는 SecurityContext 를 포함하고 있다. (추후 자세히 서술)

- 스프링 시큐리티는 SecurityContextHolder 가 어떻게 구성되어 있는지 신경쓰지 않는다. (즉, 안쪽이 어떤 형태이든 상관없다는 말인듯? 자유롭게 바꿀 수 있다.)

- SecurityContextHolder 에 user가 인증받았다고 나타내는 가장 간단한 방법은 직접 설정하는 것이다.

```java
SecurityContext context = SecurityContextHolder.createEmptyContext();
Authentication authentication =
    new TestingAuthenticationToken("username", "password", "ROLE_USER");
context.setAuthentication(authentication);

SecurityContextHolder.setContext(context);
```

> Authentication 객체가 어떤 타입이든 신경 쓰지 않고 있다. 근데 스프링 시큐리티 내부에는 이미 내부적으로 구현해놓은 클래스들이 있긴 하다.

- SecurityContextHolder 는 [ThreadLocal](https://yeonbot.github.io/java/ThreadLocal/) 사용을 기본으로 하여, 요청 전반에 걸쳐 SecurityContext 인증 객체에 접근 가능하도록 한다. 메소드를 통하여 전달되지 않아도 된다.

- ThreadLocal 말고도 다른 방법을 이용할 수 있는데 System Property 에 값을 설정하여 이를 결정할 수 있다.

---

<br>

### [2. SecurityContext](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-authentication-securitycontext)

- SecurityContext 는 SecurityContextHolder 에서 얻을 수 있고, Authentication 객체를 포함하고 있다.

- 위 특징외에는 특별한 점이 없다.

---

<br>

### [3. Authentication](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-authentication-authentication)

- Authentication 은 다음을 포함하고 있다.

  - principal

    - user 를 식별하는 ID 정도로 생각하면된다. (이메일도 될수 있고 진짜 ID가 될 수도 있다.), username/password 로 인증할 때 UserDetails 인스턴스가 된다.

  - credentials

    - 비밀번호로 외부로 노출되지 않았다는것을 보증하기 위해 인증 후에 제거된다.

  - authorities

    - 사용자의 권한, GrantedAuthority 라는 객체를 사용한다.

    - GrantedAuthority는 Authentication.getAuthorities() 메소드에서 얻을 수 있는 객체로 Collection 형태로 제공된다. (user 하나당 여러 개의 권한을 가질 수 있기 때문)

---

<br>

### [4. AuthenticationManager](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-authentication-authenticationmanager)

<br>

- 스프링 시큐리티의 필터들이 어떻게 인증하는지 정의하는 API 이다. 구현체로는 ProviderManager 가 있으며, 다른 객체를 사용할 수도 있다.

- authenticate 메소드를 가지고 있는 인터페이스이다.

---

<br>

### [5. ProviderManager](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-authentication-providermanager)

<br>

![providermanager](https://user-images.githubusercontent.com/50399804/123566307-5737c280-d7fa-11eb-89ae-5b53ef52fdb0.png)

- ProviderManager 는 AuthenticationManager 구현체중 가장 흔히 쓰이는데 AuthenticationProvider에게 일을 시킨다.

- 인증 방식에 따라(Authentication 객체 형태) AuthenticationProvider 가 선택되는데 예로 아이디, 패스워드 기반의 인증을 하기 위해서는 UsernamePasswordAuthenticationToken 객체를 authenticate 메소드에 넘겨주면 된다.

---

<br>

### [6. AuthenticationProvider](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-authentication-authenticationproviderr)

- AuthenticationProvider 는 ProviderManager 에 주입될 수 있는 다양한 Authenticatino 처리 객체이다.

- 예를 들면 DaoAuthenticationProvider 는 username/password 에 기반한 authentication을 지원하고, JwtAuthenticationProvider 는 JWT token 에 기반한 authentication 을 지원한다.

---

<br>

### [7. AbstractAuthenticationProcessingFilter](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-authentication-abstractprocessingfilter)

<br>

![usernamepasswordauthenticationfilter](https://user-images.githubusercontent.com/50399804/123569290-07102e80-d801-11eb-927c-d977a89a0f90.png)

- user의 credentials 를 인증하는 기본적인 filter 로 사용되어진다. credentials 들이 인증되기전에 AuthenticationEntryPoint 를 통해 스프링 시큐리티가 credentials 를 요청한다.

- 그 후에 AbstractAuthenticationProcessingFilter가 전송된 어떠한 authentication 요청들을 인증할 수 있다.

- 이 객체를 통하여 request 에 담긴 credentials 로 Authentication 객체를 만들 수 있다.

```java

@Log4j2
public class ApiLoginFilter extends AbstractAuthenticationProcessingFilter {

    private JWTUtil jwtUtil;

    public ApiLoginFilter(String defaultFilterProcessUrl, JWTUtil jwtUtil) {

        super(defaultFilterProcessUrl);
        this.jwtUtil = jwtUtil;
    }

    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response) throws AuthenticationException, IOException, ServletException {

        log.info("-------------------------ApiLoginFilter---------------------------");
        log.info("attemptAuthentication");

        String email = request.getParameter("email");
        String pw = request.getParameter("pw");

        UsernamePasswordAuthenticationToken authToken =
                new UsernamePasswordAuthenticationToken(email, pw);

        return getAuthenticationManager().authenticate(authToken);
    }

```

<br>

### [8. AuthenticationEntryPoint](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-authentication-authenticationentrypoint)

- AuthenticationEntryPoint는 클라이언트가 credentials을 요구하는 경우, HTTP response 를 보내기 위해 사용된다.

- 가끔 클라이언트가 자원을 요청하기 위해 username/password 와 같은 credentials를 포함시키는데 이러한 경우, 스프링 시큐리티는 credentials 을 제공할 필요가 없다.

- 클라이언트가 접근이 허가되지 않는 자원에 대해 인증되지 않은 요청을 하면, credentials 을 요청하기 위해 사용되어진다.

- 로그인 페이지로 리다이렉션 시키거나 [WWW-Authenticate](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/WWW-Authenticate) 헤더를 응답헤더에 전송시켜 인증을 요구한다.

- AccessDeniedHandler 는 서버에 요청 시 액세스가 가능한 권한이 아닐 경우에 대하여 동작하고, AuthenticationEntryPoint 는 인증이 되지 않은 사용자가 요청을 했을 때 동작한다.
