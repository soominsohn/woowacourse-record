# Annotated Controllers

- @Controller vs @RestController
    - MVC는 위와같은 어노테이션 으로 인해 편함~~
    - controller랑 ResponseBody 합친게 RestController이다.
- RequestMapping대신 사용할 수 있는 것들
    - `@GetMapping`
    - `@PostMapping`
    - `@PutMapping`
    - `@DeleteMapping`
    - `@PatchMapping`
    
    ```java
    @RestController
    @RequestMapping("/persons")
    class PersonController {
    
        @GetMapping("/{id}")
        public Person getPerson(@PathVariable Long id) {
            // ...
        }
    
        @PostMapping
        @ResponseStatus(HttpStatus.CREATED)
        public void add(@RequestBody Person person) {
            // ...
        }
    }
    ```
    
    이렇게 hierarchy로 작성가능
    
- `@GetMapping`은 이친구랑 똑같음 `@RequestMapping(method=HttpMethod.GET)`

- patternMatching
    - `"/resources/ima?e.png"` - match one character in a path segment
    - `"/resources/*.png"` - match zero or more characters in a path segment
    - `"/resources/**"` - match multiple path segments
    - `"/projects/{project}/versions"` - match a path segment and capture it as a variable
    - `"/projects/{project:[a-z]+}/versions"` - match and capture a variable with a regex
    
    ```java
    @GetMapping("/owners/{ownerId}/pets/{petId}")
    public Pet findPet(@PathVariable Long ownerId, @PathVariable Long petId) {
        // ...
    }
    ```
    
    ```java
    @Controller
    @RequestMapping("/owners/{ownerId}")
    public class OwnerController {
    
        @GetMapping("/pets/{petId}")
        public Pet findPet(@PathVariable Long ownerId, @PathVariable Long petId) {
            // ...
        }
    }
    ```
    
    The syntax `{varName:regex}`
     declares a URI variable with a regular expression that has syntax of `{varName:regex}`
    . For example, given URL `"/spring-web-3.0.5.jar"`
    , the following method extracts the name, version, and file extension:
    
    ```java
    @GetMapping("/{name:[a-z-]+}-{version:\\d\\.\\d\\.\\d}{ext:\\.[a-z]+}")
    public void handle(@PathVariable String name, @PathVariable String version, @PathVariable String ext) {
        // ...
    }
    ```
    

- For a `@RequestMapping` without HTTP method declarations, the `Allow` header is set to `GET,HEAD,POST,PUT,PATCH,DELETE,OPTIONS`. Controller methods should always declare the supported HTTP methods (for example, by using the HTTP method specific variants: `@GetMapping`, `@PostMapping`, and others).

- RequestMapping 안쓰고 굳이 나누는 이유?
    - `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`, and `@PatchMapping`
     are examples of composed annotations. They are provided because, arguably, most controller methods should be mapped to a specific HTTP method versus using `@RequestMapping`, which, by default, matches to all HTTP methods. If you need an example of composed annotations, look at how those are declared.