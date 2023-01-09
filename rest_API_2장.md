### 2장- REST API
___

:sunny: **User 도메인 클래스 생성**

**Controller와 Service는 무엇일까?**
출처(https://velog.io/@jybin96/Controller-Service-Repository-%EA%B0%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C)

### MVC 패턴
![DECSRIPTION](https://velog.velcdn.com/images%2Fjybin96%2Fpost%2Fe261b77f-8cdf-4e6c-9848-84b90d5bfabe%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-11-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.33.39.png)

**Model**
어플리케이션이 무엇을 할 것인지 정의하는 부분입니다. 즉 DB와 연동하여 사용자가 입력한 데이터나 사용자에게 출력할 데이터를 다룹니다.
**View**
사용자에게 시각적으로 보여주는 부분입니다. (UI)
**Controller**
Model이 데이터를 어떻게 처리할지 알려주는 역할을 합니다. 사용자에 의해 클라이언트가 보낸 데이터가 있으면 모델을 호출하기전에 적절히 가공을 하고 모델을 호출합니다. 그런다음 모델이 업무 수행을 완료하면 그결과를 가지고 View에게 전달하는 역할을 합니다.
___

**RestController**
RestController는 Controller에서 **@ResponseBody** 어노테이션이 붙은 효과를 지니게 됩니다.

즉 주용도는 **JSON/XML형태**로 객체 데이터 반환을 목적으로 합니다.

예시코드
```java
@RestController // JSON으로 데이터를 주고받음을 선언합니다.
public class ProductRestController {

    private final ProductService productService;
    private final ProductRepository productRepository;

    // 등록된 전체 상품 목록 조회
    @GetMapping("/api/products")
    public List<Product> getProducts() {
        return productRepository.findAll();
    }
}
```

**그렇다면 Serviec란??**

1. Client가 Request를 보낸다.(Ajax, Axios, fetch등..)
2. Request URL에 알맞은 Controller가 수신 받는다. (@Controller , @RestController)
3. Controller 는 넘어온 요청을 처리하기 위해 Service 를 호출한다.
4. Service는 알맞은 정보를 가공하여 Controller에게 데이터를 넘긴다.
5. Controller 는 Service 의 결과물을 Client 에게 전달해준다.


Service가 비즈니스 로직을 수행하고 **데이터베이스에** 접근하는 **DAO를** 이용해서 결과값을 받아 옵니다.

**DAO란?**

**DAO 는 쉽게 말해서 Mysql 서버에 접근하여 SQL문을 실행할 수 있는 객체입니다.**

___
**8:00**
**static 블록이란??**

출처(https://uoonleen.tistory.com/6)

**1. static block (스태틱 블록)**

- 클래스가 로딩되고 클래스 변수가 준비된 후 자동으로 실행되는 블록

- 한 클래스 안에 여러 개의 static 블록을 넣을 수 있다.

- 용도 :

  주로 클래스 변수를 초기화시키는 코드를 둔다.

