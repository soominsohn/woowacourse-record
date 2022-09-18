# GET POST PUT DELETE

GET: 정보를 호출할 때 (HEADER에 데이터를 담아 보낸다)

POST: 정보를 등록할 때 (HTTP body에 데이터를 담아 보낸)다ㄷ

PUT or PATCH: 업데이트 작업을 수행

DELETE: 삭제 작업을 수행

**GET vs POST**

GET:

- GET requests can be cached
- GET requests remain in the browser history
- GET requests can be bookmarked
- GET requests should never be used when dealing with sensitive data
- GET requests have length restrictions
- GET requests are only used to request data (not modify)

POST:

- POST requests are never cached
- POST requests do not remain in the browser history
- POST requests cannot be bookmarked
- POST requests have no restrictions on data length

**PUT VS PATCH**

**PUT**

자원의 전체 교체, 자원교체 시 모든 필드가 필요하다. 

만일 자원 전체가 아닌 일부만 전달할 경우 전달한 필드외 모든 필드가 null 혹은 초기값 처리가 되니 주의할 필요가 있다.

**PATCH**

자원의 부분 교체, 자원교체시 일부 필드 필요.

|  | PUT(null값 들어가는 예시) | PUT(모든 필드 교체 예시)  | PATCH |
| --- | --- | --- | --- |
| 원본 | {    
"name" : "앤지",
"age": 25
} | {    
"name" : "앤지",
"age": 25
} | {    
"name" : "앤지",
"age": 25
} |
| 요청 Body | {    
"age": 100
} | {    
"name" : "앤지",
"age": 100
} | {    
"name" : "앤지",
"age": 100
} |
| 결과 | {    
"name" : null,
"age": 25
} | {    
"name" : "앤지",
"age": 100
} | {    
"name" : "앤지",
"age": 100
} |