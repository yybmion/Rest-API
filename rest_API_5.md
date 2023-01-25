## JPA를 이용한 개별 사용자 목록 조회 GET
___

```java
@RestController
@RequestMapping("/jpa")
public class UserJpaController {

    @Autowired
    private UserRepository userRepository;

    // 전체 사용자 목록을 조회하는 findAll 메소드 사용
    @GetMapping("/users")
    public List<User> retrieveAllUsers(){
        return userRepository.findAll();
    }

    // 개별 사용자 목록 조회 (가변 데이터 id)
    @GetMapping("/users/{id}")
    public Resource<User> retrieveUser(@PathVariable int id){
        Optional<User> user = userRepository.findById(id);

        // Optional<T> findById(ID var1)
        // 리턴값이 Optional인 이유 : 데이터가 존재할수도 안할수도 있기때문에

        if(!user.isPresent()){
            throw  new UserNotFoundException(String.format("ID[%s] not found",id));
        }

        // HATEOAS 기능
        Resource<User> resource = new Resource<>(user.get());
        ControllerLinkBuilder linkTo = linkTo(methodOn(this.getClass()).retrieveAllUsers());
        resource.add(linkTo.withRel("all-users"));

        // return user.get();
        return resource;
    }
}
```

![1](https://user-images.githubusercontent.com/113106136/214712660-c8cb0a96-cb61-4f5c-9b11-dbb953c7556a.png)

![2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbkQusL%2FbtqHui5Rb7U%2FeNe0nkP7ExrlSMvILMji61%2Fimg.png)

___

## JPA를 이용한 사용자 추가와 삭제

```java
@DeleteMapping("/users/{id}")
public void deleteUser(@PathVariable int id){
    User user = service.deleteById(id);

    if(user==null){
        throw new UserNotFoundException(String.format("ID[%s] not found", id));
    }
}
```
![3](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FOfHbW%2FbtqHuiEOrNa%2F9XjZkPvuZcRRNwBMSueE40%2Fimg.png)

이번엔 POST로 데이터 추가

```java
// 새로운 사용자를 등록하는 method 구현
@PostMapping("/users")
public ResponseEntity<User> createUser(@Valid @RequestBody User user){
    User savedUser = service.save(user);

    // 사용자에게 요청 값을 변환해주기
    // fromCurrentRequest() :현재 요청되어진 request값을 사용한다는 뜻
    // path : 반환 시켜줄 값
    // savedUser.getId() : {id} 가변변수에 새롭게 만들어진 id값 저장
    // toUri() : URI형태로 변환
    URI location = ServletUriComponentsBuilder.fromCurrentRequest()
            .path("/{id}")
            .buildAndExpand(savedUser.getId())
            .toUri();

    return ResponseEntity.created(location).build();
}
```
![4](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbs8Egn%2FbtqHgqrkNSZ%2FMjKnN3kJ2zi62l639dTX61%2Fimg.png)

근데 에러 발생!

그 이유는 Primary key 값(여기서는 ID)가 자동으로 생성되는(1부터 1씩 증가하는) sequence로 설정이 되어있어 ,

본래 우리가 넣어준 데이터값과 ID가 중복됨

```sql
insert into user values(99991,sysdate(),'User1','test1111','750411-111111');
insert into user values(99992,sysdate(),'User2','test2222','850411-111111');
insert into user values(99993,sysdate(),'User3','test3333','950411-111111');
```
![5](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbFiPW9%2FbtqHwgzRuJJ%2F43vPOkmxAaqMbcaxJsInJk%2Fimg.jpg)

성공!
___

## 게시물 관리를 위한 Post Entity 추가와 초기 데이터 생성

1. Post Entity class 생성
```java
@Entity
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Post {

    @Id
    @GeneratedValue
    private Integer id;

    // 게시물 내용
    private String description;

    @JsonIgnore
    // 외부에 데이터 노출하지 않기위해
    @ManyToOne(fetch = FetchType.LAZY)
    // User : Posts -> 1 : (0~N), Main : Sub -> Parent : Child
    // LAZY : 지연 로딩 방식
    // 매번 데이터를 로딩하는것이 아니라 Post 데이터가 필요한 시점에 user(사용자)데이터를 가져오겠다는 뜻
    private User user;
}
```

![6](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FN1ciC%2FbtqHoX9mODo%2FdewIoIPuqFyq9ku8QaCDYk%2Fimg.png)


2. User domain class에서 List<Post> posts 필드 추가
  
  
```java
// 사용자가 포스팅할 수 있는 정보를 저장하는 필드
@OneToMany(mappedBy = "user")
private List<Post> posts;
```

  
  3. data.sql 초기 데이터값 설정
  
```sql
insert into user values(99991,sysdate(),'User1','test1111','750411-111111');
insert into user values(99992,sysdate(),'User2','test2222','850411-111111');
insert into user values(99993,sysdate(),'User3','test3333','950411-111111');

insert into post values(10001, 'My first post',99991);
insert into post values(10002, 'My second post',99991);

```
 
![7](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmzAcO%2FbtqHoXBwm7E%2FLs1PjL4tmaHXBFFKmo8kd1%2Fimg.png)
  
  
  
  
  
