### Gradle Wrapper

![wrapper-workflow](https://user-images.githubusercontent.com/50399804/137669851-a1b47264-b78d-4126-98a6-32049e83e6ab.png)

- Gradle build 를 실행하는 추천하는 방법은 Gradle Wrapper의 도움을 받는 것이다. 

  

- 필요한 경우 미리 선언된 Gradle의 버전을 호출하는 Script 이다.

  

- 개발자는 Gradle Project 를 수동 설치 과정 없이 빠르게 실행할 수 있다.

  

<br>



***

### Gradle Wrapper 추가하기

- Wrapper 파일을 Gradle이 미리 설치되어야 한다. 모든 기본의 Gradle build 는 wrapper 라고 불리우는 task가 내장되어 있다.

![스크린샷 2021-10-18 오후 1 41 08](https://user-images.githubusercontent.com/50399804/137670313-15266417-fdd1-4c4d-bd05-874039b8c4a4.png)

- wrapper task의 실행은 프로젝트 디렉토리 필수적인 Wrapper 파일들을 생성한다.
  - Wrapper 파일을 다른 개발자에게 이용가능하게하고 환경변수들을 실행하게 하려면 Version Control에 포함되도록 체크해야 한다.

```
$ gradle wrapper
```



- 생성된 Wrapper 프로퍼티 파일인 gradle/wrapper/gradle-wrapper.properties 는 Gradle의 배포에 대한 정보들을 저장하고 있다.

  - Gradle 배포의 서버 호스팅

    

  - Gralde 배포 타입. 기본적으로 -bin 배포가 runtime 에 포함되지만 샘플 코드나 문서는 없다.

    

  - 빌드를 실행하기 위한 Gradle 버전(기본으로는 Wrapper 파일을 생성하기 위해 사용되었던 버전)

  ```
  - gradle/wrapper/gradle-wrapper/properties
  
  distributionUrl=https\://services.gradle.org/distributions/gradle-7.2-bin.zip
  ```



- 이러한 특징들은 Wrapper 파일을 생성할 때 다음 커맨드 옵션으로 설정이 가능하다.

```
--gradle-version
	- Wrapper 를 실행하고 다운로드하기 위한 Gradle 버전
	
--distribution-type
	- Wrapper를 위한 Gradle 배포 타입, bin, all 만 가능하고 기본은 bin 이다.
	
--gradle-distribution-url
	- Gradle 배포 ZIP file 을 가리키는 URL, 회사 내부망에 있는 Gradle 배포 버전을 호스트 한다면 매우 유용하다.
	
--gradle-distribution-sha256-sum
	- https://docs.gradle.org/7.2/userguide/gradle_wrapper.html#sec:verification
```



- task 실행 결과 다음과 같은 폴더 구조가 생성된다.

```
├── a-subproject
│   └── build.gradle
├── settings.gradle
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
└── gradlew.bat
```



- Gradle 프로젝트는 전형적으로 settings.gradle 과 build.gradle 을 각 프로젝트에 제공한다.

  

- Wrapper 파일은 gradle 폴더와 프로젝트 루트 폴더의 나란히 배치된다.

  

- 몇몇 생성된 파일들의 목적은 다음과 같다.

  - gradle-wrapper.jar

    - Gradle 배포판을 다운받기위한 코드를 포함하고 있는 Wrapper JAR 파일

      

  - gradle-wrapper.properties

    - Wrapper의 runtime에 설정할 정보들을 가지고 있는 프로퍼티 파일

      

  - gradlew, gradlew.bat

    - Wrapper와 함께 실행할 쉘 스크립트나 윈도우 배치 스크립트



<br>



***

### Using the Gradle Wrapper

- 스크립트는 프로젝트 내에 숨어있는 gradle-wrapper.jar 를 이용해서 적절한 스크립트를 실행한다.

```java
// window
gradle.bat [task]
  
// linux and OSX
./gradlew [task]  
  
// gradlew.bat
 gradlew.bat [task] 
```



<br>



***

### Customizing, Upgrading, Securitying

- 관련은 [메뉴얼](https://docs.gradle.org/7.2/userguide/gradle_wrapper.html)에서 찾아 보자.