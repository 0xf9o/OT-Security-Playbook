# Cybersecurity Foundations for Operational Technology
# أسس الأمن السيبراني للتكنولوجيا التشغيلية

---

## Executive Summary / الملخص التنفيذي

This document establishes the enterprise IT cybersecurity engineering foundations required to secure and operate within Operational Technology (OT) and Industrial Control Systems (ICS) environments. As IT and OT architectures converge under modern industrial initiatives (Industry 4.0, IIoT, and Purdue Model Levels 3 through 4), enterprise security failures directly expose physical control infrastructure to cyber-physical disruption. Mastery of classical networking, operating system internals, identity federation, and passive traffic inspection is mandatory before engaging with field-level controllers.

تحدد هذه الوثيقة المبادئ الهندسية لأسس أمن تقنية المعلومات المؤسسية (IT) اللازمة لتأمين والعمل داخل بيئات التكنولوجيا التشغيلية (OT) وأنظمة التحكم الصناعي (ICS). مع تقارب معماريات تقنية المعلومات والتقنية التشغيلية ضمن المبادرات الصناعية الحديثة (الثورة الصناعية الرابعة وإنترنت الأشياء الصناعي والمستويات من 3 إلى 4 في نموذج بيردو)، تؤدي الثغرات الأمنية في أنظمة تقنية المعلومات إلى تعريض البنية التحتية للتحكم الفيزيائي لمخاطر التخريب والتعطيل المباشر. يعد التمكن الكامل من هندسة الشبكات، والآليات الداخلية لأنظمة التشغيل، وإدارة الهويات، والفحص السلبي للبيانات شرطاً إلزامياً قبل التعامل مع أجهزة التحكم الميدانية.

---

## Module 1: Networking & Infrastructure
## الوحدة 1: الشبكات والبنية التحتية

### 1. Core Technical Concepts / المفاهيم التقنية الأساسية
- **EN:**
  - **The TCP/IP & OSI Reference Stacks:** Layer-by-layer transport mechanics. Analysis of Industrial Ethernet modifications, deterministic timing constraints, and protocol overhead versus payload trade-offs.
  - **Transmission Control Protocol (TCP):** The three-way handshake (`SYN`, `SYN-ACK`, `ACK`), connection termination sequences (`FIN`, `RST`), TCP sequence and acknowledgment tracking, sliding window flow control, and susceptibility to SYN flood Denial-of-Service (DoS).
  - **User Datagram Protocol (UDP):** Connectionless transport dynamics, minimal header overhead (8 bytes), lack of flow control, and prevalence in low-latency industrial telemetry (e.g., multicast field telemetry, streaming process data).
  - **Domain Name System (DNS):** Hierarchical name resolution (Root servers, Top-Level Domains, Authoritative Name Servers), recursive and iterative queries, resource records (`A`, `AAAA`, `PTR`, `CNAME`, `SRV`, `TXT`), and threat modeling (DNS cache poisoning, DNS tunneling, and internal resolution hijacking).
  - **Hypertext Transfer Protocol (HTTP/HTTPS) & TLS:** Request and response methods (`GET`, `POST`, `PUT`, `DELETE`), status codes, TLS 1.3 cryptographic handshakes, cipher suite negotiation, Public Key Infrastructure (PKI), and X.509 certificate revocation (CRL and OCSP).
  - **IT/OT Network Segmentation:** Virtual Local Area Network (VLAN) tagging and trunking (IEEE 802.1Q), Inter-VLAN routing, Access Control Lists (ACLs), Layer 4 stateful inspection firewalls, and microsegmentation strategies enforcing boundaries between enterprise IT (Level 4/5) and the Industrial Demilitarized Zone (IDMZ / Level 3.5).
- **AR:**
  - **نماذج TCP/IP وOSI المرجعية:** آليات النقل عبر الطبقات المختلفة. تحليل تعديلات شبكات الإيثرنت الصناعية، ومحددات التوقيت الحتمي، والموازنة بين الحجم الإضافي للترويسات وحجم حمولة البيانات الفعلية.
  - **بروتوكول التحكم في النقل (TCP):** آلية المصافحة الثلاثية (`SYN`، `SYN-ACK`، `ACK`)، تسلسل إنهاء الاتصال (`FIN`، `RST`)، تتبع أرقام التسلسل والإشعار، التحكم في التدفق عبر النوافذ المنزلقة، والقابلية للتعرض لهجمات حجب الخدمة عبر فيضان حزم SYN.
  - **بروتوكول حزم بيانات المستخدم (UDP):** ديناميكيات النقل غير المتصل، الحد الأدنى لحجم الترويسة (8 بايت)، غياب التحكم في التدفق، واستخدامه الشائع في نقل بيانات القياس الصناعية ذات زمن الوصول الحرج (مثل الإرسال الجماعي لبيانات الحقل).
  - **نظام أسماء النطاقات (DNS):** معمارية التحليل الهرمي للأسماء (خوادم الجذر، نطاقات المستوى الأعلى، خوادم الأسماء المعتمدة)، الاستعلامات التكرارية والمباشرة، سجلات الموارد (`A`، `AAAA`، `PTR`، `CNAME`، `SRV`، `TXT`)، ونمذجة التهديدات (تسميم الذاكرة المؤقتة، الأنفاق المخفية لتهريب البيانات، واختراق التحليل الداخلي).
  - **بروتوكول نقل النص الفائق (HTTP/HTTPS) وتشفير TLS:** طرق الطلب والاستجابة، رموز الحالة، مصافحات بروتوكول TLS 1.3 التشفيرية، التفاوض على حزم التشفير، البنية التحتية للمفاتيح العامة (PKI)، والتحقق من صلاحية شهادات X.509 عبر قوائم الإلغاء (CRL) وبروتوكول OCSP.
  - **تقسيم شبكات IT/OT:** عزل الشبكات المحلية الظاهرية (VLAN) وتوسيم الإطارات وفق معيار IEEE 802.1Q، التوجيه بين الشبكات المحلية، قوائم التحكم بالوصول (ACLs)، جدران الحماية المعتمدة على فحص الحالة، واستراتيجيات التقسيم الدقيق لتأمين الحدود الفاصلة بين شبكات المؤسسة (المستوى 4/5) والمنطقة الصناعية منزوعة السلاح (IDMZ / المستوى 3.5).

### 2. Verifiable Official Standards / المعايير الرسمية المعتمدة
- **IETF RFC 791:** Internet Protocol (IPv4 Specification) - https://datatracker.ietf.org/doc/html/rfc791
- **IETF RFC 9293:** Transmission Control Protocol (TCP Specification) - https://datatracker.ietf.org/doc/html/rfc9293
- **IETF RFC 1035:** Domain Names - Implementation and Specification - https://datatracker.ietf.org/doc/html/rfc1035
- **IETF RFC 8446:** The Transport Layer Security (TLS) Protocol Version 1.3 - https://datatracker.ietf.org/doc/html/rfc8446
- **NIST SP 800-125B:** Secure Virtual Network Configuration for Virtual Machine Protection - https://csrc.nist.gov/publications/detail/sp/800-125b/final

---

## Module 2: OS Internals & Hardening
## الوحدة 2: البنية الداخلية لأنظمة التشغيل وتصليبها

### 1. Core Technical Concepts / المفاهيم التقنية الأساسية
- **EN:**
  - **Windows Operating System Architecture:** Dual-mode execution model (User Mode Ring 3 vs. Kernel Mode Ring 0), Hardware Abstraction Layer (HAL), Executive subsystems, and the Security Reference Monitor (SRM).
  - **Identity, Tokens, & LSASS:** Local Security Authority Subsystem Service (LSASS), Security Support Provider Interface (SSPI), Security Identifiers (SIDs), access token creation, and credential caching mechanisms.
  - **User Account Control (UAC) Mechanics:** Administrative token filtering, integrity levels (Untrusted, Low, Medium, High, System), virtualization redirection, and elevation consent prompts.
  - **Driver & Binary Integrity Enforcement:** Code Integrity (CI) subsystem, Driver Signature Enforcement (DSE), verification of digitally signed system binaries using `sigverif.exe`, and cryptographic validation via Microsoft `SignTool`.
  - **Windows Telemetry & Auditing:** Advanced Audit Policy Configuration, Windows Event Forwarding (WEF), and telemetry enrichment using Microsoft Sysmon (Event ID 1: Process Creation with full command line, Event ID 3: Network Connection, Event ID 7: Image Loaded).
  - **Linux Operating System Architecture:** Monolithic kernel structure, virtual file systems (`/proc`, `/sys`, `/dev`), process hierarchy (`PID 1 / systemd`), and process lifecycle states.
  - **Privilege Separation & Capabilities:** POSIX permission model (`rwxrwxrwx`), special bits (`SUID`, `SGID`, Sticky Bit), and Linux Capabilities (`cap_net_admin`, `cap_net_raw`, `cap_sys_admin`) eliminating the need to assign complete root access to monitoring utilities.
  - **System Hardening & MAC Frameworks:** Kernel tuning via `/etc/sysctl.conf` (disabling ICMP redirects, enabling TCP SYN cookies, disabling IPv4 forwarding), Pluggable Authentication Modules (PAM) stack hardening, and Mandatory Access Control (MAC) enforcement using AppArmor or SELinux profiles.
- **AR:**
  - **معمارية نظام التشغيل ويندوز:** نموذج التنفيذ ثنائي الأوضاع (وضع المستخدم Ring 3 مقابل وضع النواة Ring 0)، طبقة تجريد العتاد (HAL)، الخدمات التنفيذية الفرعية للنواة، ومراقب مرجعية الأمان (SRM).
  - **إدارة الهوية والرموز الأمنية وخدمة LSASS:** خدمة النظام الفرعي لهيئة الأمان المحلية (LSASS)، واجهة موفر دعم الأمان (SSPI)، معرفات الأمان (SIDs)، إنشاء الرموز المميزة للوصول، وآليات حفظ بيانات الاعتماد في الذاكرة المؤقتة.
  - **آليات التحكم في حساب المستخدم (UAC):** تصفية الرموز الإدارية، مستويات التكامل الإلزامية (غير موثوق، منخفض، متوسط، مرتفع، نظام)، إعادة التوجيه الافتراضي، ونوافذ الموافقة على رفع الصلاحيات.
  - **فرض تكاملية برامج التشغيل والملفات التنفيذية:** النظام الفرعي لتكامل الشيفرات (CI)، فرض التوقيع الرقمي لبرامج التشغيل (DSE)، التحقق من التواقيع الرقمية للملفات الثنائية عبر أداة `sigverif.exe`، والتحقق التشفيري المتقدم باستخدام `SignTool`.
  - **القياس الأمني عن بُعد وسجلات ويندوز:** التكوين المتقدم لسياسات التدقيق، إعادة توجيه الأحداث (WEF)، وإثراء السجلات باستخدام أداة Microsoft Sysmon (الحدث 1: إنشاء العمليات مع سطر الأوامر الكامل، الحدث 3: اتصالات الشبكة، الحدث 7: تحميل الملفات البرمجية).
  - **معمارية نظام التشغيل لينكس:** بنية النواة الأحادية (Monolithic Kernel)، أنظمة الملفات الافتراضية (`/proc`، `/sys`، `/dev`)، التسلسل الهرمي للعمليات (`PID 1 / systemd`)، ودورة حياة العمليات.
  - **فصل الامتيازات والقدرات (Capabilities):** نموذج أذونات POSIX القياسية، بتات الأذونات الخاصة (`SUID`، `SGID`، بت التثبيت)، وقدرات نواة لينكس المجزأة التي تتيح تشغيل أدوات فحص الشبكة بأذونات محددة دون منح صلاحيات الجذر (root) المطلقة.
  - **تصليب النظام وأطر التحكم الإلزامي (MAC):** ضبط معاملات النواة عبر ملف `/etc/sysctl.conf` (تعطيل إعادة توجيه ICMP، تفعيل حماية SYN cookies، وتعطيل التوجيه التلقائي للحزم)، تصليب وحدات المصادقة القابلة للإضافة (PAM)، وفرض سياسات التحكم الإلزامي عبر أنظمة SELinux أو AppArmor.

### 2. Verifiable Official Standards / المعايير الرسمية المعتمدة
- **CIS Benchmarks for Microsoft Windows Server & Enterprise Linux:** Center for Internet Security - https://www.cisecurity.org/cis-benchmarks
- **NIST SP 800-123:** Guide to General Server Security - https://csrc.nist.gov/publications/detail/sp/800-123/final
- **Microsoft Learn Security Architecture:** Windows Internals & Code Integrity Specifications - https://learn.microsoft.com/en-us/windows/security/

---

## Module 3: SOC & Defense Fundamentals
## الوحدة 3: أساسيات مراكز العمليات الأمنية والدفاع السيبراني

### 1. Core Technical Concepts / المفاهيم التقنية الأساسية
- **EN:**
  - **Defense-in-Depth (DiD):** Layered defensive doctrine spanning physical boundaries, network segmentation, host-level hardening, application control, and centralized telemetry auditing. Tailoring DiD to industrial boundaries without introducing latency or availability risks to operational processes.
  - **Authentication, Authorization, and Accounting (AAA):** Identity protocols (RADIUS, TACACS+, Kerberos), Multi-Factor Authentication (MFA) enforcement at perimeter transition points (Level 3.5 jump hosts), Role-Based Access Control (RBAC), and Attribute-Based Access Control (ABAC).
  - **Principles of Passive Traffic Monitoring:** Strict prohibition of active vulnerability scanning, active port scanning, or intrusive polling within industrial subnets (Level 0 through Level 2). Deployment of hardware Test Access Points (TAPs) and switch SPAN/mirror ports. Operating network interfaces in promiscuous mode without an assigned IP address to prevent physical packet transmission.
  - **Detection Engineering Foundations:** Log aggregation pipelines, Security Information and Event Management (SIEM) normalization, establishing normal communication baselines, signature-based intrusion detection (Snort / Suricata rules), and protocol anomaly detection (Zeek network security monitor).
- **AR:**
  - **الدفاع في العمق (Defense-in-Depth):** عقيدة دفاعية متعددة الطبقات تمتد عبر الحدود المادية، وتقسيم الشبكات، وتصليب الأنظمة المضيفة، والتحكم في التطبيقات، والتدقيق المركزي للبيانات التشغيلية، مع تكييفها لحماية الشبكات الصناعية دون المساس بالتوافرية أو إدخال تأخير زمني على خطوط الإنتاج.
  - **المصادقة والتخويل والمحاسبة (AAA):** بروتوكولات إدارة الهوية (RADIUS، TACACS+، Kerberos)، فرض المصادقة متعددة العوامل (MFA) عند نقاط العبور الحساسة (مثل خوادم العبور في المستوى 3.5)، التحكم بالوصول المستند إلى الأدوار (RBAC)، والتحكم المستند إلى السمات (ABAC).
  - **مبادئ المراقبة السلبية للشبكة:** الحظر الصارم لأدوات الفحص النشط للثغرات، وفحص المنافذ التفاعلي، والاستعلامات التداخلية داخل الشبكات الصناعية (من المستوى 0 إلى المستوى 2). استخدام موزعات الإشارة السلبية (TAPs) ومنافذ النسخ المتطابق (SPAN)، مع تشغيل بطاقات الشبكة في الوضع الشامل (Promiscuous Mode) دون تعيين عنوان IP لمنع إرسال أي إشارة فيزيائية.
  - **أسس هندسة الاكتشاف:** مسارات تجميع السجلات الأمنية، معالجة وتوحيد البيانات داخل أنظمة SIEM، بناء الأنماط القياسية لحركة المرور الطبيعية، الاكتشاف المعتمد على التواقيع (قواعد Snort وSuricata)، واكتشاف الانحرافات الشاذة في سلوك البروتوكولات عبر نظام Zeek.

### 2. Verifiable Official Standards / المعايير الرسمية المعتمدة
- **NIST SP 800-82 Rev. 3:** Guide to Operational Technology (OT) Security - https://csrc.nist.gov/publications/detail/sp/800-82/rev-3/final
- **CISA ICS-CERT Recommended Practices:** Improving Industrial Control System Cybersecurity with Defense-in-Depth Strategies - https://www.cisa.gov/resources-tools/resources/improving-industrial-control-systems-cybersecurity-defense-depth-strategies
- **MITRE D3FEND Matrix:** A Knowledge Graph of Cybersecurity Countermeasures - https://d3fend.mitre.org/

---

## Module 4: Verified Free Learning Path (YouTube)
## الوحدة 4: مسار التعلم المجاني المعتمد (يوتيوب)

The following courses are 100% free, publicly accessible, and verifiable on YouTube. They provide direct technical knowledge without paywalls:

جميع الدورات التدريبية المدرجة أدناه مجانية بنسبة 100%، ومتاحة بشكل دائم على منصة YouTube دون أي اشتراكات مدفوعة، وتوفر المعرفة التقنية المباشرة المطلوبة:

| Domain / المجال | Verified YouTube Channel | Search Query / Playlist Title | Direct Link / رابط القناة | Core Focus Areas / محاور التركيز |
| :--- | :--- | :--- | :--- | :--- |
| **Networking Baseline (Mandatory)** | Professor Messer | `CompTIA Network+ Training Course` (N10-008 / N10-009) | https://www.youtube.com/@professormesser | OSI 7 Layers, IP Addressing, Subnetting, TCP/UDP, Switching, VLANs, Routing |
| **Foundational Security & AAA** | Professor Messer | `CompTIA Security+ Training Course` (SY0-701) | https://www.youtube.com/@professormesser | Defense-in-Depth, Identity Governance, PKI, Network Defense Architecture |
| **Linux Fundamentals & CLI** | NetworkChuck | `Linux for Beginners` | https://www.youtube.com/@NetworkChuck | Linux Navigation, File System Structure, Permissions, SSH, Basic Shell Scripting |
| **System Security & Privilege Hardening** | The Cyber Mentor (TCM Security) | `Linux for Hackers` / `Linux Privilege Escalation` | https://www.youtube.com/@TCMSecurityAcademy | Linux Administration, SUID/SGID Exploitation & Mitigation, User Group Security |
| **Packet Analysis & Protocol Decoding** | Chris Greer | `Wireshark Masterclass` / `Wireshark Packet Analysis` | https://www.youtube.com/@ChrisGreer | Deep Packet Inspection, TCP Handshake Decoding, Frame Analysis, Retransmissions |
| **Host Internals & Investigation** | John Hammond | `Windows Internals & Reverse Engineering Basics` | https://www.youtube.com/@JohnHammond | Windows Process Inspection, Sysinternals Suite, Sysmon Triage, Persistence Analysis |

---

## Module 5: Official Documentation & Standards
## الوحدة 5: الوثائق والمعايير الرسمية وأطر العمل التنظيمية

All security designs and procedural baselines in this playbook reference the following authoritative, open-access international standards:

تعتمد جميع التصاميم الهندسية والإجراءات الأمنية في هذا الدليل على المعايير والمراجع الدولية الرسمية المفتوحة التالية:

1. **NIST SP 800-82 Rev. 3 - Guide to Operational Technology (OT) Security:**
   - Authoritative guidance on security architectures, vulnerability management, and defensive countermeasures for SCADA, DCS, and PLC ecosystems.
   - Access: https://csrc.nist.gov/publications/detail/sp/800-82/rev-3/final

2. **ISA/IEC 62443 Series - Industrial Automation and Control Systems Security:**
   - Core standards for defining Security Levels (SL 1 to SL 4), structural Zones and Conduits (ISA/IEC 62443-3-2), and technical security requirements (ISA/IEC 62443-3-3).
   - Access: https://www.isa.org/standards-and-publications/isa-standards/isa-iec-62443-series

3. **CISA Cross-Sector Cybersecurity Performance Goals (CPGs):**
   - Prioritized, risk-informed subset of IT and OT cybersecurity practices developed by CISA and NIST to protect critical infrastructure assets.
   - Access: https://www.cisa.gov/cpgs

4. **MITRE ATT&CK for Enterprise & MITRE ATT&CK for ICS:**
   - Publicly available knowledge base of real-world cyber adversary behaviors, tactical categories, and mitigation mappings.
   - Access: https://attack.mitre.org/ and https://attack.mitre.org/matrices/ics/

5. **Center for Internet Security (CIS) Critical Security Controls v8:**
   - Prescriptive technical controls for defense-in-depth implementation across enterprise endpoints, boundary networks, and infrastructure.
   - Access: https://www.cisecurity.org/controls/cis-controls-list
