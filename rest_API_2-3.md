### REST API 2-3
___

**@PostMapping이란?**
1. GET
1) 특징
URL에 변수(데이터)를 포함시켜 요청

데이터를 Header(헤더)에 포함하여 전송

URL에 데이터가 노출되어 보안에 취약

캐싱할 수 있음

2. POST
1) 특징
URL에 변수(데이터)를 노출하지 않고 요청

데이터를 Body(바디)에 포함

URL에 데이터가 노출되지 않아 GET 방식보다 보안이 높음

캐싱할 수 없음
___

**1. @PostMapping**

POST 통신을 할때는 **@RequestMapping(method=RequestMethod.POST, ...)** 를 이용하거나 **@PostMapping**을 이용한다.

**2. @RequestBody**
만약, 함께 받아야하는 데이터가 있다면, **@RequestBody**를 활용한다.

예제를 위해 VO 객체를 생성한다.

**@RequestBody를 이용하여 HTTP Body에 담긴 데이터를 매핑하여 가지고 온다.**

주의할 점으로 **GET 통신에서는 @RequestParam**을 사용하지만, **POST 통신에서는 @RequestBody**를 사용한다.
 

그리고, 받은 데이터를 그대로 return하여 응답을 보낸다.

참고. 내부적으로 Jackson라이브러리에 의해 알아서 JSON 포맷으로 변환 됨

___

**!GET과 POST의 차이???**
출처(https://gomgomkim.tistory.com/48)

**흔히들 DB로부터 데이터 리스트를 불러올 때는 GET**

**생성, 수정, 삭제 등 데이터 변경 시 POST를 사용한다고 알고 있다. 왜 그럴까?**

| GET | POST |
| ------------ | ------------- |
| **GET 요청은 실패 시 될 때까지 반복한다.** | **POST 요청은 실패 시 반복하지 않는다.**  |
| 캐시 가능 | 캐시 불가능  |
| 파라미터 노출 | 파라미터 노출 x |

제일 중요한 사항은 맨 위 사항이다.

**GET 요청은 실패 시 요청이 성공할 때까지 반복하여 요청한다.**

인터넷 비연결 시 웹 페이지를 로드하면 웹 페이지가 뜨지 않다가

인터넷이 연결됐을 때 웹 페이지가 새로고침 되면서

화면에 나타나는 경우를 보았을 것이다. 이는 GET의 특성 때문이다.


하지만 POST가 같은 동작을 한다면 실패 시 요청이 계속 들어가고

**혹여나 데이터 변형이 중복으로 적용될 수 있다. 이는 오류 발생으로 직결된다.**

따라서 POST는 설계 상 실패 시 반복 요청하지 않게 설계돼있다.

이러한 차이가 있어서 리스트와 업데이트에 GET과 POST를 쓰는 것이다.  

___
이번에 새롭게 등장한 **@PostMapping**

```java
@PostMapping("/users")
    public void createUser(@RequestBody User user){
        User savedUser=service.save(user);
    }
```

@RequestBody 써주고

내가 생각하기에 데이터를 추가하고 싶을떄 post를 쓰는듯??

postman에서 json형식으로 데이터를 전송하면 데이터가 추가된다.