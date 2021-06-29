# REST([Representational](https://datatracker.ietf.org/doc/html/rfc7231#section-3) State Transfer) API

- 2000년도에 로이 필딩(Roy Fielding)의 박사학위 논문에서 최초로 소개

- HTTP의 주요 저자 중 한사람으로, 웹(HTTP) 설계의 우수성에 비해 제대로 사용되어지지 못하는 모습이 안타까워 웹의 장점을 최대한 활용할 수 있는 아키텍처로 발표

---

## REST의 구성

- REST API는 다음의 구성으로 이루어져 있다.

  - 자원(RESOURCE) - URI

  - 행위(Verb) - HTTP METHOD

  - 표현(Representations)

---

## REST의 특징

1. Uniform(유니폼 인터페이스)

- URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행

2. Stateless(무상태성)

- 작업을 위한 상태정보를 따로 저장하고 관리하지 않는다. 세션정보나 쿠키 정보를 별도로 저장하고 관리하지 않기 때문에 API 서버는 들어오는 요청만을 단순히 처리하면 된다. 그로 인해 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 구현이 단순해진다.

3. Cacheable (캐시 가능)

- HTTP라는 기존 웹 표준을 그대로 사용하기 때문에, 웹에서 사용하는 기존 인프라를 그대로 활용 가능하여 캐싱 기능을 사용할 수 있다.
  (E-Tag, Last-Modified)

4. Self-descriptiveness (자체 표현 구조)

- REST API 메시지만 보고도 이를 쉽게 이해할 수 있는 자체 표현 구조로 되어있다.

5. Client-Server 구조

- REST 서버는 API를 제공하고, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보) 등을 직접 관리하는 구조로 각각의 역할이 확실히 구분되기 때문에 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로간 의존성이 줄어들게 된다.

6. 계층형 구조

- REST 서버는 다중 계층으로 구성될 수 있으며, 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간 매체를 사용할 수 있게 한다.

---

## REST API 디자인 가이드

- REST API 설계 시 가장 중요한 것은 다음의 2가지이다.

  1. URI는 정보의 자원을 표현해야 한다.
  2. 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE) 로 표현해야 한다.

- 예로 회원정보를 가져오는 REST API 설계 하는 경우에

```
GET /members/show/1   (x)
GET /members/1        (O)
```

로 HTTP METHOD 로는 자원에 대한 행위, URI는 자원을 표현해야 한다.

---

## URI 설계 시 주의할 점

1. 슬래시 구분자(/) 는 계층 관계를 나타내는 데 사용한다.

```
    http://restapi.example.com/houses/apartments
    http://restapi.example.com/animals/mammals/whales
```

2. URI 마지막 문자로 슬래시(/)를 포함하지 않는다.

```
    http://restapi.example.com/houses/apartments/ (X)
    http://restapi.example.com/houses/apartments  (0)
```

3. 하이픈(-)은 URI 가독성을 높이는데 사용

4. 밑줄(\_) 은 URI에 사용하지 않는다.

5. URI 경로에는 소문자가 적합하다.

6. 파일 확장자는 URI에 포함시키지 않는다. 메시지의 포맷을 나타내기 위한 파일 확장자는 Accept Header 를 사용하도록 하자.

```
http://restapi.example.com/members/soccer/345/photo.jpg (X)

 GET / members/soccer/345/photo HTTP/1.1 Host: restapi.example.com Accept: image/jpg
```

> 참조 <br> https://meetup.toast.com/posts/92
