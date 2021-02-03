### 오늘 있던 일 

회사에서 개발자 중 한분이 Slack 으로 메시지를 보내는 기능을 개발하셨다.  

![캡처](https://user-images.githubusercontent.com/50399804/106717783-824f2900-6643-11eb-8cd5-2d242114fa3b.PNG)


오오.. 그렇구나 하고, 동작 그대로 따라하고 있는데 에러가 났다.


![실패](https://user-images.githubusercontent.com/50399804/106717931-b0346d80-6643-11eb-823a-0d33668cd820.PNG)


원인 분석을 해보았다.

![2](https://user-images.githubusercontent.com/50399804/106718111-ee319180-6643-11eb-83b9-7868df32545d.PNG)

DB 프로퍼티 OK

![1](https://user-images.githubusercontent.com/50399804/106718161-00abcb00-6644-11eb-85ad-7becb06ff2ef.PNG)

context 설정 OK


아무문제 없구나 하고 실행했는데... 또 에러

그래서 아무리 고민해도 안되길래, 동료분들께 조언을 구함

![5](https://user-images.githubusercontent.com/50399804/106718352-4d8fa180-6644-11eb-8be1-a50e4dbb6015.PNG)


![2104E64155132AC014](https://user-images.githubusercontent.com/50399804/106718463-6f892400-6644-11eb-934a-93da3c5f7942.jpg)
> !!!  
  내실수아니네 원본이 잘못된거자너

![6](https://user-images.githubusercontent.com/50399804/106718684-b8d97380-6644-11eb-90df-8b9afc38839f.PNG)

![7](https://user-images.githubusercontent.com/50399804/106718685-b8d97380-6644-11eb-8a17-36d0756bae51.PNG)

![7-2](https://user-images.githubusercontent.com/50399804/106718859-f4743d80-6644-11eb-8206-66628f32624e.PNG)

![8](https://user-images.githubusercontent.com/50399804/106718677-b840dd00-6644-11eb-9837-e4d6128ca7bb.PNG)

![8-2](https://user-images.githubusercontent.com/50399804/106718864-f63e0100-6644-11eb-8990-7215d2d32969.PNG)


각 Mapper 클래스에 sqlSession 빈 주입이 다른 이름으로 되어 있던 것이다.. 그걸 모르고 한시간 삽질

근데 갑자기 궁금해졌다.. annotation Injection 방법이 몇개 있더라..? 그래서 찾아봄


***
***

![캡처](https://user-images.githubusercontent.com/50399804/106720799-3aca9c00-6647-11eb-84f4-df0a9d3921cd.PNG)

>출처는 https://codevang.tistory.com/256 






