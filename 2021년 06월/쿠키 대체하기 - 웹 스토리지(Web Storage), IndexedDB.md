![캡처](https://user-images.githubusercontent.com/50399804/123537529-d8e00f80-d76a-11eb-9182-3d94a9637ee3.JPG)

<br>

# 쿠키를 사용하지 않는 이유는 뭘까?

- 쿠키와 달리 웹 스토리지 API 객체는 네트워크 요청 시 서버로 전송되지 않는다.

- 쿠키보다 더 많은 자료를 보관가능하며, 개발자가 스토리지 구성 방식을 설정할 수 있다.

- 서버가 Http Header 를 통해 스토리지 객체를 조작할 수 없다. 자바스크립트만 가능

---

<br>
<br>

# [웹 스토리지 (Web Storage) API](https://developer.mozilla.org/ko/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API)

- 브라우저에서 쿠키를 사용하는 것보다 훨씬 직관적으로 key/value 데이터를 안전하게 저장할 수 있는 메커니즘을 제공한다.

- Storage 객체는 모두 window 객체 안에 들어가 있으며, 종류로는 2가지가 있다.

  - 로컬 스토리지(Local Storage)

  - 세션 스토리지(Session Storage)

---

### 로컬 스토리지(Local Storage)

- 각 [Origin](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Origin) 별로 스토리지를 관리한다, 브라우저가 닫히거나 다시 열리더라도 유지된다.

- Origin이 같다면 데이터는 모든 탭과 창에서 공유된다.

- 유효 기간없이 데이터 저장하며, 자바스크립트 혹은 브라우저 캐시, 내부적으로 저장된 데이터 지움으로써만 데이터 제거가 가능하다.

---

### 세션 스토리지(Session Storage)

- 각 Origin 별로 스토리지를 관리하는데, 브라우저를 종료하는 경우 데이터가 자동으로 제거 된다. 같은 도메인이라도 세션이 다를 경우 접근할 수 없다, 이 말은 같은 페이지라도 다른 탭에 있으면 다른 곳에 저장되기 때문이다.

- 즉 말하자면, Origin 뿐만 아니라 브라우저 탭에도 종속되어 있다. 이러한 이유로 잘 사용하지 않는다.

- 페이지를 새로 고침하여도 데이터가 사라지지 않고 남아 있다.

---

<br>
<br>

# [IndexedDB](https://developer.mozilla.org/ko/docs/orphaned/Web/API/IndexedDB_API/Basic_Concepts_Behind_IndexedDB)

- 파일이나 Blob 등 많은 양의 구조화된 데이터를 클라이언트에 저장하기 위한 로우 레벨 API

- 웹 스토리지 API 는 적은 양의 데이터를 저장하는데 유용하지만 많은 양의 구조화된 데이터에는 적합하지 않다.

- Indexed Database API 혹은 Indexed DB (예전엔 WebSimplDB)는 인덱스를 사용해 데이터를 고성능으로 탐색할 수 있다.

- SQL을 사용하는 RDBMS와 같이 트랜잭션을 사용하는 데이터베이스 시스템이다.
  RDBMS의 고정컬럼 테이블 대신 JavaScript 기반의 객체지향 데이터베이스이다.

- API는 대부분 비동기 방식으로 동작한다.

- 인덱스 키를 사용해 데이터를 저자하고 회수할 수 있으며, 사용하려면 데이터베이스 스키마를 지정하고, 데이터베이스와 통신을 연 후에, 일련의 트랜잭션을 통해 데이터를 가져오거나 업데이트 해야 한다.

- WebSQL 과 경쟁 관계에 있었는데 W3C 에서 2010년 11월 18일에 WebSQL을 폐기 하였다.

---

<br>
<br>

# [Trust Token??](https://techrecipe.co.kr/posts/19301)

- 링크 참조
