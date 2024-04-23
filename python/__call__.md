파이썬의 `__call__()` 메서드는 클래스 내부에서 정의되며 클래스의 인스턴스를 마치 함수처럼 호출할 수 있게 해준다.

```python
class Multiplier:
    def __init__(self, factor):
        self.factor = factor

    def __call__(self, x):
        return x * self.factor

doubler = Multiplier(2)
result = doubler(5)  # This will output 10
```

위 예시에서 doubler라는 Multiplier 클래스의 인스턴스를 만든 후 인스턴스를 함수처럼 사용하면 내부의 __call__() 메서드가 동작하게 된다.