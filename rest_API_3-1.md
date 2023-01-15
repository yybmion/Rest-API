## REST API 3-1 유효성 체크를 위한 Validation API 사용
___

말그대로 유효성 체크이다.

만약 post 로 USER를 추가 및 수정을 할 때 그 추가된 객체가 유효한지 판단한다.
```java
@Size(min=2)
    private String name;

@Past
    private Date joinDate;
```
name의 길이는 최소 2이상으로 설정한다.

```java
@PostMapping("/users")
    public ResponseEntity<User> createUser(@Valid @RequestBody User user){
        User savedUser=service.save(user);

        URI location=ServletUriComponentsBuilder.fromCurrentRequest()
                .path("/{id}")
                .buildAndExpand(savedUser.getId())
                .toUri();

        return ResponseEntity.created(location).build();
    }
```

위와 같이 **@Valid 어노테이션**을 추가해준다.

___

다음은 ResponseEntityExceptionHandler 에서 잘못된 argument를 전달했을 때 발생되는 예외를 작성해보자

ResponseEntityExceptionHandler에 들어가 해당 메소드를 끌어온다.

```java
    @Override
    @Nullable
    protected ResponseEntity<Object> handleMethodArgumentNotValid(MethodArgumentNotValidException ex,
                                                                  HttpHeaders headers,
                                                                  HttpStatusCode status,
                                                                  WebRequest request) {
        ExceptionResponse exceptionResponse=
                new ExceptionResponse(new Date(),"Validation Failed",ex.getBindingResult().toString());
        return new ResponseEntity<>(exceptionResponse,HttpStatus.BAD_REQUEST);
    }
```

**Q. 여기서 하나의 의문점은 여기는 왜 @ExcepionHandler를 사용하지 않는것일까?**

그래서 run을 하고 @valid 검사를 통해 예외가 발생하면 해당 결과값이 보여진다.



