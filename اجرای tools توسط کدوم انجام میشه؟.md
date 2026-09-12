معمولاً **Harness اجرای واقعی Tool را انجام می‌دهد**، نه خود Agent/LLM.

Agent با استفاده از LLM تصمیم می‌گیرد: «باید Tool X را صدا بزنم» و یک **Tool Call** تولید می‌کند. Harness این درخواست را دریافت می‌کند، Tool را اجرا می‌کند، نتیجه را به Agent/LLM برمی‌گرداند و چرخه ادامه پیدا می‌کند:

`Agent/LLM → Tool Call → Harness → اجرای Tool → نتیجه → Harness → Agent/LLM`

[[agent vs harness]]