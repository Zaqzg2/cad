# Cady Sales App — مشروع Bubblewrap (TWA)

هذا المشروع يبني تطبيق أندرويد (APK) يغلّف تطبيق الويب المستضاف على:
https://zaqzg2.netlify.app

## قبل الرفع لأول مرة

### 1. أضف Secrets في مستودع GitHub
اذهب إلى: `Settings → Secrets and variables → Actions → New repository secret`

أضف 3 secrets:

| الاسم | القيمة |
|---|---|
| `ANDROID_KEYSTORE_BASE64` | محتوى ملف `keystore.base64.txt` بالكامل (نسخ ولصق) |
| `ANDROID_KEYSTORE_PASSWORD` | `CadySales2026` |
| `ANDROID_KEY_PASSWORD` | `CadySales2026` |

⚠️ **لا ترفع** `salesapp.keystore` أو `keystore.base64.txt` إلى GitHub أبدًا — هذي الملفات فيها مفتاح التوقيع السري. ملف `.gitignore` جاهز يمنع رفعها تلقائيًا، لكن لا تحذف هذا الاستثناء.

### 2. ارفع المشروع
```bash
git init
git add -A
git commit -m "initial bubblewrap setup"
git branch -M main
git remote add origin https://github.com/USERNAME/REPO.git
git push -u origin main
```

### 3. شغّل البناء
اذهب لتبويب **Actions** في المستودع → اختر workflow اسمه **Build Signed APK** → **Run workflow**.

بعد ما يخلص (يأخذ عادة 5-10 دقائق أول مرة)، حمّل الملف من قسم **Artifacts** في نفس صفحة الـ run — راح تلقى `CadySalesApp-release.zip` بداخله `app-release-signed.apk`.

## ملاحظات مهمة

- **بصمة التوقيع (SHA256) ثابتة** لأننا استخدمنا نفس `salesapp.keystore` القديم، فملف `assetlinks.json` المنشور على Netlify ما يحتاج تغيير.
- تم إضافة خطوة تلقائية في الـ workflow تتأكد من وجود إذن `android.permission.INTERNET` وتضيفه لو كان ناقص (وهذا كان سبب مشكلة "يتثبت ولا يفتح" سابقًا).
- ثبّت الـ APK الجديد فوق القديم مباشرة (نفس التوقيع)، أو احذف القديم أولاً لو واجهت مشكلة تعارض توقيع.
