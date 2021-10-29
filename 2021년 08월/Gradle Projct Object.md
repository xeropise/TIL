### Gradle의 Project 객체란?

- build file 에서  Gradle 과 상호 작용하기 위한 main API, Gradle의 특성들을 프로그램적으로 접근 가능하다.

- [Java Docs](https://docs.gradle.org/current/javadoc/org/gradle/api/Project.html)

<br>

***

### LifeCycle

- Project와 "build.gradle" 은 1대1 관계를 갖는다. build 초기화 중에 Gradle 은 Project 객체를 assemble 한다. 순서는 다음과 같다.

  - build를 위해 [Settings](https://docs.gradle.org/current/javadoc/org/gradle/api/initialization/Settings.html) 인스턴스를 생성한다.

    

  - "settings.gradle" 스크립트를 평가해서 Settings 인스턴스를 설정한다.

    

  - Project 의 인스턴스의 계층 구조를 만들기 위해, 설정된 Settings object 를 사용한다.

    - 즉,  settgins.gradle 파일로 멀티 프로젝트를 구성한다는 말

    

  - "build.gradle" 파일을 실행함으로써 Project 평가하는데 너비 우선 탐색으로 부모 => 자식 순으로 평가한다.



<br>

***

### Tasks

- Project 객체는 [Task](https://docs.gradle.org/current/javadoc/org/gradle/api/Task.html) 객체의 집합이다. TaskContainer의 create() method 를 통해 생성할 수 있고, 또한 getByName(String) 을 통해 접근할 수 있다.

  

- 자세한 것은 후술



- 생성 방법은 매우 여러 개니, 링크된 Task java Docs 를 참고하자.



<br>

***

### Depedencies (여기서 말하는 것은 외부 라이브러리의 의존성을 말하는것이 아님)

- Project 보통 작업을 위해 많은 의존성을 가지고 있을 뿐만 아니라,  다른 Project가 생성하기 위해 많은 것을 만든다.

  - 설정들안에서 그룹화되어 있거나, 리포지터리에서 검색되거나 업로드 될 수 있다.

    

- [ConfigurationContainer](https://docs.gradle.org/current/javadoc/org/gradle/api/artifacts/ConfigurationContainer.html) 나 [DepedencyHandler](https://docs.gradle.org/current/javadoc/org/gradle/api/artifacts/dsl/DependencyHandler.html) 나 [ArtifactHandler](https://docs.gradle.org/current/javadoc/org/gradle/api/artifacts/dsl/ArtifactHandler.html),  [RepositoryHandler](https://docs.gradle.org/current/javadoc/org/gradle/api/artifacts/dsl/RepositoryHandler.html) 를 통해 접근하거나 관리할 수 있다. 

  - Doc에 어떻게 사용하는지 잘 설명되어 있으므로 참조하자.



<br>

***

### Multi-Project Builds

- 후술

<br>

***

### Plugins

- 기본적으로  Gradle 자체로 내장된 Task 가 있지만 특정 작업을 위해 생성된 Task를 가져올 필요가 있다. 

  - 이를 통해 기능을 확장할 수 있다.

  

- Core Plugin 이 있고, 개인이 배포한 Plugin 이 있으니 참조하자.

  

- 자세한 것은 [여기](https://docs.gradle.org/current/javadoc/org/gradle/plugin/use/PluginDependenciesSpec.html) 를 참조하자.

  - [공식 홈페이지 메뉴얼](https://docs.gradle.org/current/userguide/plugins.html)
  - [플러그인이란?](https://as-you-say.tistory.com/144)

  

- 보통 사용 유형을 보면 2가지 형태가 있다.

  - 아래의 plugins block 형태가 plugin 을 적용하는 새로운 방법의 형태로 위는 Deprecated 될 예정
  - [스택오버 플로우](https://stackoverflow.com/questions/32352816/what-the-difference-in-applying-gradle-plugin) 에서 관련 사항을 다루고 있다.
  - 아래 plugins block 에서 id 와 version, apply 를 적용하느냐에 차이가 있다. [여기](https://docs.gradle.org/current/userguide/plugins.html#sec:plugins_block) 를 참조

```groovy
apply plugin: 'maven'

plugins {
  id 'maven' version '1.1.2' apply: false
}
```



<br>

***

### 동적인 Project Properties

- Gradle 은 Project 를 구성하기 위한 Project 인스턴스를 생성하기 위해 빌드를 수행하는데, 이때 buildScript 에서 Project의 프로퍼티 및 메소드에 접근할 수 있다.

```groovy
defaultTasks('some-task') // Project.defaultTasks()
reportsDir = file('reports') // Project.file() and Java Plugin
```



- Project 내부에 project Property 로도 접근 가능하다. 코드를 깔끔하게 보여준다 .

  

- Project 는 접근 가능한 5가지의 스코프를 가지고 있다.

  - Project 객체 자체
    - Project 구현 클래스의 getter와 setter에 모두 접근 가능
  - extra
    - Project의 extra properties, 'ext' 명으로 통해 접근 가능하다. read 와 update 모두 가능

  ```groovy
  project.ext.prop1 = "foo"
  task doStuff {
  	ext.prop2 = "bar"
  }
  subproject { ext.${prop3} = false }
  
  ext.isSnapShot = version.endsWith("-SNAPSHOT")
  if (isSnapShot) {
    // do snapshot stuff
  }
  
  project.ext {
    myprop = "a"
  }
  ```

  

  - extensions

    - Plugin에 의해 Project에 추가, read-Only

    

  - convention

    - Plugin에 의해 추가, POJO의 상황에 따라 읽거나 쓰기 가능
    - [convention 이란?](http://makble.com/what-is-gradle-convention-object)

    

  - tasks

    - task는 task 명이나 프로퍼티 명으로 접근 가능, read-only

      - compile task 는 compile 프로퍼티로 접근이 가능하다.

      

  - 부모 project 로 부터 extra properties & convention properties

    - read-only
