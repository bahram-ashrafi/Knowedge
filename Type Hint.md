این صفحه از مستندات رسمی [FastAPI — Python Types Intro](https://fastapi.tiangolo.com/python-types/?utm_source=chatgpt.com) در واقع قبل از ورود جدی به FastAPI، **Type Hintهای پایتون** را توضیح می‌دهد. نکته‌ی اصلی این است که FastAPI شدیداً بر اساس همین Type Hintها ساخته شده است. ([FastAPI](https://fastapi.tiangolo.com/python-types/ "Python Types Intro - FastAPI"))

### 1. Type Hint چیست؟

مثلاً بدون Type Hint:

```python
def get_full_name(first_name, last_name):
    return first_name.title() + " " + last_name.title()
```

ما از کد نمی‌فهمیم `first_name` و `last_name` قرار است چه نوعی باشند.

با Type Hint:

```python
def get_full_name(first_name: str, last_name: str):
    return first_name.title() + " " + last_name.title()
```

عبارت:

```python
first_name: str
```

یعنی:

> `first_name` باید از نوع `str` باشد.

اما **نکته‌ی بسیار مهم:** Type Hint به‌تنهایی جلوی ارسال نوع اشتباه را در زمان اجرای عادی پایتون نمی‌گیرد؛ بیشتر برای editor، ابزارهای type checking و کتابخانه‌هایی مثل FastAPI استفاده می‌شود. همچنین `:` در اینجا با `=` فرق دارد؛ `first_name="john"` مقدار پیش‌فرض است، ولی `first_name: str` فقط نوع را مشخص می‌کند. ([FastAPI](https://fastapi.tiangolo.com/python-types/ "Python Types Intro - FastAPI"))

### 2. چند Type Hint مهم

```python
name: str
age: int
price: float
active: bool
data: bytes
```

برای collectionها هم می‌توانیم نوع داخل آن‌ها را مشخص کنیم:

```python
names: list[str]
```

یعنی:

> `names` یک `list` است که عناصر آن `str` هستند.

یا:

```python
prices: dict[str, float]
```

یعنی:

> کلیدهای `dict` از نوع `str` و مقدارهای آن `float` هستند.

همچنین:

```python
item: int | str
```

یعنی `item` می‌تواند `int` **یا** `str` باشد.

و:

```python
name: str | None = None
```

یعنی `name` یا `str` است یا `None`. این مورد در FastAPI بسیار مهم است، چون مشخص می‌کند یک مقدار می‌تواند وجود نداشته باشد. ([FastAPI](https://fastapi.tiangolo.com/python-types/ "Python Types Intro - FastAPI"))

### 3. قسمت مهم‌تر برای FastAPI

قدرت اصلی Type Hint وقتی مشخص می‌شود که وارد FastAPI شویم. مثلاً:

```python
@app.get("/users/{user_id}")
def get_user(user_id: int):
    return {"user_id": user_id}
```

اینجا FastAPI از همین:

```python
user_id: int
```

می‌فهمد که `user_id` باید `int` باشد. سپس می‌تواند **داده را تبدیل کند، اعتبارسنجی کند، خطای مناسب تولید کند و مستندات OpenAPI بسازد**. یعنی یک Type Hint ساده، برای FastAPI اطلاعات زیادی فراهم می‌کند. ([FastAPI](https://fastapi.tiangolo.com/python-types/ "Python Types Intro - FastAPI"))

اگر بخواهی این صفحه را **مثل آموزش Python قبلی‌مان و قدم‌به‌قدم** یاد بگیریم، پیشنهاد می‌کنم از همین‌جا شروع کنیم و اول دقیقاً تفاوت این سه مفهوم را باز کنیم:

**`Type Hint` → `Type Checking` → `Runtime Validation`**

چون فهم این تفاوت‌ها برای درک FastAPI و مخصوصاً **Pydantic** خیلی مهم است.

[[FastAPI]]
