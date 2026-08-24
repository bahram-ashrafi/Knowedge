بله، ولی **JavaScript کلمه‌ی `Coroutine` را به‌عنوان یک primitive یا syntax رسمی مثل Python `async def` ندارد**. با این حال، خود مفهوم coroutine در JavaScript وجود دارد و چند ابزار مختلف این رفتار را فراهم می‌کنند:

```javascript
async function getData() {
    const data = await fetch("/api/data");
    return data;
}
```

اینجا `async function` یک **Promise** برمی‌گرداند و `await` باعث می‌شود اجرای آن تابع تا آماده‌شدن Promise متوقف شود؛ در این فاصله **Event Loop** می‌تواند کارهای دیگری را اجرا کند. از نظر مفهومی این بسیار شبیه coroutine است.

همچنین JavaScript ابزارهای دیگری مثل **Generator Function** (`function*` و `yield`) دارد:

```javascript
function* numbers() {
    yield 1;
    yield 2;
    yield 3;
}
```

Generator در JavaScript حتی از نظر تاریخی و مفهومی به coroutine نزدیک‌تر است، چون می‌تواند اجرای خود را متوقف و بعداً ادامه دهد. بنابراین اگر بخواهیم مقایسه کنیم:

```text
Python                         JavaScript

def + yield                    function* + yield
Generator                      Generator

async def + await              async function + await
Coroutine                      Promise-based async function
```

اما این را دقیق‌تر به خاطر بسپار: **Generator و Coroutine یکی نیستند**؛ هر دو قابلیت suspend/resume دارند، ولی هدف و semantics متفاوتی دارند. `Generator` بیشتر برای **تولید/مصرف مرحله‌ای داده** است، در حالی که `async/await` برای **مدیریت عملیات asynchronous** طراحی شده است.

[[توضیح بده coroutine]]