### REST_API

**ResponseEntitiy란?**

**ResponseEntity란**, httpentity를 상속받는, **결과 데이터와 HTTP 상태 코드**를 **직접 제어**할 수 있는 클래스이다.

ResponseEntity에는 사용자의  HttpRequest에 대한 응답 데이터가 포함된다.

> **에러 코드와 같은 HTTP상태 코드를 전송하고 싶은 데이터와 함께 전송할 수 있기 때문에 좀 더 세밀한 제어가 필요한 경우 사용한다고 합니다.**

**Spring MVC에서 제공해주는 ResponseEntityExceptionHandler**

**ControllerAdvice를 사용하여 Exception 처리를 한 곳으로 모으는 경우**, **ResponseEntityExceptionHandler를 상속받도록 하여** Spring MVC에서 기본으로 제공되는 Exception들의 처리를 간단하게 등록할 수 있다. 각 Exception 처리를 위한 메서드들은 모두 protected로 선언되어 있으며, 하위 클래스에서 필요에 따라 Override할 수 있다.

```java
public class CustomizedResponseEntityExceptionHandler extends ResponseEntityExceptionHandler {
    @ExceptionHandler(Exception.class)  // 인자의 예외가 발생했을 때 아래의 메소드를 실행시킨다는 뜻
    public final ResponseEntity<Object> handlerAllException(Exception ex, WebRequest request){
        ExceptionResponse exceptionResponse=
                new ExceptionResponse(new Date(),ex.getMessage(),request.getDescription(false));

        return new ResponseEntity<>(exceptionResponse, HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

___
**@ExceptionHandler**

**@ExceptionHandler같은 경우는 @Controller, @RestController가 적용된 Bean내에서 발생하는 예외를 잡아서 하나의 메서드에서 처리해주는 기능을 한다.**

즉, 어떤 Excpetion이 발생하게 되면 해당 메소드(이 어노테이션 달려있는)가 실행되게 할것이다.
 
 ___
 GET:데이터베이스에 있는 자료를 조회하는 select 기능
 POST:데이터베이스의 자원을 새롭게 추가하는 INSERT 기능
 PUT:등록되어있는 리소스를 변경시켜주는 즉, 수정 업데이트 기능
 DELETE: 삭제한다.


@DeleteMapping이란 말 그대로 데이터베이스에 있는 정보를 삭제하는 것이다.

```java
@DeleteMapping("/users/{id}")
    public void deleteUser(@PathVariable int id){
        User user=service.deleteByID(id);

        if(user==null){
            throw new UserNotFoundException(String.format("ID[%s] not found",id));
        }
```