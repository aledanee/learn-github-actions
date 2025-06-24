# 05 - Troubleshooting

## لأن المشاكل جزء من كل مشروع... 🛠️

بغض النظر عن شطارتك، راح تواجه أخطاء في GitHub Actions – خصوصًا بالبداية. هذا الفصل يعلمك شلون تقرأ رسائل الخطأ، تشخّص المشكلة، وتصلحها بثقة.

---

## 🧯 أكثر الأخطاء شيوعًا

### ❌ Syntax Error in YAML
إذا شفت خطأ مثل:

```
YAMLException: bad indentation of a mapping entry
```

🔧 هذا يعني عندك مشكلة بالـ spacing. تأكد تستخدم **مسافات وليس Tabs**، وكل مستوى داخل YAML متناسق (عادة 2 مسافة).

---

### ❌ Missing or Invalid Secrets

```
Error: Process completed with exit code 1
Error: npm ERR! Missing authentication token
```

🔧 تأكد أنك ضايف Secret بالاسم الصحيح داخل إعدادات المستودع. وأيضًا أنك كاتب `secrets.NAME` بالضبط.

---

### ❌ Action أو Job ما اشتغل

```
job was not triggered because it does not have a matching on condition
```

🔧 شوف إذا الحدث (`on:`) يطابق اللي يصير فعلًا بالمستودع. إذا workflow مخصص لـ `push` وانت سويت `pull_request`، ما راح يشتغل.

---

## 🔎 كيف تقرأ Log التنفيذ؟

كل Job يحتوي Logs مفصلة:

1. افتح تبويب **Actions** بالمستودع.
2. اختر الـ Workflow اللي اشتغل.
3. شوف كل خطوة ونتيجتها.

💡 تقدر تضيف أوامر `echo` داخل `run:` حتى تطبع معلومات وتفهم وين المشكلة.

```yaml
- run: |
    echo "Starting install"
    npm install
    echo "Finished install"
```

---

## 🧪 استخدم `debug` لتفاصيل إضافية

لما تكون المشكلة غامضة، فعّل debug mode:

1. روّح لـ [GitHub Settings > Actions > Secrets](https://github.com/settings/secrets)
2. أضف secret اسمه: `ACTIONS_STEP_DEBUG` وقيمته `true`

راح يطبع كل تفصيلة ممكنة داخل الخطوات.

---

## 🛠️ أدوات تساعدك على التصحيح

| أداة / تقنيّة | فائدة |
|----------------|--------|
| `echo`         | طباعة معلومات داخل كل خطوة |
| `continue-on-error: true` | يسمح بتخطي الأخطاء لمتابعة باقي الخطوات |
| GitHub Logs    | تعرض كل تفاصيل التنفيذ بدقة |
| Secrets Log    | يكشف إذا كان secret مش موجود أو مكتوب غلط |
| Action Versions| تأكد أنك تستخدم آخر نسخة من كل action |

---

## ملخص

👨‍🔧 الأخطاء طبيعية، واللي يفرّق بين المبتدئ والمحترف هو "شلون يتعامل وياها":

- اقرأ الـ logs بعناية.
- جرّب تطبع كل خطوة.
- لا تتردد تستخدم debug mode.
- تأكد من كتابة YAML بشكل صحيح دائمًا.

---

🎯 تهانينا! الآن أنت تقدر تبني، تنفّذ، تصلّح، وتطوّر أي Workflow باستخدام GitHub Actions.

لو تحب نكمل بدليل نشر أو نسخة PDF أو GitBook – فإحنا جاهزين!

Keep building with confidence 🚀
