### X-Content-Type-Options

***

- 웹서버가 보내는 MIME 형식 이외의 형식으로 해석을 확장하는 것을 제한하는 크로스 사이트 스크립트 방어법

- 서버에서 보낸 Content-Type 헤더가 잘못 설정되었다고 생각 하는 경우, 브라우저는 스스로 컨텐츠 타입을 추론하는데 이를 악용하여 자바스크립트 파일을 이미지 MIME 로 보내고,
  이미지를 공격자가 사용할 수 있다. 이를 막기 위해, 이 헤더를 서버에서 보내주어, 브라우저가 보낸 컨텐츠 타입을 따르게 강제하는 것.


자료참조 : 

 - [https://webhack.dynu.net/?idx=20161120.001](https://webhack.dynu.net/?idx=20161120.001)

 - [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options)
