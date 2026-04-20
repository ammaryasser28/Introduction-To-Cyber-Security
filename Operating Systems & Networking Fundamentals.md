> **الهدف من الـ Section ده:**
> هتفهم إيه هو الـ Operating System وإزاي بيدير الـ Resources، وهتعرف الفرق بين الـ HDD والـ RAM وليه ده مهم جداً في الـ Digital Forensics. كمان هتاخد overview عن الـ Networking الأساسية — إيه الفرق بين الـ LAN والـ WAN، إزاي الـ Router والـ Switch بيشتغلوا، وإيه هو الـ ARP Protocol وإزاي الـ Packets بتتنقل على الشبكة.

---

## Table of Contents

- [What is an Operating System?](#what-is-an-operating-system)
- [OS As a Resource Manager](#os-as-a-resource-manager)
- [OS As an Interface Provider](#os-as-an-interface-provider)
- [OS As a System Coordinator](#os-as-a-system-coordinator)
- [The Kernel](#the-kernel)
- [Operating System Types](#operating-system-types)
- [HDD vs RAM](#hdd-vs-ram)
- [Networking Overview](#networking-overview)
- [LAN and WAN](#lan-and-wan)
- [Router vs Switch](#router-vs-switch)
- [ARP Protocol](#arp-protocol)
- [Packets](#packets)

---

## What is an Operating System?

الـ **Operating System (OS)** هو الـ Software الأساسي اللي بيربط الـ Hardware بالـ User والـ Applications. من غيره، الكمبيوتر مجرد قطع إلكترونية من غير روح.

```
بدون OS:  [ Hardware ] ← لا يوجد تواصل → [ User ]
مع OS:    [ Hardware ] ←→ [ OS ] ←→ [ User & Applications ]
```

الـ OS بيلعب 3 أدوار أساسية:

```mermaid
graph TD
    OS[Operating System]
    OS --> A[Resource Manager]
    OS --> B[Interface Provider]
    OS --> C[System Coordinator]
```

> **ملاحظة مهمة:** كل جهاز Hardware — سواء كمبيوتر، سيرفر، موبايل، أو راوتر — بيشتغل بـ Operating System. حتى أجهزة الـ IoT عندها نوع من الـ OS.

---

## OS As a Resource Manager

الـ OS بيتحكم في كل الـ Resources الموجودة على الجهاز — سواء **Physical** زي الـ RAM والـ CPU، أو **Logical** زي التقسيمات (Partitions).

```mermaid
graph LR
    OS[OS - Resource Manager]
    OS --> CPU[CPU Management]
    OS --> MEM[Memory Management]
    OS --> STOR[Storage Management]
    OS --> IO[I/O Device Management]

    CPU --> CPU1[تحديد أي Process بتشتغل]
    CPU --> CPU2[تحديد مدة الـ Process]
    MEM --> MEM1[تخصيص مساحة لكل برنامج]
    MEM --> MEM2[عزل البرامج عن بعض]
    MEM --> MEM3[Swap in/out من الـ RAM]
    STOR --> STOR1[تنظيم الملفات على الـ Drive]
    STOR --> STOR2[تتبع المساحة الحرة والمستخدمة]
    IO --> IO1[التحكم في Printer, Keyboard, Network]
    IO --> IO2[منع Conflicts بين الأجهزة]
```

### تفاصيل كل Resource:

| Resource | دور الـ OS |
|----------|------------|
| **CPU** | يحدد أي Process تشتغل، ولأد إيه، وبأي ترتيب — ده اللي بيتسمى Scheduling |
| **Memory (RAM)** | يخصص مساحة لكل برنامج، يعزلهم عن بعض، ويعمل Swap لو المساحة اتملت |
| **Storage (HDD/SSD)** | ينظم الملفات، يتتبع المساحة الفاضية، ويتحكم في الـ File Access |
| **I/O Devices** | يدير التواصل مع الـ Keyboard, Mouse, Printer, Network Card ويمنع التعارض |

> **مثال عملي:** لما بتفتح Chrome و Word في نفس الوقت، الـ OS هو اللي بيوزع الـ CPU والـ RAM عليهم عشان محدش يأكل على التاني.

---

## OS As an Interface Provider

الـ OS بيوفر طريقتين للتعامل مع الجهاز:

```mermaid
graph TD
    OS[OS - Interface Provider]
    OS --> GUI[Graphical User Interface - GUI]
    OS --> CLI[Command-Line Interface - CLI]

    GUI --> G1[سهل للمستخدم العادي]
    GUI --> G2[Windows Desktop, Mac Finder]

    CLI --> C1[قوي جداً للـ IT والـ Security Professionals]
    CLI --> C2[Terminal, PowerShell, CMD]
    CLI --> C3[أسرع وأقوى من الـ GUI]
```

### GUI vs CLI

| الخاصية | GUI | CLI |
|---------|-----|-----|
| **الاستخدام** | مستخدمين عاديين | IT & Cybersecurity Professionals |
| **السهولة** | سهل جداً | يحتاج تعلم |
| **القوة** | محدود | قوي جداً وغير محدود |
| **السرعة** | أبطأ | أسرع بكتير |
| **الـ Automation** | صعب | سهل جداً (Scripts) |

> **ليه الـ Cybersecurity Professional بيستخدم الـ CLI؟**
> لأن الـ CLI بيديه تحكم كامل في الجهاز، بيقدر يكتب Scripts تشتغل أوتوماتيك، وبيقدر يوصل لأي جزء في الـ System مش متاح من الـ GUI. في الـ Incident Response مثلاً، كل ثانية بتفرق — والـ CLI أسرع وأدق.

---

## OS As a System Coordinator

الـ OS مش بس بيدير الـ Resources، هو كمان بيلعب دور الـ Coordinator بين كل أجزاء الـ System:

```mermaid
graph TD
    OS[OS - System Coordinator]
    OS --> FM[File Management]
    OS --> SM[Security Management]
    OS --> NET[Networking]
    OS --> MON[System Monitoring]

    FM --> FM1[Create, Read, Write, Delete Files]
    SM --> SM1[User Access Control]
    SM --> SM2[Permissions and Authentication]
    NET --> NET1[تواصل الأجهزة مع بعض]
    NET --> NET2[مشاركة البيانات بأمان]
    MON --> MON1[تتبع الـ Performance]
    MON --> MON2[تسجيل الـ Logs]
    MON --> MON3[اكتشاف الـ Errors]
```

| الوظيفة | الوصف |
|---------|-------|
| **File Management** | إنشاء وقراءة وكتابة وتنظيم الملفات |
| **Security Management** | التحكم في مين يدخل إيه — Permissions و Authentication |
| **Networking** | تمكين الأجهزة من التواصل ومشاركة البيانات بأمان |
| **System Monitoring** | مراقبة الأداء، تسجيل الـ Logs، واكتشاف المشاكل |

---

## The Kernel

الـ **Kernel** هو قلب الـ Operating System. هو أول حاجة بتتحمل لما الجهاز بيشتغل، وآخر حاجة بتتقفل لما بيتقفل.

```mermaid
graph TD
    USER[User Applications]
    OS_SERVICES[OS Services]
    KERNEL[Kernel]
    HARDWARE[Hardware - CPU, RAM, Disk]

    USER --> OS_SERVICES
    OS_SERVICES --> KERNEL
    KERNEL --> HARDWARE
```

### دور الـ Kernel:

- **الـ Bridge الآمن:** بيوصل برامج الـ User بالـ Hardware من غير ما يديهم وصول مباشر (عشان الأمان)
- **أول ما يشتغل:** يتحمل مع بداية الـ Boot
- **آخر ما يتقفل:** بيفضل شغال لحد ما الجهاز يتقفل خالص
- **بيتحكم في كل حاجة:** كل عملية على الجهاز بتعدي من الـ Kernel

> **تشبيه:** الـ Kernel زي مدير المبنى — أي حد عايز يدخل غرفة (Hardware)، لازم يعدي عليه الأول ويأخد إذن. من غيره، الكل هيدخل على بعض ويحصل فوضى.

---

## Operating System Types

```mermaid
graph LR
    OS_Types[OS Types]
    OS_Types --> Windows
    OS_Types --> Linux
    OS_Types --> MacOS[Mac OS]
    OS_Types --> Others[Others - Android, iOS, etc.]

    Windows --> W1[Mostly used for Clients]
    Windows --> W2[Common in corporate environments]
    Linux --> L1[Mostly used for Servers]
    Linux --> L2[Web Servers]
    Linux --> L3[Supercomputers]
    Linux --> L4[Most powerful systems run Linux]
```

### مقارنة بين الـ Windows والـ Linux

| الخاصية | Windows | Linux |
|---------|---------|-------|
| **الاستخدام الأساسي** | Clients & Desktops | Servers & Supercomputers |
| **الـ Market Share (Servers)** | أقل | الأكثر انتشاراً |
| **الـ Security** | أكثر استهدافاً | أكثر أماناً بطبيعته |
| **الـ Cost** | مدفوع | مجاني (في معظمه) |
| **الـ CLI** | PowerShell / CMD | Bash (أقوى بكتير) |
| **التخصيص** | محدود | لا نهائي |

> **سؤال مهم:** إيه الـ OS الأكثر استخداماً في العالم ولماذا؟
>
> **الجواب:** على مستوى الـ Servers والـ Supercomputers — **Linux** هو الملك. أقوى 500 كمبيوتر في العالم بيشتغلوا على Linux. أما على مستوى الـ Desktop للمستخدم العادي، فـ Windows هو الأكثر انتشاراً.

> **تحذير أمني مهم:** الـ Linux Vulnerabilities ممكن تكون **كارثية جداً**، لأن معظم السيرفرات والبنية التحتية الحساسة (Banking، Government، Cloud) بتشتغل على Linux. ثغرة واحدة فيه ممكن تأثر على ملايين الأجهزة في وقت واحد.

---

## HDD vs RAM

ده موضوع مهم جداً للـ **SOC Analyst** وخصوصاً في الـ **Digital Forensics** — لأنك لازم تعرف **فين** و**إزاي** البيانات بتتخزن عشان تعرف تلاقيها وقت التحقيق.

```mermaid
graph TD
    STORAGE[Storage Types]
    STORAGE --> HDD[HDD - Hard Disk Drive]
    STORAGE --> RAM[RAM - Random Access Memory]

    HDD --> H1[Permanent Storage]
    HDD --> H2[بيفضل فيه البيانات حتى لو الجهاز اتقفل]
    HDD --> H3[بطيء نسبياً]
    HDD --> H4[سعة كبيرة جداً]

    RAM --> R1[Temporary Storage]
    RAM --> R2[بيتمسح لما الجهاز يتقفل]
    RAM --> R3[سريع جداً]
    RAM --> R4[سعة أصغر]
```

### الفرق الجوهري:

| الخاصية | HDD | RAM |
|---------|-----|-----|
| **نوع التخزين** | Permanent (دائم) | Temporary (مؤقت) |
| **البيانات بعد الإغلاق** | بتفضل موجودة | بتتمسح خالص |
| **السرعة** | أبطأ | أسرع بكتير |
| **الحجم النموذجي** | 500GB - 10TB | 4GB - 64GB |
| **الاستخدام** | تخزين الملفات والـ Software | تشغيل البرامج حالياً |

### مثال عملي: Microsoft Word

```mermaid
sequenceDiagram
    participant HDD as HDD
    participant OS as Operating System
    participant RAM as RAM
    participant USER as User

    Note over HDD: Word.exe مخزن هنا بشكل دائم
    USER->>OS: بيضغط على أيقونة Word
    OS->>HDD: اقرأ ملف Word.exe
    HDD->>RAM: نسخة من Word.exe اتحملت هنا
    RAM->>USER: Word فتح وشغال
    Note over RAM: لو الجهاز اتقفل، النسخة دي بتتمسح
    Note over HDD: Word.exe لسه موجود هنا
```

> **ليه ده مهم في الـ Digital Forensics؟**
>
> لما تعمل تحقيق على جهاز، البيانات الموجودة في الـ RAM (زي الـ Passwords المفتوحة، الـ Encryption Keys، الـ Active Connections) **هتتمسح لما الجهاز يتقفل**. عشان كده، الـ Forensic Analyst لازم يعمل **RAM Dump** قبل أي حاجة تانية — عشان ميضيعش الأدلة اللي في الـ RAM.

---

## Networking Overview

الـ Networking هو الأساس اللي الـ Cybersecurity بيتبنى عليه. مش ممكن تحمي حاجة مش فاهمها.

```mermaid
graph TD
    NET[Networking Basics]
    NET --> LAN_WAN[LAN and WAN]
    NET --> DEVICES[Network Devices]
    NET --> PROTO[Protocols]
    NET --> DATA[Data Transfer]

    DEVICES --> SWITCH[Switch - Layer 2]
    DEVICES --> ROUTER[Router - Layer 3]
    PROTO --> ARP[ARP Protocol]
    DATA --> PACKETS[Packets]
```

---

## LAN and WAN

### LAN - Local Area Network

الـ **LAN** هي شبكة محلية بتغطي منطقة صغيرة — زي مكتب، مدرسة، أو طابق في مبنى.

- ممكن تكون من جهاز واحد أو أكتر
- لما بتقسم الـ LAN لشبكات أصغر، كل جزء بيتسمى **Subnet**
- الـ Subnets بتتوصل ببعض عشان تكوّن الـ LAN الأكبر
- معظم كلامنا في الـ Course ده هيكون عن الـ LAN

```mermaid
graph TD
    LAN[LAN - Local Area Network]
    LAN --> S1[Subnet 1 - Floor 1]
    LAN --> S2[Subnet 2 - Floor 2]
    LAN --> S3[Subnet 3 - Floor 3]

    S1 --> PC1[PC 1]
    S1 --> PC2[PC 2]
    S2 --> PC3[PC 3]
    S2 --> PC4[PC 4]
    S3 --> PC5[PC 5]
```

### WAN - Wide Area Network

الـ **WAN** بتغطي منطقة جغرافية كبيرة — زي دولة أو قارة.

- الشركات الكبيرة بتستخدمها عشان توصل LANs في أماكن مختلفة ببعض
- الـ Internet هو أكبر مثال على الـ WAN

```mermaid
graph LR
    HQ[Headquarters - Cairo LAN]
    BR1[Branch 1 - Alex LAN]
    BR2[Branch 2 - Dubai LAN]

    HQ -- WAN Link --> BR1
    HQ -- WAN Link --> BR2
```

### LAN vs WAN

| الخاصية | LAN | WAN |
|---------|-----|-----|
| **المساحة الجغرافية** | صغيرة (مبنى، حرم جامعي) | كبيرة (دول، قارات) |
| **السرعة** | عالية جداً | أبطأ نسبياً |
| **التكلفة** | رخيصة | غالية |
| **الأمان** | أسهل في التحكم | أصعب وأعقد |
| **مثال** | شبكة المكتب | الـ Internet |

---

## Router vs Switch

```mermaid
graph TD
    SWITCH[Switch - Layer 2]
    ROUTER[Router - Layer 3]

    SWITCH --> SW1[يوصل الأجهزة ببعض داخل الشبكة]
    SWITCH --> SW2[يفهم MAC Addresses]
    SWITCH --> SW3[يوجه الـ Traffic بالـ MAC]
    SWITCH --> SW4[PCs, Servers, Printers]

    ROUTER --> RO1[يوصل شبكات ببعض]
    ROUTER --> RO2[يفهم IP Addresses]
    ROUTER --> RO3[يوجه الـ Traffic بالـ IP]
    ROUTER --> RO4[يوصل Switches ببعض]
```

### الفرق الجوهري:

| الخاصية | Switch | Router |
|---------|--------|--------|
| **الـ OSI Layer** | Layer 2 (Data Link) | Layer 3 (Network) |
| **بيفهم إيه** | MAC Addresses | IP Addresses |
| **بيوصل إيه** | أجهزة داخل نفس الشبكة | شبكات مختلفة ببعض |
| **الـ Traffic** | داخل الـ LAN | بين الـ LANs أو للـ Internet |
| **مثال** | سويتش في مكتب | الراوتر في البيت |

```mermaid
graph LR
    PC1[PC 1] --> SW[Switch]
    PC2[PC 2] --> SW
    PC3[PC 3] --> SW
    SW --> RT[Router]
    RT --> INTERNET[Internet / WAN]
    RT --> SW2[Switch - Branch 2]
```

> **تشبيه:** الـ Switch زي الـ Receptionist داخل الشركة — بيعرف كل موظف (MAC Address) وبيوصل الرسائل بينهم. أما الـ Router فزي البريد (Post Office) — بيعرف العناوين (IP Addresses) وبيوصل الرسائل لشبكات تانية خارج الشركة.

---

## ARP Protocol

### إيه هو الـ ARP؟

الـ **ARP (Address Resolution Protocol)** هو البروتوكول المسؤول عن ترجمة الـ **IP Address** لـ **MAC Address** داخل الـ LAN.

**ليه ده ضروري؟**

- الـ Data بتتوجه على أساس الـ IP Address
- لكن التوصيل الفعلي على الشبكة بيتم على أساس الـ MAC Address
- إذن لازم نعرف الـ MAC بتاع الجهاز اللي عنده الـ IP ده

```mermaid
sequenceDiagram
    participant Sys2 as Sys2
    participant Network as Network - Broadcast
    participant Gateway as Net1 Gateway

    Note over Sys2: عايزة تبعت data للـ Gateway
    Note over Sys2: تعرف الـ IP بس مش تعرف الـ MAC

    Sys2->>Network: ARP Broadcast - من عنده IP كذا؟ قولي الـ MAC بتاعك
    Note over Network: كل الأجهزة شافت الـ Broadcast
    Network->>Gateway: الـ Broadcast وصل
    Gateway->>Sys2: الـ Reply - أنا صاحب الـ IP ده، والـ MAC بتاعي هو كذا
    Sys2->>Sys2: حفظ المعلومة في الـ ARP Cache
    Note over Sys2: دلوقتي تقدر تبعت الـ Data للـ Gateway مباشرة
```

### الـ ARP Cache

بعد ما الـ Sys2 تاخد الرد، بتحطه في حاجة اسمها **ARP Cache** أو **ARP Table** — وهي موجودة في الـ **RAM**.

- الـ ARP Cache بيخزن الـ IP-to-MAC mapping مؤقتاً
- الجهاز بيرجع ليها قبل ما يبعت ARP Request تانية
- مدة الحفظ بتختلف من OS لـ OS

```mermaid
graph LR
    ARP_REQ[ARP Request Broadcast]
    ARP_REP[ARP Reply]
    ARP_CACHE[ARP Cache in RAM]

    ARP_REQ -->|من عنده IP X؟| NET[Network]
    NET -->|الـ Reply من صاحب الـ IP| ARP_REP
    ARP_REP --> ARP_CACHE
    ARP_CACHE -->|للمرة الجاية بيستخدمها مباشرة| DONE[Direct Communication]
```

### تمرين عملي

**اعرض الـ ARP Cache على جهازك:**

```bash
# على Windows:
arp -a

# على Linux/Mac:
arp -a
# أو
ip neigh show
```

**مثال على الـ Output:**

```
Interface: 192.168.1.5
  Internet Address      Physical Address      Type
  192.168.1.1           00-1a-2b-3c-4d-5e     dynamic
  192.168.1.10          00-aa-bb-cc-dd-ee     dynamic
```

> **أهمية الـ ARP في الـ Cybersecurity:**
>
> الـ ARP Protocol فيه ثغرة معروفة اسمها **ARP Spoofing** أو **ARP Poisoning** — فيها الـ Attacker بيبعت ARP Replies مزيفة عشان يربط الـ IP بتاعه بـ MAC Address تاني، فيخلي كل الـ Traffic يعدي عليه (Man-in-the-Middle Attack). ده بيخليه أحد أخطر الهجمات داخل الـ LAN.

---

## Packets

### إيه هو الـ Packet؟

الـ **Packet** هو الوحدة الأساسية اللي البيانات بتتحرك فيها على الشبكة.

```mermaid
graph LR
    PACKET[Packet]
    PACKET --> HEADER[Header]
    PACKET --> PAYLOAD[Payload - Data]

    HEADER --> H1[Source IP Address - مصدر البيانات]
    HEADER --> H2[Destination IP Address - وجهة البيانات]
    HEADER --> H3[Protocol Type]
    HEADER --> H4[Packet Number - رقم الـ Packet في التسلسل]

    PAYLOAD --> P1[البيانات الفعلية]
    PAYLOAD --> P2[جزء من الملف أو الرسالة]
```

### مكونات الـ Packet:

| الجزء | الاسم | الوصف |
|-------|-------|-------|
| الجزء الأول | **Header** | معلومات عن الـ Packet — من فين، رايح فين، ونوعه |
| الجزء التاني | **Payload** | البيانات الفعلية اللي الـ Packet بيحملها |

### إزاي البيانات بتتبعت؟

لما بتبعت Email أو ملف أو صفحة ويب، البيانات مش بتتبعت في Packet واحد — بتتقسم لآلاف أو ملايين الـ Packets.

```mermaid
sequenceDiagram
    participant SENDER as Sender
    participant NETWORK as Network
    participant RECEIVER as Receiver

    Note over SENDER: ملف كبير - 100MB مثلاً
    SENDER->>NETWORK: Packet 1 - Header + Chunk 1
    SENDER->>NETWORK: Packet 2 - Header + Chunk 2
    SENDER->>NETWORK: Packet 3 - Header + Chunk 3
    Note over NETWORK: الـ Packets ممكن تسلك طرق مختلفة
    NETWORK->>RECEIVER: Packets بتوصل
    RECEIVER->>RECEIVER: بيجمع الـ Packets ترتيب صح
    Note over RECEIVER: الملف اتجمع تاني
```

> **ملاحظة مهمة:** الـ Packets مش لازم توصل بنفس الترتيب اللي اتبعتت بيه. الـ Receiver هو اللي بيرتبها صح في الآخر باستخدام الـ Packet Numbers الموجودة في الـ Header.

### ليه الـ Packets مهمة في الـ Cybersecurity؟

الـ **Packet Analysis** أو **Packet Sniffing** هو أحد أهم مهارات الـ SOC Analyst. بنستخدم Tools زي **Wireshark** عشان:

- نشوف إيه اللي بيتبعت على الشبكة
- نكتشف الـ Suspicious Traffic
- نحلل الهجمات بعد ما تحصل (Post-Incident Analysis)
- نفهم سلوك الـ Malware على الشبكة

---

## ملخص عام للـ Lecture

```mermaid
graph TD
    L2[Lecture 2 - OS and Networking]

    L2 --> OS[Operating System]
    L2 --> NET[Networking]

    OS --> RM[Resource Manager - CPU, RAM, Storage, I/O]
    OS --> IP[Interface Provider - GUI and CLI]
    OS --> SC[System Coordinator - Files, Security, Monitoring]
    OS --> KR[Kernel - قلب الـ OS]
    OS --> TYPES[OS Types - Windows vs Linux]
    OS --> MEM[HDD vs RAM - Permanent vs Temporary]

    NET --> LNWN[LAN vs WAN]
    NET --> RSW[Router vs Switch - Layer 3 vs Layer 2]
    NET --> ARP[ARP Protocol - IP to MAC Resolution]
    NET --> PKT[Packets - Header and Payload]
```


---

*Lecture 2 — Introduction to Cyber Security Course*
