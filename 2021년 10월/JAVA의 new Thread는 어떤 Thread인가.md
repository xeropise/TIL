### 자바의 쓰레드?

- 결론부터 말하면 자바의 쓰레드는 유저 쓰레드지만, 커널 쓰레드와 1대1로 매핑된다.

  - 과거에 자바는 JVM에서 [Green thread](https://en.wikipedia.org/wiki/Green_threads) 라 불리는 유저 쓰레드를 사용했다. 
    - 유저 쓰레드가 코어를 전부 사용하지 못하는점, 쓰레드가 블락되는점등으로 인해 <u>**native 쓰레드(커널 쓰레드)를 사용**</u>하게됨

  

- 위의 이유로 쓰레드의 구현 방식이 OS에 종속적이며, 스케쥴링 정책 또한 OS 종속적이다.



<br>



***

### JVM이 어떻게 동시에 다수의 쓰레드를 동작시키는가

![maxresdefault](https://user-images.githubusercontent.com/50399804/138999780-cc8199ea-fec9-4a20-8b8a-e4eac4dbb711.jpeg)

- 자바에서의 쓰레드 스케쥴러

  - JVM에서 쓰레드를 동작시키는 부분

    

  - 코드로 컨트롤이 불가능함. 

    

  - RUNNABLE state 에 있는 쓰레드를 선택하여 동작시킨다.

    - 다중 쓰레드가 RUNNABLE 상태에 있다면 어떤 쓰레드를 먼저 동작시킬지는 보장이 없음

      

  - 사용되야하는 쓰레드를 선택하는데 몇가지 요소와 기준이 있음

    - 우선도

      ![스크린샷 2021-10-16 오후 1 29 32](https://user-images.githubusercontent.com/50399804/139000219-23ae28bd-074a-46e9-9197-870ab33b7441.png)

      - 쓰레드가  만들어지면 1~10의 우선도를 가지고 있는데, 높은 우선권을 가진 쓰레드가 실행을 위해 선택될 가능성이 높음

        

      - 단, 다중 쓰레드가 같은 우선도를 가진다면 랜덤하게 선택

        

      - 우선순위(Preemptive-priority) 스케쥴링이 이해 해당함

        

    - 도착 시간

      - 쓰레드 스케쥴러가 쓰레드의 도착시간에도 의존한다. 두개이상의 쓰레드가 같은 우선도를 가지고있다면 쓰레드의 도착시간을 체크한다.

        (코드로는 안보임, OS가 체크하나봄)

        

      - FIFS(First Come First Service) 스케쥴링이 이해 해당함



<br>



![31](https://user-images.githubusercontent.com/50399804/139000766-fe44254c-6f54-4cdd-830a-c621495d4ecd.p

> https://stackoverflow.com/questions/18278425/are-java-threads-created-in-user-space-or-kernel-space
>
> https://medium.com/@unmeshvjoshi/how-java-thread-maps-to-os-thread-e280a9fb2e06
>
> https://goyunji.tistory.com/64
>
> https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=mals93&logNo=220730379008
>
> https://javagoal.com/thread-scheduler-in-java/
>
> https://stackoverflow.com/questions/41759261/how-jvm-thread-scheduler-control-threads-for-multiprocessors
>
> https://nesoy.github.io/articles/2018-11/Context-Switching
>
> https://itmore.tistory.com/entry/%EA%B7%B8%EB%A6%B0-%EC%8A%A4%EB%A0%88%EB%93%9C-%EB%9E%80

