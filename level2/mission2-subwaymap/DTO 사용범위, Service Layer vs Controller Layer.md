# DTO 사용범위, Service Layer vs Controller Layer

DTO를 Service Layer에서 반환 vs Controller Layer에서 반환

어떤걸 사용해야 할까??

우선 Service Layer에서 Dto를 넘겨 도메인으로 변환한다면 다음과 같은 장점이 있습니다.

- Controller Layer 대신 Service Layer 에서 도메인 변환시 장점
    - View에 반환할 필요가 없는 데이터 까지 Domain 객체에 포함되어 Controller(표현계층) 까지 넘어온다.
    - Controller가 여러 Domain 객체들의 정보를 조합해서 DTO를 생성해야 하는 경우, 결국 Service(응용 계층) 로직이 Controller에 포함되게 된다.
    - 여러 Domain 객체들을 조회해야 할 경우 하나의 Controller 가 의존하는 Service 의 개수가 많아진다.
    - 복잡한 어플리케이션의 경우 Controller가 View에서 전달받은 DTO만으로 Entity를 구성하기 어렵다.
        - 예) dto + repository조회를 통한 정보를 이용해 Domain 객체를 구성해야 하기 때문.
    - 마틴 파울러는 Service 레이어란 어플리케이션의 경계를 정의하고 비즈니스 로직 등 도메인을 캡슐화하는 역할이라고 정의한다. 즉 도메인을 보호한다. dto를 Service Layer에서 반환하게 되면 도메인을 보호할 수 있다.

- Cotroller Layer 에서 dto를 반환시 장점
    - Service Layer에서 dto를 반환하면 결국 Service Layer가 View를 알고 있는 것이다.
    - 만일 여러 컨트롤러에서 한 서비스를 사용하게 된다면? dto를 서비스에서 반환하게 된다면, 한 컨트롤러만 한 서비스를 쓸 수 있게된다.
    - 또한 서비스가 서비스를 의존하는 경우 반환값이 dto가 되게 되면 또 다시 객체를 만들어줘야하는 이슈가 생긴다.