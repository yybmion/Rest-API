### REST API 2-4
___
좋은 REST-API 설계 방식에서는 서버로부터 전달받은 상태코드가 서로 다르게 설정해준다.

```java
 @PostMapping("/users")
    public ResponseEntity<User> createUser(@RequestBody User user){
        User savedUser=service.save(user);

        URI location=ServletUriComponentsBuilder.fromCurrentRequest()
                .path("/{id}")
                .buildAndExpand(savedUser.getId())
                .toUri();

        return ResponseEntity.created(location).build();
    }
```

**상태코드 관리**
**ServletUriComponentsBuilder**
이번에는 지금까지 배운 내용 + HTTP **메소드GET, POST, DELETE** 사용하여 DB에서 **Read, Create, Delete** 

아래와 같이 createUser 메소드에 URI를 검사하는 코드를 추가해준다.

**ServletUriComponentsBuilder**클래스를 사용하여 현재 요청의 URI를 가져올 수 있다. 다시 말해, 서버에서 반환시켜주려는 값을 Response 엔터티에 담아 리턴할 수 있다. 즉 상세정보 보기가 가능하다.

**fromCurrentRequest()**를 사용하여 현재 요청된 Request 값을 사용하고,

**path로** 반환시킬 위치,

**buildAndExpand에** 새롭게 설정한 id값을 path에 지정시켜준다.

마지막으로 **toUri()**로 URI로 반환시켜준다.

리턴하기 위해 void를 ResponseEntity<User>로 우리가 도메인으로 지정했던 User를 반환하게 만든다.