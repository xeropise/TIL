### 빌드 스크립트 기초 (Build Script Basics)

- 모든 Gradle build 는 1개 이상의 project (gradle에서 말하는 project) 로 이루어져 있다. 

  

- 라이브러리리 혹은 웹 어플리케이션을 작업할 프로젝트냐에 따라 build 구성이 다랄진다.

  

- project에서 Gradle은 한개 혹은 그 이상의 tasks로 정의된다.

  - task는 build를 수행하는 작업의 단위를 나타낸다.

    

  - 클래스를 컴파일 하거나, JAR 를 만든거나, Javadoc을 만들던지 repository에 구성물을 배포하는 것일 수도 있다.

    

- tasks는 plugin 을 적용함으로써 직접 task를 정의하지 않아도 사용 가능하다. task가 무엇인지 알아 보기 위해 간단히 만들어 보자.



<br>

***

### Hello World

- gradle 커맨드를 사용하여 Gradle build를 수행할 수 있다.

  

- gradle 커맨드는 현재 폴더를 기준으로 build.gradle 파일을 찾는다.

  - build.gradle 을 build script라고도 부른다.

  - build script가 project와 tasks 를 정의한다.

    

- build.gradle 이라고 불리는 파일을 만들고 다음을 작성해 보자.

```groovy
tasks.register('hello') {
  doLast {
    println 'Hello world!'
  }
}
```



- gradle [options...] [tasks...] 로 빌드를 수행할 수 있다. gradle -q hello 로 실행하면 Hello world가 찍히는걸 볼 수 있다.
  - gradle [tasks...] [options...] 도 가능하다.

<br>

***

### BuildScript는 코드이다.

- Groovy 혹은 Kotlin 을 이용하여 코드를 작성할 수 있다. 간단히 맛만 보자.

```groovy
tasks.register('upper') {
    doLast {
        String someString = 'mY_nAmE'
        println "Original: $someString"
        println "Upper case: ${someString.toUpperCase()}"
    }
}

> gradle -q upper
Original: mY_nAmE
Upper case: MY_NAME
```



```groovy
tasks.register('count') {
    doLast {
        4.times {
            print "$it"
        }
    }
}

> gradle -q count
0 1 2 3 	
```



<br>



***

### Task 의존성

- 다른 Task에 의존하는 Task도 생성 가능하다.
- 의존하는 방법은 여러가지인데 [여기](https://docs.gradle.org/current/userguide/more_about_tasks.html#sec:adding_dependencies_to_tasks) 를 참조하자.

```groovy
tasks.register('hello') {
    doLast {
        println 'Hello World'
    }
}

tasks.register('intro') {
    dependsOn tasks.hello
    doLast {
        println "I'm Gradle"
    }
}

> gradle -q intro
Hello world!
I'm Gradle
```



<br>

***

### 유연한 Task 등록

- Task 를 정의할 때, 같은 타입의 테스크를 루프로 정의할 수도 있다.

```groovy
4.times { counter ->
    tasks.register("task$counter") {
        doLast {
            println "I'm task number $counter"
        }
    }
}

> gradle -q task1
I'm task number 1
```



<br>

***

### 기존의 Tasks를 조작하기

- Task가 등록되면, API 를 통해 접근할 수 있다. 이를 통해 런타임에 의존성을 추가하는 방법이 있다.

```groovy
4.times { counter ->
	tasks.register("task$counter") {
		doLast {
			println "I'm task number $counter"
		}
	}
}

tasks.named('task0') { dependsOn('task2', 'task3') }

> gradle -q task0
I'm task number 2
I'm task number 3
I'm task number 0
```



- 혹은 action 을 추가할 수 있다.

```groovy
tasks.named('hello') {
    doFirst {
        println 'Hello Venus'
    }
}

tasks.named('hello') {
    doLast {
        println  'Hello Mars'
    }
}

tasks.named('hello') {
    doLast {
        println 'Hello Jupiter'
    }
}

gradle hello -q
Hello Venus
Hello Earth
Hello Mars
Hello Jupiter

```

> doFirst와 doLast 는 여러번 실행할 수 있는 action 으로 task가 실행되면 순서가 정렬되어 실행된다.



<br>

***

### Ant Tasks 사용하기 

- 생략, [여기](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html) 참조



<br>

***

### Method 사용하기

- 생략, [여기](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html#sec:using_methods) 참조



<br>

***

### Defaulat Tasks

- Gradle 커맨드 실행 시, 다른 task가 언급되어 있지 않다면, 기본적으로 실행할 task 를 지정할 수 있다.

```groovy
defaultTasks 'clean', 'run'

tasks.register('clean') {
    doLast {
        println 'Default Cleaning!'
    }
}

tasks.register('run') {
    doLast {
        println 'Default Running!'
    }
}

tasks.register('other') {
    doLast {
        println "I'm not a default task!"
    }
}

> gradle -q
Default Cleaning!
Default Running!
```



- 위의 경우 gradle clean run 과 동일하다. 멀티 프로젝트에서는 서브 프로젝트 각각 Default Task를 정의할 수 있으며,  subProject가 없는 경우에는 parent Project 것을 사용한다.



<br>

***

### BuildScript 외부 의존성

- BuildScript 가 외부 라이브러리를 필요로 하는 경우, buildSciprt() method 로, BuildScript 자체에 추가할 수 있다.

```groovy
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath group: 'commons-codec', name 'commons.codec', version: '1.2'
    }
}
```



- buildscript() method Block은 [ScriptHandler](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html#sec:using_methods) 라고 불리는 인스턴스를 구성한다. classpath 설정에 의존성을 추가함으로써 build script classpath 를 선언할 수 있다.
  - 이렇게 선언함으로써 build Script안에 있는 클래스들을 사용할 수 있다.

```groovy
import org.apache.commons.codec.binary.Base64

buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath group: 'commons-codec', name: 'commons-codec', version: '1.2'
    }
}

tasks.register('encode') {
    doLast {
        def byte[] encodedString = new Base64().encode('hello world\n'.getBytes())
        println new String(encodedString)
    }
}

gradle -q encode 
aGVsbG8gd29ybGQK
```



- 멀티 프로젝트를 위해, project 의 builscript() method의 선언된 의존성은 모든 서브 프로젝트에서 사용 가능하다. 
  - Build Script 의존성은 Gradle plugins 으로 볼 수 있는데, 자세한 것은 [여기](https://docs.gradle.org/current/userguide/plugins.html#plugins) 서 확인하자.



- builscript block 에서 선언된 의존성이나 리포지터리 값은 build.gradle 자체를 위한 값이지  application 을 위한 것이 아니다. 말하자면
  - 위의 경우,  build.script에서 사용 할 의존성 선언이고, 아래의 경우 application을 위함이다.

```
buildscript {
    dependencies {
        classpath group: 'commons-codec', name: 'commons-codec', version: '1.2'
    }
}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.7.0'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.7.0'
}

```