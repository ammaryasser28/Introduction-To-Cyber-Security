> **الهدف من الـ Section ده:**  
> بيشرح البروتوكولات المساعدة في الشبكة زي الـ ICMP والـ DHCP والـ DNS، ودور كل واحد في تشغيل الشبكة بكفاءة — من تشخيص الأخطاء، لتوزيع الـ IPs، لحد ترجمة أسماء المواقع لعناوين IP.

---

## Table of Contents
  - [ICMP Protocol](#icmp-protocol)
  - [DHCP Protocol](#dhcp-protocol)
  - [DNS Protocol](#dns-protocol)
  - [Summary](#summary)

---

## Supporting Network Protocols

### ICMP Protocol

#### ما هو ICMP؟

الـ **IP Protocol** هو **Best Effort Protocol** — بيبعت الـ Packet ويأمل يوصل، لكن من غير Connection ومن غير Acknowledgment. ده بيخليه **Unreliable**.

عشان كده في **ICMP (Internet Control Message Protocol)** — هو **آلية الـ Error Reporting** الخاصة بالـ IP Protocol.

#### أمثلة على ICMP Messages

| Scenario | ICMP Message |
|---|---|
| الـ TTL وصل لـ 0 عند Router | "Time to live exceeded in transit" |
| الـ Router مش عارف يبعت الـ Packet فين | "Destination unreachable, Network unreachable" |
| الشبكة وصلت لكن الجهاز مش بيرد | "Destination unreachable, Host unreachable" |

```mermaid
flowchart TD
    P[Packet Sent] --> R1[Router 1 - TTL--]
    R1 --> R2[Router 2 - TTL--]
    R2 --> R3{TTL = 0?}
    R3 -- Yes --> ICMP[Send ICMP: Time Exceeded back to Source]
    R3 -- No --> R4[Continue routing]
```

---

### DHCP Protocol

#### ما هو DHCP؟

الـ **DHCP (Dynamic Host Configuration Protocol)** هو آلية إعطاء كل جهاز على الشبكة **IP Address تلقائياً**.

بدونه، محتاج تحط IP يدوي على كل جهاز — ده ممكن مع 10 أجهزة، لكن مستحيل مع 100,000.

#### Static vs Dynamic IP

| Type | Who Sets It | Use Case |
|---|---|---|
| **Static** | المسؤول يحطها يدوي | Servers — محتاج IP ثابت دايماً |
| **Dynamic (DHCP)** | الـ DHCP Server تلقائياً | Clients — أجهزة المستخدمين |

#### ما اللي DHCP بيديه؟

- IP Address
- Subnet Mask
- Default Gateway
- DNS Server IP

#### DHCP DORA Process

```mermaid
sequenceDiagram
    participant C as Client (No IP)
    participant S as DHCP Server

    C->>S: DHCP Discover (Broadcast to 255.255.255.255 - Port 67)
    S->>C: DHCP Offer (Broadcast - Port 68)
    C->>S: DHCP Request (Broadcast - Port 67)
    S->>C: DHCP ACK (Unicast or Broadcast - Port 68)
    Note over C,S: Client now has IP, Mask, GW, DNS
```

| Step | Who | What |
|---|---|---|
| **Discover** | Client | "في DHCP Server هنا؟" |
| **Offer** | Server | "أيوه، وده الـ IP اللي هاديك إياه" |
| **Request** | Client | "عايز الـ Config ده" |
| **ACK** | Server | "تفضل، الـ Config جاهزة" |

> [!NOTE]
> الـ DHCP بيشتغل على **UDP**:
> - Client بيسمع على **Port 68**
> - Server بيسمع على **Port 67**
>
> الـ Client في البداية مالوش IP، فبيستخدم الـ Broadcast Address `255.255.255.255` كـ Source IP.

#### IP Lease

الـ IP المُعطى مش دايم — الجهاز بـ "يأجره" لفترة:
- في بيئة ثابتة (شركة): يوم أو أسبوع
- في بيئة متغيرة (كامبس جامعي): 60-90 دقيقة

---

### DNS Protocol

#### ما هو DNS؟

الـ **DNS (Domain Name System)** هو البروتوكول اللي بيترجم **Domain Names** (زي google.com) لـ **IP Addresses** حقيقية. لأن الكومبيوتر مش بيفهم `google.com` في الـ Packet Header — بيحتاج IP Address.

**TLD (Top Level Domain):** الجزء الأخير من الـ Domain زي `.com`، `.net`، `.edu`، `.org`

#### رحلة الـ DNS Resolution

```mermaid
flowchart TD
    A[User types google.com] --> B[Browser calls Resolver]
    B --> C{DNS Cache in Memory?}
    C -- Yes --> G[Return IP to Browser]
    C -- No --> D{Check local Hosts file?}
    D -- Yes --> G
    D -- No --> E[Query DNS Server - ISP or custom]
    E --> F{DNS Server knows?}
    F -- Yes --> G
    F -- No --> H[Query up the DNS Hierarchy]
    H --> G
```

#### DNS Resolution Steps

1. المستخدم بيكتب `google.com` في الـ Browser
2. الـ Browser بيستدعي الـ **Resolver**
3. الـ Resolver بيفحص الـ **DNS Cache** في الـ RAM أولاً
4. لو مش موجود، بيفحص الـ **Local Hosts File**
5. لو مش موجود، بيبعت Query لـ **DNS Server** (المُعطى من الـ ISP عبر DHCP)
6. لو الـ DNS Server مش عارف، بيسأل Server أعلى في الهيكل
7. في النهاية، الـ IP Address بيترجع للـ Browser

#### Public DNS Servers

| Provider | Primary | Secondary |
|---|---|---|
| **Cloudflare** | 1.1.1.1 | 1.0.0.1 |
| **Google DNS** | 8.8.8.8 | 8.8.4.4 |
| **OpenDNS** | 208.67.222.222 | 208.67.220.220 |

> [!TIP]
> مش لازم تستخدم الـ DNS Server بتاع الـ ISP. ممكن تغيّر لأي Server من الجدول ده. الـ Cloudflare (1.1.1.1) معروف بالسرعة والخصوصية.

---

## Summary


- الـ ICMP هو Error Reporting بتاع IP
- الـ DHCP بيدي الأجهزة IP تلقائياً (DORA Process)
- الـ DNS بيترجم Domain Names لـ IP Addresses
