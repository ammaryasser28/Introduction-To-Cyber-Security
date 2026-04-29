
## Table of Contents

- [Rule Structure](#21-rule-structure)
- [Default Policy](#22-default-policy)
- [SOC Analyst & Firewalls](#23-soc-analyst--firewalls)
- [Summary](#summary)


---

## Firewall Rules

### Rule Structure

كل Rule بتتكوّن من العناصر دي:

| العنصر | المعنى |
|--------|--------|
| **Action** | هل نسمح أو نمنع؟ (ALLOW / DENY) |
| **Protocol** | TCP، UDP، أو ANY |
| **Source** | مصدر الـ Packet (IP أو Network) |
| **Destination** | وجهة الـ Packet |
| **Port(s)** | رقم أو أرقام الـ Ports المتأثرة |

**أمثلة عملية على الـ Rules:**

```
ACTION   PROTOCOL   SOURCE            DESTINATION       PORT(S)
------   --------   ------            -----------       -------
ALLOW    TCP        Internal_Net      Internet          80, 443
DENY     ANY        Internet          Internal_Net      ANY
DENY     TCP        ANY               ANY               21
DENY     IP         Country_List      Internal_Net      ANY
DENY     UDP        Internal_Net      ANY               53
```

| الـ Rule | المعنى |
|---------|--------|
| `ALLOW TCP Internal_Net → Internet 80, 443` | اسمح للشبكة الداخلية تتصفح الـ Web (HTTP/HTTPS) |
| `DENY ANY Internet → Internal_Net` | امنع أي حاجة جاية من الـ Internet للشبكة الداخلية |
| `DENY TCP ANY → ANY PORT 21` | امنع الـ FTP على طول (Port 21 غير آمن) |
| `DENY IP Country_List → Internal_Net` | امنع دول معينة من الوصول للشبكة (Geo-blocking) |
| `DENY UDP Internal_Net → ANY PORT 53` | امنع الـ DNS Queries الخارجية (لأسباب أمنية) |

> [!WARNING]
> كلما زاد عدد الـ Rules على الـ Firewall، كلما زاد الـ **Latency** — لأن كل Packet لازم يعدي على كل الـ Rules واحدة واحدة لحد ما يلاقي Match. لازم تتوازن بين الأمان والـ Performance.

---

### Default Policy

| النوع | المعنى | التوصية |
|------|--------|---------|
| **Default Deny** | كل الـ Traffic محظور إلا اللي مسموح بيه صراحةً | ✅ الأكثر أماناً والموصى بيه |
| **Default Allow** | كل الـ Traffic مسموح إلا اللي محظور صراحةً | ❌ غير موصى بيه — ممكن يعدي Traffic ضار |

> [!IMPORTANT]
> الـ **Default Deny** هو الأساس في أي بيئة آمنة — بتسمح بس بالحد الأدنى اللي محتاجه، وباقي كل حاجة محظورة. ده بيقلل الـ Attack Surface بشكل كبير.

---

### SOC Analyst & Firewalls

كـ **SOC Analyst**، دورك مش كتابة الـ Rules (ده شغل الـ Network Security Engineer) — لكن لازم:

- تقرأ وتفهم الـ Firewall Rules
- تحلل الـ Firewall Logs
- تتحقق من الـ Alerts المتعلقة بالـ Traffic

**مثال عملي على تحقيق SOC:**

```
Alert: جهاز داخلي بيعمل DNS Requests متكررة لـ Servers خارجية غير معتمدة

الأسباب المحتملة:
  - DNS Tunneling
  - Data Exfiltration

خطوات الـ SOC:
  1. راجع الـ Firewall Rules الخاصة بالـ DNS (Port 53)
  2. تحقق من الـ DNS Servers المعتمدة
  3. افحص الجهاز المصدر
```

---
## Summary

- الـ **Firewall** هو الـ Gatekeeper بين الشبكة والـ Internet، وبيشتغل على أساس **Rules** بتحددها أنت. من غير Rules، هو مش قادر يحمي حاجة.

