> **الهدف من الـ Section ده:**  
> هتفهم الـ 5 مبادئ الأساسية اللي أي نظام Cryptography بيتقاس عليها (Confidentiality, Integrity, Authentication, Non-repudiation, Key Exchange)، عشان بعد كده لما نتكلم عن Symmetric وAsymmetric Encryption وHashing وDigital Signatures، تقدر تحكم بنفسك كل تقنية منهم بتحقق إيه بالظبط من المبادئ دي.

## Table of Contents
- [Why Do We Need These Properties?](#why-do-we-need-these-properties)
- [1. Confidentiality](#1-confidentiality)
- [2. Integrity](#2-integrity)
- [3. Authentication](#3-authentication)
- [4. Non-repudiation](#4-non-repudiation)
- [5. Key Exchange](#5-key-exchange)
- [Quick Reference Table](#quick-reference-table)
- [Summary](#summary)
- [SOC Analyst Perspective](#soc-analyst-perspective)

---

## Why Do We Need These Properties?

قبل ما تدخل في أي نوع تشفير (Symmetric, Asymmetric, Hashing, Digital Signatures...)، لازم يكون عندك **مقياس ثابت** تقيس بيه أي تقنية Cryptographic — يعني تقدر تسأل نفسك: "التقنية دي بتحمي إيه بالظبط؟ وبتسيب إيه من غير حماية؟"

الـ 5 مبادئ دي هي المقياس ده.

> [!NOTE]
> لو درست الـ Security عموماً قبل كده، غالباً سمعت عن **CIA Triad** (Confidentiality, Integrity, Availability) كأساس عام لأي نظام أمني. هنا في سياق الـ Cryptography تحديداً، الـ **Availability** مش من ضمن الخصائص اللي التشفير بيوفرها بشكل مباشر (التشفير مالوش علاقة بإن السيستم يفضل شغال أو لأ). بدالها، ضفنا مبدأين أهم في سياق الـ Cryptography: **Authentication, Non-repudiation, Key Exchange**، عشان يغطوا كل زوايا التعامل الآمن مع البيانات والهوية.

---

## 1. Confidentiality

**التعريف:** ضمان إن المعلومات الحساسة متاحة بس للأشخاص المصرح لهم بالإطلاع عليها، ومنع أي شخص غير مصرح له من اعتراض أو قراءة البيانات أثناء النقل أو التخزين.

**مثال واقعي:** لما تبعت رسالة على تطبيق فيه End-to-End Encryption، حتى لو حد اعترض الرسالة وهي ماشية، مش هيقدر يقرا محتواها لأنها متشفرة.

> [!TIP]
> فكّر فيها كإنها "خصوصية المحتوى" — مين اللي مسموحله يشوف اللي جوه الرسالة.

---

## 2. Integrity

**التعريف:** ضمان إن البيانات ما اتغيرتش أو اتلاعب فيها أو اتلفت أثناء دورة حياتها، وبيدي ثقة إن المعلومات فضلت دقيقة وموثوقة من نقطة المصدر لحد الوصول للمستقبِل.

**مثال واقعي:** لو نزّلت ملف من الإنترنت وقارنت الـ Hash بتاعه مع الـ Hash المنشور على الموقع الرسمي، وطلعوا متطابقين، يبقى الملف ما اتغيرش أثناء التنزيل.

> [!TIP]
> فكّر فيها كإنها "سلامة المحتوى" — هل اللي وصل هو نفسه بالظبط اللي اتبعت من غير أي تعديل؟

---

## 3. Authentication

**التعريف:** عملية التحقق من إن كيان معين (زي مستخدم أو نظام) هو فعلاً اللي بيدّعي إنه هو. من أشهر الطرق لتحقيق ده: كلمات المرور، الشهادات الرقمية (Digital Certificates)، والتحقق البيومتري.

**مثال واقعي:** لما تدخل على حسابك البنكي وتحط Username وPassword، البنك بيتأكد إنك فعلاً أنت صاحب الحساب مش حد بينتحل شخصيتك.

> [!TIP]
> فكّر فيها كإنها "إثبات الهوية" — هل الطرف التاني هو فعلاً مين ما بيدّعي؟

---

## 4. Non-repudiation

**التعريف:** منع طرف من إنكار مشاركته في عملية أو معاملة معينة بعد ما تكون حصلت فعلاً. بيوفر دليل قاطع مالوش رجعة عن مصدر الرسالة واستلامها، وغالباً بيتحقق باستخدام الـ Digital Signatures والـ Secure Logging.

**مثال واقعي:** لو وقّعت عقد إلكتروني بتوقيع رقمي (Digital Signature)، مش هتقدر تنكر بعد كده إنك وقّعته، لأن التوقيع مرتبط بالـ Private Key بتاعك وبس.

> [!TIP]
> فكّر فيها كإنها "مفيش إنكار" — دليل رياضي إن ده فعلاً أنت اللي عملته، ومش هتقدر ترجع فيه.

---

## 5. Key Exchange

**التعريف:** طريقة بتسمح لطرفين إنهم يتشاركوا Cryptographic Keys بأمان عبر قناة اتصال غير آمنة (Insecure Channel). العملية دي أساسية لتأسيس اتصالات آمنة قبل ما يتم نقل أي بيانات حساسة.

**مثال واقعي:** لما المتصفح والسيرفر يتفقوا على Session Key في بداية اتصال HTTPS (زي ما هيحصل في الـ TLS Handshake)، من غير ما حد يقدر ينسخ الـ Key ده حتى لو بيراقب الاتصال بالكامل.

> [!TIP]
> فكّر فيها كإنها "الاتفاق الآمن على السر" — إزاي طرفين يوصلوا لسر مشترك من غير ما حد تالت يعرفه، حتى لو بيراقب كل حاجة بينهم.

---

## Quick Reference Table

| Property | بيجاوب على السؤال | مثال تقني |
|---|---|---|
| **Confidentiality** | مين اللي يقدر يقرا البيانات؟ | Encryption |
| **Integrity** | هل البيانات اتغيرت؟ | Hashing |
| **Authentication** | مين ده فعلاً؟ | Passwords, Certificates |
| **Non-repudiation** | هل يقدر ينكر إنه عمل ده؟ | Digital Signatures |
| **Key Exchange** | إزاي نتفق على سر بأمان؟ | Diffie-Hellman |

---

## Summary

- كل نظام Cryptography بيتقاس على 5 مبادئ أساسية: **Confidentiality, Integrity, Authentication, Non-repudiation, Key Exchange**
- **Confidentiality:** حماية المحتوى من القراءة غير المصرح بها
- **Integrity:** التأكد إن البيانات ما اتغيرتش
- **Authentication:** التأكد من هوية الطرف التاني
- **Non-repudiation:** منع الإنكار بعد إتمام الفعل
- **Key Exchange:** الاتفاق على مفتاح سري بأمان حتى لو القناة غير آمنة
- الـ CIA Triad العام (اللي فيه Availability) مختلف عن الـ 5 مبادئ دي، لأن الـ Availability مش خاصية بيوفرها التشفير مباشرة
- كل موضوع تشفير جاي هيتقاس على المبادئ دي عشان تفهم بيحقق إيه بالظبط وبيسيب إيه من غير حماية

---

## SOC Analyst Perspective

- الـ 5 مبادئ دي مش مجرد نظرية — أي Incident Report محترف بيوصف تأثير الحادثة بناءً عليها (مثلاً: "الهجوم أثّر على الـ Confidentiality لأن البيانات اتسربت، لكن مأثرش على الـ Integrity لأن الملفات ما اتغيرتش")
- لما تحلل أي هجوم (زي Data Breach أو MITM Attack)، اسأل نفسك: "أنهي مبدأ من الـ 5 دول اتخرق بالظبط؟" السؤال ده بيساعدك تحدد شدة الحادثة والخطوات اللازمة للتعامل معاها
- MITRE ATT&CK بيصنف كتير من التكتيكات بناءً على المبادئ دي — مثلاً **Exfiltration** بيستهدف الـ Confidentiality، و**Impact/Data Manipulation** بيستهدف الـ Integrity
- في تقارير الـ Threat Intelligence والـ Compliance (زي ISO 27001)، هتلاقي المبادئ دي بتتكرر كأساس لتقييم المخاطر (Risk Assessment)
