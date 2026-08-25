وقتی روی یک تابع **چند Decorator** قرار می‌دهیم، Decoratorها از **پایین به بالا** روی تابع اعمال می‌شوند، ولی هنگام اجرا، نتیجه‌ی آن‌ها **از بالا به پایین** وارد می‌شود:

```python
def decorator1(func):
    def wrapper():
        print("Decorator 1 - قبل")
        func()
        print("Decorator 1 - بعد")
    return wrapper


def decorator2(func):
    def wrapper():
        print("Decorator 2 - قبل")
        func()
        print("Decorator 2 - بعد")
    return wrapper


@decorator1
@decorator2
def hello():
    print("Hello")


hello()
```

خروجی:

```text
Decorator 1 - قبل
Decorator 2 - قبل
Hello
Decorator 2 - بعد
Decorator 1 - بعد
```

دلیلش این است که Python این کد را عملاً این‌طور می‌فهمد:

```python
hello = decorator1(decorator2(hello))
```

یعنی **اول `decorator2` روی `hello` اعمال می‌شود** و نتیجه‌ی آن وارد `decorator1` می‌شود. پس می‌توانی Decoratorها را مثل چند لایه دور تابع تصور کنی: `decorator1 → decorator2 → hello → decorator2 → decorator1`.

[[Decorator در Python]]