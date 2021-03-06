- 앞의 1,2,3 을 통해 보낼 때는 어떤 태그를 이용해야하고, 어떤 것이 관련되어 있는지 좀 파악이 된거 같다. 근데 또 하나의 의문점이 들었다. 대체 Form 태그는 File 을 어떻게 변환하여 보내는 걸까? 학원에서 배운거 같은데 전혀 기억이 안났다. 또 남들에게 설명해보라고 하면 설명도 못할꺼 같아서.. 역시 기본 부터 정리가 필요했다.

- 이를 위해서는 HTTP Message 에 대한 정리가 필요하다.

<br>

_HTTP Message란_

- HTTP에서 클라이언트와 서버가 데이터를 교환하는 방식으로 메시지의 타입은 요청(Request) 과 응답(response)가 있다.

- ASCII로 인코딩된 텍스트 정보이며, 여러 줄로 되어있는데 웹 개발자가 직접 HTTP Message 를 텍스트로 작성하는 경우는 드물며 소프트웨어, 브라우저, 프록시, 또는 웹 서버가 그 일을 한다.

- HTTP Message 는 설정 파일(프록시 혹은 서버의 경우), API, 혹은 다른 인터페이스를 통해 제공된다.

- HTTP 요청과 응답의 구조는 서로 닮아 있는데, 그 구조는 보통 이렇다.

![HTTPMsgStructure2](https://user-images.githubusercontent.com/50399804/118510124-a1457700-b76b-11eb-9780-c4bef832d9e3.png)

### HTTP 요청

1. start line
   : 실행되어야할 요청, 또는 요청 수행에 대한 성공 또는 실패가 기록되어 있다. 이 줄은 항상 한 줄로 끝난다.
   처음에는 HTTP 메서드 (GET, PUT, POST, HEAD, OPTIONS) 가 있어 서버가 수행해야 할 동작을 나타낸다.
   두번째는 요청 타겟으로 URL, 프로토콜, 포트, 도메인 등 절대 경로로 나타낼 수도 있으며, 형식은 HTTP 메소드에 따라 달라진다. 형식은 3가지가 있다.

   1. origin 형식: 끝에 '?' 로 시작하는 쿼리문자열이 붙는 절대 경로, GET, POST, HEAD, OPTIONS 와 함께 사용한다.

   ```
   POST / HTTP 1.1
   GET /background.png HTTP/1.0
   HEAD /test.html?query=alibaba HTTP/1.1
   OPTIONS /anypage.html HTTP/1.0
   ```

   2. absolute 형식: 완전한 URL 형식으로 프록시에 연결하는 경우 대부분 GET과 함꼐 사용된다.

   ```
   GET http://developer.mozilla.org/en-US/docs/Web/HTTP/Messages HTTP/1.1
   ```

   3. authority 형식: 도메인 이름 및 옵션 포트(':'가 앞에 붙는다)로 이루어진 URL의 authoriuty component 이다. [CONNECT](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/CONNECT) 메소드가 사용하는데 [HTTP 터널](https://en.wikipedia.org/wiki/HTTP_tunnel)을 구축하는 경우에 사용한다.

   4. asterisk 형식: OPTIONS 와 함께 별포 ('\*') 하나로 간단하게 서버 전체를 나타낸다.

   ```
   OPTIONS * HTTP/1.1
   ```

   마지막으로 HTTP 버전이 들어간다. 메시지의 남은 구조를 결정하기 때문에, 응답메시지에서 써야할 HTTP 버전을 알려주는 역할을 한다.

2. HTTP Header Set
   : 옵션으로 HTTP Header Set 이 들어가는데, 요청에 대한 설명 혹은 메시지 본문에 대한 설명이 들어간다. 대소문자 구분이 없는 문자열 다음에 콜론(':')이 붙으며, 그 뒤에 오는 값은
   헤더에 따라 달라진다. 헤더는 값까지 포함해 한 줄로 구성되지만 꽤 길어질 수 있다.

   헤더의 종류는 다음과 같이 나눌 수 있다.

   ![HTTP_Request_Headers2](https://user-images.githubusercontent.com/50399804/118823057-a3d2d880-b8f3-11eb-8db3-b97fce821c19.png)

   - Request Header : User-Agent, Accept 같은 헤더로 요청 내용을 구체화 시키고, [Referer](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Referer) 와 같은 컨텍스트를 제공하기도 하며, 조건에 따른 제약 사항 가하기도 하면서 요청 내용을 수정한다. [조건부 요청](https://developer.mozilla.org/ko/docs/Web/HTTP/Conditional_requests)

   - General Header : 메시지 전체에 적용되는 헤더이다.

   - Entity Header : Content-Length 와 같은 헤더는 요청 본문에 적용되는데, 요청 내에 본문이 없는 경우 entity 헤더는 적용되지 않는다.

3. empty line (blank line)
   : 요청에 대한 모든 메타 정보가 전송되었음 알리는 빈 줄이 삽입된다.

4. body (Payload)
   : 요청과 관련된 내용 (HTML 폼 콘텐츠 등)이 옵션으로 들어가거나, 응답과 관련된 문서(document)가 들어간다. 본문의 존재 유무 및 크기는 첫 줄과 HTTP 헤더에 명시 된다.
   POST 메소드 일 경우, 본문이 포함되는데 본문은 두 가지 종류로 나뉜다

   - 단일-리소스 본문(single-resource bodies): 헤더 2개 (Content-Type 과 Content-Length) 로 정의된 단일 파일

   - [다중-리소스 본문](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types#multipartform-data)(multiple-resource bodies): 멀티파트 본문으로 구성되는 다중 리소스본문에서는 파트마다 다른 정보를 지니게 된다.

- HTTP Message 의 start line 과 Http Header 를 묶어서 요청 헤드 (Request Head) 라고 부르며, 이와 반대로 HTTP Message 의 Payload 는 본문(body)이라고 한다.

### HTTP 응답

1.status line
: HTTP 응답의 시작 줄은 상태 줄(status line)이라고 불리며, 다음과 같다.

1.  프로토콜 버전: 보통 HTTP/1.1 이다.

2.  상태 코드 : 요청의 성공 여부를 나타내는 코드이다. 200, 404 혹은 302

3.  상태 텍스트 : 짧고 간결하게 상태 코드에 대한 설명을 글로 나타내어 사람들이 HTTP 메시지를 이해할 때 도움이 된다.

```
HTTP/1.1 404 Not Found.
```

2. Http Header Set

![HTTP_Response_Headers2](https://user-images.githubusercontent.com/50399804/118826982-e944d500-b8f6-11eb-8e59-28fb37b9885f.png)

- General Header: 메시지 전체에 적용된다.

- Response Header: 상태 줄에 미처 들어가지 못했던 서버에 대한 추가 정보를 제공한다.

- Entity Header: 요청 본문에 적용되는 헤더로, 요청 내에 본문이 없는 경우 entity 헤더는 전송되지 않는다.

3. body
   : 응답의 마지막 부분에 들어가는데 모든 응답에 본문이 들어가지는 않는다. 201, 204 같은 상태 코드를 가진 응답에는 본문이 없다.

   응답 본문은 세가지 종류로 나뉜다.

   - 이미 길이가 알려진 단일 파일로 구성된 단일-리소스 본문 : Content-Type 과 Content-Length 로 정의

   - 길이를 모르는 단일 파일로 구성된 단일-리소스 본문: [Transfer-Encoding](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Transfer-Encoding) 가 chuncked 로 설정되어 있으며, 파일은 청크로 나뉘어 인코딩 되어 있다.

   - 서로 다른 정보를 담고 있는 멀티파트로 이루어진 다중 리소스 본문

- 헤더의 종류는 [여기](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers)서 확인하자.

- 크롬 개발자 도구의 NetWork 탭에서 이를 확인할 수 있다.

![캡처](https://user-images.githubusercontent.com/50399804/118830884-4726ec00-b8fa-11eb-9893-ac000ee93452.JPG)
