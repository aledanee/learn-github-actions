


# 03 - Advanced Concepts

## الآن دخلنا للمرحلة اللي تبين قوة GitHub Actions الحقيقية 🚀

في هذا الفصل راح نتعرف على:
- الماتريكس (Matrix) لتنفيذ نفس المهمة على أكثر من بيئة
- إعادة استخدام الأكشنات والـ Workflows
- إدارة الأسرار (Secrets) والمتغيرات
- التخزين المؤقت (Caching)

---

## 🧮 Matrix Builds – شغّل مشروعك على أكثر من بيئة

باستخدام خاصية الماتريكس، تقدر تشغّل نفس Job على نسخ مختلفة من Node.js أو على أنظمة تشغيل مختلفة.

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [16, 18, 20]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm install
      - run: npm test
```

هنا راح تنفذ نفس خطوات الاختبار 3 مرات، وحدة لكل نسخة من Node.js.

---

## 🔁 Reusable Workflows – لا تعيد نفس الكود مرتين

من تكرر نفس الـ Workflow في مشاريع متعددة؟ تقدر تفصلها وتعيد استخدامها باستخدام `workflow_call`.

### 1. سوي ملف reusable workflow:

```yaml
# .github/workflows/test-reusable.yml
name: Reusable Tests

on:
  workflow_call:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "Running reusable tests"
```

### 2. استدعِه من مشروع ثاني أو من نفس المشروع:

```yaml
# .github/workflows/main.yml
name: Main Workflow

on: [push]

jobs:
  call-tests:
    uses: ./.github/workflows/test-reusable.yml
```

---

## 🔐 Secrets & Variables – شلون تحمي معلوماتك؟

لا تكتب كلمات مرور أو مفاتيح API في الكود. استعمل GitHub Secrets:

1. افتح Settings → Secrets
2. أضف secret باسم مثل `API_KEY`
3. استخدمه داخل الكود:

```yaml
- name: Call API
  run: curl https://api.example.com --header "Authorization: Bearer ${{ secrets.API_KEY }}"
```

---

## 🧠 Caching – سرّع التنفيذ بتجنب التكرار

لو مشروعك ياخذ وقت لأن كل مرة ينزل dependencies، استخدم التخزين المؤقت:

```yaml
- name: Cache node modules
  uses: actions/cache@v4
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-
```

هذا يقلل الوقت بشكل ملحوظ خصوصًا في المشاريع الكبيرة.

---

## ملخص

| الميزة       | الفائدة |
|--------------|----------|
| Matrix        | اختبار المشروع على بيئات متعددة |
| Reusable Workflows | تقليل التكرار بين المشاريع |
| Secrets       | حماية البيانات الحساسة |
| Caching       | تسريع الأداء وتقليل الوقت |

---

🚀 في الفصل الجاي، راح نشتغل على مشاريع حقيقية ونبني Workflows تنشر موقع أو باقة أو مشروع حقيقي.

جاهز للشغل الواقعي؟ 💪