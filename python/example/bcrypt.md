# bcrpyt 사용 예제

```bash
pip install bcrypt
```

```python
import bcrypt

password = "password"

hashed_password = bcrypt.hashpw(password.encode("utf-8"), bcrypt.gensalt())

print(hashed_password) # key
```

```python
def check_password(provided_password, stored_hash):
    return bcrypt.checkpw(provided_password, stored_hash)

if check_password("my_secret_password".encode("utf-8"), hashed_password):
    print("Password matches")
else:
    print("Password does not match")
```