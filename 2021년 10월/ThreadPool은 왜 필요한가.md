

### ThreadPool은 왜 필요할까?

- Thread 생성자로 쓰레드를 같은 작업에 매번 생성한다고 가정하자.

  

- Thread는 생성하면서 시스템의 메모리를 할당받고 작업을 수행, 작업을 종료하고 메모리가 해제되고 GC에 의한 인스턴스 제거(오버헤드)

  

- Thread가 무수히 많아지면 Thread 스케쥴러가 Thread 중에 우선도와 도착 시간등을 계산하여 Thread를 할당하고 작업을 처리하게 된다.

  

- 무수히 많아지면 관리또한 어렵다. 프로그래밍 하기도 상당히 버거워진다.

  

- Thread Scheduling + Thread의 생성과 소멸로 인한 오버헤드 + Thread의 관리 + 복잡한 프로그래밍 => 비용 으로 인해 ThreadPool이 등장하게 되었다.



<br>



***

### ThreadPool은 무엇인가

![unnamed](https://user-images.githubusercontent.com/50399804/139005365-b25d8d8e-f63c-4858-9923-a6d80e451f49.jpeg)

- 특정 작업을 처리할 쓰레드를 제한된 개수만큼 미리 생성해놓고, 작업이 큐에들어오면 대기하고있던 쓰레드를 할당, 재사용함.

  

- 기존 쓰레드를 풀에서 관리하지 않았을 때의 문제점을 모두 해결할 수 있다.

  

- 하지만 문제가 없는 것은 아니다.

  - 너무 많은 쓰레드를 생성해 놓을 경우, 메모리 낭비로 이어질 수 있다.

    

  - 대기하고 있는 쓰레드가 생긴다. 쓰레드 3개가 처리 시간이 다른 3개의 작업을 동시 실행한 경우, 빨리 처리한 쓰레드는 놀고 있다.

    

- 사용법은 자바 문법에 작성해 보겠다.

> https://stackoverflow.com/questions/7728102/why-doesnt-this-thread-pool-get-garbage-collected
>
> https://velog.io/@jsj3282/Threadpool
>
> https://docs.microsoft.com/en-us/windows/win32/procthread/thread-pools
>
> https://heowc.dev/2018/02/08/thread-pool/