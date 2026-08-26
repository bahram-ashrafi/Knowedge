این صفحه‌ی W3Schools درباره‌ی **Iterator** در پایتون است. نکته‌ی اصلی این است که **Iterable و Iterator یکی نیستند**. یک `list`، `tuple`، `dict`، `set` یا `str` یک **Iterable** است؛ یعنی می‌توان از آن یک Iterator گرفت. اما خود Iterator شیئی است که طبق **Iterator Protocol** دو متد `__iter__()` و `__next__()` را دارد. مثلاً: ([W3Schools](https://www.w3schools.com/python/python_iterators.asp "Python Iterators"))

```python
numbers = [10, 20, 30]

it = iter(numbers)

print(next(it))  # 10
print(next(it))  # 20
print(next(it))  # 30
```

اینجا `numbers` یک **Iterable** است و `it` یک **Iterator**. هر بار که `next(it)` را اجرا می‌کنیم، Iterator فقط **عضو بعدی** را می‌دهد و وضعیت خودش را حفظ می‌کند. وقتی دیگر عضوی وجود نداشته باشد، `StopIteration` رخ می‌دهد. نکته‌ی بسیار مهم‌تر این است که `for` هم پشت صحنه تقریباً همین کار را می‌کند: از شیء یک Iterator می‌گیرد و مرتب `next()` را صدا می‌زند تا به `StopIteration` برسد. ([W3Schools](https://www.w3schools.com/python/python_iterators.asp "Python Iterators"))

پس اگر بخواهیم مفهوم را خیلی ساده کنیم:

```text
Iterable
   │
   │ iter()
   ▼
Iterator
   │
   ├── next() → item 1
   ├── next() → item 2
   ├── next() → item 3
   └── next() → StopIteration
```

این مفهوم مستقیماً به بحث قبلی‌مان درباره‌ی **Generator** هم وصل می‌شود: Generator در واقع یک نوع Iterator است، با این تفاوت که پایتون ساختن `__iter__()` و `__next__()` را برایمان مدیریت می‌کند. ([W3Schools](https://www.w3schools.com/python/python_iterators.asp "Python Iterators"))
[[Python]]
[[Generators در پایتون]]