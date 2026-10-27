# صقر (SAQR)

منصة تجارة محلية وتوصيل، عربية أولاً (RTL)، تعمل كتطبيق PWA على أندرويد.

## التشغيل المحلي

```bash
cp .env.example .env            # ثم ضع SESSION_SECRET (openssl rand -base64 48)
docker compose up -d            # قاعدة PostgreSQL للتطوير فقط
npm install
npx prisma generate
npm run dev
```

أوامر الفحص: `npm run typecheck` · `npm run lint` · `npm test` · `npm run db:validate` · `npm run audit`

## هيكل المشروع

- `src/app` الصفحات والتخطيط والـ manifest
- `src/components/ui` المكونات المشتركة (Button, Field, Badge, Card)
- `src/lib` المنطق المشترك: المبالغ (`money.ts`) والبيئة (`env.ts`) والأمان (`security/`)
- `prisma/schema.prisma` قاعدة البيانات
- `public/sw.js` خدمة offline آمنة (لا تخزّن أي بيانات مستخدمين)

## قواعد لا تتغير

1. كل المبالغ أرقام صحيحة بالقروش. ممنوع Float.
2. كل صلاحية وكل قاعدة مالية تُفرض في السيرفر، مهما كان شكل الواجهة.
3. أي قيمة تجارية (عمولة، مهلة، رسوم) تأتي من قاعدة البيانات وتُعدَّل من لوحة الأدمن.
4. سجل النجوم وسجل المراجعة للإضافة فقط.
5. أسرار البيئة لا تُرفع على Git أبداً.

## الأمان المطبّق في المرحلة 0

- CSP صارمة بـ nonce لكل طلب (`src/middleware.ts`) بدون `unsafe-inline` للسكربتات في الإنتاج
- HSTS وX-Frame-Options وnosniff وReferrer-Policy وPermissions-Policy وCOOP
- التحقق من متغيرات البيئة عند التشغيل (`zod`) وفشل فوري عند الخطأ
- قواعد ESLint تمنع `eval` و`dangerouslySetInnerHTML` و`any`
- خدمة PWA لا تخزّن إلا صفحة offline
- CI يشغّل الفحوصات وفحص الثغرات في الحزم، وDependabot للتحديثات

## حالة المراحل

- [x] 0 الأساس والهوية
- [ ] 1 الحسابات والصلاحيات (OTP، جلسات، RBAC، 2FA للأدمن)
- [ ] 2 إعدادات المنصة · 3 التجار والكتالوج · 4 تجربة العميل · 5 النجوم
- [ ] 6 الطلبات والدفع · 7 الاسترجاع · 8 التوصيل · 9 الإشعارات
