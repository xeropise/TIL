### Blocking vs Non-Blocking

- 제어권이 중점이 된다.

- 제어권을 넘겨주지 않으면 결과값은 리턴하지 않은게 아니다. 모두 완료되지 못했다는 결과값을 넘겨준다.

<br>

> Blocking

- 자신의 작업을 진행하다가 다른 주체의 작업이 시작되면 다른 작업이 끝날 때까지 기다렸다가 자신의 작업을 시작하는 것

- 호출된 함수가 자신이 할 일을 모두 마칠 때까지 **제어권** 을 계속 가지고서 호출한 함수에게 바로 돌려주지 않는 것

> Non-Blocking

- 다른 주체의 작업과 상관없이 내 작업을 하는 것

- 호출된 함수가 자신이 할 일을 채 마치지 않았더라도 바로 제어권을 돌려주어 호출한 함수가 다른 일을 진행할 수 있게 해주는 것

---

### Sync vs Async

- 주체들의 시간을 일치시는 것에 중점

  <br>

> Sync

- 작업을 동시에 수행하거나, 동시에 끝나거나, 끝나는 동시에 시작함을 의미

- 호출된 함수의 수행 결과 및 종료를 호출한 함수가 신경을 쓰는 경우

> Async

- 시작, 종료가 일치하지 않으며, 끝나는 동시에 시작을 하지 않음을 의미

- 호출된 함수의 수행 결과 및 종료를 호출된 함수가 혼자 직접 신경 쓰고 처리하는 경우

---

### Sync, Blocking vs Async, Blocking 아닌가?

<br>

- 그렇게 볼수도 있으나, 동작은 비슷하지만 위에서 설명했듯 중점이 다르다. Blocking, NonBlocking 은 제어권, Sync, Async 는 주체들의 시간을 일치시키는데 중점을 둔다.

<br>

- 솔직히 설명해도 어렵다, 사실 언어로서는 이렇게 확인할 수 있다.

<br>

    |       | Block | Non-Block  |
    | ----- | ----- | ---------- |
    | Sync  | JAVA  |    ??      |
    | Async |   ??  | Javascript |

<br>

- 의문점이 떠오른다, 그럼 대체 Non-Block Sync 와, Block Async는 언제 발생하는 것일까?

<br>

### NonBlocking-Sync

- 호출되는 함수를 바로 리턴하고, 호출하는 함수는 작업 완료 여부를 계속해서 신경 쓰는 것이다.
- 블로그 및 유튜브를 찾다보니 자바에서 이런 코드의 형태가 NonBlocking-Sync 의 형태가 된다고 한다.

```java
ExecutorService threadpool = Executors.newCachedThreadPool();
Future<Long> futureTask = threadpool.submit(() -> factorial(number));

while(!futureTask.isDone()) {
    System.out.println("Futuretask is not finished yet...");
}
long result = futureTask.get();

threadpool.shutdown();

// 우아한 Tech 10분 테코톡  우의 Block vs Non-Block & Sync vs Async 예제
```

- 위와 같이 지속적으로 결과가 끝나는지 확인하면서 while 문 안에 있는 작업을 계속 하는 것이다.

<br>

### Blocking-Async

- 호출되는 함수가 바로 리턴하지 않고, 호출하는 함수는 작업 완료 여부를 신경쓰지 않는 것이다.

- 이 경우는 일부러 이런 방식으로 코딩을 할 필요도 없는데 의도하지 않게 Blocking-Async 로 동작하는 경우가 있다고 한다.

- Node.js 와 Mysql 을 같이 쓰는 형태가 이렇다고 한다. Node.js 에서 Async 로 작업해도, DB 작업 호출 시 MySQL 드라이버가 Blocking 방식으로 동작한다고 한다.

- 사실 나도 이러한 방식으로 나도 모르게 코딩을 많이하고 있는거 같다. 의도하지 않더라도 과정중에 Blocking 으로 동작하는 것이 하나라도 있다면 Blocking-Async 로 동작하는 것이다.

---

\* 참조 한곳

```
https://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/

https://youtu.be/IdpkfygWIMk
```
