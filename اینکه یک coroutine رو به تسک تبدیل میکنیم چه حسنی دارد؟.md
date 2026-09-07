مزیت اصلی این است که **Coroutine به‌تنهایی فقط «یک کار async» است، اما Task باعث می‌شود آن کار توسط Event Loop به‌صورت مستقل زمان‌بندی و پیگیری شود.**

### 1. بدون Task

```python
async def get_user():
    await asyncio.sleep(2)
    return "Bahram"


async def main():
    result = await get_user()
    print(result)
```

اینجا:

```text
main
 │
 ↓
await get_user()
 │
 ↓
get_user اجرا می‌شود
 │
 ↓
2 ثانیه انتظار
 │
 ↓
نتیجه
```

یعنی `main` مستقیماً منتظر `get_user` است.

---

### 2. با Task

```python
async def main():
    task = asyncio.create_task(get_user())

    # اینجا می‌توانیم کار دیگری انجام دهیم

    result = await task
    print(result)
```

اینجا اتفاق متفاوتی افتاده:

```text
main
 │
 ├── create_task(get_user())
 │       ↓
 │     Task
 │       ↓
 │   Event Loop
 │
 ├── کارهای دیگر
 │
 └── await task
         ↓
      نتیجه
```

یعنی با `create_task()` به Event Loop می‌گوییم:

> **این کار را مستقل از ادامه‌ی کار فعلی، در برنامه زمان‌بندی کن.**

---

## 3. فایده‌ی واقعی وقتی مشخص می‌شود که چند کار داریم

مثلاً دو کار:

```python
async def get_user():
    await asyncio.sleep(2)
    return "Bahram"


async def get_orders():
    await asyncio.sleep(3)
    return ["Order 1", "Order 2"]
```

### حالت اول: بدون Task

```python
async def main():
    user = await get_user()
    orders = await get_orders()
```

زمان:

```text
get_user
├──────────── 2s ────────────┤

get_orders
├───────────────── 3s ───────┤

Total = 5s
```

چون تا `get_user()` تمام نشود، سراغ `get_orders()` نمی‌رویم.

---

### حالت دوم: با Task

```python
async def main():
    user_task = asyncio.create_task(get_user())
    orders_task = asyncio.create_task(get_orders())

    user = await user_task
    orders = await orders_task
```

حالا:

```text
get_user
├──────────── 2s ────────────┤
                             ↓
                           done

get_orders
├───────────────── 3s ───────┤
                             ↓
                           done

Total ≈ 3s
```

هر دو کار **از قبل برای Event Loop زمان‌بندی شده‌اند**.

این مهم‌ترین مزیت Task است:

> **Task اجازه می‌دهد چند coroutine مستقل را هم‌زمان در جریان داشته باشی، بدون اینکه مجبور باشی اولی تمام شود و بعد دومی را شروع کنی.**

---

### یک تفاوت ظریف ولی خیلی مهم

این:

```python
user = await get_user()
orders = await get_orders()
```

یعنی:

```text
اول get_user
بعد get_orders
```

ولی این:

```python
user_task = asyncio.create_task(get_user())
orders_task = asyncio.create_task(get_orders())

user = await user_task
orders = await orders_task
```

یعنی:

```text
Task 1 ────────┐
               ├── در جریان
Task 2 ───────────────┐
                      │
                      ↓
              منتظر نتایج
```

بنابراین می‌توانی این سه مفهوم را این‌طور به خاطر بسپاری:

```text
Coroutine
   ↓
«می‌خواهم این کار async را انجام بدهم»

Task
   ↓
«این کار را برای اجرا در Event Loop زمان‌بندی کن»

await
   ↓
«حالا نتیجه‌ی این کار را به من بده؛
تا آماده شدنش منتظر می‌مانم»
```

**پس تبدیل Coroutine به Task به این معنی نیست که کار سریع‌تر اجرا می‌شود؛ مزیت اصلی این است که می‌توانی اجرای آن را از جریان فعلی جدا کرده و هم‌زمان با کارهای async دیگر پیش ببری.**

[[تفاوت دقیق coroutine، Task و Future]]