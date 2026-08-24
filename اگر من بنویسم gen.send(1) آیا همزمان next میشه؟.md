بله، **تقریباً می‌توانی `send(1)` را مثل یک `next()` همراه با ارسال مقدار در نظر بگیری**؛ اما یک تفاوت مهم دارد: `send(value)` Generator را از حالت توقف در `yield` ادامه می‌دهد و **مقدار `value` را به همان `yield` تزریق می‌کند**.

```python
def test():
    x = yield 10
    print("x =", x)
    yield 20

gen = test()

print(next(gen))      # 10
print(gen.send(1))    # x = 1
                      # خروجی: 20
```

اتفاقات را مرحله‌به‌مرحله ببین:

```text
next(gen)
   ↓
yield 10
   ↓
Generator متوقف می‌شود

gen.send(1)
   ↓
1 وارد yield می‌شود
   ↓
x = 1
   ↓
ادامه اجرای تابع
   ↓
yield 20
   ↓
20 به caller برمی‌گردد
```

پس **بله، `send(1)` باعث ادامه‌ی اجرای Generator می‌شود، درست شبیه `next()`؛ اما علاوه بر آن یک مقدار هم وارد Generator می‌کند.** یک نکته مهم: `gen.send(1)` برای Generatorی که هنوز شروع نشده مجاز نیست؛ ابتدا باید `next(gen)` یا `gen.send(None)` انجام شود.

[[send() در Generator]]