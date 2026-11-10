| Topic | Level | Reading Time | Prerequisites |
|---|---|---|---|
| Identity, Access Control, and Security Fundamentals | Beginner | ~55 min | None |

> **Goal of this section:**
> You'll take a comprehensive tour of the most important foundational concepts in cybersecurity: from the CIA Triad and the AAA and PPT frameworks, through password security and detecting credential-guessing attacks, all the way to access control models (DAC, MAC, RBAC, ABAC) and the importance of logging, with real hands-on exercises that apply these concepts to realistic scenarios.



## Learning Objectives

By the end of this section, you will be able to:

- Explain the **CIA Triad** (Confidentiality, Integrity, Availability) and why its priority differs by industry.
- Define **Non-Repudiation** and **Authenticity**.
- Explain the **AAA** framework (Authentication, Authorization, Accounting) and the **PPT** framework (Policy, Procedure, Training).
- Apply the **Least Privilege Principle** and describe best practices for password security.
- Distinguish between a **Brute Force Attack** and a **Password Spraying Attack**, and describe how a SIEM detects each.
- List the five **Authentication Factors**.
- Compare the four major **Access Control Models**: DAC, MAC, RBAC, and ABAC.
- Explain why **Logging and Audit** matter, and compare local versus centralized logging.

## Table of Contents

- [The CIA Triad](#the-cia-triad)
- [CIA Priorities Vary by Industry](#cia-priorities-vary-by-industry)
  - [Pharmaceutical Industry: Confidentiality](#pharmaceutical-industry-confidentiality)
  - [Financial Industry: Integrity](#financial-industry-integrity)
  - [Online Services: Availability](#online-services-availability)
- [Non-Repudiation and Authenticity](#non-repudiation-and-authenticity)
- [AAA: Authentication, Authorization, Accounting](#aaa-authentication-authorization-accounting)
- [PPT: Policy, Procedure, Training](#ppt-policy-procedure-training)
- [Exercise 1: Applying AAA to a VPN Scenario](#exercise-1-applying-aaa-to-a-vpn-scenario)
- [Exercise 2: Applying PPT to a Password-Sharing Incident](#exercise-2-applying-ppt-to-a-password-sharing-incident)
- [The Least Privilege Principle](#the-least-privilege-principle)
- [Creating Strong Passwords](#creating-strong-passwords)
- [Account Lockout](#account-lockout)
- [Password Managers](#password-managers)
- [SOC Interview Question: Brute Force vs. Password Spraying](#soc-interview-question-brute-force-vs-password-spraying)
  - [Brute Force Attack](#brute-force-attack)
  - [Password Spraying Attack](#password-spraying-attack)
- [Authentication Factors](#authentication-factors)
- [Access Control Models (Authorization Models)](#access-control-models-authorization-models)
  - [DAC: Discretionary Access Control](#dac-discretionary-access-control)
  - [MAC: Mandatory Access Control](#mac-mandatory-access-control)
  - [RBAC: Role-Based Access Control](#rbac-role-based-access-control)
  - [ABAC: Attribute-Based Access Control](#abac-attribute-based-access-control)
  - [Comparing the Four Models](#comparing-the-four-models)
- [Logging and Audit](#logging-and-audit)
- [Logging Challenges](#logging-challenges)
- [Career Connection](#career-connection)
- [Key Terms Glossary](#key-terms-glossary)
- [Summary](#summary)

## The CIA Triad

The **CIA Triad** (Confidentiality, Integrity, Availability) is the **fundamental model** used to design and evaluate security policies and system architecture. A truly secure system must preserve all three dimensions **at the same time**, and a failure in any one of them significantly weakens the overall security posture.

### Confidentiality

Ensures that information is available only to the people authorized to view it.

**Techniques**: **Encryption**, strong access control lists (**ACLs**), multi-factor authentication (**MFA**), and strong password policies.

### Integrity

Protects data from modification or corruption by unauthorized parties.

**Techniques**: **Hashing**, **checksums**, **digital signatures**, and **version control** systems to track changes.

### Availability

Ensures that systems and data are reliable and available whenever authorized users need them.

**Techniques**: **redundancy**, regular backups, **replicas**, robust network design, **Disaster Recovery** plans, ransomware-resilient solutions, and protection against **DDoS** attacks.

> [!TIP]
> In short: who can **read** the file (Confidentiality)? Who can **write** to the file (Integrity)? Is the file **always available** for those reads and writes (Availability)?

## CIA Priorities Vary by Industry

The importance of each element of the CIA Triad depends on the nature of the organization's business. Different industries prioritize different security goals, and security controls must align with the business's needs and risks.

### Pharmaceutical Industry: Confidentiality

**Most important element: Confidentiality**

**Why?** Drug formulas and research are extremely valuable intellectual property. Leaking them to competitors could cause losses worth billions of dollars.

> [!NOTE]
> **Real-world example**: **AbbVie** (2017): the drug **Humira®** generated annual sales of **$18.43 billion**. Protecting that drug's formula was critical to preserving that revenue.

### Financial Industry: Integrity

**Most important element: Integrity**

**Why?** Financial data must always be accurate. Any incorrect balances or transactions can undermine trust and cause financial harm.

> [!NOTE]
> **Illustrative example**: Imagine your account balance changed from **$1,000,000** to **$1.27** because of a system error. Even if the system was available and confidential, the bank has failed because the data has lost its **Integrity**.

### Online Services: Availability

**Most important element: Availability**

**Why?** If customers can't reach the service, the business loses revenue and reputation.

> [!WARNING]
> **Real-world example - Amazon Prime Day (2018)**: The Prime Day site suffered a major outage. Customers couldn't shop for roughly **75 minutes**, and the estimated revenue loss was **$90-99 million**.
>
> **Lesson learned**: Even if the data is secure and accurate, if customers can't reach the service, the business loses money.

> [!IMPORTANT]
> **Key takeaway**: No single element of the CIA Triad is "the most important" universally. The priority depends on the organization's mission and business goals.

## Non-Repudiation and Authenticity

**Non-Repudiation**: Ensuring that a party to a given communication cannot deny its actions. It provides undeniable proof of both the origin and the delivery of data.

**Authenticity**: Proving that a communication actually comes from the claimed source (and is not forged).

## AAA: Authentication, Authorization, Accounting

**Authentication** - **Who are you?**

Verifies the user's identity before granting access.

**Example**: Logging in with a username and password, or using **Multi-Factor Authentication (MFA)**.

**Authorization** - **What are you allowed to do?**

Determines what an authenticated user is allowed to access or perform.

**Example**: An employee can read files but can't delete them.

**Accounting** - **What did you do?**

Records and monitors user activity for auditing, compliance, and incident investigation.

**Example**: Recording login times, the files accessed, and the actions performed within the system.

## PPT: Policy, Procedure, Training

**Policy** - **What should be done?**

High-level rules that define how security is managed within the organization.

**Example**: A password policy that requires strong passwords and periodic changes.

**Procedure** - **How is it done?**

Step-by-step instructions for carrying out the organization's policies.

**Example**: A documented procedure for securely resetting a user's password.

**Training** - **How do people apply it?**

Educates employees about policies, procedures, and security best practices.

**Example**: Regular phishing awareness training and security awareness sessions.

## Exercise 1: Applying AAA to a VPN Scenario

**Scenario**: Your company uses a VPN for employees who work remotely.

**Q1: Explain how AAA works together when an employee logs in from home.**

- **Authentication**: Verifies the employee's identity using credentials (such as a username/password and **MFA**).
- **Authorization**: Determines which resources the employee can access after successful authentication (such as internal servers, applications, or shared folders).
- **Accounting**: Records the user's activity, such as login time, logout time, IP address, VPN connection duration, and the resources accessed.

**Q2: What is the best authentication method?**

**Answer: Multi-Factor Authentication (MFA)**

**Why?** It combines two or more authentication factors (such as a password + a mobile authenticator app or a security key). Even if the password is stolen, the attacker still needs the second factor to authenticate.

**Q3: What data would appear in the accounting logs?**

Typical VPN accounting logs include: username, login/logout date and time, source IP address, VPN IP address, authentication status (success/failure), session duration, device name or hostname, resources or services accessed, amount of data sent and received, and the reason for disconnection.

> [!NOTE]
> **Example of a real log**:
> ```
> Username: John.Smith
> Login Time: 09:02 AM
> Logout Time: 05:15 PM
> Source IP: 197.x.x.x
> VPN IP: 10.10.20.45
> Authentication: Success (MFA)
> Session Duration: 8h 13m
> Resources Accessed: File Server, HR Portal
> Data Transferred: 1.8 GB
> ```

## Exercise 2: Applying PPT to a Password-Sharing Incident

**Scenario**: Your team discovered that employees are sharing passwords over email.

**Q1: What is the policy?**

**Password policy**:

- Employees must never share passwords with anyone.
- Every user is responsible for protecting their account credentials.
- Passwords must be unique, confidential, and stored only in an approved password manager.

**Q2: What should the procedure be?**

1. Stop sharing passwords immediately.
2. Change any passwords that were shared.
3. Report the incident to the IT team.
4. Use approved methods to share access (such as account delegation or a password manager) instead of email.
5. Review and monitor the affected accounts for suspicious activity.

**Q3: How would you train employees?**

- Explain why password sharing is dangerous.
- Show how attackers can misuse shared credentials.
- Teach employees to use password managers.
- Encourage the use of **MFA**.
- Run regular security awareness and phishing training.

**Q4: When is it no longer the employee's responsibility?**

When a breach happens even though the employee followed the security policies and procedures. For example:

- The company's system was breached through an unpatched vulnerability.
- The authentication system or the password manager itself was breached.
- A third-party service suffered a data breach.
- The employee followed all security guidelines, but the organization failed to implement adequate security controls.

> [!IMPORTANT]
> **Key point**: Employees are responsible for following security policies. The organization is responsible for providing secure systems, policies, training, and technical controls.

## The Least Privilege Principle

Users, applications, and systems should be granted only the minimum permissions needed to perform their tasks, **and no more**.

- Reduces the risk of unauthorized actions.
- Limits the impact of compromised accounts.
- Helps prevent accidental or intentional misuse of privileges.

**Examples**:

- A standard user can use installed applications but can't install new software; that's for admins only.
- A developer has write access to development/test servers but no access at all to **production** servers.

> [!IMPORTANT]
> **Key takeaway**: Administrative accounts have the highest privileges, and they must be controlled, monitored, and used only when needed.

## Creating Strong Passwords

**Best practices**:

- Use long, complex passwords with uppercase letters, lowercase letters, numbers, and special characters.
- Avoid personal information such as names, birth dates, or favorite sports teams.
- Use a unique password for every account.
- Consider using a **password manager** to generate and store strong passwords.

**Why this matters**:

- As processing power increases, weak passwords become easier to crack through **brute-force** attacks.
- Emerging technologies, including **quantum computing**, may reduce the security of some password-based authentication systems in the future.

## Account Lockout

Why does an **Account Lockout Policy** matter?

- It protects accounts from brute-force and password-guessing attacks.
- It temporarily locks the account after a set number of failed login attempts.

**Best practices**:

- Set a reasonable threshold for the number of failed login attempts.
- Choose an appropriate lockout duration that balances security and usability.
- Use a trusted password manager instead of storing passwords in text files or notes.

## Password Managers

A **Password Manager** is a secure application that stores, generates, and autofills passwords for different accounts. The user only needs to remember one strong **master password**.

**Benefits**:

- Generates strong, unique passwords.
- Eliminates password reuse across accounts.
- Stores passwords securely in an **encrypted vault**.
- Simplifies logging in through autofill features.
- Supports **MFA**.

**Popular password managers**: Bitwarden, 1Password, Keeper, Dashlane, NordPass, and Proton Pass.

> [!TIP]
> Password managers are far more secure than storing passwords in browsers, text files, or **sticky notes**.

## SOC Interview Question: Brute Force vs. Password Spraying

The following table (a common question in SOC job interviews) shows the core difference between the two types:

| Criterion | Brute Force | Password Spraying |
|---|---|---|
| Core logic | Many passwords → one account | One password (or a few) → many accounts |
| Target | Targets a specific user | Targets many users |
| Account lockout | Usually triggers account lockout | Designed to avoid account lockout |
| Example | 500 passwords against an admin account | The password "Summer2025!" against 500 users |

### Brute Force Attack

The attacker tries many passwords against a single user account until finding the correct one.

**SIEM detection**:

- Multiple failed login attempts.
- The same username.
- Usually from the same source IP address.
- Within a short time window.

> [!NOTE]
> **Example rule**: More than 10 failed login attempts for the same user within 5 minutes.

### Password Spraying Attack

The attacker tries one common password against many user accounts to avoid account lockout.

**SIEM detection**:

- Multiple failed login attempts.
- Different usernames.
- Usually from the same source IP address.
- Within a short time window.

> [!NOTE]
> **Example rule**: More than 20 different usernames failing authentication from the same IP address within 10 minutes.

## Authentication Factors

There are five categories of **authentication factors**:

| Factor | Description | Example |
|---|---|---|
| **Something you know** | Information only the user should know | Password, PIN, security question |
| **Something you have** | A physical item the user possesses | Smartphone, smart card, hardware security key |
| **Something you are** | Unique biological characteristics | Fingerprint, facial recognition, iris scan, voice pattern |
| **Somewhere you are** | The user's physical location | Logging in from a trusted office network or a specific geographic region |
| **Something you do** | Unique behavioral patterns | Typing rhythm, mouse movements, touchscreen gestures, signature dynamics |

> [!TIP]
> The more authentication factors you use from different categories (such as a password + a fingerprint), the stronger the authentication, because the attacker needs to compromise more than one type of evidence.

## Access Control Models (Authorization Models)

There are four main access control models (also known as authorization models):

1. **DAC** (Discretionary Access Control)
2. **MAC** (Mandatory Access Control)
3. **RBAC** (Role-Based Access Control)
4. **ABAC** (Attribute-Based Access Control)

### DAC: Discretionary Access Control

A security model in which the owner of a resource (such as a file or folder) decides who can access it and with what permissions (read, write, execute).

**How it works**:

- Every file or folder has an owner.
- The owner assigns permissions to other users or groups using access control lists (**ACLs**).
- The operating system enforces these permissions every time someone tries to access the resource.

**Where is it used?**: The default model in Windows and most Linux/Unix systems; common on personal computers and corporate file-sharing environments.

**Why is it called "Discretionary"?** Because the resource owner has the **discretion** to grant, modify, or revoke access permissions.

### MAC: Mandatory Access Control

A security model in which access control is determined by a central security policy, not by the resource owner. Users cannot change permissions.

**How it works**:

- Every user has a **security clearance** (such as Confidential, Secret, Top Secret).
- Every resource has a security classification.
- The system compares the user's clearance with the resource's classification.
- Access is granted or denied based on the organization's security policy.

**Where is it used?**: Military and defense organizations, government agencies, intelligence organizations, and high-security environments.

**Why is it called "Mandatory"?** Because access decisions are mandatory and enforced by the system according to security policies; neither users nor resource owners can change the permissions.

### RBAC: Role-Based Access Control

A security model in which permissions are assigned to **roles**, and users inherit permissions based on the role (or roles) they are assigned.

**How it works**:

- The organization defines roles (such as HR, Manager, Administrator).
- Permissions are assigned to each role.
- Users are assigned one or more roles.
- Users automatically receive the permissions associated with their roles.

**Where is it used?**: Companies and enterprise environments, hospitals, banks, universities.

**Why is it called "Role-Based"?** Because access decisions are based on the user's job role, not on the individual user or the resource owner.

### ABAC: Attribute-Based Access Control

A security model in which access decisions are based on the **attributes** of the user, the resource, the action, and the environment, rather than relying only on roles or ownership.

**How it works**:

- The system evaluates multiple attributes before granting access.
- Access is allowed only if the defined policy conditions are met.

**Common attributes**:

- **User**: department, job title, clearance level.
- **Resource**: classification, owner, sensitivity.
- **Action**: read, write, delete, modify.
- **Environment**: time, location, device, IP address.

**Where is it used?**: Cloud platforms (AWS, Azure, Google Cloud), **Zero Trust** architecture, and enterprise applications.

**Why is it called "Attribute-Based"?** Because access decisions are made by evaluating attributes, rather than relying solely on roles or ownership.

### Comparing the Four Models

The following table summarizes the advantages and disadvantages of each model:

| Model | Advantages | Disadvantages |
|---|---|---|
| **DAC** | Simple to implement and manage; flexible, since owners control their own resources; suitable for personal use and small businesses | Less secure because users can grant permissions to others; malware running as the owner can inherit their permissions; hard to manage in large organizations |
| **MAC** | Very high level of security; centralized policy enforcement; prevents users from changing permissions | Inflexible; complex and costly to implement; high administrative overhead |
| **RBAC** | Easy to manage in large organizations; simplifies permission management; supports the principle of least privilege | Role design can become complex (**role explosion**); less flexible for users with unique access needs |
| **ABAC** | Highly flexible and fine-grained; supports dynamic, context-aware decisions; ideal for Zero Trust environments | Complex to design and maintain; policies can be hard to troubleshoot; requires more processing than simpler models |

## Logging and Audit

**Why does logging matter?**

- It records important system and user activity, especially administrative actions.
- It helps detect security incidents and supports system troubleshooting.
- It provides evidence for audits, compliance, and **forensic investigations**.

**Why do you need to understand logs?**

- Knowing normal behavior helps you identify abnormal activity.
- SOC analysts and system administrators alike rely on logs to monitor systems and investigate problems.

> [!NOTE]
> **Example**: 40 failed login attempts within a single minute could indicate a brute-force attack. A sudden spike in activity on a **privileged account** outside business hours could indicate a compromised account.

**Common log sources**: Windows Event Logs, Linux Syslog, Firewall Logs, Authentication Logs, Web Server Logs, and Endpoint Security Logs.

**SIEM solutions**: Splunk, LogRhythm, Microsoft Sentinel, and IBM QRadar.

## Logging Challenges

**Common challenges**:

- **Storage requirements**: Large organizations generate huge volumes of log data, requiring large storage capacity.
- **Log retention**: Organizations must decide how long to keep logs based on business needs, compliance requirements, and regulations.
- **Performance impact**: Collecting and storing more logs than necessary can consume system, network, and storage resources.
- **Log management**: Managing logs from multiple systems and devices can become complex without centralized solutions.

**Logging methods**:

| Method | Advantage | Disadvantage |
|---|---|---|
| **Local Logging** | Simple to implement | Logs can be lost if the device fails or is compromised |
| **Centralized Logging** | Easier monitoring, correlation, searching, and long-term storage | Requires additional infrastructure and management |

## Career Connection

Understanding the fundamentals of identity and access control has direct applications across several career paths:

- In **SOC**, understanding detection rules for brute force and password spraying, along with log analysis, is the foundation of day-to-day alert monitoring.
- In **GRC**, the **PPT** framework (Policy, Procedure, Training) is the cornerstone for building and documenting an organization's security program.
- In **Cloud Security**, understanding **ABAC** is essential because it is the dominant model in Zero Trust environments and modern cloud platforms.
- In **Identity and Access Management (IAM)**, choosing the right model (DAC, MAC, RBAC, or ABAC) is a strategic decision that determines how permissions are managed across the entire organization.

## Key Terms Glossary

| Term | Definition |
|---|---|
| **CIA Triad** | A fundamental model covering Confidentiality, Integrity, and Availability; the basis for designing security policies. |
| **Non-Repudiation** | Ensuring that a party cannot deny its actions in a given communication. |
| **AAA** | A framework covering Authentication, Authorization, and Accounting. |
| **PPT** | A framework covering Policy, Procedure, and Training. |
| **Least Privilege** | The principle of granting only the minimum permissions necessary to perform a task. |
| **Brute Force Attack** | An attack that tries many passwords against a single account. |
| **Password Spraying** | An attack that tries one password against many accounts to avoid account lockout. |
| **Authentication Factor** | A category of evidence used to prove a user's identity (something you know, have, are, etc.). |
| **DAC** | An access control model in which the resource owner decides who can access it. |
| **MAC** | An access control model in which decisions are enforced centrally according to security classifications. |
| **RBAC** | An access control model in which permissions are granted based on job role. |
| **ABAC** | An access control model in which decisions are made based on multiple attributes. |
| **SIEM** | A system that collects and analyzes security logs from multiple sources to detect threats. |

## Summary

- The **CIA Triad** (Confidentiality, Integrity, Availability) is the fundamental security model, and its priority varies by industry: Confidentiality for pharmaceuticals, Integrity for finance, and Availability for online services.
- **Non-Repudiation** and **Authenticity** ensure that parties to a communication cannot deny their actions and that the communication is genuine.
- The **AAA** framework (Authentication, Authorization, Accounting) covers who you are, what you're allowed to do, and what you did.
- The **PPT** framework (Policy, Procedure, Training) covers what should be done, how it's done, and how people apply it.
- **Least Privilege** reduces risk by granting only the minimum permissions necessary.
- Password security depends on strong passwords, an account lockout policy, and the use of **password managers**.
- **Brute Force** targets one account with many passwords, while **Password Spraying** targets many accounts with one password to avoid lockout.
- There are five authentication factors: something you know, something you have, something you are, somewhere you are, and something you do.
- Four access control models: **DAC** (the resource owner decides), **MAC** (mandatory central policy), **RBAC** (based on role), and **ABAC** (based on multiple attributes).
- **Logging and Audit** are essential for incident detection and compliance, with challenges such as storage, retention, and performance, and solutions ranging from local to centralized logging.

