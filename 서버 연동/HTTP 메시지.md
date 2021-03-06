## HTTP 메시지

### 개요

- 서버는 메세지를 분석하여 원하는 바를 파악하고, 그에 맞는 처리를 한 다음에 응답 내용을 HTTP메시지로 만들어 전송함.
- iOS 앱이 서버와 연동하기 위해서는 서버에 요청할 HTTP메시지를 직접 만들어서 전송해 주어야 함.
  - HTTP 메시지는 텍스트 기반으로 이루어져 있음.

### HTTP 메시지의 구조

- 크게 요청과 응답 메시지로 나눌 수 있음.
- 각각의 메시지는 모두 라인 - 헤더 - 바디의 세 부분으로 구성되어 있음.
- Line(라인)은 HTTP 메시지의 맨 첫 줄에 해당하는 영역임.
  - 메시지의 가장 기본적인 내용인 응답/요청 여부, 메시지 전송 방식, 상태 정보 등이 작성되는 곳임.
  - 반드시 한 줄로 작성되어야 함.
- Header(헤더)는  메시지 본문에 대한 메타 정보다 담겨져 있음.
  - 길이는 유동적임.
  - 바디와의 구분을 위해 한 줄의 공백이 들어감.
    - HTTP메시지에서 나타나는 첫 번째 한 줄 공백이 헤더와 바디를 구분짓는 경계선이라고 할 수 있음.
- Body(바디)에는 실제로 보내고자 하는 메시지 본문 내용이 들어감.
  - 길이는 유동적임.

### Request (요청 메시지)

- 다음은 클라이언트에서 서버로 요청할 때 사용되는 HTTP메시지

```http
POST/userAccount/login HTTP/1.1
Host : swiftapi.rubypaper.co.kr:2029
Content-Type: application/x-www-form-urlencoded

account=swift%40swift.com&passwd=1234&grant_type=password
```

- 이 값은 이 메시지가 어떤 전송 방식으로 구성되어 있는지를 나타낸다.
- ***첫 줄 라인***
  - 요청 내용에 대한 경로, 요청 형식에 대한 버전 정보가 나타나 있음.
    - **어떤 버전의 HTTP 문법을 준수하고 있는가 하는 것**
- ***헤더***
  - 샘플에서는 Host 헤더와 Content-Type 헤더가 정의되어 있음.
    - Host 헤더에는 도메인 & 포트 번호가 포함되어 있음
      
      - 두 개 이상의 도메인에 연결되어 있는 서버일 경우 어느 도메인으로 요청이 들어왔는지 구분하기 위함
      
    - Content-Type은 본문이 어떤 형식으로 작성되어 있는지를 나타냄.
      - 샘플의 application/x-www-form-urlencoded는 일반 POST 방식으로 데이터를 전송한다는 것을 의미함.
        - ex) 회원가입, 로그인, 게시판 글쓰기 등 많은 UI들이 대부분 POST를 사용해 서버에 전송함.
      - API 연동을 위해 사용할 메시지 본문 형식 : JSON
        - 이 때는 Content-Type 헤더의 값을 application/json으로 설정해 주어야 함.
          - 그렇지 않을 시 서버에서 본문 해석 불가능할 수 있음.
      
    - 메시지 본문 형식은 헤더의 Content-Type에 설정된 타입과 일치해야 함.
    
      - 샘플 예제에서 전송하고자 하는 값은 account, passwd, grant_type 세 개이므로 이를 x-www-form-urlencoded 타입으로 전송하기 위해 &로 연결해 메시지 본문에 넣어야 함.
    
      - ```http
        account=swift%40swift.com&passwd=1234&grant_type=password
        ```
    
      - 특수문자의 경우 `URLEncoding`형식으로 변환함.
      
      - 지금까지의 예제 코드는 POST 방식일 경우
      
      - GET 방식으로 바꾸면 코드가 바뀜.
      
        ```http
        GET /userAccount/login?account=swift@swift.com&amp;passwd=1234&amp;grant_type=password HTTP/1.1
        
        Host: swiftapi.rubypaper.co.kr:2029
        Cache-Control: no-cache
        ```
      
        - 메시지 본문이 사라지고 분문에 있어야 할 값이 첫 번째 라인의 경로 뒤에 '?'문자열과 함께 연결됨.
        - GET 방식에서는 파라미터를 모두 URL 뒤에 연결해서 전달하기 때문임.
          - 이렇게 연결된 파라미터를 **쿼리 스트링(Query String)**이라고 함.

