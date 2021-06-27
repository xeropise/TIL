# XSS(Cross Site Scripting)

- 웹 애플리케이션에서 많이 나타나는 취약점의 하나

- 공격자가 클라이언트 측 스크립트를 웹 사이트에 삽입하여 다른 사용자의 브라우저에서 수행되게하는 공격의 유형을 말한다.

- 삽입된 코드가 피해 사용자의 사이트 권한 쿠키를 공격자에게 보내는 종류의 악성 작업을 수행할 수 있다.

- 사용자의 개인정보 및 쿠키정보 탈취, 악성코드 감염, 웹 페이지 변조 등의 공격을 수행한다.

---

## XSS 공격 종류

- Stored XSS (저장형)

  ![9920EB3359A8B2870A](https://user-images.githubusercontent.com/50399804/123546199-959b9600-d796-11eb-9b1d-1322e858284d.png)

  - 공격자가 취약한 웹서버에 악성 스크립트를 저장하면 해상자가 해당 자료를 요청할 때 해상 악성 스크립트가 삽입된 응답페이지에 전달되어 클라이언트 측에서 동작하는 것.

- Reflected XSS (반사형)

  ![99813F3359A8B26037](https://user-images.githubusercontent.com/50399804/123546213-9fbd9480-d796-11eb-8fa3-2d07b15e5e46.png)

  - 외부에 있는 악성 스크립트가 희생자 액션에 의해 보안이 취약한 웹 서버로 전달되고, 웹 서버의 응답 페이지에 해당 악성 스크립트가 삽입되어 희생자 측에서 동작하는 방식

- Dom based XSS (DOM 기반)

  ![9997D03359A8B66306](https://user-images.githubusercontent.com/50399804/123546220-a8ae6600-d796-11eb-88b3-30961685743a.png)

  - 희생자의 웹 브라우저에서 응답 페이지에 포함된 정상 스크립트가 동작하면서 DOM 객체를 실행할 때 URL 등에 포함된 악성 스크립트가 동작하는 방식. 응답 페이지에 관계 없이 웹 브라우저에서 발생

---

<br>
<br>

## 대응방안

1. 사용자 입력값 검증

   - 사용자 입력값 검증을 반드시 서버단에서 해야 한다.

   - 사용자 입력 문자열에서 HTML 코드로 인식될 수 있는 특수문자를 일반문자로 치환하여 처리해야 한다.

   - 게시판 등에서 HTML 태그를 허용해야 하는 경우, HTML 태그 화이트리스트를 선정 후, 해당 태그만 허용하는 방식을 적용해야 한다.

   - 대표적인 JAVA XSS FILTER 로는 네이버의 [LUCY](https://github.com/naver/lucy-xss-servlet-filter) 필터가 있다.

   - LUCY FILTER 는 FORM DATA 는 바꿔주는데 @RequestBody 로 전달되는 JSON 요청은 처리해주지 않는다고 한다. [여기](https://homoefficio.github.io/2016/11/21/Spring%EC%97%90%EC%84%9C-JSON%EC%97%90-XSS-%EB%B0%A9%EC%A7%80-%EC%B2%98%EB%A6%AC-%ED%95%98%EA%B8%B0/)와 [여기](https://jojoldu.tistory.com/470) 를 참조하자.

2. 쿠키 httpOnly, secure 속성 활용

   - httpOnly로 자바스크립트 실행을 통한 쿠키 접근을 막고, HTTPS 프로토콜 상에서의 암호화된 요청에서만 쿠키가 전송되도록 해야 한다.
