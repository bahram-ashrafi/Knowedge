**`systemd` سیستم اصلی مدیریت سرویس‌ها و فرایندهای پس‌زمینه در بیشتر نسخه‌های جدید Ubuntu است.** وقتی Ubuntu روشن می‌شود، `systemd` یکی از اولین برنامه‌های مهمی است که اجرا می‌شود و مسئول راه‌اندازی و مدیریت سرویس‌هایی مثل `ssh`، `docker`، `cron` و `snapd` است.

مثلاً:

```bash
systemctl status snapd
systemctl start snapd
systemctl stop snapd
systemctl restart snapd
systemctl enable snapd
```

اینجا `systemctl` **دستور کار با systemd** است و `snapd` نام سرویسی است که می‌خواهیم مدیریت کنیم. پس رابطه را این‌طور ببین:

```text
systemd
   ↓ مدیریت
systemctl  ← ابزار کنترل
   ↓
snapd / ssh / docker / ...
```

یعنی **systemd = مدیر سرویس‌های سیستم** و **systemctl = کنترل پنل خط فرمان آن**.

[[snapd چیه؟]]