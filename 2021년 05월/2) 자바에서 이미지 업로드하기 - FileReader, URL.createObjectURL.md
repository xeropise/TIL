- 1년하고도 반년전 강남에서 국비 학원을 다닐 때, 이미지 업로드 기능을 구현해보려 한 적이 있다. 적당히 인터넷에서 "스프링 이미지 업로드" 라고 검색하여, 소스를 복사하고, 메이븐에 의존성을 추가하여 시도했던게 기억이 난다.

- 당시에 "미리보기" 를 어떻게 할 것인가 고민을 했는데, 당시에는 아무 생각 없이 파일을 업로드하자마자 바로 가져와서 보여주는 방법으로 나는 미리보기 기능을 구현했구나 하고 착각했던 적이 있었다.

- 생각을 해보면 이미지 미리보기 (preview) 는 클라이언트가 서버에 데이터를 올리기 전에 확인할 수 있어야 하는건데 어떻게 구현할 수 있을까? 앞의 '자바에서 이미지 업로드하기(1)' 에서 언급한 input type='file' 태그에서 파일을 선택하는 경우, file 객체는 다음과 같은 정보를 포함하고 있다.

```javascript
{
  name: 'xeropise.png', // 파일 이름
  size: 74120, // byte 단위 파일 크기
  lastModified:  1495791249810, // 올린 시간 timestamp
  type: 'image/png'
}
```

- 근데 위를 보면 정작 데이터는 없는데, 이는 보안상의 이유로 숨겨져 있다. 이 숨겨져 있는 데이터를 읽기 위해서는 window.FileReader 에 위치하는 FileReader API 를 사용하면 된다.

<br>

## **[FileReader](https://developer.mozilla.org/ko/docs/Web/API/FileReader)**

- FileReader 객체는 웹 애플리케이션이 비동기적으로 데이터를 읽기 위해, File 혹은 Blob 객체를 이용해 파일의 내용을 (혹은 raw data 버퍼) 읽고, 사용자의 컴퓨터에 저장하는 것을 가능하게 해준다.

- File 객체는 input 태그를 이용하여 유저가 선택한 파일들의 결과로 반환된 FileList 객체, 드래그 앤 드랍으로 반환된 DataTransFer 객체 혹은 HTMLCanvasElement 의 mozGetAsFile() API로부터 얻을 수 있다.

- 속성으로는 다음이 있다.

  1. FileReader.error

     - 파일을 읽는 도중에 발생한 에러를 나타낸다.

  2. FileReader.readyState

     - FileReader의 상태를 나타내는 숫자로,  
        EMPTY 0은 데이터가 로드 되지 않음  
       LOADING 1은 데이터가 로딩 중  
       DONE 2는 모든 읽기 요청이 완료됨을 나타낸다.

  3. FileReader.result

     - 파일의 컨텐츠이다.

- FileReader 로 파일을 읽는 방법에는 4가지가 있는데 readAsText, readAsDataURL, readAsArrayBuffer, readAsBinaryString 등이 있다. 위에서 말했듯 비동기 이기 때문에 즉시 파일을 읽지 않으므로 onload 이벤트 핸들러를 붙여 콜백으로 파일을 다 읽었다 라는 것을 알려주도록 해야 한다.

- Mozila 에서 미리보기 혹은 이미지 썸네일을 보여주기 위해, 다음과 같은 함수의 예를 보여주고 있다.

```javascript
function handleFiles(files) {
  for (let i = 0; i < files.length; i++) {
    const file = files[i];

    if (!file.type.startsWith("image/")) {
      continue;
    }

    const img = document.createElement("img");
    img.classList.add("obj");
    img.file = file;
    preview.appendChild(img); // "preview"가 결과를 보여줄 div 출력이라 가정.

    // FileReader의 onload 함수에 함수를 달아 파일을 다 읽을 경우,
    // img 태그의 src를 바꿔 출력
    const reader = new FileReader();
    reader.onload = (function (aImg) {
      return function (e) {
        aImg.src = e.target.result;
      };
    })(img);
    reader.readAsDataURL(file);
  }
}
```

- multiple 로 여러 가지 이미지를 업로드 하였을 경우, 다음과 같이 확인할 수 있다.

```javascript
function setThumbnail(event) {
  for (var image of event.target.files) {
    var reader = new FileReader();
    reader.onload = function (event) {
      var img = document.createElement("img");
      img.setAttribute("src", event.target.result);
      document.querySelector("div#image_container").appendChild(img);
    };

    console.log(image);
    reader.readAsDataURL(image);
  }
}
```

- 근데 Mozila 에서는 이미지 미리보기 기능 구현 시 FileReader 말고도 createObjectURL 을 이용하는 방법을 알려주고 있다.

<br>

## **[URL.createObjectURL](https://developer.mozilla.org/ko/docs/Web/API/URL/createObjectURL)**

- 주어진 객체를 가리키는 URL을 DOMString (UTF-16 문자열) 으로 반환한다. 해당 URL은 자신을 생성한 창의 document 가 사라지면 함께 무효화된다. FileReader 와 달리 동기적으로 즉시 실행된다.

```javascript
const objectURL = URL.createObjectURL(object);
```

- URL.createObjectURL 을 매번 호출할 때마다 새로운 객체 URL 을 생성하는데, 각각의 URL 을 더는 쓰지 않을 땐 메모리 참조를 해제하기 위해, URL.revokeObjectURL() 을 사용해 하나씩 해제해줘야 한다.

- Mozila 의 이미지 썸네일 표시 방법은 다음과 같이 안내하고 있다.

```html
<input
  type="file"
  id="fileElem"
  multiple
  accept="image/*"
  style="display:none"
  onchange="handleFiles(this.files)"
/>
<a href="#" id="fileSelect">Select some files</a>
<div id="fileList">
  <p>No files selected!</p>
</div>
```

```javascript
window.URL = window.URL || window.webkitURL;

const fileSelect = document.getElementById("fileSelect"),
  fileElem = document.getElementById("fileElem"),
  fileList = document.getElementById("fileList");

fileSelect.addEventListener(
  "click",
  function (e) {
    if (fileElem) {
      fileElem.click();
    }
    e.preventDefault(); // "#" 해시로 이동을 방지
  },
  false
);

function handleFiles(files) {
  if (!files.length) {
    fileList.innerHTML = "<p>No files selected!</p>";
  } else {
    fileList.innerHTML = "";
    const list = document.createElement("ul");
    fileList.appendChild(list);
    for (let i = 0; i < files.length; i++) {
      const li = document.createElement("li");
      list.appendChild(li);

      const img = document.createElement("img");
      img.src = window.URL.createObjectURL(files[i]);
      img.height = 60;
      img.onload = function () {
        window.URL.revokeObjectURL(this.src);
      };
      li.appendChild(img);
      const info = document.createElement("span");
      info.innerHTML = files[i].name + ": " + files[i].size + " bytes";
      li.appendChild(info);
    }
  }
}
```

- 이 단계까지 와서 의문.. 그럼 대체 두개 사이엔 어떤 차이가 있을까? 이에 대해서는 스택 오버 플로우에서 간단히 [비교](https://stackoverflow.com/questions/31742072/filereader-vs-window-url-createobjecturl) 해 주고 있다.
