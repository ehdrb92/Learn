# pyjwt 사용 예제

```bash
pip install pyjwt
```

```python
import jwt
import datetime
from functools import wraps

SECRET_KEY = ""

def encode_jwt(payload):
    """
    JWT 생성 함수

    payload에 유저 관련 정보 저장
    """
    return jwt.encode(
        {'exp': datetime.datetime.now() + datetime.timedelta(hours=1), **payload},
        SECRET_KEY,
        algorithm="HS256"
    )

def decode_jwt(token):
    """
    JWT 확인 함수
    """
    try:
        return jwt.decode(token, SECRET_KEY, algorithms=["HS256"])
    except jwt.ExpiredSignatureError:
        return "Token has expired"
    except jwt.InvalidTokenError:
        return "Invalid token"
```