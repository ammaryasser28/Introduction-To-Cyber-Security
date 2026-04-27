> **الهدف من الـ Section ده:**  
> هنتعرف على أساليب Defense & Detection المستخدمة لاكتشاف ومنع الـ Malware والهجمات.
---

## Defense & Detection

### Antivirus Solutions

#### النوع الأول: Signature-Based Detection

الـ **Signature-Based Antivirus** بيعمل كده:

1. يحسب الـ **Hash** (SHA-256، MD5) للـ File
2. يقارنه بقاعدة بيانات من الـ Hashes المعروفة للـ Malware
3. كمان بيبحث عن **Unique Patterns** و**Strings** مميزة في الكود

```mermaid
flowchart TD
    A[File يتم فحصه] --> B[Antivirus يحسب Hash]
    B --> C{Hash موجود في قاعدة البيانات؟}
    C -- نعم --> D[Alert: Malware Detected]
    C -- لا --> E[يبحث عن Patterns وStrings]
    E --> F{Pattern مطابق؟}
    F -- نعم --> D
    F -- لا --> G[File Clean]
```

> [!WARNING]
> الـ Signature-Based Antivirus عنده عيب كبير: **Zero-Day Malware** — أي Malware جديد لم يضاف بعد لقاعدة البيانات — هيعدي من غير اكتشاف. الـ Antivirus updates كل بضع دقائق، بس الـ Attackers دايماً بيعدلوا على الـ Malware عشان يتجنبوا الـ Signature.

#### النوع الثاني: Modern Behavioral Detection

المنظمات الناضجة مش بتعتمد على الـ Signature-Based بس. الـ **Modern Anti-Malware** بيجمع:

| الطريقة | الوصف |
|---|---|
| **Signature-Based** | مقارنة الـ Hashes والـ Patterns |
| **Machine Learning** | نماذج تعلم آلي لاكتشاف السلوك الغريب |
| **Behavioral Analysis** | مراقبة نشاط البرنامج في الـ Runtime |

```mermaid
flowchart TD
    A[File or Process] --> B[Signature Check]
    A --> C[ML Model Analysis]
    A --> D[Behavioral Monitoring]
    B --> E{Any Match?}
    C --> E
    D --> E
    E -- نعم --> F[Alert and Block]
    E -- لا --> G[Allow]
    D --> H[Is it doing something suspicious?]
    H -- نعم --> F
```

> [!IMPORTANT]
> الـ Behavioral Analysis هو الأهم دلوقتي. الـ Antivirus مش بس بيقارن Hashes، هو كمان بيراقب **إيه اللي البرنامج بيعمله**. لو برنامج بدأ يعمل Encrypt لملفات بدون إذن — هيتوقف حتى لو مش في قاعدة البيانات.

---

## Summary

**Defense:**
- Signature-Based لوحده مش كافي — بيفوته الـ Zero-Day
- الحل الحديث: Signature + Machine Learning + Behavioral Analysis
- دايماً راقب الـ Behavior مش بس الـ Hash
