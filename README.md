# دليل الكليات | DALIL COLLEGES

منصة عربية RTL للتعريف بالمؤسسات التعليمية السودانية وتجارب الطلاب. هذا المستودع يحتوي على واجهة React/TypeScript، وCloudflare Worker API، وقاعدة D1، ومصدر البيانات والاستبيان المرفقين.

> **حالة المشروع:** تم تنفيذ نسخة MVP وظيفية على مستوى الواجهة وواجهات API الأساسية، لكنها ليست جاهزة للإنتاج. لم يتم تثبيت حزم npm أو إكمال `vite build` وTypeScript typecheck بسبب انتهاء مهلة الاتصال في بيئة التنفيذ. كما أن موارد Cloudflare وGoogle OAuth لم تُنشأ أو تُربط. راجع `docs/IMPLEMENTATION_STATUS.md` للحدود التفصيلية.

## المتطلبات
- Node.js 20+ وnpm 10+
- حساب Cloudflare عند ربط الخدمات
- Google OAuth Client ID/Secret عند تفعيل تسجيل الدخول

## التشغيل المحلي
```bash
npm install
cp .env.example apps/api/.dev.vars
cp .env.example apps/web/.env
npm run db:migrate:local
# استيراد البيانات المرحلية اختيارياً إلى قاعدة محلية جديدة بعد مراجعتها:
# wrangler d1 execute dalil-colleges-db --local --config apps/api/wrangler.toml --file=imports/staging-import.sql
npm run dev:api
# في نافذة طرفية ثانية
npm run dev
```

`npm test` يشغّل اختبارات Python. شغّل أيضاً `npm run build` بعد تثبيت الحزم؛ لا تعتبر نجاح اختبارات Python بديلاً عن بناء الواجهة وفحص الأنواع.

## البيانات
- ملف Excel الأصلي محفوظ خارج حزمة التطبيق في المحادثة، والنتائج المستخرجة داخل `imports/`.
- ملف HTML الأصلي في `imports/source/coc3.html`.
- تعريف الاستبيان المستخرج في `apps/web/src/survey-source.json`.
- بيانات Excel: 161 جهة و2,339 اسماً تابعاً. تُدخل كل الجهات `unknown/draft/needs_review`، وتُدخل الوحدات `unclassified/draft/needs_review`. لا يوجد نشر أو توثيق تلقائي.
- migration `0007_full_survey_source.sql` تنشئ نسخة استبيان كاملة من 57 حقلاً/سؤالاً و171 خياراً، وتبقيها `draft` حتى مراجعتها ونشرها من مدير مخوّل.

راجع `docs/DATA_IMPORT.md` قبل استيراد أي بيانات.

## الصلاحيات والإعداد الإداري
لا يُمنح أي مستخدم صلاحية إدارية عند التسجيل. سجّل دخول الحساب الإداري المراد اعتماده أولاً، ثم اتبع `docs/ADMIN_BOOTSTRAP.md` لإسناد دور المدير العام يدوياً على قاعدة D1 الجديدة بعد التحقق من البريد.

## التوثيق
- `docs/ARCHITECTURE.md`
- `docs/API.md`
- `docs/GOOGLE_OAUTH.md`
- `docs/CLOUDFLARE.md`
- `docs/DATA_IMPORT.md`
- `docs/IMPLEMENTATION_STATUS.md`
- `docs/ADMIN_BOOTSTRAP.md`

## حدود الأمان والنشر
لا تضع أسرار OAuth في الواجهة أو Git. لا تنفذ migrations على قاعدة remote إلا بعد التأكد من اسم قاعدة D1 وأنها قاعدة جديدة مخصصة للمشروع. لم يتم تنفيذ نشر أو تغيير على قواعد خارجية في هذه الحزمة.
