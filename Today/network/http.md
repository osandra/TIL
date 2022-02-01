### HTTP (하이퍼텍스트 전송 프로토콜)

HTTP는 클라이언트/서버에 기반을 둔 요청/응답 프로토콜이다. 클라이언트가 서버로 HTTP 요청 메시지를 보내면 서버는 그에 대한 요청을 처리한 뒤 HTTP 응답 메시지를 만들어서 클라이언트에게 보낸다.

클라이언트는 DNS를 통해 찾은 서버와 TCP 연결을 맺은 후 HTTP 메시지를 보낸다. 클라이언트가 https://developer.mozilla.org/en-US/docs/Web/Guide 라는 주소를 입력하면 요청 메시지은 아래와 같이 시작한다.<br>
`GET /en-US/docs/Web/Guide HTTP/1.1`<br>
`HOST: developer.mozilla.org `
<br>

이 부분에서 URL는 /en-US/docs/Web/Guide 다. 이를 통해 서버는 어디에 액세스 해야 하는 지 알 수 있다. 맨 앞에 있는 GET은 메소드로, 클라이언트가 서버에게 원하는 행동의 유형을 말한다.

[→ URI, URL, URN 정리](../web/url.md)

---
### HTTP 메시지 포맷

클라이언트에서 서버에게 보내는 `request 메시지`에는 무엇을 어떻게 해서 보낼 지에 대한 정보가 적혀있다. 웹서버는 이를 해독한 뒤 요구에 따라 진행하고 결과 데이터를 응답 메시지에 저장한다.

**리퀘스트(request) 메시지는 리퀘스트 라인+ 헤더 + 메시지 본문(선택)으로 구성되어 있다.**

**응답(response) 메세지는 스테이스터스 라인(status code 내용 포함) + 헤더 + 메시지 본문(선택)으로 구성되어 있다.**

서버는 응답 메시지에서 `상태 코드(status code)`를 통해 클라이언트의 요청에 대한 처리 결과 정보를 3자리 숫자를 통해 제공한다. 상태 코드는 요청 중에 무엇이 일어났는 지를 설명한다. 각 코드의 첫 번째 자릿수는 상태의 일반적인 분류(ex 성공, 오류)등을 나타낸다. <br>

리퀘스트 라인, 스테이스터스 모두 라인 마지막에 http 버전 번호가 적혀있다. 서버는 클라이언트가 보낸 요청의 http 버전보다 높지 않은 버전 번호를 돌려준다.

- `리퀘스트와 응답메시지의 헤더에 모두 들어갈 수 있는 헤더 내용` 
    - Date: 메시지를 생성한 날짜와 시간 
    - Cache-Control: 캐시 제어 관련 정보
    - Connection: 응답 후 tcp 접속 계속 허용할 지
    - Transfer-Encoding: 인코딩 방식 관련 정보
    - Via: 경유한 프록시 및 게이트웨이 기록 관련 정보 등이 있다.
<br><br>

- `Request 헤더에서 부가적으로 사용하는 정보`
    - Authorization: 클라이언트가 인증 정보를 서버에게 제출하기 위해 사용
    - HOST: 서버의 도메인 이름과 포트 번호(선택)를 제공
    - User-Agent: 클라이언트 소프트웨어에 대한 정보를 제공. 프로그램 이름, 버전 정보등을 포함
    - IF-Modified-Since: 헤더에 지정된 시간 이후에 실체가 변경되었을 경우에만 변경된 값을 반환하도록 요구
    - Referer: 현재 요청된 링크 이전의 웹 페이지 URL을 제공
    - Accept: 클라이언트가 응답으로 어떤 인터넷 메체 유형을 받아들일지 서버에게 알리는 역할 수행
    - Accept-Encoding: 서버에게 서버가 보내도 되는 인코딩을 알리는 역할 수행
    - Cookie: 클라이언트가 서버에게 쿠키를 전달할 때 사용
<br><br>

- `Response 헤더에서 부가적으로 사용하는 정보`
    - Sever: 서버 소프트웨어 관련 정보
    - Acces-Control—Allow-Origin: 프론트 주소를 적어주어야 CORS 에러 발생가 발생하지 않음
    - Retry-After: 리소스를 사용할 수 없을 때, 언제 가능해지는지에 대한 날짜 혹은 시각을 전달
    - Set-Cookie: 서버가 클라이언트에게 쿠키를 전달할 때 사용

메시지 본문(엔티티)에서도 헤더 필드를 사용하는데, 대표적인 헤더에는 Content-Type, Allow, Content-Encoding, Content-Length 본문의 언어 등이 있다. <br>
Content-Type은 메시지 본문의 데이터 형식 정보를 나타내기 위해 사용된다.

👉 **HTTP request message 예시**

| 이름 | 예 | 포함하는 값 |
|---|:---|:---|
| `request line` | GET /hello.htm HTTP/1.1| method, URL, 클라이언트가 사용하는 http 버전
| `header line` | User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)<br>Host: www.tutorialspoint.com<br>Accept-Language: en-us:<br>Accept-Encoding: gzip, deflate:<br>Connection: Keep-Alive | 헤더 필드 이름: 해당하는 값
| `entity body` | | POST 일 경우 폼(입력) 값이 포함됨 |
<br>

👉 **HTTP response message 예시**

| 이름 | 예 | 포함하는 값 |
|---|:---|:---|
| `status line` | HTTP/1.1 200 OK | http 버전, status code
| `header line` | Date: Mon, 27 Jul 2009 12:28:53 GMT<br>Server: Apache/2.2.14 (Win32)<br>Last-Modified: Wed, 22 Jul 2009 19:15:56 GMT<br>Content-Length: 88<br>Connection: Keep-Alive<br>Content-Type: text/html | 헤더 필드 이름: 해당하는 값
| `entity body` | `<html>`<br>~~~<br>`<html>` | request 메시지가 요청한 데이터 |
<br>

👉 **HTTP 주요 메서드**

| 이름 | 의미 | 메시지 본문 여부 |
|---|:---|:---|
| `GET` | 특정한 리소스를 가져오도록 요청. 데이터를 읽을 때 사용 | 없음 |
| `POST` | 클라이언트 측에서 서버로 데이터 송신. 주로 폼을 이용해 데이터 수신 (설문조사, 입력 필드 등)|  있음 |
| `HEAD` | GET과 유사. 다른 점은 서버가 응답으로 헤더만 돌려준다는 것이다. 엔티티 본문은 반환되지 않는다. <br>웹 서버 버전 정보 및 파일 최종 갱신 일시와 같은 속성 정보 조사 시 사용 | 없음 |
| `OPTIONS` | 1.1 버전에서만 사용 가능. 특정 리소스에 대해 어떤 메서드가 지원되는지를 조사할 때 사용 | 없음 |
| `PUT` | 새로운 리소스를 생성하거나, 이미 존재한다면 대상 리소스를 나타내는 데이터를 완전히 교체할 때 사용 | 있음 |
| `PATCH` | 리소스를 부분적으로 교체할 때 사용 | 있음 |
| `DELETE` | 요청한 URL로 지정한 리소스 삭제 요청 | 없음 |
| `TRACE` | 1.1 버전만 사용 가능, 서버 측에서 받은 리퀘스트 라인과 헤더를 클라이언트에게 반송함으로써, 클라이언트에게 자신의 요청이 서버에 도달했을 때 어떻게 보이는지 알려줌 | 없음 |
| `CONNECT` | 1.1 버전만 사용 가능, 암호화한 메시지를 프록시로 전송할 때 이용하는 메소드. 요청한 리소스에 대해 양방향 연결을 시작함. <br>터널 열기 위해 사용되기도 함. HTTPS 사용하는 웹사이트 접속 시 사용 가능 | 없음 |


👉 **응답(response)에서 전송하는 status code 개요**

- `100번대` → 요청을 받고 작업 진행 중
    -  100 Continue: 클라이언트가 서버로 보낸 요청에 문제가 없으니 다음 요청을 이어서 보내도 된다는 것을 의미함. 이미 요청을 완료했다면 응답 무시해도 된다.
<br><br>

- `200번대`→ 정상 종료 (성공)
    - 200 OK: 요청 메시지에 대한 성공적인 응답
    - 201 Created: 요청 성공했고 자원을 서버 내에 만들었음
    - 202 Accepted: 서버가 요청을 받아들였지만 아직 처리하진 않음
<br><br>

- `300번대`→ 요청을 마치기 위해 추가 조치 필요
    - 301 Moved Permanently: 요청한 리소스의 URI가 `영구적으로` 이동되었음을 알린다. 이에 대한 응답으로 Location 헤어에 바뀐 리소스를 담아서 보낸다. 이 경우, 검색엔진이 해당 리소스로 다시 요청을 보내게 된다. POST에 대한 요청을 301로 처리할 경우, 이어지는 요청 방법을 GET으로 변경할 수 있으므로, 301은 GET 또는 HEAD 메서드에만 사용하는 것이 좋다.
    - 302 Found: 요청한 리소스의 URI가 `임시적으로` 변경된 것을 말한다.
    - 304 NOT Modified: 클라이언트가 조건부 GET 요청 보냈지만 자원이 정해진 날짜/시간 이후로 변하지 않았기 때문에 요청된 리소스를 재전송할 필요가 없음을 나타낸다. 이 경우는 요청한 정보가 캐시에 있을 경우, 이를 확인하기 위한 요청에서 If-Modifed-Since 헤더를 사용해 확인한 경우에 발생한다.
<br><br>

- `400번대` → 클라이언트 측 오류
    - 400 Bad Request: 클라이언트가 잘못된 요청을 보낸 경우
    - 401 Unauthorized: 클라이언트가 인증되지 않은 경우
    - 403 Forbidden: 자원에 대한 접근 권한이 불춘분한 경우
    - 404 NOT FOUND: 서버가 자원을 찾을 수 없는 경우
    - 405 Method Not Allowed: 지원하지 않는 메서드 방식으로 요청받았을 때 사용한다. 예로 GET 방식만 허용하는 리소스에 POST를 시도하는 경우 발생한다.
<br><br>

- `500번대` → 서버 측 오류
    - 503 Service Unavailable: 현재는 서버가 요청을 처리할 순 없지만 나중에는 가능하다는 것을 알린다. 즉 추후 사용자가 같은 URL로 요청을 보낼 경우 이를 처리할 수 있다. 서버에서 해당 리소스를 사용할 수 있게 되는 시간을 안다면 응답에 Retry-After 헤더를 포함시킬 수 있다.
<br><br>

 👉 **HTTP 캐싱**

- `웹 클라이언트 상의 캐시 (사설 캐시)`<br>
    클라이언트가 접근한 문서 이미지 파일 등을 브라우저가 사용자 컴퓨터 안에 저장하는 것을 말한다.

- `매개체 상의 캐시 (프록시 서버)`<br>
    유저는 같은 웹사이트를 자주 방문할 가능성이 높다. 매번 요청을 실제 서버에까지 전송하면 시간도 많이 걸리고 서버에 트래픽도 많아지게 된다. 이 경우에 따로 클라이언트와 서버 사이에 존재하는 `프록시 서버`를 사용하여 이를 완화시킬 수 있다.

    프록시 서버는 클라이언트가 요청한 내용들을 캐시를 이용하여 저장한다. 따라서 다음 번 요청에선 클라이언트가 실제 서버에게 요청을 보내지 않아도, 프록시 서버에서 응답을 보내주게 된다. 만약 프록시 서버에 해당 정보가 없으면 프록시 서버에서 실제 서버에 요청을 보낸 뒤, 받은 응답을 클라이언트에게 보내게 된다.

    캐싱은 보안 성능 향상의 장점이 있지만 단점 또한 존재한다. 서버에서 변경한 정보가 반영되지 않아 업데이트 되지 않은 정보가 보여질 수 있다. 이는 보여주기 전에 서버에게 이를 확인하거나(If-Modified-Since) 리소스의 최대 유효 시간 등을 설정하는 방식으로 대응할 수 있다.

<details>
<summary>출처</summary>

- 도서 [성공과 실패를 결정하는 1%의 네트워크 원리] ~ 54p

- 도서 [TCP/IP 완벽 가이드]

- 도서 [HTTP 완벽 가이드]

- [HTTP METHOD_PUT](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/PUT)

- [HTTP METHOD_CONNECT](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/CONNECT)

- [HTTP-Status](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)

- [HTTP 응답 헤더](https://www.zerocho.com/category/HTTP/post/5b4c4e3efc5052001b4f519b)

- [HTTP 헤더 종류](https://gmlwjd9405.github.io/2019/01/28/http-header-types.html)

- [http request message](https://www.tutorialspoint.com/http/http_requests.htm)

- [http response message](https://www.tutorialspoint.com/http/http_responses.htm)

- [자세한 status code 종류](https://www.w3.org/Protocols/rfc2616/rfc2616-sec6.html)

- [캐싱](https://developer.mozilla.org/ko/docs/Web/HTTP/Caching)

</details>