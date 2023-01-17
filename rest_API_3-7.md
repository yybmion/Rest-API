## REST API / URI를 이용한 REST API Version 관리
___

> @AllArgsConstructor / @NoArgsConstructor

```java
@NoArgsConstructor
@AllArgsConstructor
@RequireArgsConstructor
public class User {

    private Long id;

    @NonNull
    private String name;

    @NonNull
    private String pw;

    private int age;

}
User user1 = new User(); // @NoArgsConstructor
User user2 = new User("user1", "1234"); // @AllArgsConStructor
User user3 = new User(10000L, "user3", "1234", null); // @RequireArgsConstructor
```

Q. 상속할 때 부모클래스에 기본생성자가 꼭 있어야 하는 것인가??

어쨋든 기본 생성자를 자동으로 생성시켜주는 @NoArgsConstructor
___

버전을 관리해주어야한다.

우리는 V2에서 등급 제도도 추가할 것이고 보이는 정보도 다르게 설정해볼 것이다.

새롭게 UserV2 클래스를 만들고 기존 User는 상속받는다.

![9](https://user-images.githubusercontent.com/113106136/212900397-4a92983e-5eff-4e46-a13e-10e82326ce78.png)

```java
@Data
@AllArgsConstructor
@JsonFilter("UserInfoV2")
@NoArgsConstructor   
public class UserV2 extends User{
    private String grade;
}
```

그리고 컨트롤러에서 User의 정보를 UserV2에 복사할 것이다.

Q. 내 말은 어짜피 UserV2에서 User를 상속받았는데 그러면 필드도 상속받은거잖아 근데 왜 또 BeanUtils.copyProperties로 필드를 복제한다는 거야??

![10](https://user-images.githubusercontent.com/113106136/212903318-35dbdd76-b314-4233-bbbf-07376fb7a9ea.png)

결과로는 v1과 v2의 결과가 당연히 다르게 나올것이다.

**V1**
![11](https://user-images.githubusercontent.com/113106136/212903737-fec6ecf1-6491-42ce-9fe0-4a3a22d9b5f4.png)

**V2**
![13](https://user-images.githubusercontent.com/113106136/212903759-5649daf8-fcde-4a10-b487-5929e3233987.png)

