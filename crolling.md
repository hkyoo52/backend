* 라우터 -> 라우터 -> 라우터 -> Naver 서버 
* 새로고침, 접속 -> 서버와 통신
* 여러개 사용하면 서버의 부하가 감
* Http : 데이터를 보는 것
  * 개발자 도구 -> Network -> 원하는거 클릭
  * Request URL : 주소
  * Request Method : 요청 방법
  * State Code , IP,,,
  * Request Header : 무엇을 요청했는지 봄
  * 어디서 왔는지, 보안 등등 정보 존재
  


* 데이터베이스 -> 서버 -> 사용자
* 서버는 유저한테 제공할 템플렛을 만듬
* 데이터베이스로부터 정보를 얻어서 사람에게 보여줌
* 크롤링을 할경우 서버에 많은 부하를 줌
* 잘못된 정보를 데이터베이스에 요청할경우 db가 rock 될 수 있음

## HTTP
* 통신 규격
* SP = spacebar
* CRLF = Enter
* host와 requset-url이 합쳐저서 생각

![image](https://user-images.githubusercontent.com/63588046/179491969-5e8ac655-4f4d-4ecc-9506-bb4c9ddf873d.png)

* GET : 데이터를 받기만 하겠다
* HEAD : 헤더만 가져오겠다.
* POST : 주고 받겠다.
* PUT : 수정하겠다.
* DELETE : 삭제하겠다.

## 브라우저
* Ex. 크롬
* 웹서버는 문자열을 가지고 만듬 (HTML)
* 브라우저는 UI 만듬 (HTML -> UI)

#### 브라우저 필요성
* 편의성
* 보안 (쿠키 등등)
* 스크립트 실행(동적 기능) Ex. 이미지 옆으로 이동시키는 등등


## 웹앱과 api
* 크롤링 할때 UI에 대해 몰라도 됨
* 서버가 어떻게 작동하는지 알면 됨 (= API)
 
#### 만들어지는 구저
* 서버가 전체적인 골격을 만듬
* 브라우저가 각각에 골격에 있는데로 서버에 요청 = API
* 즉 각 골격에 있는 곳을 API가 만듬

## 주의 사항
* 불법행위
* 규격 : robots.txt

![image](https://user-images.githubusercontent.com/63588046/179495138-ffe51fe7-9570-452a-8475-97705029d570.png)





