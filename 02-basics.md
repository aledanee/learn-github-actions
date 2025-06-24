


# 02 - Basics

## قبل لا نبدي: شنو يعني Workflow فعليًا؟

Workflow هو ملف بصيغة YAML موجود داخل `.github/workflows` بمشروعك. هذا الملف هو قلب GitHub Actions – به تحدد شنو تريد يصير، ومتى يصير، وعلى أي منصة يصير.

مثال بسيط:
- لما يصير Push على فرع `main`
- خلّينا نثبّت Node.js
- ونشغل أوامر الاختبار

---

## هيكل أي Workflow

كل Workflow يتكون من:

1. `name:` – اسم تعريفي للعملية (اختياري لكنه مفيد)
2. `on:` – الحدث اللي يفعّل الـ Workflow (مثلاً `push`, `pull_request`, أو وقت محدد `schedule`)
3. `jobs:` – مجموعة من الوظائف (Jobs) اللي راح تتنفّذ
4. `steps:` – كل Job يحتوي خطوات (Steps)، كل وحدة تنفّذ أمر أو Action

---

## مثال عملي

```yaml
name: Node CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

هذا الـ Workflow:
- يشتغل كل ما يصير Push
- يشتغل على Linux (Ubuntu)
- ينفذ الخطوات: يسحب الكود، يثبت Node، ينفذ أوامر `npm`

---

## شرح للمصطلحات المهمة

| المصطلح         | المعنى |
|------------------|--------|
| `runs-on`        | نظام التشغيل اللي راح يشغّل الـ Job (Ubuntu, Windows, macOS) |
| `uses:`          | Action جاهز من Marketplace |
| `run:`           | أمر تكتبه بنفسك للتنفيذ في سطر الأوامر |
| `with:`          | مدخلات أو إعدادات إضافية لـ Action |

---

## شنو الفرق بين Job و Step؟

- **Step:** خطوة واحدة داخل Job (مثل سطر `npm install`)
- **Job:** تجمّع خطوات تنفذ على نفس البيئة (Runner)
- تقدر تسوي أكثر من Job، وكل Job يشتغل بشكل مستقل (أو يعتمد على Job ثاني)

---

## أين أضع ملفات الـ Workflows؟

ببساطة داخل المجلد التالي في مشروعك:

```
.github/workflows/
```

سوي ملف جديد مثل:  
```
.github/workflows/main.yml
```

وبداخله تكتب الكود مثل الأمثلة السابقة.

---

## اختبر نفسك 💡

- هل تقدر تكتب Workflow ينفّذ `echo "Hello World"` كل ما يصير Push؟
- هل تعرف تضيف أكثر من Job؟
- هل فكرت تسوي Workflow يتكرر كل يوم باستخدام `schedule`؟

إذا قدرت تجاوب على هاي الأسئلة، فأنت فهمت الأساسيات! 🎉

---

🚀 في الفصل الجاي، راح نغوص بأشياء أعمق مثل Matrix builds، وإعادة استخدام الأكشنات، واستخدام الأسرار (Secrets).

Ready to go deeper؟