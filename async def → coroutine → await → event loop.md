حتماً. بیاییم همین بخش را **از پایه و عمیق** باز کنیم. مستندات FastAPI در این صفحه دقیقاً سه مفهوم را به هم وصل می‌کند: **Asynchronous Code → `async/await` → Coroutine**. ([FastAPI](https://fastapi.tiangolo.com/async/ "Concurrency and async / await - FastAPI"))

## 1. اول مشکل را بفهمیم: `I/O` یعنی انتظار

فرض کن FastAPI این درخواست را دریافت می‌کند:

```python
@app.get("/users")
def get_users():
    users = get_users_from_database()
    return users
```

اجرای برنامه به این شکل است:

```text
Python
  │
  ├── اجرای get_users()
  │
  ├── درخواست به Database
  │
  ├── ⏳ منتظر Database
  │
  ├── Database جواب می‌دهد
  │
  └── return users
```

قسمت مهم اینجاست:

```text
⏳ منتظر Database
```

CPU در این مدت واقعاً کار زیادی ندارد. همین موضوع برای موارد زیر هم اتفاق می‌افتد:

```text
Database
API خارجی
Network
File
Client
```

FastAPI این نوع عملیات را **I/O-bound** می‌نامد؛ یعنی بخش زیادی از زمان اجرای آن صرف انتظار برای I/O می‌شود. ([FastAPI](https://fastapi.tiangolo.com/async/ "Concurrency and async / await - FastAPI"))

---

# 2. `async` چه مشکلی را حل می‌کند؟

حالا فرض کنیم سه کار داریم:

```text
Request A → Database → 3 ثانیه انتظار
Request B → API      → 2 ثانیه انتظار
Request C → Database → 1 ثانیه انتظار
```

در مدل ساده‌ی sequential:

```text
A ────────────────┐
                  ↓
                  3s

B ──────────┐
            ↓
            2s

C ─────┐
       ↓
       1s

Total ≈ 6s
```

اما در asynchronous:

```text
A ──→ ⏳ Database
       │
       └────┐
            ↓
B ──→ ⏳ API
       │
       └────┐
            ↓
C ──→ ⏳ Database
       │
       └────┐
            ↓

A ← جواب
B ← جواب
C ← جواب
```

یعنی وقتی A در حال **انتظار** است، Python می‌تواند سراغ B برود.

این همان چیزی است که FastAPI می‌خواهد از `async/await` استفاده کنی. ([FastAPI](https://fastapi.tiangolo.com/async/ "Concurrency and async / await - FastAPI"))

---

# 3. حالا `async def` را بفهمیم

این:

```python
def get_data():
    ...
```

یک تابع معمولی است.

اما:

```python
async def get_data():
    ...
```

یک **تابع asynchronous** است.

مثلاً:

```python
async def get_data():
    return "Hello"
```

یک نکته‌ی بسیار مهم:

وقتی می‌نویسی:

```python
result = get_data()
```

تابع مثل تابع معمولی اجرا نمی‌شود و نتیجه‌ی نهایی را مستقیماً نمی‌دهد.

در واقع چیزی به نام **coroutine** ایجاد می‌شود.

به زبان ساده:

```text
async def
    ↓
coroutine function
    ↓
calling it
    ↓
coroutine object
```

پس:

```python
async def get_data():
    return "Hello"


result = get_data()

print(result)
```

تقریباً چنین چیزی خواهی دید:

```text
<coroutine object get_data at ...>
```

نه:

```text
Hello
```

---

# 4. اینجاست که `await` وارد می‌شود

برای اجرای coroutine باید بنویسی:

```python
result = await get_data()
```

اما `await` فقط داخل `async def` مجاز است. مستندات FastAPI هم دقیقاً روی همین نکته تأکید می‌کند. ([FastAPI](https://fastapi.tiangolo.com/async/ "Concurrency and async / await - FastAPI"))

مثلاً:

```python
async def get_data():
    return "Hello"


async def main():
    result = await get_data()
    print(result)
```

اینجا:

```text
main()
 │
 ├── await get_data()
 │
 └── نتیجه → "Hello"
```

---

# 5. معنی واقعی `await`

این قسمت خیلی مهم است.

وقتی می‌نویسی:

```python
result = await get_data()
```

نباید `await` را صرفاً این‌طور معنی کنی:

> «صبر کن.»

معنی دقیق‌ترش این است:

> **«من اینجا به نتیجه‌ی این عملیات نیاز دارم؛ اگر عملیات هنوز تمام نشده، اجرای این coroutine را موقتاً متوقف کن تا کار دیگری بتواند اجرا شود، و وقتی نتیجه آماده شد، از همین‌جا ادامه بده.»**

مثلاً:

```python
async def get_user():
    user = await database.get_user()
    return user
```

وقتی Database هنوز جواب نداده:

```text
get_user()
   │
   ├── database.get_user()
   │
   ├── ⏸ pause
   │
   │       ← فرصت برای اجرای Request دیگر
   │
   │
   ├── Database جواب داد
   │
   └── ادامه از همین نقطه
```

این دقیقاً ایده‌ی اصلی asynchronous programming است. ([FastAPI](https://fastapi.tiangolo.com/async/ "Concurrency and async / await - FastAPI"))

---

# 6. چرا `await` بدون `async` ممکن نیست؟

این اشتباه است:

```python
def main():
    result = await get_data()
```

Python خطا می‌دهد.

چون:

```text
await
  ↓
نیازمند async context
  ↓
async def
```

بنابراین:

```python
async def main():
    result = await get_data()
```

درست است.

یعنی یک زنجیره ایجاد می‌شود:

```text
async def main()
       │
       │ await
       ↓
async def get_data()
       │
       │ await
       ↓
async operation
```

مستندات FastAPI نیز همین رابطه را توضیح می‌دهد: تابعی که `async def` است معمولاً باید توسط یک `async` context با `await` فراخوانی شود. ([FastAPI](https://fastapi.tiangolo.com/async/ "Concurrency and async / await - FastAPI"))

---

# 7. حالا FastAPI چه کار می‌کند؟

اینجا قضیه خیلی جالب می‌شود.

تو می‌نویسی:

```python
@app.get("/users")
async def get_users():
    users = await database.get_users()
    return users
```

اما سؤال:

**چه کسی اولین `await` را اجرا می‌کند؟**

تو که `main()` نداری.

FastAPI اینجا وارد می‌شود.

FastAPI/ASGI محیط اجرای asynchronous را مدیریت می‌کند و endpoint تو را در آن محیط اجرا می‌کند. بنابراین تو لازم نیست خودت برای اولین coroutine دنبال `asyncio.run()` بگردی. مستندات FastAPI هم صراحتاً می‌گوید در FastAPI لازم نیست نگران این «اولین async function» باشی؛ path operation function توسط FastAPI مدیریت می‌شود. ([FastAPI](https://fastapi.tiangolo.com/async/ "Concurrency and async / await - FastAPI"))

پس ذهنی این تصویر را داشته باش:

```text
                FastAPI
                   │
                   ↓
             Event Loop
                   │
                   ↓
        async def get_users()
                   │
                   ↓
          await database()
                   │
             ┌─────┴─────┐
             ↓           ↓
          waiting     کار دیگر
             │
             ↓
       Database جواب
             │
             ↓
       ادامه‌ی function
```

---

## 8. اما یک دام بسیار مهم ⚠️

این کد:

```python
async def get_users():
    time.sleep(5)
    return users
```

**واقعاً asynchronous نیست.**

چرا؟

چون:

```python
time.sleep(5)
```

کل thread/event loop را block می‌کند.

اما:

```python
async def get_users():
    await asyncio.sleep(5)
    return users
```

اجازه می‌دهد event loop در این مدت کار دیگری انجام دهد.

بنابراین این قانون را فعلاً حفظ کن:

> **`async def` به تنهایی کافی نیست؛ عملیات داخل آن هم باید قابلیت asynchronous داشته باشد.**

---

### یک تصویر ذهنی نهایی

فعلاً این چهار مفهوم را این‌طور به هم وصل کن:

```text
                async def
                    │
                    ↓
                Coroutine
                    │
                    ↓
                  await
                    │
                    ↓
        ┌─────────────────────┐
        │  عملیات در حال I/O  │
        └─────────────────────┘
                    │
             ⏸ موقتاً pause
                    │
                    ↓
           Event Loop می‌رود
           سراغ کار دیگری
                    │
                    ↓
             I/O تمام شد
                    │
                    ↓
             ▶ ادامه coroutine
```

اگر این مدل را بفهمی، قدم بعدی بسیار مهم است: **Event Loop دقیقاً چیست و چگونه چند coroutine را مدیریت می‌کند؟** این همان قسمتی است که تفاوت `asyncio`، `async def`، `await` و `thread` کاملاً روشن می‌شود.

[[FastAPI]]