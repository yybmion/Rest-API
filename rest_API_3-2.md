## Rest-API 3-2 다국어 처리를 위한 Internatinalization 구현 방법

헤더에 관한 정보

> 출처(https://kawaii-jordy.tistory.com/113)

**@RequestHeader 는 String 을 받는다.**

Locale 클래스
> Locale(String language)
Locale(String language, String country)
Locale(String language, String country, String variant)

```java
@Bean
	public LocaleResolver localeResolver(){
		SessionLocaleResolver localeResolver=new SessionLocaleResolver();
		localeResolver.setDefaultLocale(Locale.KOREA);
		return localeResolver;
	}
```

**LocaleResolver란?**
다국어 처리를 위해 Spring MVC는 LocaleResolver를 이용하여 웹 요청과 관련해서 Locale을 추출하고 이 Locale 객체를 이용해서 알맞은 언어의 메시지를 선택한다.
> 쉽게 말해 웹 브라우저의 언어 설정을 한국어, 영어 혹은 스페인어 등 설정한 언어에 따라 알맞은 메시지를 출력한다.

LocaleResolver 종류
         타입	                                                  설명
AcceptHeaderLocaleResolver	웹 브라우저가 전송한 Accept-Language 헤더로부너 Locale 선택한다. setLocale()메서드를 지원하지 않는다
CookieLocaleResolver	쿠키를 이용해서 Locale 정보를 구한다. setLocale() 메서드는 세션에 Locale 정보를 저장한다.
SessionLocaleResolver	세션으로부터 Locale 정보를 구한다. setLocale() 메서드는 세션에 Locale 정보를 저장한다.
FixedLocaleResolver	웹 요청에 상관없이 특정한 Locale로 설정한다. setLocale() 메서드를 지원하지 않는다.

___

다음 다국어 파일 이름을 yml 파일에다 지정해주어야한다.

```java
spring:
  messages:
    basename: messages
```

그리고 각 messages.properties를 생성한다

해당 국가에 맞추어 언어 설정한다.

![2](https://user-images.githubusercontent.com/113106136/212544940-cc041487-6f3c-4871-8c0a-93bc55db6e5f.png)

그리고 Controller에서 설정을 해주어야하는데

먼저 MessageSource에 대한 의존성 주입을 해준다.

이번에는 생성자를 통한 의존성 주입이 아닌 @Autowired를 통해 의존성 주입을 해주고
```java
@GetMapping(path="/hello-world-internationalized")
    public String helloWorldIdInternationalized(
            @RequestHeader(value = "Accept-Language",required = false) Locale locale){ //Accept-Language라는 헤더값이 포함되지 않았을 경우에는 default locale 값(한국어)을 설정된다
        return messageSource.getMessage("greeting.message",null,locale);
    }
 ```
 
 컨트롤러를 작성해준다. 먼저 @RequestHeader는 말그래도 Header 부분에서 어떠한 요청(부가 정보)이 들어갈때 
 특정 값을 클라이언트에게 반환하게 해주는 것이다.

만약 특정 값이 없을 때는 그냥 초기값 KOREA로 반환되게 설정되어있는 것이다.
greeting.message는 아까 properties에서 작성한 것이다. 

![3](https://user-images.githubusercontent.com/113106136/212545217-c5c3e51f-4c44-4a9d-af18-410ae38f3ed3.png)

위와 같이 key value 없을 때는 그냥 안녕하세요
![4](https://user-images.githubusercontent.com/113106136/212545290-fc162bd0-fb3b-4dd5-a74e-1a6911fc72a7.png)

전달하면 해당 국가에 맞는 언어 출력

