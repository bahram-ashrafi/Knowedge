این بخش درباره‌ی یک مشکل مهم در Decoratorهاست: وقتی تابع را داخل `wrapper` می‌پیچیم، **اطلاعات اصلی تابع ممکن است از بین برود**؛ مثلاً `__name__` و `__doc__`.

```python
def logger(func):
    def wrapper():
        """Wrapper documentation"""
        print("Before")
        func()
        print("After")

    return wrapper


@logger
def hello():
    """Say hello to the user."""
    print("Hello")


print(hello.__name__)
print(hello.__doc__)
```

نتیجه چیزی شبیه این است:

```text
wrapper
Wrapper documentation
```

در حالی که انتظار داریم `hello` و `Say hello to the user.` را ببینیم. دلیلش این است که بعد از Decorate شدن، نام `hello` به تابع `wrapper` اشاره می‌کند.

راه استاندارد حل این مشکل استفاده از `functools.wraps` است:

```python
from functools import wraps

def logger(func):
    @wraps(func)
    def wrapper():
        print("Before")
        func()
        print("After")

    return wrapper


@logger
def hello():
    """Say hello to the user."""
    print("Hello")


print(hello.__name__)
print(hello.__doc__)
```

حالا خروجی:

```text
hello
Say hello to the user.
```

پس `@wraps(func)` به Python می‌گوید: **این `wrapper` در واقع قرار است جای `func` را بگیرد، بنابراین metadata تابع اصلی را حفظ کن.** این موضوع مخصوصاً وقتی چند Decorator، ابزارهای Debugging، Documentation یا Frameworkهایی مثل Flask و FastAPI داریم اهمیت زیادی پیدا می‌کند.

[[Decorator در Python]]