برای فهم عمیق Generator باید این سه مفهوم را از هم جدا کنیم:

- **Iterable** → چیزی که می‌توان روی آن `for` زد.
    
- **Iterator** → شیئی که می‌تواند «مقدار بعدی» را با `next()` بدهد.
    
- **Generator** → یک روش ساده و خاص برای ساخت Iterator.
    

مثلاً `list` یک Iterable است:

```python
numbers = [10, 20, 30]

for x in numbers:
    print(x)
```

اما خود `list` مستقیماً Iterator نیست:

```python
next(numbers)  # TypeError
```

پایتون برای اجرای `for`، از Iterable یک Iterator می‌سازد:

```python
iterator = iter(numbers)

print(next(iterator))  # 10
print(next(iterator))  # 20
print(next(iterator))  # 30
```

### 2. Generator خودش Iterator است

وقتی می‌نویسیم:

```python
def numbers():
    yield 10
    yield 20
    yield 30

g = numbers()
```

`g` هم **Iterable** است و هم **Iterator**:

```python
iter(g) is g
# True

next(g)
# 10
```

در واقع می‌توانیم رابطه را این‌طور ببینیم:

```text
Iterable
   │
   │ iter()
   ▼
Iterator
   │
   │ next()
   ▼
value
```

و Generator یکی از مهم‌ترین پیاده‌سازی‌های Iterator در پایتون است. نکته‌ی مهم Data Model اینجاست: Iterator باید رفتار `__next__()` را داشته باشد و معمولاً `__iter__()` نیز خودش را برگرداند؛ Generator این سازوکار را برای ما به‌صورت خودکار فراهم می‌کند.

Iterator، Iterable، `__iter__()` و `__next__()`

[[for دقیقاً چگونه Generator را مصرف می‌کند؟]]
[[Generators در پایتون]]