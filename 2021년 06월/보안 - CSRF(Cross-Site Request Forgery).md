# CSRF(Cross-Site Request Forgery)

![다운로드 (2)](https://user-images.githubusercontent.com/50399804/123547208-d8f80380-d79a-11eb-8342-82c70ec8ef8d.png)

<br>

- 웹 애플리케이션의 취약점 중 하나로 사용자가 자신의 의지와 무관하게 공격자가 의도한 행동을 해서 특정 웹페이지를 보안에 취약하게 한다거나 수정, 삭제 등의 작업을 하게 만드는 공격

- 공격할 사이트를 분석하여 공격 가능 패턴을 알아내고 (예를 들면 http://localhost:8080/update/1 로 요청을 하면 내용을 변경한다든지) 사용자 패스워드 변경 이나, 타 시스템 로그인 등을 유도하는 것이다.

- 토큰 기반 인증 시스템에서는 LOCAL STORAGE 에 토큰이 저장되어 있는 경우가 많으므로, CSRF 로 인한 공격 방법이 먹힐 것이다.
- XSS 와 차이점으로는 공격 대상이 XSS 는 클라이언트인 반면, CSRF 는 서버에 있다. (근데 내가 볼땐 비슷한거 같다.)

---

<br>
<br>

## 대응 방안

- 가장 기초적인 방법으로는 Http Header 의 [Referer](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Referer) 를 통해 같은 도메인으로 들어오는지 체크하는 것이다. 단, 외부링크를 막는 방법이기 때문에 모두 적용해서는 안되고 잘 분류해서 사용해야 한다. Header가 프로그램으로 조작이 가능하다고 하니 사실 2차 방어수단이라고 해야겠다.

- 대표적인 수단으로는 CSRF 토큰을 만드는 것이다. 백엔드에서 요청을 받을 때마다 세션에 저장되어 있는 토큰값과 요청 파라미터에 전달되는 토큰 값이 일치하는지 검증하는 것이다.

- 예를 들자면

  1. 로그인 하거나 요청을 날릴 경우, CSRF 토큰을 생성하여 세션에 저장한다.

  ```java
  session.setAttribute("CSRF_TOKEN", UUID.randomUUID().toString());
  ```

  2. 페이지에 hiddetn 으로 숨겨 놓는다.

  ```html
  <input type="hidden" name="_csrf" value="${CSRF_TOKEN}" />
  ```

  3. 그리고 들어온 요청을 세션에 저장된 값과 일치하는지 확인한다.

  ```java
  String csrfToken = request.getParameter("_csrf");

  if(csrfToken.equals(session.getAttribute("CSRF_TOKEN"))) {
      return true;
  }else{
      response.sendRedirect("/");
      return false;
  }
  ```
