1. @JsonFormat(without = [JsonFormat.Feature.ACCEPT_SINGLE_VALUE_AS_ARRAY])

    - 프론트에서 String 형태의 List를 보내면 다음과 같은 에러가 발생한다.

    <br>
    
    ```
    "JSON parse error : Cannot deserialize instance of 'java.util.ArrayList' out of VALUE_STRING token; nexted exception is com.fasterxml.jackson.databind.exc.MismatchedInputException
    ```
  
    <br>
    
    - 위 어노테이션을 클래스 혹은 변수 위에 추가시켜주면 해결 된다.

<br>

2. @JsonProperty

    - 객체의 JSON 변환 시 키 값을 마음대로 수정할 수 있도록 해 준다.

    <br>
    
    - Maven 을 기준으로 다음 의존성이 필요하다. 

    ```XML
    <dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.8</version>
    </dependency>
    ```

    <br>
    
    - @JsonProperty("원하는이름") 으로 변수명 위에 사용하면 된다.
    
    <br>
    
    ```java
    @JsonProperty("user_id")
    private String userId;
    ```
    
    <br>
    
    - 위의 경우 {"userId":"xeropise"} 에서 {"user_Id":"xeropise"} 로 변환된다.
