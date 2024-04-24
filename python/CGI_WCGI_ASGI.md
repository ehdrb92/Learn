## CGI

CGI(Common Gateway Interface)는 웹 페이지에서 동적 콘텐츠를 생성하는 방법 중 하나이다.

웹 서버가 외부 프로그램을 호출하고 환경 변수를 통해 HTTP 요청 데이터를 전달할 수 있게 해준다. 외부 프로그램은 요청을 처리하고 응답(주로 HTML)을 생성한 후 표준 출력을 통해 서버로 다시 전송한다.

Nginx, Apache 서버가 이에 해당한다.

## WCGI

WSGI(Web Server Gateway Interface)는 파이썬으로 작성된 웹 서버 소프트웨어와 웹 애플리케이션 간의 표준 인터페이스를 위한 사양이다. 서버와 애플리케이션간에 데이터를 이동하는 일관되고 효율적인 방법을 제공한다.

WSGI는 보통 application 혹은 app이라는 이름의 파이썬 함수를 웹 서버에 드러낸다.

```python
def application(environ, start_response):
    start_response("200 OK", [("Content-Type", "text/plain")])
    return [b'Hello, World']
```

- environ: 웹 서버가 제공한 환경 변수와 현재 요청에 대한 정보가 있는 dict
- start_response: 클라이언트에 보내는 응답에 대한 작업을 하는 함수

한번에 하나의 요청과 응답만 처리하며 응답이 즉시 반환된다고 전제하기에 웹 소켓 또는 롱폴링 HTTP 연결과 같은 장시간 지속 연결을 처리할 수 없다.

Gunicorn, uWSGI가 이에 해당한다.

WSGI는 단독으로 사용될 수는 있으나 다음과 같은 이유로 Nginx와 함께 사용되는 경우가 많다.

- 파이썬 코드를 실행시키는데 최적화되어 있고, 정적 파일을 제공하는데는 그렇지 않다. 이에 비해 Nginx는 정적 파일을 제공하는데 최적화되어 있다.
- 느리고 악의적인 클라이언트 연결을 관리하는 데 효율성이 떨어질 수 있다. Nginx는 적은 메모리 사용량으로 수 천개의 연결을 관리할 수 있다.
- 높은 수준의 트래픽을 분산하는 데 약하다. Nginx는 리버스 프록시 및 로드 밸런서 역할을 할 수 있다.
- SSL/TLS를 설정하는 데 최적화되어 있지 않다. Nginx를 사용하면 SSL/TLS를 효율적으로 처리한다. 인증서 관리의 중앙 집중화가 가능하다.
- 요청/응답 버퍼링 혹은 캐싱을 지원하지 않는다. Nginx는 버퍼링과 캐싱 통해 급증하는 트래픽을 조절하여 서버의 부하를 줄일 수 있다.


## ASGI

ASGI(Asynchronous Server Gateway Interface)는 동기와 비동기 프로토콜을 모두 처리하도록 설계된 WSGI의 비동기 진화 버전이다. 각 애플리케이션이 여러 개의 비동기 이벤트를 처리할 수 있게 허용한다.

WSGI와 마찬가지로 application 함수 객페를 정의한다. 다른 점은 async 함수이며 매개변수가 3개라는 점이다.

```python
async def application(scope, receive, send):
    await send({
        "type": "http.response.start",
        "status": 200,
        "headers": [
            [b'content-type', b'text/plain'],
        ],
    })

    await send({
        "type": "http.response.body",
        "body": b'Hello, world!',
    })
```

- scope : 현재 요청에 대한 정보가 포함된 dict로, WSGI의 environ과 비슷하지만 세부적인 명명 규칙이 약간 다르다.
- send : 애플리케이션이 클라이언트로 메시지를 돌려보낼 수 있게 해주는 async callable(함수)이다.
- receive : 애플리케이션이 클라이언트로부터 메시지를 수신할 수 있게 해주는 async callable이다.