# 04 - Real Projects

## حان وقت التطبيق العملي! 🛠️

بعد ما فهمنا الأساسيات والمفاهيم المتقدمة، الآن راح نشتغل على مشاريع حقيقية باستخدام GitHub Actions.

كل مشروع راح يحتوي:
- شرح للفكرة
- ملف Workflow
- ملاحظات للتخصيص

---

## 📦 مشروع 1: نشر باقة npm تلقائيًا

### الفكرة
كل ما تسوي `push` لفرع `main` مع وجود تغييرات في النسخة (package.json)، يتم نشر الباقة على npm تلقائيًا.

### المتطلبات
- يكون عندك حساب على [npmjs.com](https://www.npmjs.com)
- تولد توكن API وتضيفه إلى GitHub Secrets باسم `NPM_TOKEN`

### الـ Workflow

```yaml
name: Publish to npm

on:
  push:
    branches: [main]

jobs:
  publish:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 20
          registry-url: 'https://registry.npmjs.org'

      - run: npm install
      - run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

---

## 🌐 مشروع 2: نشر موقع على GitHub Pages

### الفكرة
تستخدم GitHub Actions لتحويل ملفات HTML أو مشروع React/Vite إلى GitHub Pages.

### التهيئة
- تأكد أن الفرع `gh-pages` هو فرع النشر.
- فعّل GitHub Pages من إعدادات المستودع.

### الـ Workflow

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Build static files
        run: |
          npm install
          npm run build

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

---

## 🧪 مشروع 3: اختبار تلقائي مع إرسال تنبيه إلى Discord

### الفكرة
تشغل اختبارات المشروع بعد كل Push، وإذا فشلت ترسل تنبيه إلى Discord.

### المتطلبات
- أنشئ Webhook في خادم Discord
- أضفه كـ Secret باسم `DISCORD_WEBHOOK`

### الـ Workflow

```yaml
name: Test and Notify

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm test

  notify:
    if: failure()
    runs-on: ubuntu-latest
    needs: test

    steps:
      - name: Send Discord alert
        run: |
          curl -H "Content-Type: application/json" \
          -X POST \
          -d '{"content":"🚨 Tests failed in repo!"}' \
          ${{ secrets.DISCORD_WEBHOOK }}
```

---

## نصائح للتخصيص

- تقدر تربط الـ Workflows مع أي API أو خدمة عبر curl أو actions مخصصة.
- تأكد دائمًا من فصل بياناتك الحساسة باستخدام Secrets.
- جرّب دمج أكثر من Job في Workflow واحد.

---

🔥 في الفصل الجاي، راح نتعلم شلون نحل المشاكل ونفهم رسائل الأخطاء ونستخدم أدوات التصحيح داخل GitHub Actions.

جاهز تتعلم شلون تتعامل مع المشاكل بثقة؟ 🧠
