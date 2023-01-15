## Rest-API 3-3 XML 데이터로 변환
___

POSTMAN에서 우리는 JSON 형태로 데이터를 반환받았다.

하지만 XML로 받고 싶다면 어떻게 해야할까??

먼저
DEPENDENCY를 추가한다.
```java
<dependency>
			<groupId>com.fasterxml.jackson.dataformat</groupId>
			<artifactId>jackson-dataformat-xml</artifactId>
			<version>2.10.2</version>
		</dependency>
 ```
 
 그리고 value에 Accept 와 KEY에 application/xml 을 설정하면 xml 파일로 변환되어서 나온다.
 
 ![5](https://user-images.githubusercontent.com/113106136/212545996-19d0caa5-9a70-476f-8e91-749600e0adfb.png)
