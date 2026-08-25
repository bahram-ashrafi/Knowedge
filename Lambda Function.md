در این صفحه W3Schools مفهوم **Lambda Function** در پایتون را توضیح می‌دهد. ([W3Schools](https://www.w3schools.com/python/python_lambda.asp "Python Lambda"))

### 1. Lambda چیست؟

`lambda` در اصل یک **تابع کوچک و بدون نام (anonymous)** است. ساختارش این است:

```python
lambda arguments: expression
```

مثلاً:

```python
square = lambda x: x * x

print(square(5))
# 25
```

اگر همین را با `def` بنویسیم:

```python
def square(x):
    return x * x
```

پس می‌توانی فعلاً این‌طور در ذهن نگه داری:

> **lambda = ساختن یک تابع کوچک، معمولاً برای استفاده کوتاه و یک‌باره.**

یک نکته مهم: Lambda محدود به یک آرگومان نیست؛ می‌تواند چند آرگومان داشته باشد، اما بدنه‌اش فقط **یک expression** دارد. مثلاً `lambda a, b: a + b`. ([W3Schools](https://www.w3schools.com/python/python_lambda.asp "Python Lambda"))

### 2. نکته مهم‌تر: Lambda معمولاً کجا ارزش پیدا می‌کند؟

قدرت واقعی Lambda زمانی مشخص می‌شود که بخواهیم **یک تابع را به تابع دیگری بدهیم**:

```python
numbers = [1, 2, 3, 4, 5]

result = list(map(lambda x: x * 2, numbers))

print(result)
# [2, 4, 6, 8, 10]
```

اینجا `lambda x: x * 2` خودش یک **function object** است و به `map()` داده شده است. W3Schools هم کاربرد Lambda را به‌خصوص در `map()`, `filter()` و `sorted()` نشان می‌دهد. ([W3Schools](https://www.w3schools.com/python/python_lambda.asp "Python Lambda"))

برای فهم عمیق‌تر، پیشنهاد می‌کنم Lambda را فقط به‌عنوان «تابع کوتاه» نبینی؛ ارتباطش با **First-Class Functions، Higher-Order Functions، `map`، `filter` و `sorted(key=...)`** خیلی مهم‌تر است.

[[Python]]