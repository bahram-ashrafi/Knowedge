**Dunder** مخفف **Double UNDERscore** است؛ یعنی متدهایی که نامشان با دو `_` شروع و با دو `_` تمام می‌شود:

```python
__init__
__str__
__len__
__iter__
__next__
```

این‌ها معمولاً **متدهای ویژه‌ی Python** هستند و رفتار اشیاء را در مقابل syntax یا توابع داخلی زبان مشخص می‌کنند. مثلاً وقتی می‌نویسی:

```python
len(x)
```

پایتون در پشت صحنه از `x.__len__()` استفاده می‌کند. یا وقتی می‌نویسی:

```python
for x in obj:
    ...
```

پایتون از **Iterator Protocol** یعنی `__iter__()` و `__next__()` استفاده می‌کند. بنابراین Dunder Methodها را می‌توان نوعی **رابط بین کلاس تو و مکانیزم داخلی Python** دانست.

مثلاً:

```python
class Person:
    def __init__(self, name):
        self.name = name

    def __str__(self):
        return self.name
```

حالا:

```python
p = Person("Bahram")

print(p)
```

عملاً باعث می‌شود Python رفتار تعریف‌شده در `p.__str__()` را استفاده کند. **نکته‌ی مهم:** Dunder Method با متد معمولی فرق مفهومی دارد؛ متد معمولی را خودت برای API کلاس طراحی می‌کنی (`login()`, `calculate()`)، ولی Dunder Methodها برای **پیاده‌سازی Protocolهای Python** هستند.
[[چرا به اینصورت نوشته میشه؟__next__]]
[[Python]]