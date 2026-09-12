بیشتر **یک محیط اجرایی (Runtime/Environment)** است که داخل خودش می‌تواند یک **pipeline یا loop اجرایی** داشته باشد.

یعنی Harness معمولاً مسئول این چیزهاست:

1. مدیریت **Agent**
    
2. مدیریت **Context**
    
3. اجرای **Skill**
    
4. فراخوانی **Toolها**
    
5. ارتباط با **RAG**
    
6. مدیریت چرخه‌ی `LLM → Tool → نتیجه → LLM`
    
7. کنترل خطا، state، لاگ و محدودیت‌ها
    

پس اگر بخواهیم خیلی ساده بگوییم: **Harness = محیط اجرای Agent + orchestration/pipeline آن**. Pipeline یکی از اجزای Harness است، نه اینکه Harness صرفاً یک pipeline باشد.

[[agent vs harness]]