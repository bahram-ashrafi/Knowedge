حتماً. بیاییم از یک مثال خیلی ساده شروع کنیم:

```python
def logger(func):
    def wrapper():
        print("تابع در حال اجراست...")
        func()
        print("تابع تمام شد.")

    return wrapper


def hello():
    print("Hello!")


hello = logger(hello)

hello()
```

خروجی:

```text
تابع در حال اجراست...
Hello!
تابع تمام شد.
```

اینجا `logger` تابع `hello` را می‌گیرد و یک تابع جدید به نام `wrapper` می‌سازد. سپس `hello` را با `wrapper` جایگزین می‌کنیم. حالا همان کار را با سینتکس **Decorator** می‌نویسیم:

```python
def logger(func):
    def wrapper():
        print("تابع در حال اجراست...")
        func()
        print("تابع تمام شد.")

    return wrapper


@logger
def hello():
    print("Hello!")


hello()
```

یعنی این:

```python
@logger
def hello():
    ...
```

عملاً معادل این است:

```python
def hello():
    ...

hello = logger(hello)
```

**نکته‌ی کلیدی:** Decorator یعنی «یک تابع را بگیر، یک قابلیت به آن اضافه کن، و نسخه‌ی جدید تابع را برگردان». این الگو بعداً در چیزهایی مثل **Logging، Authentication، Cache، اندازه‌گیری زمان اجرای تابع و Permission** خیلی کاربردی می‌شود.

[[Decorator در Python]]