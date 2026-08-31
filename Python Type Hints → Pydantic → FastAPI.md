بریم یک مرحله عمیق‌تر؛ اینجا دقیقاً جایی است که **Type Hintهای Python به Pydantic و بعد به FastAPI وصل می‌شوند.**

### 4. از Type Hint به Data Model

فرض کن می‌خواهیم کاربر ایجاد کنیم. اطلاعات کاربر این‌هاست:

```python
name: str
age: int
email: str
```

در Python می‌توانیم یک کلاس معمولی بنویسیم:

```python
class User:
    def __init__(self, name: str, age: int, email: str):
        self.name = name
        self.age = age
        self.email = email
```

اما این Type Hintها به‌تنهایی **اعتبارسنجی نمی‌کنند**:

```python
user = User(
    name=123,
    age="hello",
    email=True
)
```

ممکن است Python این object را بسازد، چون Type Hint به‌تنهایی Runtime Validation انجام نمی‌دهد.

اینجاست که **Pydantic** وارد می‌شود:

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int
    email: str
```

حالا `User` دیگر فقط یک کلاس معمولی نیست؛ یک **مدل داده‌ی قابل اعتبارسنجی** است.

مثلاً:

```python
user = User(
    name="Bahram",
    age=43,
    email="bahram@example.com"
)
```

Pydantic داده را بررسی می‌کند.

---

### 5. Pydantic دقیقاً چه کار می‌کند؟

مثلاً:

```python
user = User(
    name="Bahram",
    age="43",
    email="bahram@example.com"
)
```

در بسیاری از موارد Pydantic می‌تواند:

```text
"43"  →  43
```

را تبدیل کند.

اما:

```python
user = User(
    name="Bahram",
    age="hello",
    email="bahram@example.com"
)
```

دیگر قابل تبدیل نیست و validation error ایجاد می‌شود.

پس اینجا تفاوت مهمی داریم:

```text
Type Hint
    │
    │  می‌گوید نوع مورد انتظار چیست
    ↓
Pydantic
    │
    │  داده را در Runtime بررسی/پردازش می‌کند
    ↓
Validated Data
```

---

## 6. حالا FastAPI را وارد کنیم

می‌توانیم همین مدل را مستقیماً در API استفاده کنیم:

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class User(BaseModel):
    name: str
    age: int
    email: str


@app.post("/users")
def create_user(user: User):
    return user
```

حالا کاربر یک HTTP Request می‌فرستد:

```json
{
    "name": "Bahram",
    "age": 43,
    "email": "bahram@example.com"
}
```

FastAPI تقریباً این مسیر را طی می‌کند:

```text
HTTP Request
     │
     ▼
JSON
     │
     ▼
Pydantic User Model
     │
     ├── name → str
     ├── age  → int
     └── email → str
     │
     ▼
Validation
     │
     ▼
user: User
     │
     ▼
create_user()
```

یعنی وقتی می‌نویسیم:

```python
def create_user(user: User):
```

FastAPI متوجه می‌شود:

> این endpoint یک `User` دریافت می‌کند.

و چون `User` از `BaseModel` آمده، FastAPI می‌تواند ساختار JSON ورودی را با آن مقایسه و اعتبارسنجی کند.

---

## 7. یک نکته‌ی خیلی مهم درباره `:`

حالا می‌توانیم یکی از مهم‌ترین syntaxهای Python را بهتر بفهمیم.

این:

```python
name: str
```

به معنی **Type Annotation** است.

اما این:

```python
name = "Bahram"
```

به معنی **Assignment** است.

و این:

```python
name: str = "Bahram"
```

هر دو را با هم دارد:

```text
name
 │
 ├── : str
 │     └── Type Annotation
 │
 └── = "Bahram"
       └── Default Value
```

مثلاً در Pydantic:

```python
class User(BaseModel):
    name: str
    age: int = 18
```

یعنی:

```text
name → اجباری
age  → اختیاری، مقدار پیش‌فرض = 18
```

پس اگر بفرستیم:

```json
{
    "name": "Bahram"
}
```

مدل می‌تواند تبدیلش کند به چیزی معادل:

```python
User(
    name="Bahram",
    age=18
)
```

---

## 8. و اینجا یک مفهوم بسیار مهم دیگر داریم: `Optional`

مثلاً:

```python
email: str | None = None
```

یعنی:

```text
email
 │
 ├── str
 └── None
```

پس `email` می‌تواند مقدار داشته باشد:

```python
email="test@example.com"
```

یا نداشته باشد:

```python
email=None
```

این syntax در Python مدرن بسیار مهم است:

```python
str | None
```

و برای درک FastAPI باید با مفهوم **Union Type** آشنا باشی.

مثلاً:

```python
id: int | str
```

یعنی:

> `id` می‌تواند `int` یا `str` باشد.

در واقع `|` اینجا یک syntax مربوط به **type system پایتون** است، نه عملگر معمولی OR منطقی.

---

### تصویر ذهنی نهایی

فعلاً این معماری را در ذهنت داشته باش:

```text
              Python
                │
          Type Annotation
                │
        ┌───────┴────────┐
        │                │
        ▼                ▼
   Type Checker       Pydantic
                         │
                         ▼
                    Validation
                         │
                         ▼
                      FastAPI
                         │
                         ▼
                    HTTP / JSON
```

**قدم بعدی خیلی مهم است:** برویم سراغ تفاوت `list[str]`، `dict[str, int]`، `tuple`، `set`، `str | None` و `Union`؛ چون مستندات FastAPI بعد از Type Hintهای ساده دقیقاً به سمت **Python Standard Types** می‌رود و این‌ها پایه‌ی تعریف Request و Response در FastAPI هستند.

[[Type Hint]]