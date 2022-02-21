# 스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술
> 우아한형제들 기술이사 김영한님 강의를 듣고 내용 정리

## table of contents
- [2022.02.16](#20220216)
- [2022.02.21](#20220221)
***

## `2022.02.16`
### 강의 내용
- 프로젝트 생성
- View 환경설정
- 빌드하고 실행하기

`hello_spring/src/main/java/hello/controller/helloController.java`
```java
package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HelloController {
    @GetMapping("hello")
    public String hello(Model model) {
        model.addAttribute("data", "hello!!");
        return "hello";
    }
}
```
`hello_spring/src/main/resources/templates/hello.html`
```html
<!DOCTYPE html>
<html xmlns:th ="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Hello</title>
</head>
<body>
<p th:text="'안녕하세요 ' + ${data}">안녕하세요. 손님</p>
</body>
</html>
```
동작 환경 그림
<img width="807" alt="스크린샷 2022-02-16 오후 10 14 14" src="https://user-images.githubusercontent.com/45463495/154274530-c4046da5-e946-4597-8313-5ba88358b612.png">

`@GetMapping("route")`
- `/route`로 들어오면 해당하는 메서드 실행

`model.addAttribute(String name, Object value)`
- `view`에 데이터 전달하는 메서드

`return "viewName"`
- `viewResolver`가 `resource:templates/viewName.html`을 찾아서 화면에 띄어줌

`<html xmlns:th ="http://www.thymeleaf.org">`
- `thymeleaf` 템플릿을 사용하기 위해 추가

**빌드하고 실행하기**
- `cd 프로젝트 디렉터리`
- `./gradlew build`
- `cd build`
- `java -jar hello-sping-0.0.1-SNAPSHOT.jar`

## `2022.02.21`
### 강의 내용
* 정적 컨텐츠
* MVC와 템플릿 엔진
* API

**정적 컨텐츠**
- 스프링 부트 정적 컨텐츠 기능
- `http://localhost:8080/htmlName.html` 들어가면 실행이 됨
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>static content</title>
</head>
<body>
정적 컨텐츠 입니다.
</body>
</html>
```

**MVC와 템플릿 엔진**
- MVC❓
  - Model, View, Controller

`Controller`
```java
@Controller
public class HelloController {
    ...
    
    @GetMapping("hello-mvc")
    public String helloMvc(@RequestParam("name") String name, Model model) {
        model.addAttribute("name", name);
        return "hello-template";
    }
}
```
`View`
```html
<!DOCTYPE html>
<html lang="en", xmlns:th="http://www.thymelear.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
  <p th:text="'hello ' + ${name}">hello! empty</p>
</body>
</html>
```

`@RequestParam`
- 단일 파라미터를 전달 받을 때 사용하는 애노테이션
- 예시) `http://localhost:8080/hello-mvc?name=spring!`

**API**

`@ResponseBody`

![image](https://user-images.githubusercontent.com/45463495/154891857-70533419-b44f-4836-bb9f-132df4846d1d.png)

- HTTP의 BODY에 직접 반환하는 애노테이션
- `viewResolver`대신 `HttpMessageConverter`가 동작
- 기본 문자 처리 : `StringHttpMessageConverter`
- 기본 객체 처리 : `MappingJackson2HttpMessageConverter`
- byte 처리 등등 기타 여러 HttpMessageConverter가 기본으로 등록되어 있음

예시)
```java
@Controller
public class HelloController {
    // 문자 반환
    @GetMapping("hello-string") 
    @ResponseBody 
    public String helloString(@RequestParam("name") String name) {
        return "hello " + name;
    }
    // 객체 반환
    @GetMapping("hello-api")
    @ResponseBody 
    public Hello helloApi(@RequestParam("name") String name) {
        Hello hello = new Hello();
        hello.setName(name);
        return hello;
    }
    
    static class Hello { 
        private String name;
        
        public String getName() {
            return name;
        }
        
        public void setName(String name) {
            this.name = name;
        }
    }
}
```
