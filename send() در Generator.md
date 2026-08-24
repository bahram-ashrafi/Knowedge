موضوع مهم بعدی **`send()` در Generator** است. تا اینجا فقط Generator از بیرون مقدار تولید می‌کرد (`yield → بیرون`)، اما `send()` اجازه می‌دهد **از بیرون هم مقداری را به Generator وارد کنیم**:

```python
def calculator():
    x = yield
    while True:
        x = yield x * 2

gen = calculator()
next(gen)          # Generator را تا اولین yield می‌رساند
print(gen.send(10))  # 20
print(gen.send(5))   # 10
```

اینجا `yield` فقط «خروجی دادن» نیست؛ می‌تواند **نقطه‌ی ارتباط دوطرفه** باشد: `yield` مقدار را به بیرون می‌دهد و `send(value)` مقدار را از بیرون به همان نقطه برمی‌گرداند. البته برای شروع Generator باید معمولاً یک `next()` انجام شود، چون `send()` با مقدار غیر-`None` نمی‌تواند به Generatorی که هنوز شروع نشده مقدار بفرستد. این قابلیت در ساخت **coroutineهای قدیمی پایتون، pipelineها و بعضی state machineها** استفاده می‌شد؛ هرچند امروزه برای [[امروزه برای asynchronous programming معمولاً async await انتخاب مناسب‌تری است.]]


[[Generators در پایتون]]