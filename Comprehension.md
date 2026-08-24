این مقاله دقیقاً ادامه‌ی بحثی است که داشتیم. **Generator Expression** روشی کوتاه برای ساختن یک Generator است؛ یعنی به‌جای اینکه یک `generator function` با `yield` بنویسی، از syntax شبیه List Comprehension استفاده می‌کنی. ([Python Tutorial](https://www.pythontutorial.net/advanced-python/python-generator-expressions/ "Python Generator Expressions"))

```python
# Generator Function
def squares():
    for n in range(5):
        yield n ** 2

g1 = squares()
```

همان مفهوم را می‌توان خیلی کوتاه‌تر نوشت:

```python
# Generator Expression
g2 = (n ** 2 for n in range(5))
```

تفاوت بسیار مهم با List Comprehension:

```python
list_ = [n ** 2 for n in range(5)]
gen    = (n ** 2 for n in range(5))
```

`list_` **همان لحظه تمام مقادیر را محاسبه و ذخیره می‌کند**، ولی `gen` فقط یک Generator می‌سازد و هر مقدار را هنگام درخواست تولید می‌کند؛ بنابراین **eager vs lazy** و همچنین **memory usage** تفاوت اصلی هستند. ([Python Tutorial](https://www.pythontutorial.net/advanced-python/python-generator-expressions/ "Python Generator Expressions"))

یک نکته‌ی جالب دیگر: Generator Expression حتی می‌تواند شرط داشته باشد:

```python
gen = (x ** 2 for x in range(10) if x % 2 == 0)

print(list(gen))
# [0, 4, 16, 36, 64]
```

یعنی اگر قبلاً List Comprehension را خوب فهمیده باشی، تقریباً فقط کافی است `[]` را به `()` تبدیل کنی تا به Generator Expression برسی. ولی **تفاوت پشت این دو علامت بسیار مهم است**: `[]` یک container می‌سازد، `()` در این حالت یک جریان lazy از داده‌ها می‌سازد. ([Python Tutorial](https://www.pythontutorial.net/advanced-python/python-generator-expressions/ "Python Generator Expressions"))
[[Generators در پایتون]]