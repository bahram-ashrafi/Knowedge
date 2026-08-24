حالا می‌رسیم به نکته‌ی مهم‌تر: **`for` دقیقاً چگونه Generator را مصرف می‌کند؟** وقتی می‌نویسی:

```python
def numbers():
    yield 10
    yield 20
    yield 30

for x in numbers():
    print(x)
```

پایتون تقریباً چنین کاری انجام می‌دهد:

```python
g = numbers()

while True:
    try:
        x = next(g)
        print(x)
    except StopIteration:
        break
```

یعنی `for` در اصل با `iter()` و `next()` کار می‌کند. هر بار `next(g)` اجرا می‌شود، Generator از آخرین `yield` ادامه پیدا می‌کند. وقتی دیگر `yield`ای باقی نمانده باشد، Generator با `StopIteration` تمام می‌شود. بنابراین می‌توانیم Generator را یک **ماشین حالت (State Machine)** در نظر بگیریم: اجرای تابع در نقطه‌ی `yield` متوقف می‌شود و وضعیت محلی آن (`x`، متغیرهای حلقه، محل اجرای کد و...) برای ادامه‌ی بعدی حفظ می‌شود.

مثلاً:

```python
def test():
    print("A")
    yield 1

    print("B")
    yield 2

    print("C")
```

```python
g = test()

print("X")
print(next(g))
print("Y")
print(next(g))
print("Z")
```

ترتیب خروجی:

```text
X
A
1
Y
B
2
Z
```

دقت کن `print("A")` هنگام `test()` اجرا نمی‌شود؛ **اولین اجرای واقعی تابع با `next()` اتفاق می‌افتد**. این همان مفهوم **lazy evaluation** است که Generator را از یک تابع معمولی متمایز می‌کند.