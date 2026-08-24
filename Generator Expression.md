یک موضوع خیلی مهم دیگر در Generatorها، **Generator Expression** است. شبیه List Comprehension است، با این تفاوت که به‌جای ساختن کل لیست، مقادیر را یکی‌یکی تولید می‌کند:

```python
# List Comprehension
numbers = [x * 2 for x in range(1_000_000)]

# Generator Expression
numbers = (x * 2 for x in range(1_000_000))
```

در حالت اول، یک `list` بزرگ ساخته می‌شود و تمام نتایج در حافظه قرار می‌گیرند؛ در حالت دوم فقط یک Generator داریم و هر مقدار زمانی محاسبه می‌شود که لازم باشد:

```python
gen = (x * 2 for x in range(5))

print(next(gen))  # 0
print(next(gen))  # 2
print(next(gen))  # 4
```

یک کاربرد بسیار مهم این موضوع در **pipeline پردازش داده** است؛ یعنی خروجی یک Generator را مستقیماً وارد Generator بعدی می‌کنی:

```python
numbers = (x for x in range(1_000_000))
even = (x for x in numbers if x % 2 == 0)
squared = (x * x for x in even)

print(next(squared))
```

اینجا لازم نیست یک میلیون عدد را بسازیم، بعد یک لیست از اعداد زوج بسازیم، بعد یک لیست از مربع‌ها بسازیم. داده‌ها **مرحله‌به‌مرحله و فقط هنگام نیاز** از pipeline عبور می‌کنند. این مفهوم برای درک پردازش داده‌های بزرگ و بعداً چیزهایی مثل **stream processing** و حتی بعضی الگوهای پردازش در Odoo بسیار مهم است.

[[Generators در پایتون]]