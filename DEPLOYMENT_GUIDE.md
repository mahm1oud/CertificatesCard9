# دليل نشر التطبيق على استضافة Hostinger

## مقدمة

هذا الدليل يشرح كيفية نشر تطبيق منصة الشهادات والبطاقات الإلكترونية على استضافة Hostinger. تتميز هذه الاستضافة بدعمها لـ PHP و MySQL، وهي مناسبة للمواقع ذات حركة المرور المتوسطة.

## متطلبات النشر

1. خطة استضافة Hostinger تدعم PHP 8.0+ و MySQL
2. نطاق مخصص للموقع (Domain)
3. حساب FTP/SFTP للوصول إلى المخدم

## الخطوات التحضيرية

### 1. بناء التطبيق للنشر

1. قم بتنفيذ سكريبت البناء الشامل:
```bash
chmod +x build-all.sh
./build-all.sh
```

2. سيقوم السكريبت تلقائياً بما يلي:
   - بناء الواجهة الأمامية (Frontend)
   - بناء الواجهة الخلفية (Backend)
   - نسخ الملفات اللازمة إلى مجلد `build`
   - إنشاء ملفات التثبيت PHP

3. تأكد من وجود جميع الملفات المطلوبة في مجلد `build`:
   - مجلد `client` (يحتوي على الواجهة الأمامية)
   - مجلد `server` (يحتوي على الواجهة الخلفية)
   - مجلد `install` (يحتوي على ملفات التثبيت)
   - مجلد `uploads` و `temp` و `fonts`
   - ملف `.htaccess`
   - ملف `index.php`

## خطوات النشر

### 1. رفع الملفات إلى الخادم

1. قم بتسجيل الدخول إلى حساب Hostinger الخاص بك
2. قم بالوصول إلى مدير الملفات أو استخدم FTP/SFTP
3. قم برفع جميع محتويات مجلد `build` إلى المجلد الرئيسي `public_html`

### 2. إنشاء قاعدة بيانات MySQL

1. من لوحة تحكم Hostinger، انتقل إلى قسم "قواعد البيانات"
2. أنشئ قاعدة بيانات MySQL جديدة واحتفظ بما يلي:
   - اسم قاعدة البيانات
   - اسم المستخدم
   - كلمة المرور
   - اسم المضيف (عادة `localhost`)

### 3. تثبيت التطبيق

1. قم بزيارة الموقع الخاص بك عبر المتصفح
2. سيتم توجيهك تلقائياً إلى صفحة التثبيت (`install/installer.php`)
3. اتبع خطوات التثبيت:
   - أدخل بيانات قاعدة البيانات التي أنشأتها في الخطوة السابقة
   - قم بإعداد معلومات الموقع
   - قم بإعداد حساب المدير (أو احتفظ بالحساب الافتراضي: admin/700700)

4. بعد اكتمال التثبيت، ستتم إعادة توجيهك إلى الموقع الرئيسي

## الإعدادات المتقدمة

### 1. تكوين البريد الإلكتروني

إذا كنت ترغب في استخدام وظائف البريد الإلكتروني (إعادة تعيين كلمة المرور، إشعارات، إلخ):

1. من لوحة تحكم Hostinger، قم بإنشاء عنوان بريد إلكتروني للنطاق الخاص بك
2. قم بتعديل ملف `.env` في المجلد الرئيسي:
```
MAIL_HOST=mail.yourdomain.com
MAIL_PORT=587
MAIL_USERNAME=noreply@yourdomain.com
MAIL_PASSWORD=your-email-password
MAIL_FROM=noreply@yourdomain.com
MAIL_FROM_NAME="Certificates System"
```

### 2. تكوين HTTPS

لتأمين موقعك باستخدام HTTPS:

1. من لوحة تحكم Hostinger، قم بتفعيل شهادة SSL (مجانية عادة)
2. تأكد من أن جميع روابط الموقع تستخدم `https://` بدلاً من `http://`

### 3. تعديل حدود التحميل

إذا كنت بحاجة إلى رفع ملفات كبيرة:

1. قم بإنشاء ملف `php.ini` في المجلد الرئيسي:
```
upload_max_filesize = 32M
post_max_size = 32M
max_execution_time = 300
```

## استكشاف الأخطاء وإصلاحها

إذا واجهت مشاكل بعد التثبيت:

1. **مشاكل قاعدة البيانات**: تأكد من صحة بيانات قاعدة البيانات في ملف `.env`
2. **مشاكل التصاريح**: تأكد من أن المجلدات التالية لديها تصاريح كتابة (`755` أو `775`):
   - `/uploads`
   - `/temp`
3. **مشاكل الواجهة الأمامية**: تحقق من سجلات الأخطاء في المتصفح (F12 > Console)
4. **مشاكل PHP**: تحقق من سجلات PHP في لوحة تحكم Hostinger

## النسخ الاحتياطي

من المهم إجراء نسخ احتياطي منتظم لموقعك:

1. **قاعدة البيانات**: استخدم أداة phpMyAdmin من لوحة تحكم Hostinger لتصدير قاعدة البيانات
2. **الملفات**: استخدم FTP أو مدير الملفات لتنزيل نسخة من جميع ملفات الموقع
3. **الإعدادات**: احتفظ بنسخة من ملف `.env` في مكان آمن

## تحديث النظام

عندما تريد تحديث النظام:

1. قم ببناء الإصدار الجديد باستخدام `./build-all.sh`
2. قم بعمل نسخة احتياطية من الموقع الحالي وقاعدة البيانات
3. استبدل الملفات القديمة بالملفات الجديدة، مع الحفاظ على:
   - مجلد `uploads`
   - ملف `.env`
4. قم بزيارة `your-domain.com/install/update.php` لتحديث قاعدة البيانات

## الملاحظات النهائية

- تأكد من تغيير كلمة مرور المستخدم الافتراضي `admin` بعد التثبيت
- قم بتعطيل وصول المجلد `install` بعد اكتمال عملية التثبيت
- قم بالتحقق من توافق الخوادم والإعدادات مع متطلبات النظام

---

إذا واجهت أي مشاكل أو كان لديك أسئلة حول عملية النشر، يرجى التواصل مع فريق الدعم.