# FE 배포자동화 도입기

1. 프론트엔트용 젠킨스 설정
2. 젠킨스에 배포 스크립트 작성
3. 프론트엔드 WS 설정
4. 배포!!

[https://songjang.tistory.com/m/112](https://songjang.tistory.com/m/112)

### 프론트엔드용 젠킨스를 설정한다.

1. Nodejs 플러그인을 설치한다.
    
    Nodejs: 자바스크립트 빌드환경
    
    Dashboard → Jenkins관리 → 플러그인 매니저 → Node.js Plugin 설치
    
    ![Untitled](FE%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%E1%84%8C%E1%85%A1%E1%84%83%E1%85%A9%E1%86%BC%E1%84%92%E1%85%AA%20%E1%84%83%E1%85%A9%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%80%E1%85%B5%20fe0a6114664f4d95a66b9e894bfd5575/Untitled.png)
    
2. Nodejs 버전 추가해준다. 
    
    Jenkinsr관리 → Global Tool Configuration → NodeJS → Add NodeJS
    
    ![Untitled](FE%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%E1%84%8C%E1%85%A1%E1%84%83%E1%85%A9%E1%86%BC%E1%84%92%E1%85%AA%20%E1%84%83%E1%85%A9%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%80%E1%85%B5%20fe0a6114664f4d95a66b9e894bfd5575/Untitled%201.png)
    
    사용하는 NodeJS 버전에 맞게 설정해준다.
    
3. 빌드 환경 설정
    
    item → 설정 → 빌드 환경 → Provide Node & npm bin/ folder to PATH 선택
    
    ![Untitled](FE%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%E1%84%8C%E1%85%A1%E1%84%83%E1%85%A9%E1%86%BC%E1%84%92%E1%85%AA%20%E1%84%83%E1%85%A9%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%80%E1%85%B5%20fe0a6114664f4d95a66b9e894bfd5575/Untitled%202.png)
    
    다음과 같이 설정된 것을 확인할 수 있다.
    

### 젠킨스에 빌드 스크립트 작성

```bash
cd frontend
mkdir env
echo "환경변수 내용" > env/.env.production
npm i
npm run build:prod
sudo cp -rf public/assets dist
tar -cvf ~/dist.tar dist
cd ~
scp -o StrictHostKeyChecking=no -i {사용-pem-key}.pem dist.tar ubuntu@{WS-private-ip}:~
ssh -o StrictHostKeyChecking=no -i {사용-pem-key}.pem ubuntu@{WS-private-ip} ./deploy.sh
```

### 프론트엔드 WS 설정

1. [deploy.sh](http://deploy.sh) 쉘 스크립트 작성
    
    ```bash
    sudo tar -xvf dist.tar
    sudo rm -rf /var/www/dist
    sudo mv dist /var/www/dist
    sudo service nginx restart
    ```
    
    정적 파일들 압축풀기
    
    정적 파일들 위치 이동
    
2. nginx conf 파일 location 확인
    
    ```bash
    location / {
        root   /var/www/dist;
        index  index.html index.htm;
        try_files $uri $uri/ /index.html;
    }
    ```
    
    `/` 위치로 요청 들어올 시 /var/www/dist 폴더의 index.html 반환