چون `__next__` یک **متد ویژه (special method / dunder method)** در پایتون است. دو آندرلاین قبل و بعد اسم، بخشی از قرارداد نام‌گذاری پایتون برای متدهایی است که **خود زبان Python در شرایط خاص آن‌ها را صدا می‌زند**؛ مثل `__init__`، `__str__`، `__len__` و `__iter__`.

در Iterator، `__next__` معنای مشخصی دارد: **«عضو بعدی را بده»**. وقتی تو می‌نویسی:

```python
next(it)
```

پایتون در اصل از پروتکل Iterator استفاده می‌کند و متد `it.__next__()` را فراخوانی می‌کند. مثلاً:

```python
class Counter:
    def __init__(self):
        self.n = 0

    def __iter__(self):
        return self

    def __next__(self):
        self.n += 1
        return self.n
```

بنابراین `__next__` را این‌طور در ذهنت ببین: **اسم قراردادی‌ای که Python برای عملیات «عنصر بعدی Iterator را بده» تعیین کرده است.** این دو آندرلاین تزئینی نیستند؛ باعث می‌شوند این متد جزو **Data Model / Protocolهای خود Python** باشد.

[[Iterable و Iterator]]
[[Python]]