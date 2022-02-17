# 스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술
> 우아한형제들 기술이사 김영한님 강의를 듣고 내용 정리

## table of contents
- [2022.02.16](#20220216)
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