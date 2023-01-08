출처(https://cheershennah.tistory.com/179)

스프링에서 비동기 처리를 하는 경우 **@RequestBody** , **@ResponseBody**를 사용한다. 비동기 처리를 위해 이 어노테이션들은 어떻게 작동할까?

### 클라인언트와 서버의 비동기 통신

![description](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsxcOu%2Fbtq4eKsIpSZ%2FkntlVrm6YznC7PKyBHemH1%2Fimg.png)

- 클라이언트에서 서버로 통신하는 메시지를 **요청**(request) 메시지라고 하며, 서버에서 클라이언트로 통신하는 메시지를 **응답**(response) 메시지라고 한다.

- 웹에서 화면전환(새로고침) 없이 이루어지는 동작들은 대부분 비동기 통신으로 이루어진다.

- 비동기통신을 하기위해서는 클라이언트에서 서버로 요청 메세지를 보낼 때, 본문에 데이터를 담아서 보내야 하고, 서버에서 클라이언트로 응답을 보낼때에도 본문에 데이터를 담아서 보내야 한다. 

- 이 본문이 바로 body 이다.

- 즉, 요청본문 **requestBody**, 응답본문 **responseBody** 을 담아서 보내야 한다. 

 

- 이때 본문에 담기는 데이터 형식은 여러가지 형태가 있겠지만 가장 대표적으로 사용되는 것이 **JSON** 이다.

- 즉, 비동기식 클라-서버 통신을 위해 **JSON** 형식의 데이터를 주고받는 것이다. 
___

**@RequestBody** 
이 어노테이션이 붙은 파라미터에는 http요청의 본문(body)이 그대로 전달된다.

일반적인 GET/POST의 요청 파라미터라면 @RequestBody를 사용할 일이 없을 것이다.

반면에 xml이나 json기반의 메시지를 사용하는 요청의 경우에 이 방법이 매우 유용하다.

HTTP 요청의 바디내용을 통째로 자바객체로 변환해서 매핑된 메소드 파라미터로 전달해준다. 

**@ResponseBody** 
자바객체를 HTTP요청의 바디내용으로 매핑하여 클라이언트로 전송한다.

**@ResponseBody** 가 붙은 파라미터가 있으면 HTTP요청의 미디어타입과 파라미터의 타입을 먼저 확인한다.

```java
@ResponseBody
@RequestMapping(value = "/ajaxTest.do")
public UserVO ajaxTest() throws Exception {

  UserVO userVO = new UserVO();
  userVO.setId("테스트");

  return userVO;
}
```

**즉, @Responsebody 어노테이션을 사용하면 http요청 body를 자바 객체로 전달받을 수 있다.**

___

#### lombok에 대해서
-  lombok을 사용하면 자바에서 자주 사용한 toString, getter setter, equals 등을 자동으로 생성해줌
- **@AllArgsConstructor** //여기에 필드에 쓴 모든생성자만 만들어줌
**@NoArgsConstructor** //기본(defualt) 생성자를 만들어줌
**@Data** // getter, setter 만들어줌

___
#### restController 와 Controller의 차이

[Controller로 View 반환하기]
전통적인 Spring MVC의 컨트롤러인 **@Controller**는 주로 **View**를 반환하기 위해 사용합니다. 아래와 같은 과정을 통해 Spring MVC Container는 **Client의 요청으로부터 View를 반환합니다.**

![description](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbED6o9%2Fbtrx1wyKwpF%2FNtSlrTohpAI79l6MA95SZ1%2Fimg.png)

1. Client는 URI 형식으로 웹 서비스에 요청을 보낸다.
2. DispatcherServlet이 요청을 위임할 HandlerMapping을 찾는다.
3. HandlerMapping을 통해 요청을 Controller로 위임한다.
4. Controller는 요청을 처리한 후에 ViewName을 반환한다.
5. DispatcherServlet은 ViewResolver를 통해 ViewName에 해당하는 View를 찾아 사용자에게 반환한다.

[Controller로 Data 반환하기]

하지만 Spring MVC의 컨트롤러를 사용하면서 Data를 반환해야 하는 경우도 있습니다. 컨트롤러에서는 데이터를 반환하기 위해 **@ResponseBody** 어노테이션을 활용해주어야 합니다. 이를 통해 Controller도 **Json** 형태로 데이터를 반환할 수 있습니다.

![description](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbED6o9%2Fbtrx1wyKwpF%2FNtSlrTohpAI79l6MA95SZ1%2Fimg.png)

1. Client는 URI 형식으로 웹 서비스에 요청을 보낸다.
2. DispatcherServlet이 요청을 위임할 HandlerMapping을 찾는다.
3. HandlerMapping을 통해 요청을 Controller로 위임한다.
4. Controller는 요청을 처리한 후에 객체를 반환한다.
5. 반환되는 객체는 Json으로 Serialize되어 사용자에게 반환된다.

**엄청난 이해도를 높혀주는 짤**
![description](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fdz8rUC%2Fbtrac9N6JdL%2Fks92BmSKrKGfREXjB9NKg0%2Fimg.png)
___

#### Path Variable

url 값에 가변 변수를 사용할 수 있다.

```java
public class HelloWorldController {
  @GetMapping(path="/hello-world-bean/path-variable/{name}") //가변 변수 적용
    public HelloWorldBean helloWorldBean(@PathVariable String name) {    //만약 여기 name과 위에 name 이름이 다르다면 @PathVariable(value="name")이라고 지정해줘야함
        return new HelloWorldBean(String.format("Hello World, %s",name));
    }
}
```
