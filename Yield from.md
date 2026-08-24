`yield from` برای این است که یک Generator بتواند **مقادیر Generator یا Iterable دیگری را مستقیماً به بیرون منتقل کند**؛ یعنی لازم نباشد خودمان یک `for` بنویسیم.

مثلاً:

```python
def numbers():
    yield 1
    yield 2
    yield 3

def all_numbers():
    yield from numbers()

print(list(all_numbers()))
# [1, 2, 3]
```

این تقریباً معادل این است:

```python
def all_numbers():
    for x in numbers():
        yield x
```

اما `yield from` فقط یک کوتاه‌نویسی ساده نیست؛ **Generatorهای تو‌در‌تو را به هم متصل می‌کند** و حتی `send()`، `throw()` و مقدار `return` Generator داخلی را نیز مدیریت می‌کند. مثلاً:

```python
def child():
    yield 1
    yield 2
    return "finished"

def parent():
    result = yield from child()
    print(result)

print(list(parent()))
```

در اینجا `yield from` باعث می‌شود `parent` مستقیماً مقادیر `child` را تولید کند و مقدار `return` آن (`"finished"`) را نیز دریافت کند. `yield from` در **Python 3.3** و طبق **PEP 380** معرفی شد و یکی از پایه‌های مهم تکامل Generatorها به سمت Coroutineهای قدیمی Python بود.

[[Generators در پایتون]]

[[generator throw در]]