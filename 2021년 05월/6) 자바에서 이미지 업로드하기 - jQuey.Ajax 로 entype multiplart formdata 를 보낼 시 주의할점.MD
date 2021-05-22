- 이미지 업로드 기능 구현 중, input type="file" 태그로 form 으로 직접 보내는 것이 아닌, Ajax로 보내려고 하는데 에러가 발생했다.

```
Uncaught TypeError: Illegal invocation

java.lang.IllegalStateException: Current request is not of type
```

- 해당 에러는 Ajax로 데이터를 보낼 떄 파라미터 값이 정상적으로 들어가지 않아 발생하는 에러인데 아무리봐도 parameter 에 제대로 값을 넣었는데도 데이터가 전달이 되지 않는 것이다.

_예시_

```javascript
var files = $("#file")[0].files[0];

$.ajax({
  url: "/url",
  type: "POST",
  data: {
    file: files,
  },
}).done(function (data) {});
```

- 왜 동작을 안하는것인가 하고 검색해봤는데 [스택 오버 플로우](https://stackoverflow.com/questions/5392344/sending-multipart-formdata-with-jquery-ajax)에서 관련 답변을 얻을 수 있었다.

- 답변을 이렇다. javascript 객체중 [FormData](https://developer.mozilla.org/ko/docs/Web/API/FormData)를 사용하고, multipart 방식으로 보낼 때 contentType,processData 옵션을 false 로 설정해야 한다는 것이다.

- [Ajax](https://api.jquery.com/jquery.ajax/) 옵션 중 contentType은 default 값은 'application/x-www-form-urlencoded; charset=UTF-8'로 설정되어 있으므로 직접 String으로 입력해주거나 false 를 입력하여 multipart/form-data 로 전송할 수 있다.

```
Content-Type:multipart/form-data; boundary=----WebKitFormBoundarybuOGBs9coioS5Kb9
```

- processData의 default는 ture 로 설정되어 있는데, 이런 경우에는 서버에 전달되는 데이터가 query string 형태로 전달된다. data 파라미터로 전달한 데이터를 내부적으로 변환하는데 파일의 전송하는 경우 이를 하지 않아야 하므로 false로 설정하는 것이다.

```javascript
var data = new FormData();
jQuery.each(jQuery("#file")[0].files, function (i, file) {
  data.append("file-" + i, file);
});

jQuery.ajax({
  url: url,
  data: data,
  cache: false,
  contentType: false,
  processData: false,
  method: "POST",
  type: "POST", // For jQuery < 1.9
  success: function (data) {
    alert(data);
  },
});
```
