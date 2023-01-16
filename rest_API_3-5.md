## REST API 프로그래밍으로 제어하는 Filtering 방법
___
> 출처(https://prinha.tistory.com/entry/Spring-Boot-RESTful-Service-%EA%B0%95%EC%9D%98-%EC%A0%95%EB%A6%AC-4-Response-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%A0%9C%EC%96%B4%EB%A5%BC-%EC%9C%84%ED%95%9C-Filtering-%EC%82%AC%EC%9A%A9%EC%9E%90-%EC%A1%B0%ED%9A%8C-%EC%98%88%EC%A0%9C)
저번에는 그저 어노테이션으로 특정 필드를 제어하는 방법을 배웠다.

이번에는 User 라는 도메인이 존재하고 해당 클래스에 필터를 처리하고 싶을 때에

**@JsonFilter("필터명")** 을 선언 해준다.

![7](https://user-images.githubusercontent.com/113106136/212774580-a4cdd73c-5d3d-4066-8208-c457e1abce9a.png)


```java
@Data
@AllArgsConstructor
// @JsonIgnoreProperties(value={"password"}) //여러가지 추가 가능
@JsonFilter("UserInfo")
public class User {
    private Integer id;
    @Size(min=2,message="Name은 2글자 이상 입력해 주세요")
    private String name;
    @Past
    private Date joinDate;

    private String password;
    private String ssn;
}
```

![8](https://user-images.githubusercontent.com/113106136/212774791-9e9734b3-ef41-4345-86d6-4070e79bda5f.png)

```java
@GetMapping("/users/{id}")
    public MappingJacksonValue retrieveUser(@PathVariable int id) {  //MappingJacksonValue은 밑에 Filter 값을 반환받기 위한 클래스 타입
        User user = service.findOne(id);

        if (user == null) {
            throw new UserNotFoundException(String.format("ID[%s] not found", id));
        }

        SimpleBeanPropertyFilter filter = SimpleBeanPropertyFilter
                .filterOutAllExcept("id","name","joinDate","ssn");

        FilterProvider filters = new SimpleFilterProvider().addFilter("UerInfo",filter);

        MappingJacksonValue mapping=new MappingJacksonValue(user);
        mapping.setFilters(filters);

        return mapping;
    }
```

> 즉 지금 admin 클래스를 따로 생성해주어 특정 클래스에서만 특정 정보를 볼 수 있도록 필터를 해줄 것이다.

SimpleBeanPropertyFilter filter = SimpleBeanPropertyFilter.filterOutAllExcept()로 보여줄 필드만 정의를 한다.

serializeAllExcept() 를 통해서 제외시킬 필드를 정의할 수도 있다.

SimpleFilterProvider.addFilter("정의해준 JsonFilter 필터명", 위의 필터명)  으로 필터 등록을 하고

MappingJacksonValue에 json 형태로 리턴해줄 object value를 적어주고 필터를 추가해준 다음에 return 해주면 된다.

