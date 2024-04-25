네임스페이스는 프로그래밍에서 이름 충돌을 피하기 위해 코드를 별도의 영역으로 구성하는데 사용된다.

클래스, 함수, 변수 등과 같은 코드 엔터티를 구성하는 데 사용되는 식별자 집합을 보관하는 컨테이너이다. 같은 네임스페이스 내부에서 각 식별자는 고유해야한다.

네임스페이스 유형은 다음과 같다.

- 전역(global) 네임스페이스: 모든 전역 변수와 함수를 포함한다.
- 지역(local) 네임스페이스: 함수 내에 존재하는 이름을 포함한다.
- 내장(built-in) 네임스페이스: 내장 함수 및 예외 이름을 포함한다.

파이썬에서 각 모듈은 각각의 네임스페이스를 가진다. 그리고 점연산자 "."로 중복되는 이름의 변수, 함수 등을 구별할 수 있다.

```python
import module1
import module2

result1 = module1.add(1, 2)
result2 = module2.add(1, 2)
```

만약 중복되는 이름의 변수, 함수 등을 직접적으로 불러올 경우에는 별칭(aliase)으로 구분해주어야 한다.

```python
from module1 import add as add1 
from module2 import add as add2

result1 = add1(1, 2)
result2 = add2(1, 2)
```

파이썬은 특정 전역 또는 지역 네임스페이스의 식별자 집합을 dict 타입의 자료형으로 저장한다.

```python
x = 10
y = 100

def fun1():
    x = 20
    y = 200
    print(locals()) # {'x': 20, 'y': 200}

print(globals())
# {'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, 'x': 10, 'y': 100}
```

globals(), locals() 함수를 호출하면 전역 네임스페이스의 식별자와 각 지역 네임스페이스의 식별자를 확인할 수 있다.

그리고 다른 네임 스페이스의 변수를 global, nonlocal 키워드를 사용하여 불러와 사용할 수 있다.

```python
x = 0

def increment_global():
    global x 
    x += 1
    print("Inside function:", x)

increment_global()
print("Outside function:", x)

# Inside function: 1
# Outside function: 1

def outer():
    y = 5

    def inner():
        nonlocal y  # Declare that we want to use the nonlocal y defined in outer()
        y += 3
        print("Inside inner:", y)

    inner()
    print("Inside outer:", y)

outer()

# Inside inner: 8
# Inside outer: 8
```