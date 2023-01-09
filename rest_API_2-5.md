### REST_API

예외처리 정리
출처(https://bvc12.tistory.com/196)

```java
try {
    // throw로 강제 예외 발생
    throw new Exception("강제 예외 발생!!!");
} catch (Exception e)    {
    System.out.println("err_msg : " + e.getMessage());
    e.printStackTrace();
}
```
**자바에서 강제로 예외를 발생시키기 위해서는 throw를 사용하면 된다. 아래 예시에서는 강제로 Exception을 발생시키면 catch문에서 예외를 잡고 Exception에 대한 메시지를 출력한다.**

___

**@ResponseStatus**는 예외발생시 상태코드 메시지를 자기가 원하는대로 변경 가능

```java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class UserNotFoundException extends RuntimeException {
    public UserNotFoundException(String message) {
        super(message);
    }
}
```![20230109_213536](https://user-images.githubusercontent.com/113106136/211309336-9bde3ece-ffb4-4a00-87db-ed82c36a8bcd.png)
