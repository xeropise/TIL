- 결론부터 말하면 자바의 쓰레드는 유저 쓰레드지만, 커널 쓰레드와 1대1로 매핑된다.

  - 과거에 자바는 JVM에서 [Green thread](https://en.wikipedia.org/wiki/Green_threads) 라 불리는 유저 쓰레드를 사용했다. 

    ​	

  - 

  

- 위의 이유로 쓰레드의 구현 방식이 OS에 종속적이며, 스케쥴링 정책 또한 OS 종속적이다.



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

