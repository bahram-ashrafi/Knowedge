روش صحیح و رایج این است که Generator را به `list()` بدهی:

```python
def numbers():
    yield 1
    yield 2
    yield 3

generator = numbers()

result = list(generator)

print(result)
# [1, 2, 3]
```

اینجا `list()` شروع به مصرف Generator می‌کند؛ یعنی عملاً چندین بار `next()` را تا رسیدن به `StopIteration` اجرا می‌کند و تمام مقادیر را در یک `list` قرار می‌دهد. بنابراین **Generator → List یعنی Lazy بودن را از دست می‌دهیم و تمام داده‌ها یکجا در حافظه قرار می‌گیرند.**

[[Generators در پایتون]]