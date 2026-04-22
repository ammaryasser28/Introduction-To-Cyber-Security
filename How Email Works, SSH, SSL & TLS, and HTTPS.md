> **الهدف من الـ Section ده:**  
> هتفهم إزاي الـ Email بيتبعت من شخص لشخص تاني خطوة بخطوة، وإيه دور الـ Mail Server والـ DNS في العملية دي، وهتشوف الـ SMTP من جوه على Wireshark. كمان هتعرف ليه الـ Telnet خطر وإزاي الـ SSH بيحل المشكلة دي. وفي الآخر هتفهم الفرق بين SSL وTLS وإزاي الـ HTTPS بيحمي بياناتك على الإنترنت.

---

# Lecture 8: How Email Works, SSH, SSL & TLS, and HTTPS

---

## Table of Contents

- [How Email Works](#how-email-works)
  - [المفهوم الأساسي](#المفهوم-الأساسي)
  - [خطوات إرسال الـ Email بالتفصيل](#خطوات-إرسال-الـ-email-بالتفصيل)
  - [البروتوكولات المستخدمة](#البروتوكولات-المستخدمة)
- [Important Lab — Email PCAP Analysis (Wireshark)](#important-lab--email-pcap-analysis-wireshark)
  - [Client to Mail Server (Ahmed's PC → MTA-1)](#client-to-mail-server-ahmeds-pc--mta-1)
  - [Server to Server Relaying (MTA-1 → MTA-2)](#server-to-server-relaying-mta-1--mta-2)
- [SSH — Secure Shell](#ssh--secure-shell)
- [SSL & TLS](#ssl--tls)
- [HTTPS](#https)
- [Summary](#summary)

---

## How Email Works

### المفهوم الأساسي

تخيل إنك بعتلت حد رسالة في الزمن القديم — إنت مش بتوصلها بنفسك، بتديها لـ **البريد** اللي بيوصلها. نفس الفكرة بالظبط بتحصل في الـ Email.

لما Ahmed بيكتب إيميل ويضغط Send، الكمبيوتر بتاعه **مش هو** اللي بيبعت الإيميل مباشرةً. في حاجة اسمها **Mail Server** بتعمل دور الـ Postman.

```
Ahmed@company.com  ──→  يبعت إيميل لـ  ──→  Aya@organization.com
```

كل شركة عندها **Mail Server** خاص بيها:
- **Company.com Mail Server** — بيخدم Ahmed
- **Organization.com Mail Server** — بيخدم Aya

---

### خطوات إرسال الـ Email بالتفصيل

```mermaid
sequenceDiagram
    participant A as Ahmed PC
    participant MS1 as Company Mail Server (MTA-1)
    participant DNS as DNS Server
    participant MS2 as Organization Mail Server (MTA-2)
    participant AY as Aya Outlook

    A->>MS1: SMTP (TCP Port 25) - Send Email
    MS1->>DNS: DNS Query for MX record of organization.com
    DNS-->>MS1: MX Record = IP of Organization Mail Server
    MS1->>MS2: SMTP (TCP Port 25) - Relay Email
    AY->>MS2: POP3 (TCP Port 110) - Retrieve Email
    MS2-->>AY: Deliver Email to Aya
```

**الخطوات واحدة واحدة:**

**1. Ahmed يكتب الإيميل ويضغط Send**
- الـ Email Client (زي Outlook) بيتصل بـ Company Mail Server على **TCP Port 25** باستخدام بروتوكول **SMTP**.

**2. Mail Server بتاع Ahmed بيشتغل**
- بيبص على الـ Recipient: `Aya@organization.com`
- بيلاقي إن المفروض يبعت لـ `organization.com`
- المشكلة: مش عارف عنوان الـ IP بتاع Mail Server بتاع Organization

**3. DNS Query عشان يعرف الـ MX Record**

> [!IMPORTANT]
> الـ DNS Query هنا مش بتسأل عن الـ Website بتاع organization.com، لكن بتسأل عن الـ **MX Record** (Mail Exchange Record). الـ MX Record ده هو اللي بيدي الـ IP بتاع الـ Mail Server مش الـ Web Server.

**4. الـ Server-to-Server Transfer (Relaying)**
- بعد ما MTA-1 جاب الـ MX Record، فتح connection على **TCP Port 25** مع MTA-2
- بعتله الإيميل — العملية دي اسمها **Relaying**

**5. Aya بتفتح الإيميل**
- Aya بتفتح Outlook، هو بيتصل بـ Organization Mail Server على **TCP Port 110**
- بيجيب الإيميل باستخدام بروتوكول **POP3**

---

### البروتوكولات المستخدمة

| Protocol | Full Name | Port | الوظيفة |
|----------|-----------|------|---------|
| **SMTP** | Simple Mail Transfer Protocol | TCP 25 | إرسال الإيميل (Client→Server & Server→Server) |
| **POP3** | Post Office Protocol v3 | TCP 110 | استلام الإيميل من الـ Mail Server |
| **DNS MX** | Mail Exchange Record | UDP/TCP 53 | معرفة عنوان الـ Mail Server المقصود |

> [!NOTE]
> في بروتوكول تاني بيُستخدم لاسترجاع الإيميل اسمه **IMAP (TCP Port 143)**، وهو أحدث من POP3 ومش بيمسح الرسائل من الـ Server. لكن اللي اتشرح في المحاضرة دي هو POP3.

---

## Important Lab — Email PCAP Analysis (Wireshark)

### إعداد الـ Lab

| الجهاز | IP Address | الدور |
|--------|-----------|-------|
| Ahmed's PC | 10.0.0.10 | المُرسِل (Sender) |
| Company Mail Server (MTA-1) | 10.0.0.20 | Mail Server بتاع Ahmed |
| Organization Mail Server (MTA-2) | 10.0.0.30 | Mail Server بتاع Aya |
| Sender Email | ahmed@company.com | — |
| Recipient Email | aya@othercompany.com | — |

> [!TIP]
> في الـ Lab ده هنفترض إن كل Server عارف الـ IP بتاع التاني، عشان نتجنب خطوة الـ DNS ونركز على تفاصيل الـ SMTP.

---

### Client to Mail Server (Ahmed's PC → MTA-1)

```mermaid
sequenceDiagram
    participant A as Ahmed PC (10.0.0.10)
    participant S as Mail Server (10.0.0.20)

    A->>S: SYN (TCP Port 25)
    S-->>A: SYN-ACK
    A->>S: ACK  [3-Way Handshake Done]

    S-->>A: 220 mail.company.com ESMTP Ready
    A->>S: EHLO ahmed-pc.company.com
    S-->>A: 250 Capabilities List (AUTH, SIZE, 8BITMIME...)

    A->>S: MAIL FROM:<ahmed@company.com>
    S-->>A: 250 OK

    A->>S: RCPT TO:<aya@othercompany.com>
    S-->>A: 250 OK

    A->>S: DATA
    S-->>A: 354 End data with dot on single line

    A->>S: [Message Body + single dot]
    S-->>A: 250 Message queued

    A->>S: QUIT / FIN
    S-->>A: FIN/ACK [Session Closed]
```

**شرح كل خطوة:**

**الـ 3-Way Handshake**
- أول 3 Packets هي الـ TCP Handshake المعتادة على **Port 25**
- بعد ما الـ Connection اتفتح، الـ Server يعلن عن نفسه

**`220` — Server Greeting**
- `220 mail.company.com ESMTP Ready`
- الـ `220` ده Response Code يعني "الـ Server جاهز"

**`EHLO` — Client Identity**
- `EHLO ahmed-pc.company.com`
- الـ Client بيعرّف بنفسه وبيطلب قائمة الـ Capabilities بتاعة الـ Server

**`250` — Server Capabilities**
- الـ Server بيرد بقائمة من الـ Capabilities زي: حجم أقصى للإيميل، هل بيدعم Authentication، هل بيدعم Encryption...

**`MAIL FROM`**
- بيحدد المُرسِل: `MAIL FROM:<ahmed@company.com>`
- ده اسمه **Envelope Sender** — يعني زي ما بتكتب اسمك على الظرف

**`RCPT TO`**
- بيحدد المستلم: `RCPT TO:<aya@othercompany.com>`
- الـ Server بيرد بـ `250 OK` لو الـ Recipient مقبول

**`DATA` و الرسالة**
- الـ Client بيبعت أمر `DATA`
- الـ Server بيرد بـ `354` يعني "ابعت الرسالة وخلّيها تنتهي بنقطة منفردة على سطر لوحدها"
- الرسالة المبعوتة:

```
Subject: Hello Aya
From: ahmed@company.com
To: aya@othercompany.com

Hello Aya,
This is a test mail.
Regards,
Ahmed
.
```

> [!IMPORTANT]
> الـ `.` (نقطة منفردة على سطر) دي هي العلامة اللي بتقول للـ Server إن الرسالة خلصت. ده Standard في SMTP.

**إنهاء الـ Session**
- الـ Server بيرد `250 Message queued` يعني الرسالة اتقبلت وفي الـ Queue
- Ahmed بيبدأ الـ FIN Sequence لإنهاء الـ TCP Connection

> [!NOTE]
> الـ Server بعت الـ `FIN/ACK` في Packet واحدة بدل اتنين — ده اسمه **Piggybacking** وبيحصل لما الـ Server يقرر يعمل ACK وFIN في نفس الوقت.

---

### Server to Server Relaying (MTA-1 → MTA-2)

```mermaid
sequenceDiagram
    participant S1 as MTA-1 (10.0.0.20)
    participant S2 as MTA-2 (10.0.0.30)

    S1->>S2: SYN (TCP Port 25)
    S2-->>S1: SYN-ACK
    S1->>S2: ACK [New TCP Session]

    S2-->>S1: 220 mta2.othercompany.com ESMTP Exim
    S1->>S2: EHLO mta1.company.com
    S2-->>S1: 250 Capabilities

    S1->>S2: MAIL FROM:<ahmed@company.com>
    S2-->>S1: 250 OK

    S1->>S2: RCPT TO:<aya@othercompany.com>
    S2-->>S1: 250 OK

    S1->>S2: DATA + Message Body + dot
    S2-->>S1: 250 Accepted

    S1->>S2: FIN [Session Closed]
```

**مفاهيم مهمة في الـ Relaying:**

**ما هو الـ MTA؟**
- **MTA = Mail Transfer Agent**
- ده الـ Server-side Software المسؤول عن إرسال، استقبال، وتوجيه الإيميلات بين الـ Mail Servers
- أمثلة شهيرة: **Postfix، Sendmail، Exim، Microsoft Exchange**

**ليه في Handshake تاني؟**
- الـ TCP Connection الأولانية كانت بين **Ahmed's PC** و**MTA-1**
- دلوقتي MTA-1 بيفتح **Connection جديد خالص** مع MTA-2 — عشان كده في Handshake تاني
- نفس بروتوكول SMTP، نفس Port 25

**`220 mta2.othercompany.com ESMTP Exim`**
- Exim ده اسم الـ MTA Software اللي شغّال على MTA-2
- `ESMTP` = Extended SMTP (نسخة موسّعة بتدعم Capabilities إضافية)

**`EHLO` و `250` Capabilities**
- نفس العملية — الـ Servers بيتفاوضوا على الـ Capabilities قبل ما يبدأوا
- مهم عشان بيحددوا هل هيعملوا Authentication ولا لأ، وإيه الـ Features المتاحة

> [!WARNING]
> الـ SMTP في المثال ده **مش مُشفَّر** — كل حاجة بتتبعت بـ Cleartext. في الـ Real World لازم تُستخدم **SMTPS (Port 465)** أو **STARTTLS (Port 587)** لحماية الإيميلات أثناء النقل.

---

## SSH — Secure Shell

### المشكلة: Telnet

**Telnet** كان بروتوكول قديم بيُستخدم للـ Remote Connection — يعني تتحكم في جهاز تاني من بُعد عبر الـ CLI.

المشكلة الكبيرة إن Telnet بيبعت **كل حاجة بـ Cleartext**:
- الـ Username
- الـ Password
- كل الأوامر اللي بتكتبها
- كل الـ Output اللي بترجع

يعني لو في Attacker عمل **Packet Sniffing** على الشبكة، هيشوف كل حاجة بالحرف الواحد!

```mermaid
graph LR
    A[Admin PC] -->|Username + Password in Cleartext| B[Telnet Server]
    C[Attacker] -->|Sniffs the traffic| A
    C -->|Reads credentials easily| B
```

---

### الحل: SSH (Secure Shell)

**SSH** هو النسخة المُشفَّرة من Telnet — بيعمل نفس الوظيفة (Remote CLI Access) لكن بيشفّر كل حاجة.

| الخاصية | Telnet | SSH |
|---------|--------|-----|
| **التشفير** | لا — Cleartext | نعم — Encrypted بالكامل |
| **Port** | TCP 23 | TCP 22 |
| **الأمان** | غير آمن | آمن |
| **الاستخدام** | متروك (Legacy) | الـ Standard الحالي |
| **حماية الـ Password** | مكشوف | مُشفَّر |

> [!IMPORTANT]
> SSH بيشفّر **كل حاجة**: الـ Username، الـ Password، الأوامر، والـ Output. حتى لو Attacker عمل Packet Capture، هيلاقي Gibberish — مش هيعرف يقرأ حاجة.

> [!TIP]
> في الـ Real World، SSH مش بس بيُستخدم لـ Remote Login — ممكن كمان تعمل بيه **Secure File Transfer (SFTP/SCP)** و**Port Forwarding** و**Tunneling**.

---

## SSL & TLS

### العلاقة بينهم

- **SSL (Secure Sockets Layer)** — الإصدار القديم، اتطوّر على مدار سنين
- **TLS (Transport Layer Security)** — الإصدار الأحدث والأكثر أماناً، هو في الأساس SSL بنسخة أحسن
- الاتنين بيعملوا نفس الوظيفة: **تشفير الاتصالات على مستوى الـ Transport Layer**

> [!NOTE]
> في الواقع العملي لما الناس بتقول "SSL"، هم في الغالب بيتكلموا عن "TLS" — لأن SSL الآن اعتُبر Deprecated وغير آمن، وTLS هو اللي بيُستخدم فعلياً.

---

### يشتغلوا فين؟

```mermaid
graph TD
    A[Application Layer - HTTP, FTP, SMTP...]
    B[Transport Layer - SSL / TLS]
    C[Network Layer - IP]
    D[Data Link Layer]

    A --> B
    B --> C
    C --> D
```

SSL/TLS بيشتغلوا على **OSI Layer 4 (Transport Layer)**.

ده معناه إنهم ممكن يشفّروا **أي بروتوكول بيشتغل فوق TCP**:

| البروتوكول | النسخة المُشفَّرة | Port |
|-----------|-----------------|------|
| HTTP | **HTTPS** | 443 |
| FTP | **FTPS** | 990 |
| SMTP | **SMTPS** | 465 |
| IMAP | **IMAPS** | 993 |
| POP3 | **POP3S** | 995 |

---

### الغلطة الشائعة جداً!

> [!WARNING]
> **الغلطة الشائعة:** كتير من الناس بيقولوا إن SSL/TLS بيشتغلوا على **TCP Port 443** — ده غلط!
>
> **الصح:** الـ **Port 443** هو Port بتاع **HTTPS** (يعني HTTP + TLS) — مش بتاع TLS نفسه.
>
> الـ SSL/TLS هم **Transport Layer Protocols**، وبروتوكولات الـ Transport Layer مش بيتعيّنلها Port Numbers. الـ Port Numbers دي بتتعيّن فقط لـ **Application Layer Protocols**.

---

## HTTPS

### ما هو HTTPS؟

**HTTPS = HTTP + SSL/TLS**

يعني هو بروتوكول الـ HTTP العادي بتاع الويب، لكن مُشفَّر باستخدام SSL أو TLS.

```mermaid
graph LR
    A[Browser] -->|Encrypted HTTPS - TCP Port 443| B[Web Server]
    B -->|Encrypted Response| A
    C[Attacker] -->|Sees only encrypted data - useless| A
```

### خصائص HTTPS

- **Port:** TCP 443
- **التشفير:** بيشفّر **كل حاجة** بين الـ Browser والـ Web Server في الاتجاهين
- **الـ Certificate:** الـ Web Server لازم يكون عنده **SSL/TLS Certificate** صادر من **CA (Certificate Authority)** موثوقة
- **المؤشر:** في المتصفح بيظهر **القفل (🔒)** في الـ Address Bar

> [!IMPORTANT]
> لما بتشوف `https://` وقفل في المتصفح، ده معناه إن:
> 1. الاتصال مُشفَّر — محدش يقدر يقرأه
> 2. الـ Server عنده Certificate موثوق — أنت فعلاً بتكلم الموقع الصح مش Fake

> [!WARNING]
> وجود الـ HTTPS مش معناه إن الموقع **آمن أو موثوق** — معناه بس إن الاتصال **مُشفَّر**. مواقع الـ Phishing كمان بتستخدم HTTPS!

---

## Summary

### ملخص المحاضرة الثامنة

#### How Email Works
- الإيميل مش بيتبعت من جهاز لجهاز مباشرة — بيمر عبر **Mail Servers** تعمل دور الـ Postman
- بروتوكول الإرسال هو **SMTP على TCP Port 25**
- الـ Mail Server بيستخدم **DNS MX Record** عشان يعرف عنوان Mail Server الطرف التاني
- عملية نقل الإيميل بين سيرفرين اسمها **Relaying** والـ Software المسؤول اسمه **MTA**
- Aya بتستلم إيميلها عبر **POP3 على TCP Port 110**

#### Lab Highlights (Wireshark)
- أول 3 Packets دايماً هي الـ **3-Way Handshake** على Port 25
- تسلسل SMTP: `EHLO` → `MAIL FROM` → `RCPT TO` → `DATA` → `250 OK` → `QUIT`
- الرسالة بتنتهي بـ **نقطة منفردة على سطر** (`.`)
- الـ Server Codes المهمة: `220` (Ready)، `250` (OK)، `354` (Start Data)

#### SSH
- **Telnet** = Remote CLI بدون تشفير — كل حاجة Cleartext على TCP 23
- **SSH** = نفس الوظيفة لكن مُشفَّر بالكامل على TCP 22
- SSH هو الـ Standard الحالي وTelnet متروك تماماً

#### SSL & TLS
- **SSL** قديم، **TLS** هو النسخة الأحدث والأكثر أماناً
- بيشتغلوا على **OSI Layer 4** ويقدروا يشفّروا أي TCP Protocol
- Port 443 مش بتاع TLS — هو بتاع HTTPS تحديداً

#### HTTPS
- **HTTP + TLS** = **HTTPS على TCP Port 443**
- بيشفّر كل الـ Traffic في الاتجاهين
- القفل في المتصفح يعني التشفير موجود — مش بالضرورة إن الموقع آمن أو موثوق


