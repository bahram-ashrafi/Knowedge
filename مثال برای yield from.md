فرض کن می‌خواهیم یک Generator داشته باشیم که چند دسته عدد را پشت سر هم تولید کند. بدون `yield from` باید Generatorهای داخلی را با `for` پیمایش کنیم:

```python
def numbers1():
    yield 1
    yield 2

def numbers2():
    yield 3
    yield 4

def all_numbers():
    for x in numbers1():
        yield x

    for x in numbers2():
        yield x

print(list(all_numbers()))
# [1, 2, 3, 4]
```

با `yield from` خیلی ساده‌تر می‌شود:

```python
def all_numbers():
    yield from numbers1()
    yield from numbers2()

print(list(all_numbers()))
# [1, 2, 3, 4]
```

یعنی می‌توانی `yield from numbers1()` را ذهنی این‌طور بخوانی: **«هر چیزی که `numbers1` تولید می‌کند، من هم مستقیماً تولیدش می‌کنم.»** بنابراین `yield from` برای **زنجیره‌کردن یا ترکیب چند Generator** بسیار مفید است.

[[Yield from]]