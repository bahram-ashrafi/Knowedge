این صفحه FastAPI درباره‌ی **Concurrency و `async/await`** است. با توجه به اینکه قبلاً درباره‌ی Generator و `yield` و تفاوت `def`/`async def` صحبت کردیم، بهتر است این بخش را مفهومی جلو ببریم، نه صرفاً ترجمه‌ی مستندات. ([FastAPI](https://fastapi.tiangolo.com/async/ "Concurrency and async / await - FastAPI"))

### 1. مسئله‌ی اصلی چیست؟

فرض کن FastAPI سه درخواست هم‌زمان دریافت کند:

```text
Request A ──> درخواست به Database ──────── انتظار 2s
Request B ──> درخواست به API خارجی ─────── انتظار 3s
Request C ──> پردازش سریع ──────────────── 0.1s
```

در حالت **synchronous**، برنامه ممکن است این‌طور رفتار کند:

```text
A: شروع ───── انتظار ───── پایان
                         B: شروع ───── انتظار ───── پایان
                                                    C: شروع ─ پایان
```

یعنی وقتی A منتظر Database است، اجرای همان مسیر متوقف می‌شود.

اما در **asynchronous/concurrent**:

```text
A: شروع ── انتظار Database ─────────────── ادامه
B:          شروع ── انتظار API ─────────── ادامه
C:                 شروع ─ پایان
A:                            نتیجه Database
B:                                      نتیجه API
```

نکته‌ی خیلی مهم این است که **Concurrency به معنی اجرای هم‌زمان روی چند CPU نیست**. در اینجا برنامه هنگام انتظار برای I/O، کار دیگری را انجام می‌دهد. FastAPI مستنداتش نیز تأکید می‌کند که این مدل برای کارهای **I/O-bound** مثل Database، API، شبکه و فایل مناسب است. ([FastAPI](https://fastapi.tiangolo.com/async/ "Concurrency and async / await - FastAPI"))

### 2. نقش `async` و `await`

این کد را ببین:

```python
async def get_data():
    data = await fetch_data()
    return data
```

دو مفهوم داریم:

```python
async def
```

یعنی:

> این تابع می‌تواند **متوقف شود و بعداً ادامه پیدا کند**.

و:

```python
await fetch_data()
```

یعنی:

> اجرای این coroutine فعلاً منتظر نتیجه‌ی `fetch_data()` است؛ در این فاصله event loop می‌تواند کار دیگری انجام دهد.

مثلاً:

```python
@app.get("/users")
async def users():
    users = await get_users_from_database()
    return users
```

اگر `get_users_from_database()` یک عملیات واقعاً asynchronous باشد، هنگام انتظار Database، FastAPI می‌تواند به درخواست‌های دیگر رسیدگی کند. مستندات FastAPI دقیقاً همین الگو را برای کتابخانه‌هایی که `await` ارائه می‌کنند توصیه می‌کند. ([FastAPI](https://fastapi.tiangolo.com/async/ "Concurrency and async / await - FastAPI"))

اما یک نکته‌ی بسیار مهم:

```python
async def
```

**به‌تنهایی کد را asynchronous نمی‌کند.**

مثلاً:

```python
@app.get("/")
async def test():
    time.sleep(5)
    return {"message": "hello"}
```

اینجا `time.sleep()` یک عملیات blocking است و event loop را متوقف می‌کند. بنابراین باید از APIهایی استفاده کنی که واقعاً asynchronous هستند، مثلاً:

```python
await asyncio.sleep(5)
```

یا یک کتابخانه‌ی async برای Database/HTTP و غیره.

همچنین FastAPI اجازه می‌دهد `def` و `async def` را با هم استفاده کنی؛ اگر کتابخانه‌ای که استفاده می‌کنی `await` ندارد، مستندات FastAPI می‌گوید می‌توانی endpoint را به شکل معمول `def` تعریف کنی. ([FastAPI](https://fastapi.tiangolo.com/async/ "Concurrency and async / await - FastAPI"))

**پیشنهاد من برای ادامه:** بخش بعدی را از خود مفهوم **`async def` → coroutine → `await` → event loop** باز کنیم؛ چون اگر این چهار مفهوم را درست بفهمی، تقریباً تمام بخش async در FastAPI برایت قابل فهم می‌شود.