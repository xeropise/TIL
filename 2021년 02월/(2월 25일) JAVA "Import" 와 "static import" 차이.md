### 자바 import 와 static import 의 차이 



***

#### Java Static Import

- 자바 5에서 도입된 static import 는 개발자가 클래스의 어떤 스태틱 멤버도 직접적으로 접근 가능하게 해준다. 클래스명.멤버명 으로 접근할 필요가 없다.

```java
import static java.lang.System.*;
class StaticImportExample {
  public static void main(String args[]){
    
    out.println("Hello"); System.out 으로 접근할 필요가 없다.
    out.println("Java");
  }
}
```

> 하지만, 과용하는 경우에는 프로그램의 가독성이 떨어지고, 유지보수가 힘들 수 있다.
> 

#### import 와 static import 차이점은 뭘까?

- import 는 클래스나 인터페이스에 접근 권한을 주는 반면, static import 는 클래스의 스태틱 멤버에 접근 권한을 주는 것이다.
