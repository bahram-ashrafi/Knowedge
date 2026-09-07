در Generator، متد **`throw()`** به تو اجازه می‌دهد **یک Exception را از بیرون، داخل Generator و دقیقاً در نقطه‌ی `yield` فعلی وارد کنی**. یعنی به‌جای اینکه فقط با `next()` مقدار بعدی را بخواهی، می‌توانی به Generator بگویی: «در همین نقطه یک خطا رخ داده است.»

```python
def generator():
    try:
        yield 1
        yield 2
    except ValueError:
        print("خطا داخل Generator دریافت شد")

gen = generator()

print(next(gen))          # 1
gen.throw(ValueError)     # خطا را وارد Generator می‌کند
```

اتفاق مهم این است که `ValueError` **داخل `try` و در محل `yield 1`** وارد می‌شود؛ بنابراین `except` می‌تواند آن را بگیرد. این قابلیت وقتی جالب‌تر می‌شود که با `yield from` ترکیب شود، چون `throw()` می‌تواند از Generator بیرونی به Generator داخلی منتقل شود. در برنامه‌های معمولی امروزی کمتر با این الگو سروکار داری، اما برای فهم عمیق **Generator، Coroutineهای قدیمی و `yield from`** بسیار مهم است.

[[Generators در پایتون]]
[[Yield from]]