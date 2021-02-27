### Invalid Hook Call Warning - Hook 을 사용할 때, 다른 컴포넌트를 불러올 때 주의점

ZeroCho 의 리액트 웹게임 강의 라우터 편 듣는 중, 알 수 없는 에러 발생

![5mbO4](https://user-images.githubusercontent.com/50399804/109388956-883bdf80-794d-11eb-8ea3-2b240a33a8ed.png)



지금까지, 만들 게임 컴포넌트를 한 컴포넌트에 라우터 기능을 이용하여, 불러오는 중이 었다.

![캡처](https://user-images.githubusercontent.com/50399804/109389186-ca195580-794e-11eb-8b0f-39f6aff403f7.JPG)


원인은 3번이고 다음과 같다.

훅의 경우, 불러오는 리액트의 대상이 다르면 에러가 난다. 클래스의 경우 제대로 임포트가 된다.

클래스를 쓰는 경우 @babel/plugin-proposal-class-properties 사용하여, 클래스 컴포넌트가 정상적으로 돌아가도록 보장해줘야 한다.

이와 관련된 내용은 리액트 공식홈페이지 [여기](https://ko.reactjs.org/warnings/invalid-hook-call-warning.html) 에서도 설명해 주고 있다.
