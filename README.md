# Codera Tech — موقع الأكاديمية

موقع تعريفي لأكاديمية **Codera Tech (أكاديمية قدرة تك)** في دمنهور - إيتاي البارود، مصر.
الموقع **ملف HTML واحد** (HTML + CSS + JavaScript) من غير build ولا frameworks ولا dependencies، ما عدا خطين من Google Fonts.
عربي (RTL) هو الأساسي، والإنجليزي (LTR) متاح بزرار تبديل.

> **لو إنت AI Agent:** اقرأ القسم [2. قواعد الشغل](#2-قواعد-الشغل-إلزامية) و[3. مناطق الخطر](#3-مناطق-الخطر-ما-تلمسهاش-من-غير-فهم) قبل ما تعدّل أي حاجة. المحتوى كله في `CMS`، والتصميم في `<style>`، والمنطق في `<script>`، وكل جزء ليه قواعد مختلفة.

---

## الفهرس

1. [نظرة سريعة](#1-نظرة-سريعة)
2. [قواعد الشغل (إلزامية)](#2-قواعد-الشغل-إلزامية)
3. [مناطق الخطر](#3-مناطق-الخطر-ما-تلمسهاش-من-غير-فهم)
4. [هيكل الملف](#4-هيكل-الملف)
5. [طبقة المحتوى `CMS`](#5-طبقة-المحتوى-cms)
6. [نظام التصميم](#6-نظام-التصميم)
7. [التفاعل الرئيسي (Signature)](#7-التفاعل-الرئيسي-signature)
8. [الصفحات والـ Router](#8-الصفحات-والـ-router)
9. [الحجز والقارئ والمناسبات](#9-الحجز-والقارئ-والمناسبات)
10. [الـ Responsive: القواعد وسجل الـ patches](#10-الـ-responsive-القواعد-وسجل-الـ-patches)
11. [الاختبار](#11-الاختبار)
12. [مشاكل معروفة](#12-مشاكل-معروفة-لسه-ما-اتصلحتش)
13. [مهام شائعة (وصفات)](#13-مهام-شائعة-وصفات)
14. [قبل الإطلاق](#14-قبل-الإطلاق)

---

## 1. نظرة سريعة

| البند | التفاصيل |
|---|---|
| النوع | Single-file SPA، `index.html` واحد |
| التقنية | Vanilla JS، CSS variables، `color-mix()`، `container queries` |
| الـ Routing | بالـ hash: `#/`، `#/courses/python-fundamentals`، إلخ |
| اللغات | عربي (افتراضي، `dir="rtl"`) وإنجليزي (`dir="ltr"`)، بيتحفظ في `localStorage` (`codera-lang`) |
| الثيم | فاتح/داكن، بيتحفظ في `localStorage` (`codera-theme`) |
| الخطوط | Readex Pro (نص) + IBM Plex Mono (كود)، من Google Fonts |
| الحجز | 3 خطوات؛ من غير endpoint بينتهي برسالة WhatsApp جاهزة |
| المحتوى | كله جوه `const CMS = {...}`، مجهّز لربطه بلوحة تحكم لاحقًا |

**ملف واحد معناه:** أي تعديل بيلمس نفس الملف، فالانضباط في الـ diffs مهم جدًا. اشتغل بتعديلات صغيرة ومحددة، وما تعيدش كتابة الملف كله.

---

## 2. قواعد الشغل (إلزامية)

دي القواعد اللي اتبنى عليها المشروع، وهي سبب إن الـ prototype المعتمد لسه سليم.

### 2.1 قواعد عامة (لأي تعديل)

1. **ما تحذفش ولا تعدّل HTML/CSS/JS موجود من غير سبب مكتوب.** الأفضل إضافة جديدة في مكانها الصح.
2. **ما تغيّرش أي نص أو محتوى** إلا لو الطلب صريح. النصوص العربي والإنجليزي بتتعدّل مع بعض (`D(ar, en)`)، ومفيش نص بيتغير في لغة بس.
3. **ما تخترعش محتوى.** مفيش شهادات طلاب، ولا أرقام، ولا أسماء مدرّبين حقيقيين، ولا إحصائيات متتألفش. أي حاجة مؤقتة تتعلّم `demo: true` (بتظهر بـ tag "تجريبي" طالما `settings.demo = true`).
4. **ما تضيفش features أو animations جديدة من دماغك.** لو شايف إضافة ضرورية، اقترحها واستنى موافقة.
5. **ما تستخدمش `!important`** إلا لو مضطر، واكتب السبب في تعليق جنبه. (حاليًا مفيش ولا واحد في الكود.)
6. **أي تعديل بيمس منطق JavaScript** (خصوصًا الـ Signature) لازم يتقال صراحة قبل التنفيذ، وسببه يتكتب.

### 2.2 قواعد الـ Responsive

1. التعديل بيكون **بإضافة كتلة `@media` جديدة في آخر `<style>`** (بعد آخر patch)، مش بتعديل قاعدة موجودة.
2. **الـ desktop (>1120px) ما يتلمسش.** كل كتلة جديدة لازم يبقى فيها `max-width` أقل من 1120.
3. كتل الـ patch بتكسب القديمة **بالترتيب** (نفس الـ specificity)، فالترتيب جزء من الكود. ما تنقلش كتلة من مكانها.
4. لو قاعدة قديمة أقوى (specificity أعلى)، ارفع الـ specificity في الكتلة الجديدة (زي `.btn.sm.nav-cta`) بدل `!important`.
5. **ما تثقش في غياب الـ scrollbar.** `body{overflow-x:hidden}` بيخبّي أي overflow أفقي. قيس `scrollWidth` (انظر [الاختبار](#11-الاختبار)).
6. كل رقم في تقرير بيتحسب من الكود هو **تقدير**، ولازم يتقاس في DevTools بعد التنفيذ.

### 2.3 طريقة الشغل المتفق عليها (للمهام الكبيرة)

1. **تقرير قبل التعديل:** كل مشكلة، سببها من الكود، والـ selector/السطر.
2. **اقتراح الحل** (ولو فيه بدائل، قارنها واختار).
3. **انتظر الموافقة.**
4. **نفّذ** داخل `@media` فقط.
5. **اكتب جدول تسليم:** `selector | قبل | بعد | ليه`.
6. لو العرض أو الارتفاع أو اللغة مش معروفين، اسأل أو اطلب صورة وقياسات، ولا تخمّن.

---

## 3. مناطق الخطر (ما تلمسهاش من غير فهم)

### 3.1 الـ Signature (`mountSignature` + `.sig` CSS)

ده أحسن جزء في الموقع وأهشّه. الـ JS بيحسب أحجام وأماكن بناءً على قيم **مكتوبة بالرقم في الاتجاهين**. لو غيّرت واحد من غير التاني، الكود هيتغطى أو يتقص.

| القيمة | في CSS | في JS | لو اتغيّرت |
|---|---|---|---|
| ارتفاع شريط التبويبات | `.sig .bar{height:calc(38px * var(--chrome))}` | `barH = 38 * chrome` | ارتفاع الـ body يتحسب غلط |
| ارتفاع الـ terminal | `.sig .term{height:calc(56px * var(--term))}` | `termH = 56 * term` | نفس المشكلة |
| `line-height` للكود | `.sig .ln{line-height:1.75}` | `lh = fs * 1.75` | الأسطر تتزحزح |
| حد الموبايل | `@media (max-width:860px)` | `matchMedia('(max-width: 860px)')` | أحجام الخط غلط |
| مساحة الـ hint | `.sig .hint{...}` | `reserve = lerp(0, 46, s1)` | الـ hint يغطي الكود |
| طول التراك | `.sig{height:560svh}` | الـ timeline (قيم من 0 لـ 1) | التوقيتات كلها تتغير |

**قاعدة المتغيرات:** الـ JS بيكتب CSS variables على `documentElement` (`--d, --chrome, --frac, --prev, --site, --tabs, --url, --term, --t1, --t2, --t3, --skill`). أي متغير جديد يتحكم فيه JS لازم:
- يتضاف لمصفوفة `VARS` في `mountSignature` (عشان يتنضّف في `destroy()`).
- يتضاف له default في `:root` (انظر [المشاكل المعروفة](#12-مشاكل-معروفة-لسه-ما-اتصلحتش)، `--skill` لسه ناقص).

### 3.2 الـ `CMS` object
هيكله مستخدم في كل الصفحات. ما تغيّرش أسماء الحقول (`slug`, `cat`, `stage`, ...) من غير ما تدوّر على استخداماتها كلها. الحقول الاختيارية بتتعامل معها الصفحات بأمان (`c.age && ...`)، فالإضافة آمنة والحذف خطر.

### 3.3 الـ Router والـ modals
`render()` و`openModal()` و`openBooking()` و`openReader()` مترابطين (focus trap، `html.lock`، `onLeave`). أي تعديل فيهم يتختبر بالكيبورد (Tab / Esc).

---

## 4. هيكل الملف

الترتيب جوه `<script>` (ابحث عن الـ banner comments):

| # | القسم | بيعمل إيه |
|---|---|---|
| 1 | `CODERA TECH — CONTENT LAYER` | `const CMS` كل المحتوى + `D()` و`day()` |
| 2 | `CORE` | `L()`، `t()`، `esc()`، `vis()`، الأيقونات `ICONS`/`ico()`، `ph()` (placeholder)، `setMeta()` (SEO)، `openModal()` |
| 3 | `CHAPTER 01 — SIGNATURE` | `sigHTML()` و`mountSignature()` |
| 4 | `HOMEPAGE CHAPTERS` | كائن `Home` (كل فصل دالة بترجع HTML أو `''`) |
| 5 | `PAGES` | `pageHome/About/Courses/Course/Services/Instructors/Instructor/Events/Event/Guides/Guide/Contact/404` |
| 6 | `BOOKING` | الحجز بـ 3 خطوات |
| 7 | `READER` | قارئ الأدلة |
| 8 | `APP` | المناسبات، `renderChrome()`، الـ router، `updateUI()`، الـ event delegation، `init()` |

وجوه `<style>` (الترتيب مهم):

1. Design tokens و`:root` و tones
2. base، type، icons، buttons، small pieces، forms
3. navigation، footer، celebration، booking dialog، guide cover، reader
4. `SIGNATURE INTERACTION`
5. `HOMEPAGE CHAPTERS` (02 → 10)
6. `INNER PAGES`
7. `prefers-reduced-motion`
8. **Responsive patches P1 → P13** (آخر الملف، انظر [القسم 10](#10-الـ-responsive-القواعد-وسجل-الـ-patches))

---

## 5. طبقة المحتوى `CMS`

### 5.1 قواعد عامة لأي قائمة

- `visible: false` يخفي العنصر. `order: n` يرتّبه (الأصغر أولًا؛ لو مفيش order بيتبع ترتيب الكتابة). ده كله عن طريق `vis()`.
- أي نص ثنائي اللغة يتكتب بـ `D('عربي', 'English')`، وبيتقرا بـ `T(v)` (مع escape) أو `t(v)` (من غير escape).
- الصور والفيديو كلها `null` دلوقتي، والـ `ph()` بيعرض placeholder مصمّم لحد ما يتحط URL.
- `demo: true` يظهر tag "تجريبي" لو `settings.demo = true`.

### 5.2 الأقسام

| المفتاح | الوظيفة |
|---|---|
| `settings` | `demo`، `siteUrl`، `urlPattern`، `brand`، `name`، `tagline`، `logo`، `booking.endpoint` |
| `contact` | `phones[]` (الشكل المحلي `010...`)، `email`، `address`، `whatsapp`، `mapsUrl`، `social[]` (`id` لازم يكون موجود في `ICONS`) |
| `nav` | عناصر القائمة؛ `needs: 'courses'` يخفي العنصر لو القائمة دي فاضية |
| `sections` | فصول الصفحة الرئيسية: `id`، `visible`، `order`. فصل `happening` عنده `empty: 'state' \| 'hide'` |
| `celebration` | `{ active, text }` (انظر [المناسبات](#93-المناسبات)) |
| `copy` | عناوين وفقرات الفصول (path، doing، people، community، stories، library، happening) |
| `why` | فصل "مش بنعلّمك تكتب Code بس": `points[]` و`diff[]` (أزواج قبل/بعد) |
| `categories` | `id`، `label`، `color` |
| `stages` | مراحل المسار البصري (`start → foundation → build → specialize → grow`)؛ المرحلة الفاضية بتختفي |
| `courses` | الكورسات (انظر أسفل) |
| `services` | الخدمات؛ ليها `courses[]` أو `href` |
| `instructors` | المدرّبون |
| `events` | الفعاليات |
| `moments` | لقطات المجتمع (mosaic) |
| `stories` | قصص الخريجين |
| `guides` | الأدلة |
| `about` | من نحن، رؤية، رسالة، فلسفة |
| `branches` / `modes` / `levels` | قوائم تستخدمها استمارة الحجز والفلاتر |

### 5.3 حقول العناصر

**course**
`slug` (فريد، حروف إنجليزي وشرطات)، `cat`، `stage`، `level` (`beginner|intermediate|advanced`)، `featured`، `title`، `summary`، `leadsTo[]` (slugs).
اختياري لصفحة التفاصيل: `age`، `mode`، `duration`، `sessions` (كلها `D()`)، `description`، `learn[]`، `audience[]`، `outcomes[]`، `curriculum[{t,d}]`، `projects[]`، `faq[{q,a}]`.

**instructor**
`slug`، `featured`، `order`، `name`، `role`، `photo`، `expertise[]`، `courses[]` (slugs)، `bio`، `social[{id,label,url}]`، `demo`.

**event**
`slug`، `type`، `date` (ISO string)، `title`، `location`، `summary`، `description`، `cover`، `gallery[]`، `featured`، `demo`.
القادم أو الماضي بيتحدد بمقارنة `date` بالوقت الحالي. الدالة `day(n)` خاصة بالـ demo فقط، والتواريخ الحقيقية تتكتب ISO ثابتة.

**guide**
`slug`، `tone` (`blue|ink|slate`)، `category`، `title`، `description`، `outline[]`، `cover` (صورة؛ لو `null` بيتولّد غلاف CSS)، `file` (رابط تحميل)، `pages[]` (**روابط صور** لصفحات القارئ، مش PDF)، `pageCount`، `language` (`ar|en`).

**story**
`slug`، `name`، `course` (slug)، `quote`، `video`، `image`، `achievement`، `demo`. **ممنوع اختراع اقتباسات أو إنجازات.**

**moment**
`type`، `title`، `media` (وصف الـ placeholder)، `img`.

### 5.4 ملاحظة عن نبرة النصوص (ملاحظة، مش قاعدة)
النصوص الحالية بتمزج العامية المصرية في الدعوات للتفاعل ("تحب تتعلّم إيه؟") والفصحى في الأقسام الرسمية (رؤية/رسالة/أدلة). حافظ على نفس الأسلوب في المكان اللي بتضيف فيه. الإنجليزي بسيط ومباشر.

---

## 6. نظام التصميم

### 6.1 الألوان (قواعد الاستخدام)

| Token | القيمة | الاستخدام |
|---|---|---|
| `--brand` | `#0074D9` (Codera Blue) | أزرار الإجراء، العلامة |
| `--ink` / `--deep` | نيفي داكن | العمق والخلفيات الداكنة |
| `--grow` | `#FFB43A` (عنبر) | **للتقدّم والنجاح فقط** (شرائط التقدم، أيقونة تم، نقطة "منشور"). ما تستخدمهوش كلون زخرفة. |
| `#128C4A` | أخضر واتساب | أزرار WhatsApp فقط |

### 6.2 الـ Tones (إيقاع الصفحة)
كل فصل أو قسم بيختار tone بـ class: `tone-paper` / `tone-paper2` / `tone-ink` / `tone-deep` / `tone-blue`. الـ tone بيضبط `--bg --fg --accent --muted --line --win` تلقائيًا. **الإيقاع (فاتح/داكن) بيتحدد بالمحتوى، مش بترتيب الـ CSS.** لما تضيف فصل، ما تحطش tone زي جاره المباشر.

### 6.3 الـ Typography
`.display` / `.h2` / `.h3` / `.h4` / `.lead` / `.statement` / `.mono`. في الإنجليزي (`[lang="en"]`) الـ line-height والـ letter-spacing بيتغيروا. `.mono` بيجبر `direction:ltr` (مهم للإيميل والأرقام).

### 6.4 مكونات متكررة
`.btn` (+ `.btn-solid/.btn-ghost/.btn-wa` و`.sm/.lg`)، `.chip`، `.tag`، `.ph` (placeholder وسائط)، `.empty` (حالة فاضية مصمّمة)، `.rv` (reveal مرة واحدة)، `.field/.in` (حقول)، `.sheet` (حوار الحجز)، `.cover` (غلاف الدليل بـ CSS).
**استخدم المكونات الموجودة** قبل ما تعمل جديد.

### 6.5 الاتجاه (RTL/LTR)
- استخدم **logical properties** (`margin-inline-start`, `inset-inline-end`, `padding-inline`) في أي CSS جديد.
- الأسهم بتتقلب في RTL بـ `[dir="rtl"] .i-arrow`.
- نافذة الـ Signature `direction:ltr` عمدًا (هي محرر كود).

---

## 7. التفاعل الرئيسي (Signature)

أول فصل في الرئيسية: سطر `<h1>Hello, Codera</h1>` بيكبر مع التمرير: **أول خطوة ← تدريب ← مشروع ← مهارة**.

### 7.1 كيف يشتغل
- `.sig` طوله `560svh`، وجواه `.stage` بـ `position:sticky`. التمرير بيتحوّل لقيمة `p` من 0 لـ 1.
- `mountSignature()` بيحسب `p`، وبيعمل smoothing (`cur += (target-cur)*.14`)، وفي كل frame بيستدعي `render(p)` اللي بيكتب CSS variables ويعدّل الـ DOM.
- بيتعمل `destroy()` عند ترك الصفحة الرئيسية (`onLeave`)، وبينضّف الـ listeners والـ `VARS`.

### 7.2 الـ Timeline (`p` من 0 إلى 1)

| المرحلة | النطاق | ما بيحصل |
|---|---|---|
| 1 · الخطوة الأولى | 0 – .22 | الكتابة الأولى، ظهور النافذة والـ preview |
| 2 · التدريب | .22 – .50 | خطأ `colr` ← hint خطأ (من .305) ← الإصلاح عند `FIX = .37` ← hint "تم الإصلاح" |
| 3 · المشروع | .50 – .78 | `<section class="projects">` ← ثلاث `article` ← الـ preview يتحول لموقع |
| 4 · المهارة | .78 – 1 | الـ URL والـ terminal (`git push`) ← "منشور" ← الـ chips (تقنية + شخصية) |

الثوابت: `B = [.22,.50,.78]` (حدود المراحل)، `ANCHOR` (مواضع الأزرار `data-go`)، `CENTER`، `FIX`.

### 7.3 المتغيرات والاعتماد بين CSS و JS
- `--d` يمزج الخلفية من الورقي (0) للنيفي (1)، ويؤثر على `--bg --fg --accent --prop --str --err` في الصفحة كلها (مش بس الـ Signature).
- `--frac` عرض لوحة الكود؛ `--prev` و`--site` للـ preview؛ `--chrome` لشريط النافذة؛ `--term` و`--url` و`--tabs`؛ `--t1..t3` لبلاطات الموقع؛ `--skill` للـ chips (انظر P13).
- **الفصول الأربعة (`.ch`) متكدسة في نفس خلية الـ grid** (`grid-area:1/1`)، فارتفاع `.copy` = أطول فصل، حتى لو الفصل الظاهر أقصر. ده سبب مساحة فاضية فوق النافذة على الموبايل.
- الفصول المخفية بيتحط عليها `inert` و`aria-hidden` من الـ JS.

### 7.4 Accessibility الـ Signature
`.visual` عليه `aria-hidden="true"` (كله زخرفة)، وفيه `.sr-only` بيوصف الرحلة. أزرار الـ rail (`data-go`) و`aria-current="step"` هي التحكم الفعلي. الحركة بتتقلل مع `prefers-reduced-motion` (`reduce`).

---

## 8. الصفحات والـ Router

| المسار | الدالة |
|---|---|
| `#/` | `pageHome()`، بتجمع `Home[...]` حسب `CMS.sections` |
| `#/about` | `pageAbout()` |
| `#/courses`، `#/courses/:slug` | `pageCourses()`، `pageCourse(slug)` (فلاتر المجال والمستوى في `S.cat`/`S.level`) |
| `#/services` | `pageServices()` |
| `#/instructors`، `#/instructors/:slug` | `pageInstructors()`، `pageInstructor(slug)` |
| `#/events`، `#/events/:slug` | `pageEvents()`، `pageEvent(slug)` |
| `#/guides`، `#/guides/:slug` | `pageGuides()`، `pageGuide(slug)` |
| `#/contact` | `pageContact()` |
| غير ذلك | `page404()` |

كل دالة صفحة بترجع `{ html, meta, after? }`:
- `meta` → `setMeta()` (title، description، canonical، og، hreflang، JSON-LD).
- `after()` → يتنفذ بعد حقن الـ HTML (مثلًا تركيب الـ Signature).
- تسجيل تنظيف: `onLeave(fn)`.

`render(keepScroll)` بينضّف، ويطابق الـ route، ويحقن `#app`، ويعمل `setMeta` و`renderChrome` و`bindReveal` و`updateUI`. تبديل اللغة يعيد `render(true)`.

**الـ Chrome** (الـ nav والقائمة والفوتر والزرار اللاصق) بيتولّد في `renderChrome()` وبيتعاد مع كل `render`.

**`updateUI()`** (على التمرير): وضع الـ nav (`solid` / `compact` / `tone-paper`)، إظهار `#sticky` على الموبايل، وتقدّم خط `--p` في `[data-prog]`.

---

## 9. الحجز والقارئ والمناسبات

### 9.1 الحجز (`openBooking`)
- 3 خطوات: بيانات الشخص ← الكورس والفرع والنمط ← وسيلة التواصل.
- أي زر `data-book` يفتحه؛ `data-book="slug"` يحدد الكورس مسبقًا، و`data-note="..."` يضيف ملاحظة (مستخدم في تسجيل الفعاليات).
- التحقق: الاسم ≥ 2 حرف، السن 3–99، `PHONE_RE = /^(\+?20|0)?1[0125]\d{8}$/` (موبايل مصري)، والإيميل لو الوسيلة بريد.
- **بدون endpoint** (`settings.booking.endpoint = null`): ينتهي برسالة WhatsApp جاهزة (`bookingMessage()`) ويظهر تنبيه "وضع المعاينة" طالما `demo`.
- **مع endpoint:** `POST` JSON بالحقول: `name, age, phone, whatsapp, course, branch, mode, contact, email, message, lang`. بعد النجاح يظهر "تم استلام طلبك". في الحالتين بيتبعت `CustomEvent('codera:booking', { detail })` على `document` (مفيد للـ analytics).
- على الموبايل (≤640px) الحوار بيتحوّل لـ bottom sheet. حقول الإدخال 16px فمفيش zoom في iOS.

### 9.2 قارئ الأدلة (`openReader`)
- لو `guide.pages` فيه روابط صور، بيعرضها. لو لأ، بيولّد صفحات معاينة (غلاف + محتويات + صفحة لكل بند من `outline`).
- أدوات: تكبير/تصغير (`ZOOMS`)، ملء الشاشة، تنقل. اختصارات: الأسهم (معكوسة في RTL)، `+` و`-` و`F`.
- زر التحميل معطّل لحد ما `guide.file` يتحط.

### 9.3 المناسبات
طبقة اختيارية خفيفة (لا تستبدل الهوية): `ramadan`, `eid-fitr`, `eid-adha`, `anniversary`, `graduation`, `back-to-school`, `custom`.
- تفعيل: `CMS.celebration.active = 'ramadan'` أو بالرابط `?celebrate=ramadan`.
- بتضيف زخرفة SVG (`#cel`) وشارة في الـ nav؛ `anniversary` فيه confetti (بيتعطّل مع reduced-motion).
- زرار المعاينة في الفوتر (`#celPick`) **بيظهر بس طالما `settings.demo = true`**.

---

## 10. الـ Responsive: القواعد وسجل الـ patches

### 10.1 نقاط الكسر الموجودة

| الحد | الاستخدام |
|---|---|
| `≤1119px` | الـ nav يتحول لبرجر + قائمة ملء الشاشة |
| `≤860px` | **نقطة الموبايل الرئيسية** (Signature عمود واحد، معظم الشبكات) |
| `≤640px` | حوار الحجز bottom sheet، صف الفعالية |
| `≤480px` / `≤420px` / `≤372px` | تدرجات الموبايل الصغير (القارئ، الـ nav، إخفاء زرار الحجز من الـ nav) |
| ارتفاع `≤760 / ≤720 / ≤700 / ≤600 / ≤500` | شاشات قصيرة (انظر الجدول) |

### 10.2 سجل الـ patches (آخر الـ `<style>`)

الترتيب مهم. كل كتلة تعتمد على اللي قبلها.

| Patch | الشرط | بيعمل إيه |
|---|---|---|
| **P1** | `≤1119` | safe-area يمين/شمال للـ `.wrap/.nav-in/.menu-in`، وسكرول للقائمة |
| **P2** | `≤1119` و ارتفاع `≤720` | روابط القائمة أصغر عشان تدخل في 667 |
| **P3** | `≤420` / `≤372` | الـ nav أضيق؛ زرار الحجز يختفي من الـ nav تحت 372 (البرجر دايمًا ظاهر). الـ selector `.btn.sm.nav-cta` ضروري (القاعدة القديمة `.nav-cta` كانت ميتة بسبب الـ specificity) |
| **P4** | `≤860` | نافذة الـ Signature `height:auto; flex:1 1 0; min-height:170px; max-height:360px`، `.visual{overflow:hidden}`، منطقة لمس الـ rail ≈44px، safe-area للـ sticky |
| **P5** | `≤480` | الـ hint أصغر، شريط القارئ يلف على سطرين، زرار واتساب الكبير أصغر |
| **P6** | `≤860` و ارتفاع `≤700` | عناوين وفقرات وأزرار الـ Signature أصغر |
| **P7** | `≤860` و ارتفاع `≤600` | `min-height` أقل للنافذة وchips أصغر (320×568) |
| **P8** | `≤860` و ارتفاع `≤500` | **landscape موبايل:** عمودين `1fr/1fr`، `--hh:56px` |
| **P9** | `≤1119` و ارتفاع `≤500` | القائمة في landscape: روابط عمودين |
| **P10** | `≤860` / `≤480` | **إصلاح overflow الفوتر:** `minmax(0,1fr)` + `overflow-wrap:anywhere` + عمود واحد تحت 480 (الإيميل mono كان بيوسّع الصفحة 80px) |
| **P11** | `≤860` | `.sig .rail{align-items:start}` عشان الشرائط تبقى على خط واحد حتى لو label على سطرين |
| **P12** | `≤860` / (`≤860`، ارتفاع 501–760) | الـ hint بخلفية معتمة؛ مساحة رأسية أكبر للنافذة (padding أقل، line-height أضيق، label الـ rail سطر واحد). **portrait فقط** (landscape مستثنى) |
| **P13** | `≤860` / (`≥761` ارتفاع) | الـ chips **ما بتحجزش مساحة** إلا لما تظهر: `max-height: calc(var(--skill) * 120px)`. وعلى الموبايل الطويل يتشال سقف النافذة |

> **اعتماد CSS ↔ JS في P13:** بيعتمد على المتغير `--skill` اللي الـ JS بيكتبه (`ss(.82,.88,p)`). لو اتشال من الـ JS، الـ chips ترجع تحجز مساحتها دايمًا.

### 10.3 إزاي تضيف patch جديد
1. حدد الـ viewport والمشكلة بقياس (مش بالنظر بس).
2. اكتب كتلة جديدة **بعد P13**، بتعليق `/* P14 · وصف */` يذكر الشرط والسبب.
3. حدد الشرط بدقة (`max-width` + `min/max-height` لو لزم) عشان ما تأثرش على مقاسات تانية.
4. ارجع اختبر القائمة في [القسم 11](#11-الاختبار) كلها، مش بس المقاس اللي كنت بتصلحه.

---

## 11. الاختبار

### 11.1 المقاسات المطلوبة
`320` · `360` · `375×667` (الأساسي) · `390` · `393` · `414` · `768` · landscape `667×375`.
في كل مقاس: **عربي وإنجليزي**، و**فاتح وداكن**، والـ Signature في **المراحل الأربعة**.

### 11.2 فحص الـ overflow الأفقي (اعمل ده الأول)

لأن `body{overflow-x:hidden}` بيخبّي المشكلة، والـ Chrome ممكن يصغّر الصفحة تلقائيًا (فالـ nav يبان مقطوع)، شغّل في الـ Console:

```js
document.documentElement.scrollLeft = 0;           // مهم في RTL
const W = document.documentElement.clientWidth;
console.log('inner', innerWidth, 'client', W, 'scroll', document.documentElement.scrollWidth);
console.log([...document.querySelectorAll('body *')]
  .filter(e => getComputedStyle(e).position !== 'fixed')
  .filter(e => { const r = e.getBoundingClientRect(); return r.width && (r.right > W + 1 || r.left < -1); })
  .slice(0, 12)
  .map(e => (e.className.baseVal ?? e.className) + ' ' + e.tagName + ' L' + Math.round(e.getBoundingClientRect().left) + ' R' + Math.round(e.getBoundingClientRect().right)));
```

المفروض `innerWidth = clientWidth = scrollWidth`. أي فرق = فيه عنصر بيعمل overflow، والـ selector المسبّب في الناتج.
**أعراض overflow:** الـ nav مقطوع من ناحية، الهامش اليمين مختلف عن الشمال، الصفحة تبان مصغّرة.

### 11.3 قائمة الفحص

- [ ] مفيش overflow أفقي (11.2).
- [ ] البرجر ظاهر بالكامل عند 320، وزرار الحجز مخفي من الـ nav تحت 372 (متاح من القائمة والـ hero والـ sticky).
- [ ] الـ Signature: النص والنافذة والـ chips والـ rail جوه الشاشة، من غير تداخل أو قص، والـ hint ما بيغطيش آخر سطر كود.
- [ ] شرائط الـ rail على خط واحد.
- [ ] القائمة: كل الروابط وزرار الحجز متاحين (بسكرول لو لزم).
- [ ] الحجز: 3 خطوات بالكيبورد، رسائل الخطأ، الإغلاق بـ `Esc`.
- [ ] القارئ: تنقل، zoom، إغلاق، ملء الشاشة.
- [ ] الفوتر: الإيميل والأرقام ما بيكسروش الصفحة.
- [ ] desktop (>1120) مفيش اختلاف بصري عن قبل التعديل.
- [ ] `prefers-reduced-motion`: الصفحة شغالة من غير حركة.

---

## 12. مشاكل معروفة (لسه ما اتصلحتش)

> دي مشاكل اتلاقت في مراجعة الكود. **ما تصلحهاش من غير موافقة** لأن بعضها بيلمس JS أو نص.

| # | الخطورة | المشكلة | السبب والحل المقترح |
|---|---|---|---|
| 1 | **متوسطة** | أيقونة فاضية في الحالة الفاضية (`empty()` وفصل `happening` لما مفيش فعاليات قادمة) | الكود بينادي `ico('sparkle')` لكن `ICONS` مفيهاش `sparkle`، فالدايرة بتظهر من غير أيقونة. الحل: إضافة `sparkle` في `ICONS` أو استبدالها بـ `star` الموجودة |
| 2 | منخفضة | `--skill` ناقص من defaults الـ `:root` | الـ JS بيكتبه فورًا عند تركيب الـ Signature فمفيش أثر، لكن للاتساق يتضاف `--skill:0` جنب `--t3:0` |
| 3 | متوسطة (قبل الإطلاق) | **الـ SEO مع hash routing** | كل الصفحات بتظهر للزاحف كرابط واحد (`#/...`)، و`canonical`/`og:url`/`hreflang` بتتحسب بنمط `/{lang}{path}` اللي مش حقيقي لسه. محتاج routing حقيقي (server/prerender) قبل الاعتماد على الـ SEO |
| 4 | للتحقق | **Contrast وسط انتقال الـ Signature** | `--d` بيمزج الخلفية والنص عكس بعض؛ عند `d≈.5` (تقريبًا `p` .34–.39) اللون والخلفية متقاربين (**تقدير من حساب الألوان، مش قياس**). افحصه في DevTools؛ الإصلاح المحتمل (قلب لون النص عند المنتصف) يحتاج موافقة لأنه من الـ prototype المعتمد |
| 5 | منخفضة | landscape على موبايلات أعرض من 860 (مثل 932×430) | بتاخد تخطيط الـ desktop بارتفاع قصير، والـ copy ممكن يتقص. محتاج patch لنطاق `861–1119` مع `max-height:500px` |
| 6 | منخفضة | كود غير مستخدم | `S.evTab`، `coursesLine()`، معامل `labelledby` في `openModal`، `Book.data`، و`gs.length > 3 \|\| true` في `Home.library` |
| 7 | منخفضة | قاعدة ميتة | `@media(max-width:420px){ .nav-cta{...} }` القديمة ما بتشتغل (specificity أضعف من `.btn.sm`)؛ P3 عوّضتها. ما تحذفهاش من غير موافقة |
| 8 | متوسطة (قبل الإطلاق) | الحجز في وضع المعاينة | `booking.endpoint = null` يعني الطلب مش بيوصل لسيرفر، بيروح بس لـ WhatsApp. لازم endpoint + حماية من الـ spam (rate limit/honeypot) من جهة السيرفر |
| 9 | منخفضة | الاعتماد على Google Fonts | تحميل خارجي (خصوصية/سرعة). فكّر في استضافة الخطوط محليًا |
| 10 | تنبيه | `body{overflow-x:hidden}` | بيخبّي أي overflow أفقي مستقبلي؛ استخدم فحص 11.2 دايمًا |

---

## 13. مهام شائعة (وصفات)

### إضافة كورس
1. ضيف عنصر في `CMS.courses` (انظر 5.3). `slug` فريد، `cat` من `categories`، `stage` من `stages`.
2. `leadsTo` يحط slugs موجودة بس.
3. لو عايز تظهره لمدرّب، ضيف الـ slug في `courses[]` بتاعه.
4. خدماتنا: ضيفه في `courses[]` الخاص بالخدمة المناسبة.

### إضافة مدرّب
عنصر في `CMS.instructors` بـ `slug` فريد. احذف `demo: true` لما تحط بيانات حقيقية. `featured: true` على واحد بس (اللي بيظهر كبير في الرئيسية).

### إضافة فعالية
عنصر في `CMS.events` بـ `date` ISO (مثال: `'2026-12-01T17:00:00.000Z'`). تتصنّف قادمة/أرشيف تلقائيًا.

### إضافة دليل
عنصر في `CMS.guides`. حط `file` (رابط تحميل) و`pages[]` (روابط صور لصفحات القارئ) و`pageCount` و`language` لما الملف يجهز.

### تفعيل الحجز على سيرفر
`CMS.settings.booking.endpoint = 'https://...'` (يستقبل POST JSON بالحقول في 9.1).

### تغيير ترتيب أو إخفاء فصل في الرئيسية
عدّل `order` أو `visible` في `CMS.sections`. لو فصل رجع `''` (مفيش محتوى) بيختفي تلقائيًا من غير فراغ.

### إضافة فصل جديد في الرئيسية
1. دالة جديدة في `Home` ترجع HTML بـ `sec(id, tone, inner, cls)` و`secHead(id, copy)` (الـ `id-t` للـ `aria-labelledby`).
2. ترجع `''` لو مفيش محتوى.
3. ضيف `{ id, visible, order }` في `CMS.sections`.
4. اختار tone مختلف عن الفصل اللي جنبه.
5. اختبر الـ responsive (≤860 و≤480) واللغتين.

### إضافة صفحة جديدة
1. دالة `pageX()` ترجع `{ html, meta }` (استخدم `phero()` و`ctaBand()` و`crumbs()`).
2. سطر في `ROUTES`.
3. عنصر في `CMS.nav` (مع `needs` لو بتعتمد على قائمة).
4. وفّر حالة فاضية بـ `empty()`.

### إضافة أيقونة
ضيف مفتاح في `ICONS` (محتوى `<svg>` داخلي بـ viewBox `24`، stroke). استخدمها بـ `ico('name')`. أيقونات `social[].id` لازم تكون هنا.

### إضافة نص ثنائي اللغة
`D('عربي', 'English')` في الـ CMS، أو `L('عربي', 'English')` داخل الـ HTML. الاتنين مع بعض دايمًا.

### تغيير لون أو token
عدّل في `:root` (أول الـ `<style>`). ما تحطش ألوان hex مباشرة في قواعد جديدة إلا لو مفيش token مناسب؛ والـ amber (`--grow`) للتقدّم والنجاح فقط.

---

## 14. قبل الإطلاق

- [ ] `settings.demo = false` (يخفي tags "تجريبي" وزرار معاينة المناسبات ورسالة وضع المعاينة).
- [ ] استبدال كل `demo: true` بمحتوى حقيقي (مدرّبون، فعاليات، قصص، صور، أدلة).
- [ ] ضبط `settings.siteUrl` و`urlPattern` وحل مشكلة الـ SEO (#3 أعلاه).
- [ ] ربط `booking.endpoint` بسيرفر مع حماية.
- [ ] إضافة `og:image` (`meta.image`) وأيقونة الموقع (favicon).
- [ ] مراجعة المشاكل المعروفة (القسم 12)، خصوصًا #1 و#4.
- [ ] تشغيل قائمة الفحص كاملة (11.3).
- [ ] مراجعة الـ accessibility: كيبورد، قارئ شاشة، contrast.
