## (3월 08일) X-Frame-Options



- X-Frame-Options 는 Http Response Header 중 하나로, 해당 페이지를 \<frame> 또는 \<iframe>, \<object> 에서 렌더링 할 수 있는지 여부를 나타내는데 사용된다. 사이트 내 콘텐츠들이 다른 사이트에 포함되지 않도록하여. clickjacking 공격을 막기 위해 이 헤더를 사용한다.



>  _clickjacking 공격?_
>
> - 웹 사용자가 자신이 클릭하고 있다고 인지하는 것과 다른 어떤 것을 클릭하게 속이는 악의적인 기법으로, 비밀정보를 유출시키거나 컴퓨터에 대한 제어를 획득한다. 다양한 웹 브라우저들과 컴퓨팅 플랫폼들에서의 취약점으로서 브라우저 보안 이슈다.
> - iframe 요소와 CSS 를 교묘하게 이용해 투명하게 링크를 만들어 의도되지 않은 행동을 하게 만드는 것이다.

![clickjacking-attacks](https://user-images.githubusercontent.com/50399804/110280178-bbe9ca00-801d-11eb-873e-f963ee3f97ea.png)



-  X-Frame-Options 와 관련해서는 3가지 설정이 가능하다.

```
X-Frame-Options: deny
X-Frame-Options: sameorigin
X-Frame-Options: allow-from https://example.com/
```



- deny

  - 어떠한 사이트에서도 frame 상에서 보여질 수 없다.
  - 다른 웹사이트에서는 표시할 수 없음

- sameorigin

  - 동일한 사이트의 frame 에서만 보여진다. 

  - 동일한 도메인의 페이지 내에서만 표시

- allow-from _uri_

  - 지정된 특정 uri의 frame 에서만 보여진다.



![캡처](https://user-images.githubusercontent.com/50399804/110280373-09663700-801e-11eb-9a93-28a423f8e0f1.PNG)

> 페이스북에서는 deny로 설정 하였다.

_참조_ 

https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/X-Frame-Options