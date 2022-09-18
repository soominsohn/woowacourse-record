# JWT Token이란?

[http://www.opennaru.com/opennaru-blog/jwt-json-web-token/](http://www.opennaru.com/opennaru-blog/jwt-json-web-token/)

## **JWT 토큰 구성**

![https://i2.wp.com/www.opennaru.com/wp-content/uploads/2018/08/JWT_Stacks.png?zoom=2&fit=1200%2C300](https://i2.wp.com/www.opennaru.com/wp-content/uploads/2018/08/JWT_Stacks.png?zoom=2&fit=1200%2C300)

JWT는 세 파트로 나누어진다. 각 파트는 점로 구분하여 xxxxx.yyyyy.zzzzz 이런식으로 표현됩니다. 순서대로 헤더 (Header), 페이로드 (Payload), 서명 (Sinature)로 구성합니다. Base64 인코딩의 경우 “+”, “/”, “=”이 포함되지만 JWT는 URI에서 파라미터로 사용할 수 있도록 URL-Safe 한  Base64url 인코딩을 사용합니다.

**Header**: 토큰의 타입(JWT), 해시 암호화 알고리즘(HMAC, SHA256, RSA와 같은 해시 알고리즘) 으로 구성

**Payload**: “데이터" 토큰에 담을 클레임 정보. name-value으로 이루어짐.

(등록된 클레임, 공개 클레임, 비공개 클레임)

**Signature**: secret key를 포함하여 암호화 되어있음.

![https://i1.wp.com/www.opennaru.com/wp-content/uploads/2018/08/jwt_process_image_v2.png?zoom=2&fit=1920%2C1080](https://i1.wp.com/www.opennaru.com/wp-content/uploads/2018/08/jwt_process_image_v2.png?zoom=2&fit=1920%2C1080)

1. 사용자가 id와 password를 입력하여 로그인을 시도합니다.

2. 서버는 요청을 확인하고 secret key를 통해 Access token을 발급합니다.

3. JWT 토큰을 클라이언트에 전달 합니다.

4. 클라이언트에서 API 을 요청할때  클라이언트가 Authorization header에 Access token을 담아서 보냅니다.

5. 서버는 JWT Signature를 체크하고 Payload로부터 사용자 정보를 확인해 데이터를 반환합니다.

6. 클라이언트의 로그인 정보를 서버 메모리에 저장하지 않기 때문에 토큰기반 인증 메커니즘을 제공합니다.

인증이 필요한 경로에 접근할 때 서버 측은 Authorization 헤더에 유효한 JWT 또는 존재하는지 확인한다.

JWT에는 필요한 모든 정보를 토큰에 포함하기 때문에 데이터베이스과 같은 서버와의 커뮤니케이션 오버 헤드를 최소화 할 수 있습니다.

Cross-Origin Resource Sharing (CORS)는 쿠키를 사용하지 않기 때문에 JWT를 채용 한 인증 메커니즘은 두 도메인에서 API를 제공하더라도 문제가 발생하지 않습니다.

일반적으로 JWT 토큰 기반의 인증 시스템은 위와 같은 프로세스로 이루어집니다.

### **JWT 장점**

- 사용자 인증에 필요한 모든 정보가 토큰에 포함되어 있기 때문에 별도의 인증 저장소가 필요 없다.
- 데이터베이스에 의존하지 않는 인증 및 인가 방법을 제공한다.

**JWT 단점**

- 토큰은 클라이언트에 저장되어 데이터베이스에서 사용자 정보를 조작하더라도 토큰에 직접 적용할 수 없다.
- 비상태 애플리케이션에서 토큰은 거의 모든 요청에 대해 전송되므로 데이터 트래픽 크기에 영향을 미칠 수 있다.