- 자바에서 이미지를 다루기전에 자바에서 파일의 입출력을 어떻게 다루는지 정의 해보자.

### 자바 IO (입출력)

![img_c_stream](https://user-images.githubusercontent.com/50399804/118989209-d0065c00-b9bc-11eb-950c-22092a35848a.png)

- 자바에서는 파일이나 콘솔의 입출력을 직접 다루지 않고, 스트림(stream) 이라는 흐름을 통해 다룬다. 영어의 뜻으로는 '흐르는 시냇물' 을 의미한다.

- 스트림(stream) 이란 실제의 입력이나 출력이 표현된 **데이터의 이상화된 흐름** 을 의미한다. 운영체제에 의해 생성되는 가상의 연결 고리를 의미하며, 중간 매개자 역할을 한다.

- 물리 디스크상의 파일, 장치들을 통일된 방식으로 다루기 위한 가상적인 개념이므로 스트림은 어디서 나왔는지 어디로 가는지 신경을 쓸 필요 없이 자유롭게 어떤 장치, 프로세스, 파일들과 연결될 수 있어 많은 편리성을 준다.

- 일반적으로 데이터, 패킷, 비트 등의 일련의 연속성을 갖는 흐름을 의미한다. 많은 비유 중에 흐르는 시냇물에 비유하는 이유는 **물이 한쪽 방향으로 흐르는 것** 처럼 **단방향 통신** 만 가능하기 때문에 **하나의 스트림으로는 입력과 출력을 동시에 처리할 수 없다**

![5WrVE](https://user-images.githubusercontent.com/50399804/118990406-d2b58100-b9bd-11eb-9905-6f969a8280bd.png)

- 따라서 스트림은 사용 목적에 따라 입력 스트림과 출력 스트림으로 구분된다. 자바에서는 java.io 패키지를 통해 [InputStream](https://docs.oracle.com/javase/8/docs/api/java/io/InputStream.html) 과 [OutputStream](https://docs.oracle.com/javase/8/docs/api/java/io/OutputStream.html) 클래스를 별도로 제공하고 있다. 즉, 자바에서 스트림 생성이란 이러한 스트림 클래스 타입의 인스턴스를 생성한다는 의미이다.

- InputStream 클래스에서는 read() 메소드가, OutputStream 클래스에는 write() 메소드가 각각 추상 메서드로 포함되어 있는데 적절히 구현해야 입출력 스트림을 생성하여 사용할 수 있다.

| 클래스       | 메소드                                 | 설명                                                                       |
| ------------ | -------------------------------------- | -------------------------------------------------------------------------- |
| InputStream  | abstract int read()                    | 해당 입력 스트림으로부터 다음 바이트를 읽어들임.                           |
| InputStream  | int read(byte[] b)                     | 해당 입력 스트림으로부터 특정 바이트를 읽어들인 후, 배열 b에 저장함.       |
| InputStream  | int read(byte[] b, int off, int len)   | 해당 입력 스트림으로부터 len 바이트를 읽어들인 후, 배열 b[off]부터 저장함. |
| OutputStream | abstract void write(int b)             | 해당 출력 스트림에 특정 바이트를 저장함.                                   |
| OutputStream | void write(byte[] b)                   | 배열 b의 특정 바이트를 배열 b의 길이만큼 해당 출력 스트림에 저장함.        |
| OutputStream | void write(byte[] b, int off, int len) | 배열 b[off]부터 len 바이트를 해당 출력 스트림에 저장함.                    |

- 참고로 [Byte](https://namu.wiki/w/%EB%B0%94%EC%9D%B4%ED%8A%B8) 란 데이터로 생각하면 쉬우며, 정보처리의 기본단위로 생각하면 되겠다. 자세한 것은 링크 참조

<br>

![java-Byte-Stream](https://user-images.githubusercontent.com/50399804/118994725-88360380-b9c1-11eb-8ddd-90d686efe9dc.png)

**바이트 기반 스트림**

- 자바에서 스트림은 기본적으로 바이트 단위로 데이터를 전송하며, 다양한 바이트 기반의 입출력 스트림을 제공하고 있다. 그림, 멀티미디어, 문자 등 모든 종류의 데이터를 주고받을 수 있다는 특징이 있다.

| 입력 스트림          | 출력 스트림           | 입출력 대상 |
| -------------------- | --------------------- | ----------- |
| FileInputStream      | FileOutputStream      | 파일        |
| ByteArrayInputStream | ByteArrayOutputStream | 메모리      |
| PipedInputStream     | PipedOutputStream     | 프로세스    |
| AudioInputStream     | AudioOutputStream     | 오디오 장치 |

<br>

**보조 스트림**

- 자바에서 제공하는 보조 스트림은 실제로 데이터를 주고받을 순 없지만, 다른 스트림의 기능을 향상시키거나 새로운 기능을 추가해 주는 스트림이다.

| 입력 스트림         | 출력 스트림          | 설명                                                                            |
| ------------------- | -------------------- | ------------------------------------------------------------------------------- |
| FilterInputStream   | FilterOutputStream   | 필터를 이용한 입출력                                                            |
| BufferedInputStream | BufferedOutputStream | 버퍼를 이용한 입출력                                                            |
| DataInputStream     | DataOutputStream     | 입출력 스트림으로부터 자바의 기본 타입으로 데이터를 읽어올 수 있게 함.          |
| ObjectInputStream   | ObjectOutputStream   | 데이터를 객체 단위로 읽거나, 읽어 들인 객체를 역직렬화시킴.                     |
| SequenceInputStream | X                    | 두 개의 입력 스트림을 논리적으로 연결함.                                        |
| PushbackInputStream | X                    | 다른 입력 스트림에 버퍼를 이용하여 push back이나 unread와 같은 기능을 추가함.   |
| ObjectInputStream   | PrintStream          | 다른 출력 스트림에 버퍼를 이용하여 다양한 데이터를 출력하기 위한 기능을 추가함. |

<br>

**문자 기반 스트림**

- 자바에서 가장 작은 타입인 char 형이 2바이트이므로, 1바이트씩 전송되는 바이트 기반 스트림으로는 원활한 처리가 힘들다. 이러한 이유로 문자 기반의 스트림도 별도로 제공하는데 InpuStream 을 Reader 로, OutputStream 을 Writer 로 변경하면 사용할 수 있다.

| 입력 스트림     | 출력 스트림     | 입출력 대상 |
| --------------- | --------------- | ----------- |
| FileReader      | FileWriter      | 파일        |
| CharArrayReader | CharArrayWriter | 메모리      |
| PipedReader     | PipedWriter     | 프로세스    |
| StringReader    | StringWriter    | 문자열      |

<br>

**문자 기반 보조 스트림**

| 입력 스트림    | 출력 스트림    | 입출력 대상                                                                     |
| -------------- | -------------- | ------------------------------------------------------------------------------- |
| FileReader     | FileWriter     | 필터를 이용한 입출력                                                            |
| BufferedReader | BufferedWriter | 버퍼를 이용한 입출력                                                            |
| PushbackReader | X              | 다른 입력 스트림에 버퍼를 이용하여 push back이나 unread와 같은 기능을 추가함.   |
| X              | PrintWriter    | 다른 출력 스트림에 버퍼를 이용하여 다양한 데이터를 출력하기 위한 기능을 추가함. |

- 위와 같은 클래스는 아니지만 콘솔과 같은 표준 입출력 장치를 위해 System 이라는 표준 입출력 클래스도 있다. java.lang 패키지에 포함되어 있다. 자바에서 기본적으로 생성하기 때문에 별도로 생성할 필요가 없다.

<br>

### [File](https://docs.oracle.com/javase/8/docs/api/java/io/File.html) 클래스

- 앞에서 설명한 자바 IO 클래스를 이용하면 파일을 통해 입출력 작업을 수행할 수 있으나, 파일의 제거나 디렉터리에 관한 작업은 이를통해 수행할 수 없다.

- 자바는 입출력 작업을 제외한 파일과 디렉터리에 관한 작업을 File 클래스를 통해 처리하도록 하고 있다. File 인스턴스는 파일 일수도, 디렉터리 일 수도 있다.

- Windows 에서는 \ (역 슬래시) 가 구분자이나, Linux 및 Unix 시스템에서는 / (슬래시) 가 구분자로 사용되는데 개발 시 [JVM 이 운영체제를 판단하여 알맞은 값을 설정해주는 메소드](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator)도 있다.

- 자세한 것은 [이 블로그](https://dololak.tistory.com/436?category=636500)를 참조하자.
