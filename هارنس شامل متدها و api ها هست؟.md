بله، **معمولاً Harness شامل کد، متدها، APIها و زیرساخت اجرایی** است؛ اما Harness خودش صرفاً «مجموعه API» نیست. مثلاً متدهایی برای `execute_tool()`، مدیریت context، ارسال درخواست به LLM، ثبت نتایج و کنترل loop اجرای Agent دارد.

به‌صورت ساده:

`Agent → درخواست استفاده از Tool → Harness API/Method → اجرای Tool → نتیجه → Agent`

مثلاً ممکن است Harness یک متد `run_terminal(command)` داشته باشد که Agent از طریق آن درخواست اجرای یک command را می‌دهد؛ **خود Harness آن command را اجرا و نتیجه را برمی‌گرداند**.

[[AI، هارنس (AI Harness)]]