اگر منظورت **قابلیت‌ها و متدهای مهم خود Generator** است، بعد از `send()`، `throw()` و `yield from`، مهم‌ترین مورد **`close()`** است:

```python
def gen():
    try:
        yield 1
        yield 2
    finally:
        print("Generator closed")

g = gen()

print(next(g))  # 1
g.close()       # Generator را متوقف می‌کند
```

`close()` در Generator یک `GeneratorExit` ایجاد می‌کند و باعث می‌شود Generator به‌صورت کنترل‌شده بسته شود؛ به همین دلیل `finally` فرصت اجرای cleanup را دارد. در نتیجه اگر بخواهیم قابلیت‌های اصلی Generator را دسته‌بندی کنیم، تقریباً با این مجموعه روبه‌رو هستیم: **`yield`** برای تولید مقدار، **`next()`** برای درخواست مقدار بعدی، **`send()`** برای فرستادن مقدار به Generator، **`throw()`** برای وارد کردن Exception، **`close()`** برای بستن Generator، و **`yield from`** برای واگذار کردن تولید مقدار به Iterable/Generator دیگر.

یک نکته ظریف هم این است که `yield from` و `yield` **متد نیستند**؛ syntax زبان Python هستند. `next()` هم در واقع یک built-in function است، هرچند پشت صحنه به متد `__next__()` Iterator متکی است. بنابراین از دید **Python Data Model**، قدم بعدی خیلی جالب این است که برویم سراغ `__iter__()` و `__next__()` و ببینیم Generator دقیقاً چگونه این پروتکل Iterator را پیاده‌سازی می‌کند.

[[Generators در پایتون]]