مثلاً Agent می‌تواند این **Tool Definition** را به LLM معرفی کند:

```json
{
  "name": "get_order_status",
  "description": "وضعیت یک سفارش را از سیستم سفارش‌ها دریافت می‌کند.",
  "parameters": {
    "type": "object",
    "properties": {
      "order_id": {
        "type": "integer",
        "description": "شناسه سفارش"
      }
    },
    "required": ["order_id"]
  }
}
```

بعد LLM سؤال «وضعیت سفارش ۱۲۳ را بررسی کن» را می‌بیند و متوجه می‌شود ابزار مناسب `get_order_status` است. سپس چیزی شبیه این **Tool Call** تولید می‌کند:

```json
{
  "name": "get_order_status",
  "arguments": {
    "order_id": 123
  }
}
```

یعنی LLM خودش تابع را اجرا نمی‌کند؛ فقط می‌گوید **«این Tool را با این ورودی اجرا کن»** و Agent واقعاً آن را اجرا می‌کند.

[[llm چطور میفهمد که ایجنت ابزار  get_order_status را دارد؟]]
