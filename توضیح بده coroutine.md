**Coroutine** را می‌توان یک «تابع قابل توقف و ادامه» در نظر گرفت که برخلاف تابع معمولی، می‌تواند اجرای خودش را موقتاً متوقف کند و بعداً از همان نقطه ادامه دهد. در Python امروزی معمولاً با `async def` ساخته می‌شود:

```python
async def download():
    data = await get_data()
    return data
```

وقتی `download()` را صدا می‌زنی، تابع مثل یک تابع معمولی فوراً تا انتها اجرا نمی‌شود؛ یک **coroutine object** ایجاد می‌کند:

```python
coro = download()
```

برای اجرای آن معمولاً از `await` داخل یک coroutine دیگر یا از event loop استفاده می‌شود.

نکته‌ی مهم این است که **Coroutine خودش Thread نیست.** مثلاً:

```python
async def task():
    print("A")
    await something()
    print("B")
```

وقتی به `await` می‌رسد، اگر عملیات واقعاً asynchronous باشد، coroutine می‌تواند موقتاً متوقف شود و **event loop سراغ coroutine دیگری برود**. وقتی عملیات آماده شد، coroutine دوباره از همان نقطه ادامه پیدا می‌کند. بنابراین مدل ذهنی خوبی این است:

```text
Coroutine
   │
   ├── اجرا
   │
   ├── await → موقتاً توقف
   │
   │      Event Loop → اجرای کار دیگر
   │
   └── آماده شد → ادامه از همان نقطه
```

این همان چیزی است که برای کارهایی مثل **HTTP request، شبکه، WebSocket و عملیات I/O زیاد** بسیار مفید است. و ارتباطش با بحث قبلی ما هم جالب است: در Python قدیمی‌تر، **Generator + `yield`** برای ساخت coroutine استفاده می‌شد؛ اما Python امروزی برای این منظور syntax اختصاصی **`async` / `await`** دارد.

[[Generators در پایتون]]