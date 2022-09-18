# ExceptionHandler와 ControllerAdvice

ControllerAdvice: 전역에서 예외처리하기

ExceptionHandler: 컨트롤러 클래스 안에서 예외처리하기

- 전역에서 처리를 한다고한다.
- 우선 **전역에서 예외 처리를 하는 이유**는 무엇일까?
    - `@ExceptionHandler`를 사용하여 해당 컨트롤러안에서 그 컨트롤러에 대한 예외 처리를 한다고 하자.
    - 하나의 컨트롤러서 발생하는 예외는 그 컨트롤러 안에 `handleLineException` 메서드를 만들어 처리할 수 있다. 하지만 컨트롤러가 늘어난다면 어떨까? 컨트롤러가 늘어나면서 예외 처리에 대한 중복 코드도 늘어날 것이고 많은 양의 코드를 다룰 것이다. 그러면 유지보수 또한 어려워질 것이다.
    
    ```java
    @RestController
    public class LineController {
    
        // ...
    
        @GetMapping("/lines")
        public ResponseEntity<List<LineResponse>> getLines() {
            // ...
        }
        
        @PostMapping("/lines")
        public ResponseEntity<LineResponse> createLine(@RequestBody LineRequest lineRequest) {
            // ...
        }
        
        @ExceptionHandler(LineException.class)
        public ResponseEntity<ErrorResponse> handleLineException(final LineException error) {
            // ...
        }
    ```
    

- 전역에서 예외처리 하는 법
    - 전역에서 예외 처리를 하기 위해 `@ControllerAdvice`또는 `@RestControllerAdvice`를 쓴다.
    - • `@RestControllerAdvice`는 `@ControllerAdvice` + `@ResponseBody`이다.
    - (`@Controller`와 `@RestController` 같은 느낌)
    - 이 어노테이션은 [AOP(Aspect Oriented Programming)](https://heeyeah.github.io/spring/2019/03/24/spring-controller-advice.html)
    의 방식이고, Application 전역에 발생하는 모든 컨트롤러의 예외를 한 곳에서 관리할 수 있게 해준다.
    
    - `@RestController` = `@Controller` + `@ResponseBody`
    - `@RestControllerAdvice` = `@ControllerAdvice` + `@ResponseBody`
    - 단순하게 생각하면 `@RestController`와 `@RestControllerAdvice`는 `@Controller`
    와 `@ControllerAdvice`에 `@ResponseBody`를 추가한 것뿐이다.
    - 따라서 서로 예외를 잡아 줄 수는 있다. 하지만 `@RestControllerAdvice`는 `@RestController`와 마찬가지로 `@ResponseBody`가 있어서 자바 객체를 Json/Xml 형태로 반환하여 HTTP Response Body에 담을 수 있다. **그러면 `@RestControllerAdvice`를 사용해서 `@RestController`에서 발생한 예외만 받을 수는 없을까?**당연히 있다. `@RestControllerAdvice`를 사용할때 `@RestControllerAdvice(annotations = RestController.class)` 이렇게 셋팅하면 `@RestController`에서 발생하는 예외만 가져올 수 있다. `@ControllerAdvice`도 마찬가지다.
    

ref.

[https://tecoble.techcourse.co.kr/post/2020-07-28-global-exception-handler/](https://tecoble.techcourse.co.kr/post/2020-07-28-global-exception-handler/)

[https://supawer0728.github.io/2019/04/04/spring-error-handling/](https://supawer0728.github.io/2019/04/04/spring-error-handling/)