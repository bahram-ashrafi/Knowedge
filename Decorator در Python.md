این صفحه از W3Schools درباره‌ی **Decorator در Python** است. ایده‌ی اصلی خیلی ساده است: **Decorator تابعی است که یک تابع دیگر را می‌گیرد و بدون تغییر دادن کد آن، رفتار جدیدی به آن اضافه می‌کند.** ([w3schools.com](https://www.w3schools.com/python/python_decorators.asp "Python Decorators"))

برای درک عمیق، اول بدون `@` نگاه کن:

```python
def decorator(func):
    def wrapper():
        print("قبل از تابع")
        func()
        print("بعد از تابع")

    return wrapper


def hello():
    print("Hello")


hello = decorator(hello)

hello()
```

اینجا اتفاق مهم این است که `hello` اولیه را به `decorator` می‌دهیم و **تابعی که `decorator` برمی‌گرداند را دوباره داخل `hello` قرار می‌دهیم**. بنابراین وقتی بعداً `hello()` را صدا می‌زنیم، در واقع `wrapper()` اجرا می‌شود. سینتکس `@decorator` فقط شکل کوتاه همین کار است: `@decorator` تقریباً معادل `hello = decorator(hello)` است. W3Schools همچنین `*args` و `**kwargs` را برای ساخت wrapperهایی که بتوانند هر نوع آرگومان تابع اصلی را دریافت کنند، و چند Decorator و Decoratorهای دارای آرگومان را توضیح می‌دهد. ([w3schools.com](https://www.w3schools.com/python/python_decorators.asp "Python Decorators"))

[صفحه Decorators در W3Schools](https://www.w3schools.com/python/python_decorators.asp?utm_source=chatgpt.com) 