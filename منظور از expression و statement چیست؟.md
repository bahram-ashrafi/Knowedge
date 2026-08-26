در پایتون، **Expression** چیزی است که وقتی اجرا می‌شود، در نهایت **یک مقدار (value) تولید می‌کند**. مثلاً `2 + 3` یک expression است چون نتیجه‌اش `5` است؛ `x * 2` هم expression است؛ حتی فراخوانی تابع مثل `print("Hello")` از نظر نحوی یک expression است، چون نتیجه‌ی آن یک مقدار (`None`) است. نمونه‌ها:

```python
2 + 3
x * 2
"Hello"
len("Python")
lambda x: x * 2
```

اما **Statement** یک دستور برای انجام یک کار است؛ لزوماً هدفش تولید یک مقدار نیست. مثل:

```python
x = 10          # assignment statement
if x > 5:       # if statement
    print(x)

for x in numbers:
    print(x)

return x
```

بنابراین یک راه شهودی برای تشخیص این است: **Expression → «چه مقداری؟»** و **Statement → «چه کاری انجام بده؟»**. مثلاً در `x = 10`، قسمت `10` یک expression است، اما کل `x = 10` یک statement محسوب می‌شود. و دلیل اینکه `lambda` نمی‌تواند چند دستور داشته باشد این است که بعد از `:` باید یک **expression** بیاید: `lambda x: x * 2`، نه مجموعه‌ای از statementها.

[[Lambda Function]]
[[Python]]