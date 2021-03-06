- 개발자를 시작한지 어언 1년.. 곧 퇴사를 앞두고 있는데, 마지막 일로 이미지 업로드 기능을 구현해야 했다. 업로드 하기전에 썸네일을 보여주고, 저장을누르면 업로드를 하는 방식이다.

- 근데 생각해보니 나는 1년동안 이미지 업로드를 해본적이 없다. 그래서 막상 시작하려니, **내가 뭐부터 알아야하지?** input 태그? 자바 파일 입출력? 혼란이 찾아왔다. 나의 미래를 위해 차근차근 정리해보도록 하자.

---

### 0) HTML form 태그의 enctype 속성

- HTTP POST 메서드는 서버로 데이터를 전송하고, 브라우저에서 서버와 통신하기 위해 form 태그를 사용하는데, 파일을 전송하려면 form 태그에 enctype="multipart/form-data" 속성을 추가해야 파일을 웹서버에 전송할 수 있다. 그런데 왜 해당 속성은 기본으로 지정되어 있지 않고 별도로 지정해야 할까?

- HTTP Header 는 사용자의 브라우저와 웹 서버가 요청 또는 응답으로 부가적인 정보를 전송할 수 있도록 도와주는데, form 에 입력한 enctype 은 HTTP Header 의 내용 중 Content-type 을 수정하는 것이다.

```
Content-Type: text/html; charset=utf-8
Content-Type: multipart/form-data; boundary=something
```

- Content-type 에는 다양한 것들이 있지만, 브라우저가 POST 로 전송할 때는 대표적으로 3개가 있다.

  - text/plain

    - 공백 문자(space)는 '+' 기호로 변환하지만, 나머지 문자는 모두 인코딩되지 않음을 명시한다.

  - application/x-www-form-urlencoded

    - 일반적인 브라우저에서 form 에 enctype 을 별도로 지정하지 않을 때 이 타입으로 전송되는데, key=value&key=value 형태로 전송되며, 영문, 숫자가 아닌 문자는 % 기호 및 문자의 ASCII 코드를 나타내는 두 개의 16진수로 변환된다.

    - 전송하려는 데이터가 영문, 숫자가 아닌 3바이트로 표현하기 때문에 바이너리 파일을 전송할 경우, payload 를 3배로 만들기에 무척 비효율적이다.

    - 이런 이유로 브라우저에서 파일을 전송할 때는 적합하지 않은 enctype 이다.

    - 그럼 왜 항상 multipart/form-data 로 데이터를 전송하지 않는 것 일까?

  - ## multipart/form-data

    - 일반적으로 HTTP request 는 Body에 클라이언트가 전송하려고 하는 데이터를 넣을 수 있는데 데이터의 타입을 HTTP Header 에 명시해 줌으로써 서버가 타입에 따라 알맞게 데이터를 처리하게 된다. 이 Body의 타입을 명시하는 header 가 Content-Type 이다.

    - 파일 업로드를 하는경우, 자신이 업로드한 이미지를 올리는 form 의 경우, 사진에 대한 설명을 위한 input 과 사진 파일을 위한 input 2개가 들어갈 것인데, 이 두 input 간에 Content-type 이 전혀 다르다.

    - 예로, 사진 설명은 text/html 이겠지만, 사진 파일은 image/jpeg 일 것

    - Http Header 의 Content-Type 에는 동시에 이를 명시할 수 없어 두 종류의 데이터가 하나의 HTTP Request Body에 들어가야 하는데, 한 Body 에서 이 두 종류의 데이터를 구분해서 넣어주는 방법이 필요한데, 이를 위해 등장한 것이 multipart 타입이다.

    - MultiPart/form-data 로 enctype 을 지정하여 요청을 날리게 되면 Header 에 Content-Disposition 이란 옵션이 추가되어, 데이터를 구분하여 보내는 것을 알 수 있다.

    ```
    Content-Type: multipart/form-data; boundary=aBoundaryString
    (other headers associated with the multipart document as a whole)

    --aBoundaryString
    Content-Disposition: form-data; name="myFile"; filename="img.jpg"
    Content-Type: image/jpeg

    (data)
    --aBoundaryString
    Content-Disposition: form-data; name="myField"

    (data)
    --aBoundaryString
    (more subparts)
    --aBoundaryString--

    ```

    - 텍스트로만 이루어진 데이터 전송에 multipart 를 사용하는 경우, MIME 헤더가 추가되는 오버헤드가 발생하기 때문에 텍스트로만 이루어진 데이터 전송은 오히려 비효율적인 것이다. (안된다는 것은 아니다) 따라서 전송되는 데이터의 유형과 양에 따라 효율적인 Content-type 을 선택해야 한다.

### 1) HTML input type='file'

- 웹페이지에서 사용자의 로컬 파일을 입력받기 위해서는 input 태그의 type 속성을 file 로 지정하는 방법을 사용해야 한다.

- 기본적으로 브라우저에서는 보안 상 로컬 파일에 직접 접근할 수 없기 때문이다.

- 이렇게 선택한 파일을 File 로 정의되고, FileList 에 담기게 된다.

![캡처](https://user-images.githubusercontent.com/50399804/117667793-3b437780-b1e0-11eb-986e-948ea35e2173.JPG)

- input type='file' 의 속성으로는 다음이 있다.

  _1. accept_

  - 입력받을 수 있는 파일 유형을 허용하는 문자열로, "," 로 구분한 고유 파일 유형 지정자의 목록으로 다음 형태 중 하나를 취할 수 있다.

  - 마침표로 시작하는 유효한 파일 이름 확장자, 대소문자를 구분하지 않는다.  
    ex) .jpg, .pdf, .doc.

  - 확장자를 포함하지 않는 유효한 [MIME](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types) 유형 문자열

  - audio/\* 는 모든 오디오 파일을 의미
  - video/\* 는 모든 비디오 파일을 의미
  - image/\* 는 모든 이미지 파일을 의미 한다.
    ```html
    <input type="file" accept="image/jpeg>
    ```

  _2. capture_

  - accept 특성이 이미지나 비디오 캡처 데이터를 요구할 경우, capture 특성으로는 어떤 카메라를 사용할지 지정할 수 있다. _user_ 값은 전면 카메라와 마이크를, _environment_ 값은 후면 카메라와 마이크를 사용하게 설정할 수 있다. capture 특성을 누락한 경우, User Agent(브라우저) 가 어떤 쪽을 선택할지 스스로 결정한다. 요청한 방향의 카메라를 사용할 수 없는 경우, User Agent 는 자신이 선호하는 기본 모드로 대체할 수 있다.

- 모바일 같은 특정 기기에서 capture 방법을 설정할 수 있다.

  1.  capture ='camera' 카메라
  2.  capture ='camcorder' 동영상
  3.  capture ='microphone' 마이크

- capture 속성을 지원하지 않는 브라우저의 경우, accept="image/\*;capture=camera" 으로 사용할 수도 있다.

_3. files_

- 선택한 모든 파일을 나열하는 [FileList](https://developer.mozilla.org/ko/docs/Web/API/FileList) 객체로, multiple 특성을 지정하지 않았다면 두 개 이상의 파일을 포함하지 않는다.

- FileList 는 Symbol (Symbol.iterator) 가 정의되어 있어, 순차적으로 참조하기 위해 for of 를 사용할 수 있다. 혹은 [Array.from()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/from) 로 변환하여 참조 할 수 있다.

- HTMLInputElement.files 속성은 선택한 파일 목록을 FileList 객체로 반환한다. FileList 는 배열이므로, length 속성을 사용해 현재 선택한 파일의 수를 알 수 있다.

- [File](https://developer.mozilla.org/ko/docs/Web/API/File) 은 FileList 객체에서 가져올 수 있는 객체로, 특정 종류의 [Blob](https://developer.mozilla.org/ko/docs/Web/API/Blob) 이다. FileReader, URL.createObjectURL(), createImageBitmap(), XMLHttpRequest.send() 는 Blob 과 File 을 모두 허용 한다.

  \# Blob 에 대한 자세한 설명은 [이 블로그](https://heropy.blog/2019/02/28/blob/)를 참조 하자.

_4. multiple_

- multiple 을 지정한 경우, 사용자가 파일 선택 창에서 복수의 파일을 선택할 수 있다.
- input 요소의 type 속성 값이 email 인 경우에는 이메일 사이에 콤마(,) 를 추가하여, 여러 이메일 주소를 동시에 입력할 수 있으며, type 속성값이 file 인 경우에는 CTRL 이나 SHIFT 키를 사용하여 여러 파일을 동시에 선택할 수 있다.

- multiple 은 불리언 속성으로 해당 속성을 명시하지 않으면, 자동으로 false 값을 가지게 된다.

_5. webkitdirectory_

- 사용자의 파일 선택 창에서 폴더를 선택하여 업로드 가능하게 한다. 다만 일부 브라우저에서는 미지원한다. 글 작성 기준, Internet Explorer 나 Firefox Android

- input.value 에는 파일의 경로를 가지는데, 브라우저에서 보안 상 로컬 파일에 직접 접근 할 수 없으며, 로컬 파일 구조의 노출을 막고자 c:\fakepath\ 포함하여 숨긴다. 브라우저마다 구현된 형태가 다를 수 있다.

> 다음에는 그럼 업로드 하기 전 미리보기를 어떻게 할지 고민해 보자.
