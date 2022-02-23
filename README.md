# 스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술
> 우아한형제들 기술이사 김영한님 강의를 듣고 내용 정리


<details>
<summary>Table of Contents</summary>

- [2022.02.16](#20220216)
- [2022.02.21](#20220221)
- [2022.02.22](#20220222)
- [2022.02.23](#20220223)
</details>

***

## 2022.02.16
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
- `$ cd 프로젝트 디렉터리`
- `$ ./gradlew build`
- `$ cd build`
- `$ java -jar hello-sping-0.0.1-SNAPSHOT.jar`

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
<html xmlns:th ="http://www.thymeleaf.org">
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

## 2022.02.22
### 회원 도메인과 리포지토리 만들기 및 회원 서비스 개발
**회원 객체 만들기**
1. `domain`, `repository` package 생성
2. 데이터 저장소가 선정되지 않았으므로 `MemberRepository`는 인터페이스로 구현
3. 개발 진행을 위해 초기 개발 단계에서 메모리 기반의 데이터 저장소 사용

**회원 서비스 개발**
1. 회원 가입, 회원 조회 구현

**코드**
- [회원 객체](src/main/java/hello/hellospring/domain/Member.java)
- [회원 리포지토리 인터페이스](src/main/java/hello/hellospring/repository/MemberRepository.java)
- [회원 리포지토리 메모리 구현체](src/main/java/hello/hellospring/repository/MemoryMemberRepository.java)
- [회원 서비스](src/main/java/hello/hellospring/service/MemberService.java)

`Optional<T>`
- `null`이 올 수 있는 값을 감싸는 Wrapper 클래스
- `isPresent()`
  - `null`이라면 `false`, 값이 있다면 `true`를 반환
- `ofNullable()`
  - `null`이 아닐 때 값을 가지는 `Optional` 객체 반환
  - `null`일 경우 비어있는 `Optional` 객체 반환
- `ifPresent()`
  - `null`이 아닐 경우 람다식을 실행

### 테스트 케이스 작성
- `JUnit` 프레임워크로 테스트를 실행
- 테스트는 각각 독립적으로 실행되어야 한다. 테스트 순서에 의존관계가 있는 것은 좋은 테스트가 아니다.

**코드**
- [회원 리포지토리 메모리 구현체 테스트](src/test/java/hello/hellospring/repository/MemoryMemberRepositoryTest.java)
- [회원 서비스 테스트](src/test/java/hello/hellospring/service/MemberServiceTest.java)

**테스트를 작성하면서 변경된 점**
- 기존에는 회원 서비스가 메모리 회원 리포지토리를 직접 생성하게 했다.
- 회원 서비스 코드를 의존성 주입(Dependency Injection, DI)이 가능하게 변경한다.
```java
// 기존 코드
public class MemberRepository {
  private final MemberRepository memberRepository = new memberRepository();
}
// 변경 후 코드
public class MemberRepository {
  private final MemberRepository memberRepository;
  
  public MemberService(MemberRepository memberRepository) {
      this.memberRepository = memberRepository;
  }
}
```

- `@Test`
  - 애노테이션 아래에 있는 메소드를 테스트 메소드 대상으로 선정
- `Assertions.asstertThat(String actual).isEqualTo(String excepted)`
  - 두 값이 같은지 여부를 체크
- `@AfterEach`
  - 각 테스트 메소드를 실행 후 `@AfterEach` 아래 메소드를 실행
- `@BeforeEach`
  - 각 테스트 메소드를 실행 전 `@BeforeEach` 아래 메소드를 실행

## 2022.02.23
### 스프링 빈과 의존관계

#### 컴포넌트 스캔과 자동 의존관계 설정
회원 컨트롤러가 회원 서비스와 회원 리포지토리를 사용할 수 있게 의존관계를 주입해야 한다.
```java
// 회원 컨트롤러에 의존관계 추가

@Controller
public class MemberController {
    
    private final MemberService memberService;
    
    @Autowired
    public MemberController(MemberSerivce memberSerivce) {
        this.memberService = memberSerivce;
    } 
}
```
```java
// 회원 서비스 스프링 빈 등록
@Service
public class MemberService {
    private final MemberRepository memberRepository;
    
    @Autowired
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
}
```
```java
// 회원 리포지토리 스프링 빈 등록
@Repository
public class MemoryMemberRepository {  }
```
- 생성자에 `@Autowired`가 있으면 스프링이 연관된 객체를 스프링 컨테이너에서 찾아서 넣어준다.
- 객체 의존관계를 외부에서 넣어주는 것을 DI(Dependency Injection), 의존성 주입이라 한다.
- 이전 테스트에서는 개발자가 직접 주입했고, 이번에는 스프링이 주입해준다.
- 하지만 `MemberService`와 `MemberRepository`가 스프링 빈으로 등록되지 않았기 때문에 실행하면 오류가 발생한다.

**의존성 주입(Dependency Injection, DI)**
- 필드 주입, setter 주입, 생성자 주입 3가지 방법이 존재
- 의존관계가 실행 중에 동적으로 변하는 경우는 거의 없으므로 생성자 주입을 권장한다.

**스프링 빈을 등록하는 2가지 방법**
- 컴포넌트 스캔과 자동 의존관계 설정
- 자바 코드로 직접 스프링 빈 등록하기

**컴포넌트 스캔 원리**
- `@Component` 애노테이션이 있으면 스프링 빈이 자동 등록된다.
- `@Component`를 포함하는 다음 애노테이션들은 스프링 빈으로 자동 등록된다.
  - `@Controller`
  - `@Service`
  - `@Repository`

**스프링 빈 등록 이미지**
![image](https://user-images.githubusercontent.com/45463495/155277133-0375fc9e-2182-471d-b556-079a698728e6.png)

> 💡 참고 : 스프링은 스프링 컨테이너에 스프링 빈을 등록할 때, 기본으로 싱글톤으로 등록한다. 따라서 같은 스프링 빈이면 모두 같은 인스턴스다.

#### 자바 코드로 직접 스프링 빈 등록하기
- 우선 회원 서비스, 회원 리포지토리에서 작성한 `@Service`, `@Repository`, `@Autowired` 애노테이션을 제거한다.

```java
package hello.hellospring;

import hello.hellospring.repository.MemberRepository;
import hello.hellospring.repository.MemoryMemberRepository;
import hello.hellospring.service.MemberService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringConfig {

  @Bean
  public MemberService memberService() {
    return new MemberService(memberRepository());
  }

  @Bean
  public MemberRepository memberRepository() {
    return new MemoryMemberRepository();
  }
}
```
