## Rest API 3-4 Response 제어를 위한 Filtering
___

get으로 데이터를 추가하고 싶다.

근데 password나 ssd 같은 중요한 정보를 client가 쉽게 보게 되면 보안에 심각한 위험을 끼칠 수 있다.

그때 우리는 존재는 하지만 안보이게 설정할 수있다.

```java
    @JsonIgnore
    private String password;
    @JsonIgnore
    private String ssn;
```

이렇게 **@JsonIgnore** 해준다면 표시된 부분은 무시되어 안보인다.

또 다른 방법은
```java
@Data
@AllArgsConstructor
@JsonIgnoreProperties(value={"password"}) //여러가지 추가 가능
public class User {
    private Integer id;
    @Size(min=2,message="Name은 2글자 이상 입력해 주세요")
    private String name;
    @Past
    private Date joinDate;

//  @JsonIgnore
    private String password;
    private String ssn;
}
```

다음과 같이 클래스 블록 전체에 적용하기 위해 클래스 위에 @JsonIgnoreProperties(여기에 무시할 부분 추가) 추가한다.

만약 위와 같이 작성하면 결과는

![6](https://user-images.githubusercontent.com/113106136/212551030-d5b7bd62-a8f1-406e-81f1-6858427bf80c.png)

위와 같다.
