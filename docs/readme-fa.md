# راهنمای نصب و فعال‌سازی ionCube Loader مخصوص PHP X

این مستند راهنمای کامل نصب، تنظیم و تست ionCube Loader روی سروری با PHP X است. این راهنما با فرض داشتن دسترسی ترمینال (SSH) و سطح root یا sudo تهیه شده است.
* منظور از X ورژن هدف php است.

---

## ۱. پیش‌نیازها

- سیستم عامل لینوکسی (اوبونتو/دبیان/سنت‌اواس و...)
- نصب بودن PHP X (عدد X را با نسخه دقیق جایگزین کنید، مثلاً 8.1)
- دسترسی به ترمینال (SSH)
- ترجیحاً تهیه نسخه پشتیبان از سایت و تنظیمات سرور

---

## ۲. دانلود و استخراج ionCube Loader

```
cd /tmp
wget https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz
tar xzf ioncube_loaders_lin_x86-64.tar.gz
cd ioncube
```

---

## ۳. پیدا کردن مسیر extension_dir مخصوص PHP X

```
phpX -i | grep extension_dir
```
خروجی این دستور مثلاً شبیه به `/usr/lib/php/20210902/` است. (در این بخش X را با شماره نسخه اصلی PHP که می‌خواهید Loader برای آن فعال شود، جایگزین کنید.)

---

## ۴. کپی Loader به مسیر extension_dir

```
cp ioncube_loader_lin_X.so {output of the previous step}
```
- مسیر مقصد را طبق خروجی مرحله قبل جایگزین کنید.
- `ioncube_loader_lin_X.so` را براساس نسخه دقیق (مثلاً برای PHP 8.1: `ioncube_loader_lin_8.1.so`) انتخاب کنید.

---

## ۵. ویرایش php.ini نسخه PHP X

### مسیرهای رایج برای php.ini:
- PHP-FPM: `/etc/php/X/fpm/php.ini`
- CLI: `/etc/php/X/cli/php.ini`
- Apache: `/etc/php/X/apache2/php.ini`

برای مثال، جهت ویرایش فایل FPM:
```
nano /etc/php/X/fpm/php.ini
```
سپس این خط را در ابتدای فایل (بعد از توضیحات) اضافه کنید:

```
zend_extension = /usr/lib/php/20210902/ioncube_loader_lin_X.so
```
- مسیر را براساس خروجی مرحله ۳ تنظیم کنید.

---

## ۶. ریستارت سرویس‌های PHP و وب سرور

### برای PHP-FPM:
```
systemctl restart phpX-fpm
```

### برای Apache:
```
systemctl restart apache2
```

### برای Nginx:
```
systemctl restart nginx
```

---

## ۷. تست صحت نصب ionCube Loader

### تست با کامند:
```
phpX -v
```
در خروجی این دستور باید عبارتی مانند زیر دیده شود:
```
with the ionCube PHP Loader (enabled) ...
```

### تست با فایل info.php:
۱. در مسیر اصلی سایت یک فایل با محتوای زیر بسازید:
```
<?php phpinfo(); ?>
```
۲. آدرس آن را در مرورگر باز کنید (مثلاً http://your-domain.com/info.php).

۳. در صفحه، اطلاعات مربوط به ionCube Loader باید نمایش داده شود.

***در پایان فایل info.php را جهت امنیت حذف کنید.***

---

## ۸. تنظیم در پنل‌های مدیریت مانند ISPConfig (در صورت وجود)

۱. وارد پنل ISPConfig شوید.

۲. از بخش Websites/Sites، سایت موردنظر را انتخاب کنید.

۳. در Options یا تنظیمات سایت، نسخه PHP را روی X قرار دهید و تغییرات را ذخیره کنید.

* توجه کنید که Loader باید فقط یک‌بار برای هر نسخه نصب شود و تمام سایت‌ها با انتخاب نسخه جدید، از آن بهره می‌برند.
  
---

## ۹. توصیه‌ها و نکات تکمیلی

- اگر چند نسخه PHP دارید، مراحل نصب Loader را برای هر نسخه به‌صورت جدا انجام دهید.
- در سیستم‌های چندسایتی، فقط کافی است Loader را یک بار برای هر نسخه PHP سرور نصب کنید.
- همیشه برای نسخه جدید PHP، Loader مخصوص همان نسخه را استفاده کنید؛ Loaderهای قدیمی با نسخه جدید کار نمی‌کنند.
- اگر سایت پس از اعمال تغییرات بالا نمی‌آید:
  - مسیر Loader یا ویرایش php.ini را بررسی کنید
  - لاگ‌های مربوط به php-fpm، Apache یا nginx و php error log‌ را بررسی کنید

---

## نمونه جدول مسیرها و فایل Loader برای نسخه‌های متداول

| نسخه PHP | فایل Loader                  | مسیر php.ini                    | مسیر extension_dir                 |
|----------|------------------------------|----------------------------------|------------------------------------|
| 7.4      | ioncube_loader_lin_7.4.so    | /etc/php/7.4/fpm/php.ini         | /usr/lib/php/20190902/             |
| 8.0      | ioncube_loader_lin_8.0.so    | /etc/php/8.0/fpm/php.ini         | /usr/lib/php/20200930/             |
| 8.1      | ioncube_loader_lin_8.1.so    | /etc/php/8.1/fpm/php.ini         | /usr/lib/php/20210902/             |
| 8.2      | ioncube_loader_lin_8.2.so    | /etc/php/8.2/fpm/php.ini         | /usr/lib/php/20220829/             |
