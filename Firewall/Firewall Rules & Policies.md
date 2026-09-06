| Topic | Level | Reading Time | Prerequisites |
|---|---|---|---|
| Network Security Fundamentals: Firewall Rules and Default Policies | Intermediate | ~30 min | Basic understanding of firewalls, TCP/IP, and common port numbers |

> **الهدف من الـ Section ده:**  
> هتفهم إزاي الـ Firewall فعليًا بيشتغل من خلال الـ rules، مين المسؤول عن كتابتها، هنشرح خمس أمثلة حقيقية لقواعد firewall شائعة، وأخيرًا هتفهم الفرق الجوهري بين فلسفتين لضبط الفايروول: **Default Deny** و **Default Allow**.


## Learning Objectives

By the end of this section, you will be able to:

- Explain why a firewall is not "smart" on its own and depends entirely on rules written by humans.
- Define what a **rule** is in the context of a firewall, and explain the **Default Deny** principle.
- Identify who is typically responsible for writing firewall rules and why a **SOC Analyst** still needs to understand them.
- Read and interpret the logic of real-world firewall rules based on source, destination, protocol, port, and action.
- Explain the security reasoning behind common rules such as blocking FTP, geo-blocking, and forcing DNS through a controlled path.
- Compare **Default Allow** and **Default Deny** as overall firewall policies and explain their trade-offs.
- Explain why a SOC Analyst must know which default policy an organization uses before interpreting firewall logs.

## Table of Contents

- [Firewalls Are Not Smart on Their Own](#firewalls-are-not-smart-on-their-own)
- [What Is a Rule, Really](#what-is-a-rule-really)
- [Default Deny: The Core Principle](#default-deny-the-core-principle)
- [Who Writes These Rules](#who-writes-these-rules)
- [Why SOC Analysts Must Understand Rules Too](#why-soc-analysts-must-understand-rules-too)
- [How Rules Are Configured](#how-rules-are-configured)
- [Real Examples of Firewall Rules](#real-examples-of-firewall-rules)
  - [Example 1: Allowing Normal Web Browsing](#example-1-allowing-normal-web-browsing)
  - [Example 2: Blocking Unsolicited Inbound Traffic](#example-2-blocking-unsolicited-inbound-traffic)
  - [Example 3: Blocking Insecure FTP](#example-3-blocking-insecure-ftp)
  - [Example 4: Geo-Blocking](#example-4-geo-blocking)
  - [Example 5: Forcing DNS Through a Controlled Path](#example-5-forcing-dns-through-a-controlled-path)
- [Rule Logic Diagram](#rule-logic-diagram)
- [Default Allow vs. Default Deny: Two Competing Philosophies](#default-allow-vs-default-deny-two-competing-philosophies)
- [Default Deny: The Safer, More Common Approach](#default-deny-the-safer-more-common-approach)
- [Default Allow: The Riskier Approach](#11-default-allow-the-riskier-approach)
- [Side-by-Side Comparison](#side-by-side-comparison)
- [Why This Matters for a SOC Analyst](#why-this-matters-for-a-soc-analyst)
- [Which Model Do Organizations Actually Use](#which-model-do-organizations-actually-use)
- [Decision Flow Diagram](#decision-flow-diagram)
- [Career Connection](#career-connection)
- [Key Terms Glossary](#key-terms-glossary)
- [Summary](#summary)

## Firewalls Are Not Smart on Their Own

الـ **Firewall** مش ذكي بطبيعته. هو مش بيقرر لوحده إيه اللي خطير وإيه اللي مش خطير. هو أساسًا آلة "غبية" ومطيعة (**dumb, obedient machine**)، بتعرف بس اللي إنت قلتله عليه، من خلال الـ **rules**.

> [!IMPORTANT]
> الفكرة دي مهمة جدًا نفهمها من البداية: الـ Firewall مش عنده أي "فهم" حقيقي للخطر، هو بس بينفذ التعليمات اللي اتحطتله بالظبط. أي غلط في القواعد دي، سواء بالزيادة أو النقصان، هيتنفذ من غير أي تشكيك.

## What Is a Rule, Really

الـ **Rule** ببساطة هي جملة بتقول:

- النوع ده من حركة المرور (**traffic**)، اسمحله (**allow it**).
- النوع ده من حركة المرور، امنعه (**block it**).

كل حاجة تانية في الموضوع ده بتبني على المفهوم البسيط ده.

## Default Deny: The Core Principle

فيه مفهوم أساسي ومهم جدًا في السلوك الافتراضي (**default behavior**) للفايروول: معظم الفايروولات بتتبع مبدأ اسمه **Default Deny**.

معنى المبدأ ده: لو مفيش rule بتسمح تحديدًا بحاجة معينة، الحاجة دي بتتحظر تلقائيًا.

> [!NOTE]
> الفكرة الأساسية هنا إن الافتراض الأمني هو "الرفض"، مش "القبول". يعني الفايروول بيفترض إن أي حركة مرور مشبوهة أو غير معروفة لحد ما يتأكد إنها مسموح بيها صراحة.

## Who Writes These Rules

**Network Security Team** هي عادةً المسؤولة عن تصميم وكتابة القواعد دي، هي اللي بتقرر السياسة الأمنية الفعلية (**security policy**).

لكن كمحلل **SOC Analyst**، إنت مش برة الموضوع خالص. لازم تقدر تقرأ وتفهم وتفسّر القواعد دي كويس، للأسباب التالية:

- هتحقق في تنبيهات (**alerts**) بتشاور لقواعد معينة.
- هتحتاج تعرف هل حركة المرور اتحظرت ولا اتسمحلها، وليه.
- ممكن تحتاج تلاحظ rule متضبطة بشكل خطأ أو متساهلة (**permissive**) أكتر من اللازم.

## Why SOC Analysts Must Understand Rules Too

النقطة دي بتوضح إن فهم منطق القواعد مش شغل مقصور على فريق الـ Network Security بس. محلل الـ SOC اللي بيقدر يقرأ ويفهم القواعد دي بيقدر يكتشف بسرعة أكبر إذا كان في الموضوع مشكلة فعلية أو مجرد سلوك متوقع.

> [!TIP]
> لو جالك تنبيه بيقول إن حركة مرور اتحظرت، أول سؤال لازم تسأله هو: "أي rule بالظبط اللي حظرت الحركة دي؟" الإجابة دي بتوضحلك السياق الكامل وتساعدك تحكم هل ده تهديد حقيقي أو مجرد تصرف طبيعي متوقع.

## How Rules Are Configured

معظم الفايروولات الحديثة عندها **GUI** (واجهة رسومية)، إنت غالبًا مش بتكتب أوامر سطر أوامر مخيفة. إنت بس بتملى حقول زي: source، destination، port، action (allow/deny).

لكن الجزء الصعب فعليًا مش الواجهة نفسها، الجزء الصعب هو **بناء منطق الـ rule بشكل صحيح**. rule متبنية غلط ممكن إما تحظر حركة مرور شرعية مهمة للبيزنس (وتسبب شكاوى غضب من الموظفين)، أو بالغلط تسمح بمرور حاجة خطيرة.

> [!WARNING]
> قواعد كل مؤسسة مختلفة تمامًا عن التانية. قواعد فايروول بنك مش هتشبه خالص قواعد فايروول مدرسة. **مفيش كتاب قواعد عالمي موحّد**، كل حاجة بتعتمد على احتياجات البيزنس والمخاطر اللي المؤسسة بتحاول تديرها.

## Real Examples of Firewall Rules

دلوقتي هنشوف إزاي القواعد دي فعليًا بتتكتب في الواقع. متقلقش بخصوص الصيغة الدقيقة (بتختلف من مزوّد لآخر)، ركّز على فهم **المنطق (logic)**.

### Example 1: Allowing Normal Web Browsing

```
ALLOW TCP Internal_Network → Internet
PORTS 80, 443
```

**المنطق**: خلّي الموظفين جوه الشركة يقدروا يتصفحوا الإنترنت باستخدام مواقع آمنة عادية. دي rule طبيعية جدًا وموجودة في كل مكان تقريبًا، أساسًا بتسمح للموظفين يستخدموا الإنترنت للتصفح العادي.

### Example 2: Blocking Unsolicited Inbound Traffic

```
DENY ANY Internet → Internal_Network
```

**المنطق**: محدش من الإنترنت الخارجي مسموحله يدخل ببساطة على شبكتنا الداخلية. دي rule دفاعية أساسية جدًا، بتمنع أي حد عشوائي على الإنترنت إنه يوصل مباشرة لأنظمة الشركة الداخلية.

### Example 3: Blocking Insecure FTP

```
DENY TCP ANY → ANY PORT 21 (FTP)
```

**المنطق**: امنع بروتوكول FTP في كل مكان، لأي حد. الـ **FTP** بروتوكول قديم لنقل الملفات وغير آمن، هو بيبعت أسماء المستخدمين وكلمات المرور بنص عادي (غير مشفر). كتير من المؤسسات بتمنعه تمامًا لأنه هدف سهل للمهاجمين إنهم يسرقوا بيانات اعتماد بمجرد مراقبة حركة المرور.

### Example 4: Geo-Blocking

```
DENY IP Country_List → Internal_Network
```

**المنطق**: امنع حركة المرور الجاية من قائمة دول معينة. الممارسة دي بتُسمى **Geo-Blocking**. الشركات غالبًا بتعمل كده لو ماعندهاش تعاملات تجارية في دول معينة، وشايفة حجم كبير من حركة مرور هجومية جاية من هناك، فبتقرر تقطع الاتصال بالكامل عشان تقلل المخاطر.

### Example 5: Forcing DNS Through a Controlled Path

```
DENY UDP Internal_Network → ANY PORT 53
```

**المنطق**: امنع طلبات الـ DNS الخارجة اللي بتحاول تستخدم UDP port 53 عشان توصل لأي مكان. البورت 53 مستخدم للـ **DNS**. ده ممكن يبان في الظاهر كإنه هيعطّل الإنترنت لكل حد، لكن الـ rule دي غالبًا بتُستخدم عشان تجبر كل حركة مرور DNS تمر عبر سيرفر DNS داخلي محدد وتحت المراقبة، بدل ما تسيب الأجهزة تستعلم من سيرفرات DNS خارجية عشوائية مباشرة.

> [!IMPORTANT]
> السبب وراء الـ rule دي؟ الـ **Malware** غالبًا بيستخدم طلبات DNS خارجية بشكل خفي عشان يتواصل مع الهاكرز (وده بيُسمى **DNS Tunneling**)، فحظر الـ DNS الخارج المباشر بيجبر كل حاجة إنها تمر عبر مسار متحكم فيه ومراقب.

كل rule عندها منطق واضح ورا: **source**، **destination**، **protocol**، **port**، و **action**. بمجرد ما تريّح في قراءة القواعد دي، هتقدر تبص على **أي** firewall rule في العالم وتفهم هدفها.

## Rule Logic Diagram

المخطط التالي بيوضح المنطق العام اللي أي firewall rule بيتبني عليه، من وصول حركة المرور لحد اتخاذ قرار السماح أو الحظر:

```mermaid
flowchart LR
    A["Incoming or Outgoing Traffic"] --> B{"Match a Rule<br/>Source, Destination, Protocol, Port"}
    B -->|"Matches an ALLOW Rule"| C["Traffic is Permitted"]
    B -->|"Matches a DENY Rule"| D["Traffic is Blocked"]
    B -->|"No Matching Rule Found"| E["Default Deny<br/>Traffic is Blocked"]
```

## Default Allow vs. Default Deny: Two Competing Philosophies

ده واحد من أهم المفاهيم التأسيسية في إعداد الفايروول، وهو بيأثر على **الوضع الأمني الكامل (entire security posture)** للمؤسسة.

الاختيار بين الفلسفتين مش تفصيلة تقنية بسيطة، هو قرار استراتيجي بيحدد إزاي الشبكة كلها هتتعامل مع أي حركة مرور جديدة أو غير معروفة.

## Default Deny: The Safer, More Common Approach

في فلسفة **Default Deny**: كل حاجة بتتحظر افتراضيًا. بيتم السماح بس بحركة المرور اللي حد صراحة وافق عليها.

> [!NOTE]
> تخيل مبنى حصري (**exclusive building**) خاص بالأعضاء بس. لو اسمك مش موجود على قائمة الموافقين، الأمن مش هيسمحلك تدخل، نقطة على السطر.

**ده أكتر أمانًا**، لأن مفيش حاجة بتعدي بالصدفة.

**السلبية؟** بياخد مجهود أكبر مقدمًا، حد لازم يفكّر في كل نوع من أنواع حركة المرور الشرعية ويكتب لها rule سماح يدويًا. لو نسيت واحدة، المستخدمين هيشتكوا إن حاجة معينة مش شغالة.

## Default Allow: The Riskier Approach

في فلسفة **Default Allow**: كل حاجة مسموح بيها افتراضيًا، وبيتم حظر بس اللي اتحدد صراحةً إنه ضار.

> [!NOTE]
> تخيل حديقة عامة (**public park**) بدخول مفتوح، أي حد يقدر يدخل إلا لو اتحدد تحديدًا إنه ممنوع أو موجود في قائمة "ممنوع الدخول".

**ده أقل أمانًا**، لأن التهديدات الجديدة اللي معتحددتش لسه هتعدي بحرية، بما إنها لسه مش موجودة في قائمة الحظر.

**الميزة؟** مجهود إعداد أولي أقل، وشكاوى أقل بخصوص حظر حركة مرور شرعية.

## Side-by-Side Comparison

الجدول التالي بيلخّص الفرق الجوهري بين الفلسفتين:

| المعيار | Default Deny | Default Allow |
|---|---|---|
| السلوك الافتراضي | حظر كل شيء ما لم يُسمح به صراحة | السماح بكل شيء ما لم يُحظر صراحة |
| مستوى الأمان | أعلى، لأن التهديدات غير المعروفة تُحظر تلقائيًا | أقل، لأن التهديدات غير المعروفة تمر بحرية |
| مجهود الإعداد الأولي | أكبر، يتطلب كتابة قاعدة سماح لكل نوع حركة شرعي | أقل، يتطلب فقط تحديد ما هو ضار ومعروف |
| شكاوى المستخدمين | أكثر احتمالًا عند نسيان قاعدة سماح لحركة شرعية | أقل احتمالًا، لأن معظم الحركة تمر افتراضيًا |
| التعامل مع تهديدات جديدة | تُحظر تلقائيًا حتى تتم الموافقة عليها | تمر بحرية حتى يتم تحديدها كضارة |

> [!IMPORTANT]
> الفرق الجوهري بين الفلسفتين مش في "قوة" الحماية بس، لكن في **اتجاه الافتراض نفسه**: هل الافتراض هو الثقة (Allow) أو عدم الثقة (Deny)؟ الإجابة على السؤال ده بتحدد طبيعة كل قرار أمني تاني في الشبكة.

## Why This Matters for a SOC Analyst

لما تكون بتحقق في حادثة أمنية (**incident**)، معرفتك بإن المؤسسة بتستخدم **Default Allow** أو **Default Deny** بتغيّر تمامًا الطريقة اللي بتفسّر بيها الـ firewall logs.

- في بيئة **Default Deny**: لو شفت حركة مرور عدّت، معناها إن حد صراحة وافق عليها. فالسؤال اللي هتسأله: "هل الـ rule دي كانت مبررة أصلًا؟"
- في بيئة **Default Allow**: بدل كده، هتسأل: "هل الحركة دي المفروض كانت متحظورة ومحظرتش؟"

> [!TIP]
> السؤالين مختلفين تمامًا في طبيعتهم، وفهم الفلسفة اللي المؤسسة بتتبعها هو أول خطوة قبل ما تبدأ تحلل أي firewall log فعليًا، عشان تسأل السؤال الصح من الأول.

## Which Model Do Organizations Actually Use

معظم المؤسسات الناضجة أمنيًا (**security-mature organizations**) بتميل لاستخدام **Default Deny**، لأنه بيقلل المخاطر.

> [!WARNING]
> رغم إن Default Deny هو الأكتر شيوعًا في المؤسسات الناضجة أمنيًا، مهم جدًا إنك تعرف إن الموديلين موجودين فعليًا في الواقع، ولازم **دايمًا تتأكد من المؤسسة بتستخدم أنهي موديل** قبل ما تبدأ تحلل سلوك الفايروول بتاعها.

## Decision Flow Diagram

المخطط التالي بيوضح الفرق في طريقة تفكير محلل الـ SOC عند تحليل حركة مرور تحت كل موديل:

```mermaid
flowchart TB
    A["Traffic Observed in Firewall Log"] --> B{"Which Model Does<br/>the Organization Use"}
    B -->|"Default Deny"| C["Traffic Passed Through"]
    C --> D["Ask: Was This Allow Rule<br/>Actually Justified"]
    B -->|"Default Allow"| E["Traffic Passed Through"]
    E --> F["Ask: Should This Traffic<br/>Have Been Blocked"]
```

## Career Connection

فهم منطق قواعد الفايروول والفرق بين Default Allow وDefault Deny له تطبيقات مباشرة في مسارات مهنية متعددة:

- في مجال **SOC**، القدرة على قراءة وفهم firewall logs والقواعد المرتبطة بيها، مع معرفة الفلسفة الافتراضية المستخدمة، أساس التحقيق في معظم التنبيهات الأمنية اليومية.
- في مجال **Network Security Engineering**، تصميم قواعد فايروول دقيقة ومتوازنة، واختيار الفلسفة المناسبة (Default Deny غالبًا)، هو جوهر الشغل.
- في مجال **Pentesting**، فحص قواعد الفايروول بحثًا عن إعدادات متساهلة أو مضبوطة بشكل خطأ (**misconfigured**) جزء أساسي من تقييم أمان الشبكة.
- في مجال **GRC**، توثيق الفلسفة المتبعة (Default Allow أو Default Deny) جزء من متطلبات الامتثال والتدقيق الأمني.

## Key Terms Glossary

| Term | Definition |
|---|---|
| **Firewall Rule** | جملة تحدد نوع معين من حركة المرور وتقرر إما السماح لها أو حظرها. |
| **Default Deny** | مبدأ إعداد يقضي بحظر كل حركة المرور افتراضيًا، ما لم يوجد rule صريح يسمح بها. |
| **Default Allow** | مبدأ إعداد يقضي بالسماح بكل حركة المرور افتراضيًا، ما لم يوجد rule صريح يحظرها. |
| **Source / Destination** | عنوان أو شبكة المنشأ والوجهة الخاصة بحركة المرور المحددة في الـ rule. |
| **FTP (File Transfer Protocol)** | بروتوكول قديم وغير آمن لنقل الملفات، يرسل بيانات الاعتماد بنص غير مشفر. |
| **Geo-Blocking** | حظر حركة المرور القادمة من دول أو مناطق جغرافية محددة. |
| **DNS Tunneling** | تقنية يستخدمها المهاجمون لتمرير بيانات أو أوامر عبر طلبات DNS خارجية للتواصل خفية مع خوادمهم. |
| **Security Posture** | الوضع الأمني العام للمؤسسة، الناتج عن مجموع سياساتها وإعداداتها الأمنية. |
| **Security-Mature Organization** | مؤسسة طبّقت ممارسات وسياسات أمنية متقدمة ومدروسة بعناية. |

## Summary

- الـ **Firewall** مش ذكي بطبيعته، هو بينفذ بس القواعد (**rules**) اللي المسؤولين كتبوها له، وكل rule أساسًا جملة بسيطة بتقول: النوع ده من الحركة اسمحله أو امنعه.
- مبدأ **Default Deny** بيقول إن أي حركة مرور من غير rule صريحة بتسمح بيها، بتتحظر تلقائيًا.
- **Network Security Team** هي المسؤولة عادةً عن كتابة القواعد، لكن محلل **SOC** لازم يقدر يقرأها ويفهمها ويفسّرها، لأنها أساس التحقيق في التنبيهات اليومية.
- إعداد القواعد بيتم غالبًا من خلال **GUI**، لكن التحدي الحقيقي مش في الواجهة، لكن في **بناء المنطق الصحيح** للقاعدة.
- الأمثلة الحقيقية بتوضح تنوع القواعد: السماح بتصفح الإنترنت العادي، منع الوصول العشوائي من الإنترنت، حظر بروتوكول **FTP** غير الآمن، تطبيق **Geo-Blocking**، وإجبار حركة **DNS** إنها تمر عبر مسار داخلي مراقب لمنع تقنيات زي **DNS Tunneling**.
- **Default Deny** بيحظر كل حركة مرور افتراضيًا، ولا يسمح إلا بما تمت الموافقة عليه صراحةً، وهو الأكثر أمانًا لكنه يتطلب مجهود إعداد أكبر.
- **Default Allow** بيسمح بكل حركة مرور افتراضيًا، ولا يحظر إلا ما تم تحديده كضار، وهو أقل أمانًا لكنه يتطلب مجهود إعداد أقل.
- كمحلل **SOC**، السؤال اللي بتسأله عند تحليل حركة مرور بيختلف تمامًا حسب الموديل المستخدم: "هل الـ rule دي كانت مبررة؟" في Default Deny، مقابل "هل ده كان المفروض يتحظر؟" في Default Allow.
- معظم المؤسسات الناضجة أمنيًا بتميل لـ **Default Deny** لأنه بيقلل المخاطر، لكن الموديلين موجودين فعليًا، ولازم دايمًا تتأكد من الموديل المستخدم قبل ما تبدأ تحليلك.

