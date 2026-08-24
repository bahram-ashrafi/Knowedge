تفاوتشان را اگر از پایه ببینیم خیلی ساده می‌شود:

1. **Syntax زبان Python** → چیزی است که جزو **قواعد خود زبان** است و Python interpreter آن را به‌عنوان ساختار زبان می‌شناسد. مثل `if`، `for`، `yield`، `return`، `class` و `def`. مثلاً `yield` تابع نیست و نمی‌توانی آن را مثل `yield()` صدا بزنی؛ بخشی از grammar زبان Python است.
    
2. **Built-in Function** → یک تابع آماده است که Python در اختیارمان گذاشته و معمولاً بدون `import` می‌توانیم از آن استفاده کنیم. مثل:
    

```python
len([1, 2, 3])
next(gen)
print("hello")
list(gen)
```

`next()` یک **تابع built-in** است. می‌توانی حتی ببینی از کجا آمده:

```python
print(next)
# <built-in function next>
```

3. **Method** → تابعی است که به یک **object یا class وابسته است** و از طریق آن object صدا زده می‌شود:
    

```python
gen.send(10)
gen.throw(ValueError)
gen.close()
```

اینجا `send`، `throw` و `close` متدهای Generator هستند. بنابراین یک مقایسه‌ی خوب:

```text
yield          → Syntax زبان
next(gen)      → Built-in Function
gen.send(10)   → Method
gen.close()    → Method
```

و نکته‌ی خیلی مهم: **`next()` خودش در نهایت از پروتکل Iterator و متد `__next__()` استفاده می‌کند**. یعنی وقتی می‌نویسی `next(gen)`، در سطح Data Model اتفاقی شبیه این رخ می‌دهد:

```python
next(gen)
      ↓
gen.__next__()
```

پس `next()` یک built-in function است، ولی `__next__()` یک **special method** است.
[[Python]]