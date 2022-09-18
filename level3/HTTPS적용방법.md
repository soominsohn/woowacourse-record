# HTTPS

## 도입 배경

터놓고는 로그인 시 슬랙로그인API를 사용하고 있다.

슬랙 정책상 슬랙로그인 API를 사용하려면 HTTPS 적용이 필요하다. 

## 도입 방법

1. [api.ternoko.site](http://api.ternoko.site) 도메인을 구매한다.
2. nginx ec2 설정 및 nginx 설치
3. domain 이랑 nginx 서버 public ip 연결
4. certbot에서 인증서 발급 
5. nginx conf파일 변경하면 됨