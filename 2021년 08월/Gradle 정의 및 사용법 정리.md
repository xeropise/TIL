### Gradle 이란?

- 오픈 소스 빌드 자동화 도구, 어떤 소프트웨어의 타입이라도 충분히 유연성하게 동작하도록 설계되었다.

- 중요한 특징들만 나열하자면 다음과 같다.

  - 높은 성능

    - Gradle은 불필요한 작업을 task 만 실행함으로써 피할 수 있다.

    - 빌드 캐시를 사용하여 이전에 혹은 다른 기계에서 실행된 task의 결과들을 다시 사용할 수 있다.

  - JVM 기반

    - Gradle은 JVM 위에서 동작해 사용하기 위해서는 JDK가 설치되어야 한다. 자바 플랫폼에 친숙한 사용자라면 많은 도움이 된다.

    - 그렇다고 JVM 프로젝트들만 빌딩하도록 제한되어 있는것은 아니다.

  - Conventions

    - Gradle은 Maven's book을 카피한 conventions 을 실행해서 자바 프로젝트와 같은 프로젝트의 흔한 타입을 구성할 수 있다.

    - 적절한 플러그인들을 적용하고 가벼운 빌드 스크립트로 많은 프로젝트들 만들 수 있다.

    - 그렇다고, 그것들을 override 하라는 것은 아니며 자기만의 tasks, 그리고 많은 다른 커스텀을 통해 나만의 convention-based 빌드를 만들 수 있다.

  - 확장성

    - 즉시 Gradle을 나만의 task 타입 혹은 빌드 모델 제공을 통해 확장할 수 있다.

    - Android 빌드 서포트 예를 보면 많은 새로운 빌드 컨셉(예로 flavors) 을 볼 수 있다.

  - IDE support

    - 몇몇의 주요 IDE 들은 Gradle 빌드를 가져오게 하고 상호작용하다.

    - Android Studio, Intellij IDEA, Eclipse, NetBeans

    - Gradle은 Visual Studio 에 프로젝트를 로드하기 위해 솔루션 파일들을 생성하는 것을 지원한다.

<br>

---

- Gradle 에 대해 알아야 할 5가지 핵심 특징

  1. Gradle의 일반적인 목적은 빌드 도구이다.

     - Gradle은 어떤 소프트웨어도 빌드할 수 있다. 어떻게 만들어야 하는지 어떻게 끝나야 하는지 제안을 하지 않는다.

     - 예를 들면 리포지터리와 파일 시스템이 dependency management 가 오직 Maven 과 Ivy-compatiable 만 지원한다는 것이다.

     - Gradle은 프로젝트의 공통 타입들을 쉽게 빌드할 수 있다. [plugins](https://docs.gradle.org/current/userguide/plugins.html#plugins) 을 통해 미리 만들어진 기능 혹은 layer of conventions 를 추가할 수 있다.

     - 커스텀 플러그인은 만들거나 publish하고 나만의 conventions 이나 빌드 기능들 캡슐화 할 수 있다.

  2. 코어모델은 tasks 에 기반한다

     - Gradle은 빌드를 tasks의 Directed Acyclic Graphs (DAGs) 라는 일의 단위로 모델한다.

     - 이말은 빌드가 핵심적으로 tasks 셋을 구성하고 연결한다는 말이다.

       - depedencies에 기반을 두고 DAG 를 만들고, task graph 가 만들어지면 Gradle 은 어떤 tasks 가 순서대로 실행되어야 하는지 결정한다.

       ![task-dag-examples](https://user-images.githubusercontent.com/50399804/130358358-9af693b9-3bfa-4985-86b0-da1650890ae9.png)

       > 위 다이아그램은 task graph 의 두가지 예를 보여주고 있다. 하나는 abstract 다른 것은 concrate 이다.

     - 거의 모든 build 프로세스가 위와 같은 방법으로 graph of tasks 로 모델 되어진다. Gradle 이 유연성있는 이유 중 하나이다. plugins와 작성된 scripts 로 task graph 가 정의될 수 있다. [task depedency 메카니즘](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html#sec:task_dependencies) 을 통해 tasks 가 링크될 수 있다.

     - Tasks 는 다음과 같이 구성되어 있다.

       - Actions - 어떤 것을 해야하는 작업 조각(구성), 파일을 복사하거나 소스를 컴파일

       - Inputs - Actions 가 사용하거나 동작할 값, 파일, 폴더들

       - Outputs - Actions 가 수정하거나 만들어낼 파일과 폴더들

     - 사실상, 방금 언급했던 것들은 어떤 tasks 가 필요로 해지는가에 따라서 선택적일 수 있다. 몇몇 tasks ( 예로 [standard lifecycle tasks](https://docs.gradle.org/current/userguide/base_plugin.html#sec:base_tasks) ) 는 어떠한 Actions 도 가지고 있지 않다. 편리를 위해 많은 tasks를 군집해 놓은 것 들이다.

     - 마지막으로 하나 중요한 것이 있는데, Gradle 의 [incremental build](https://docs.gradle.org/current/userguide/more_about_tasks.html#sec:up_to_date_checks) 지원은 거대하고 믿을만하다. 만약 실제로 clean 작업을 실행하는것을 원치 않는다면, 속도 향상을 위해 사용하지 마라.

  3. Gradle 은 몇몇의 고정된 build 단계를 가지고 있다.

     - Gradle은 3단계로 build scripts 를 평가하고 실행한다. 이것을 이해하는 것은 매우 중요하다.

       1. 초기화

       - build 를 위한 환경을 설치하고, 어떤 프로젝트들이 참여되어야 하는지 결정한다.

       2. 구성

       - 빌드를 위한 task graph 를 만들고 설정하고, 사용자가 실행하려는 task에 기반하여 어떤 tasks 가 실행되고 어떤 순서여야 하는지 결정한다.

       3. 실행

       - 설정 단계 끝에 선택된 tasks 을 실행한다.

     - 위 3단계가 [Build Lifecycle](https://docs.gradle.org/current/userguide/build_lifecycle.html#build_lifecycle) 을 만든다.

     - 잘 설계된 build scripts 는 [중요한 로직이보다는 선언적인 설정](https://docs.gradle.org/current/userguide/authoring_maintainable_build_scripts.html#sec:avoid_imperative_logic_in_scripts) 으로 구성된다.

     - 그러한 설정들은 설정 단계 중에 이해할만하게 평가된 것들이다. 그런 이유로 많은 빌드가 task actions 를 가지고 있다.

       - 예를 들면 doLast{} 혹은 doFirst {} block

       - 설정단계에서 평가된 코드들이 실행 단계에서 어떠한 변화도 보이지 않는 이유이다.

     - 설정 단계의 중요한 특징은 포함된 모든 것들이 build runs 마다 평가되어진다는 것이다. 그런 이유로 [설정 단계에서 비싼 작업들을 피하기](https://docs.gradle.org/current/userguide/authoring_maintainable_build_scripts.html#sec:minimize_logic_executed_configuration_phase) 것이 매우 중요하다.
