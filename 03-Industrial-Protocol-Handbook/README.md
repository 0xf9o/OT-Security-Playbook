# Industrial Protocol Handbook
# دليل بروتوكولات الشبكات الصناعية

---

## Executive Summary / الملخص التنفيذي

This handbook provides an authoritative, technical analysis of foundational Industrial Control Systems (ICS) and Operational Technology (OT) communication protocols. Unlike standard enterprise IT protocols that prioritize data confidentiality, industrial protocols were historically engineered for deterministic latency, zero interruption, and physical process control. Consequently, legacy fieldbus and Ethernet protocols operate without native cryptographic validation, authentication, or integrity checks. This document details protocol architectures, message structures, packet analysis methodologies, and verifiable open datasets for practical security engineering.

يقدم هذا الدليل تحليلاً تقنياً وهندسياً لبروتوكولات الاتصال الأساسية في أنظمة التحكم الصناعي (ICS) والتكنولوجيا التشغيلية (OT). بخلاف بروتوكولات تقنية المعلومات التقليدية التي تعطي الأولوية لسرية البيانات، صُممت البروتوكولات الصناعية تاريخياً لتوفير الحتمية الزمنية، الاستجابة الفورية، واستمرارية التحكم في العمليات الفيزيائية. ونتيجة لذلك، تعمل البروتوكولات الميدانية القديمة دون آليات أصلية للمصادقة أو التشفير أو التحقق من سلامة البيانات. توضح هذه الوثيقة البنى المعمارية للبروتوكولات، هياكل الحزم، منهجيات تحليل حركة المرور الشبكية، والمصادر المفتوحة المعتمدة للتدريب العملي.

---

## Module 1: Modbus TCP
## الوحدة 1: بروتوكول مودباص عبر إيثرنت (Modbus TCP)

### 1. Protocol Architecture & Mechanics / بنية البروتوكول وآليات العمل
- **EN:**
  - **Transport & Port Mapping:** Modbus TCP operates over TCP port 502 as a client/server (historically master/slave) request-response protocol. The client initiates unicast command queries; the server processes the command and returns a response.
  - **Modbus Application Protocol (MBAP) Header (7 Bytes):**
    - `Transaction Identifier` (2 bytes): Request/response synchronization tracking.
    - `Protocol Identifier` (2 bytes): Always `0x0000` for Modbus TCP.
    - `Length` (2 bytes): Byte count of remaining payload including Unit ID and PDU.
    - `Unit Identifier` (1 byte): Identifies a remote slave address located on serial bridging networks (e.g., Modbus RTU gateways).
  - **Protocol Data Unit (PDU):** Consists of a 1-byte `Function Code` followed by a variable-length `Data` payload.
  - **Industrial Memory Models:**
    - Discrete Inputs (Read-only binary, 1-bit, addressed 10001-19999).
    - Coils (Read-write binary, 1-bit, addressed 00001-09999).
    - Input Registers (Read-only analog/measurement, 16-bit word, addressed 30001-39999).
    - Holding Registers (Read-write configuration/setpoints, 16-bit word, addressed 40001-49999).
  - **Critical Function Codes (FC):**
    - `FC01 (0x01)`: Read Coils.
    - `FC02 (0x02)`: Read Discrete Inputs.
    - `FC03 (0x03)`: Read Holding Registers.
    - `FC04 (0x04)`: Read Input Registers.
    - `FC05 (0x05)`: Write Single Coil (Force actuator/relay state).
    - `FC06 (0x06)`: Write Single Register.
    - `FC15 (0x0F)`: Write Multiple Coils.
    - `FC16 (0x10)`: Write Multiple Holding Registers (Setpoint modification).
    - `FC43 (0x2B)`: Read Device Identification (Encapsulated Interface Transport).
  - **Inherent Security Vulnerabilities:**
    - Absence of Authentication: Any network host reaching TCP 502 can execute command writes (e.g., trip breaker coils, alter turbine speed registers).
    - Absence of Encryption: Telemetry and process data are transmitted in plaintext, enabling passive eavesdropping, man-in-the-middle (MitM) alterations, and replay attacks.
    - Lack of Session State: Each transaction is treated independently without stateful validation.
- **AR:**
  - **طبقة النقل وتعيين المنافذ:** يعمل بروتوكول Modbus TCP عبر منفذ TCP 502 بنموذج العميل والخادم (تاريخياً: السيد/التابع) القائم على الطلب والاستجابة. يبدأ العميل بإرسال أوامر أحادية، بينما يقوم الخادم بمعالجة الأمر وإرجاع الاستجابة.
  - **ترويسة بروتوكول تطبيق مودباص (MBAP Header - 7 بايت):**
    - `معرف المعاملة (Transaction ID)` (2 بايت): لمزامنة وتتبع الطلب والاستجابة المقابلة.
    - `معرف البروتوكول (Protocol ID)` (2 بايت): يحمل دائماً القيمة `0x0000` للدلالة على Modbus TCP.
    - `حقل الطول (Length)` (2 بايت): عدد البايتات المتبقية في الحمولة بما يشمل معرف الوحدة ووحدة PDU.
    - `معرف الوحدة (Unit ID)` (1 بايت): لتحديد عنوان الجهاز الطرفي المتصل عبر بوابات التحويل التسلسلي (Modbus RTU Gateways).
  - **وحدة بيانات البروتوكول (PDU):** تتكون من 1 بايت يمثل `رمز الوظيفة (Function Code)` متبوعاً بحمولة `البيانات (Data)` متغيرة الحجم.
  - **نماذج الذاكرة الصناعية:**
    - المدخلات الرقمية المنفصلة (Discrete Inputs): قراءة فقط، 1 بت، العناوين من 10001 إلى 19999.
    - الملفات الثنائية (Coils): قراءة وكتابة، 1 بت، العناوين من 00001 إلى 09999 (تتحكم في المرحلات والقواطع).
    - سجلات المدخلات (Input Registers): قراءة فقط، 16 بت، العناوين من 30001 إلى 39999 (لقياسات أجهزة الاستشعار).
    - سجلات التخزين (Holding Registers): قراءة وكتابة، 16 بت، العناوين من 40001 إلى 49999 (لنقاط الضبط وإعدادات التحكم).
  - **رموز الوظائف الحرجة (Function Codes):**
    - `FC01 (0x01)`: قراءة الملفات الثنائية.
    - `FC02 (0x02)`: قراءة المدخلات الرقمية.
    - `FC03 (0x03)`: قراءة سجلات التخزين.
    - `FC04 (0x04)`: قراءة سجلات المدخلات.
    - `FC05 (0x05)`: كتابة ملف منفرد (تغيير حالة المشغل أو القاطع الميكانيكي قسرياً).
    - `FC06 (0x06)`: كتابة سجل تخزين منفرد.
    - `FC15 (0x0F)`: كتابة ملفات متعددة دفعة واحدة.
    - `FC16 (0x10)`: كتابة سجلات تخزين متعددة (تعديل نقاط الضبط الحساسة).
    - `FC43 (0x2B)`: قراءة المعرفات الفنية للجهاز والشركة المصنعة.
  - **نقاط الضعف الهيكلية للبروتوكول:**
    - غياب المصادقة: يستطيع أي جهاز قادر على الوصول إلى المنفذ 502 إرسال أوامر كتابة مباشرة للتحكم بالمعدات.
    - انعدام التشفير: تُبث جميع البيانات والقياسات بنص صريح، مما يتيح التجسس السلبي، هجمات الوسيط (MitM)، وهجمات إعادة الإرسال.
    - غياب إدارة الحالة: تعامل كل معاملة بشكل منفصل دون تحقق تسلسلي من سياق الجلسة.

### 2. Standard References / المراجع القياسية
- **Modbus Application Protocol Specification v1.1b3:** Modbus Organization - https://modbus.org/docs/Modbus_Application_Protocol_V1_1b3.pdf
- **Modbus Messaging on TCP/IP Implementation Guide v1.0b:** Modbus Organization - https://modbus.org/docs/Modbus_Messaging_Implementation_Guide_V1_0b.pdf
- **Wireshark Protocol Specification:** Modbus/TCP Dissector - https://gitlab.com/wireshark/wireshark/-/wikis/Modbus

---

## Module 2: DNP3 & OPC UA
## الوحدة 2: بروتوكول DNP3 ومعمارية OPC UA

### 1. DNP3 Protocol Mechanics (IEEE 1815) / آليات بروتوكول DNP3
- **EN:**
  - **Operational Purpose:** Designed for the power generation, electrical transmission, water distribution, and utility SCADA environments over low-bandwidth or unreliable networks.
  - **Transport Layer:** Typically encapsulated over TCP or UDP port 20000, as well as serial links.
  - **Master/Outstation Paradigm:** The Outstation (RTU/PLC in substations) gathers field telemetry. The Master (Control Center SCADA) issues polling queries or receives spontaneous updates.
  - **Polling vs. Unsolicited Responses:** Unlike Modbus, DNP3 allows outstations to independently initiate event reports (`Unsolicited Responses`) when critical process thresholds are violated, reducing polling latency.
  - **Internal Indications (IIN) Flags:** Two bytes appended to every application-layer response indicating internal device states (e.g., `Device Restart`, `Hardware Failure`, `Point Trouble`, `Buffer Overflow`).
  - **Security Deficiencies & DNP3 SAv5:** Traditional DNP3 provides zero cryptographic integrity or confidentiality. DNP3 Secure Authentication version 5 (IEEE 1815-2012 SAv5) introduces challenge-response message authentication using keyed-HMAC (SHA-256) to verify master and outstation identities, though transmission payloads remain unencrypted.
- **AR:**
  - **الغرض التشغيلي:** صُمم البروتوكول لقطاعات توليد ونقل الطاقة الكهربائية، وشبكات توزيع المياه، والبنى التحتية للمرافق العامة عبر شبكات ذات نطاق ترددي منخفض أو موثوقية متذبذبة.
  - **طبقة النقل:** يُنقل عادة عبر منفذ TCP أو UDP رقم 20000، بالإضافة إلى الروابط التسلسلية الميدانية.
  - **نموذج المحطة الرئيسية والمحطة التابعة:** تقوم المحطة التابعة (Outstation / RTU في محطات التحويل) بتجميع بيانات الحقل، بينما يصدر مركز التحكم الإشرافي (Master) طلبات الاستعلام الدوري أو يستقبل التحديثات الفورية.
  - **الاستعلام الدوري مقابل الاستجابات غير المطلوبة:** بخلاف مودباص، يدعم DNP3 إمكانية إرسال المحطات التابعة لتقارير أحداث فورية عند تجاوز حدود تشغيلية حرجة، مما يقلل التأخير الزمني للاستعلام.
  - **رايات مؤشرات الحالة الداخلية (IIN):** بايتان يُلحقان بكل استجابة في طبقة التطبيقات للإشارة إلى الحالة الداخلية للجهاز (مثل: إعادة تشغيل الجهاز، عطل عتادي، مشكلة في نقطة قياس، أو امتلاء الذاكرة المؤقتة).
  - **القصور الأمني وتحديث DNP3 SAv5:** يفتقر DNP3 التقليدي للتشفير وسلامة البيانات. يقدم معيار DNP3 Secure Authentication (الإصدار الخامس SAv5) آلية التحدي والاستجابة للتحقق من هوية الأطراف باستخدام مفاتيح HMAC (SHA-256)، مع بقاء حمولة البيانات غير مشفرة.

### 2. OPC Unified Architecture (IEC 62541) / معمارية OPC UA
- **EN:**
  - **Evolution from Legacy OPC DA:** Replaces vendor-locked Microsoft DCOM architecture (OPC Data Access) with an open, cross-platform, service-oriented architecture operating over TCP port 4840.
  - **Information Modeling & Address Space:** Exposes real-time process data, historical alarms, and engineering methods via an object-oriented node structure (Objects, Variables, Methods) referenced by specific `NodeIDs`.
  - **Native Cryptographic Framework:**
    - Transport Profiles: `opc.tcp://` binary transport.
    - Security Modes: `None` (insecure testing), `Sign` (message authentication and integrity), `SignAndEncrypt` (complete confidentiality and integrity).
    - Cryptographic Suites: Standardized profiles including `Basic256Sha256` and `Aes128_Sha256_RsaOaep`.
    - Public Key Infrastructure (PKI): Mutual authentication using X.509 digital certificates managed via Trust Lists and Certificate Revocation Lists (CRLs).
    - User Authentication: Independent user token authentication (Anonymous, Username/Password, X.509 Certificate, JSON Web Tokens).
- **AR:**
  - **التطور من معيار OPC DA القديم:** تم استبدال معمارية DCOM المرتبطة بنظام ويندوز بمعمارية خدماتية مفتوحة ومستقلة عن أنظمة التشغيل تعمل عبر منفذ TCP 4840.
  - **نمذجة المعلومات وفضاء العناوين:** يتيح قراءة البيانات في الوقت الحقيقي، سجلات الإنذارات، والوظائف الهندسية عبر هيكل كائني يعتمد على العقد (Nodes) والمعرفة بأرقام فريدة (`NodeIDs`).
  - **إطار العمل الأمني الأصيل:**
    - بروتوكول النقل: النقل الثنائي المحسن `opc.tcp://`.
    - أوضاع الأمان: `None` (غير آمن للاختبار فقط)، `Sign` (التوقيع الرقمي لضمان السلامة والمصادقة)، `SignAndEncrypt` (التوقيع والتشفير الشامل لضمان السرية والسلامة).
    - الحزم التشفيرية: حزم معيارية تشمل `Basic256Sha256` و`Aes128_Sha256_RsaOaep`.
    - البنية التحتية للمفاتيح العامة (PKI): المصادقة التبادلية بين العميل والخادم باستخدام شهادات X.509 الرقمية وقوائم الثقة وقوائم الإلغاء (CRLs).
    - مصادقة المستخدمين: رموز تحقق مستقلة (مستخدم مجهول، اسم مستخدم وكلمة مرور، شهادة X.509، أو رموز JWT).

### 3. Protocol Comparison Matrix / مصفوفة المقارنة الفنية
| Metric / المعيار الفني | Modbus TCP | DNP3 (IEEE 1815) | OPC UA (IEC 62541) |
| :--- | :--- | :--- | :--- |
| **Default Port** | TCP 502 | TCP / UDP 20000 | TCP 4840 |
| **Transport Model** | Request-Response | Polling & Unsolicited | Publish-Subscribe & Client-Server |
| **Data Encoding** | Big-Endian Binary | Binary Object Blocks | Binary / XML / JSON |
| **Confidentiality (Encryption)** | None (Plaintext) | None (Plaintext) | Yes (`SignAndEncrypt` AES-128/256) |
| **Integrity & Authentication** | None | Optional via SAv5 (HMAC-SHA256) | Native via X.509 Certificates & Signatures |
| **Primary Industry** | Manufacturing, Water, Utilities | Electrical Utilities, Power Grid | Cross-Sector, Industrial IoT, Process DCS |

### 4. Standard References / المراجع القياسية
- **IEEE Std 1815-2012:** Standard for Electric Power Systems Communications - Distributed Network Protocol (DNP3) - IEEE.
- **IEC 62541-SER:** OPC Unified Architecture - Parts 1 through 14 - International Electrotechnical Commission.
- **OPC Foundation Unified Architecture Security Guidelines:** OPC Foundation - https://opcfoundation.org/developer-tools/documents/

---

## Module 3: ICS Packet Analysis (PCAP)
## الوحدة 3: تحليل حزم بروتوكولات التحكم الصناعي (PCAP)

### 1. Packet Dissection in Wireshark / تفكيك وتحليل الحزم عبر برنامج Wireshark
- **EN:**
  - **Modbus TCP Packet Structure Analysis:**
    - Frame breakdown: Ethernet II -> IP -> TCP -> `Modbus/TCP` -> `Modbus`.
    - Verification of MBAP Header fields: `Transaction ID`, `Protocol ID (0x0000)`, `Length`, `Unit ID`.
    - Dissection of Function Code payloads and byte counts.
  - **DNP3 Protocol Dissection:**
    - Frame breakdown: Ethernet II -> IP -> TCP/UDP -> `DNP 3.0` Data Link Layer -> Transport Function -> Application Layer.
    - Identification of Application Layer Function Codes and response IIN status bits.
  - **Spotting Protocol Anomalies:**
    - High-frequency write bursts indicating unauthorized setpoint manipulation or adversary scripts.
    - Cold Restart (`Function Code 13`) or Warm Restart (`Function Code 14`) commands dispatched to RTUs.
    - Diagnostic / Vendor-specific function codes used for memory dumping or undocumented firmware updates.
    - Modbus Exception Codes indicating reconnaissance: `Exception Code 01` (Illegal Function), `Exception Code 02` (Illegal Data Address).
- **AR:**
  - **تحليل هيكل حزم Modbus TCP:**
    - تسلسل الطبقات: إطار Ethernet II -> حزمة IP -> جلسة TCP -> ترويسة `Modbus/TCP` -> حمولة `Modbus`.
    - التحقق من حقول ترويسة MBAP: معرف المعاملة، معرف البروتوكول (0x0000)، الطول، ومعرف الوحدة.
    - فحص حمولات رموز الوظائف وعدد البايتات المحمولة.
  - **تفكيك وفحص بروتوكول DNP3:**
    - تسلسل الطبقات: إطار Ethernet II -> حزمة IP -> نقل TCP/UDP -> طبقة ربط البيانات `DNP 3.0` -> وظيفة النقل -> طبقة التطبيقات.
    - التعرف على رموز وظائف طبقة التطبيقات، وقراءة رايات الحالة الداخلية (IIN).
  - **اكتشاف الانحرافات والأنشطة المعادية في الشبكة:**
    - التدفق المفاجئ والكثيف لأوامر الكتابة مما يشير إلى محاولة تغيير نقاط الضبط أو تشغيل نصوص برمجية معادية.
    - رصد أوامر إعادة التشغيل البارد (`FC 13`) أو الدافئ (`FC 14`) الموجهة لأجهزة RTU في محطات التحويل.
    - رموز الوظائف التشخيصية الخاصة بالمورد المستخدمة في تفريغ محتويات الذاكرة أو تثبيت تحديثات برمجية غير موثقة.
    - استجابات أخطاء مودباص الدالة على عمليات الاستطلاع والمسح: `Exception Code 01` (وظيفة غير مسموحة)، `Exception Code 02` (عنوان سجل غير موجود).

### 2. Wireshark Display Filter Cheatsheet / مرجع مرشحات العرض في Wireshark
| Inspection Objective / هدف الفحص | Wireshark Display Filter Syntax | Technical Description |
| :--- | :--- | :--- |
| **All Modbus Traffic** | `mbtcp` | Filters all Modbus Application Protocol frames. |
| **Unauthorized Modbus Writes** | `modbus.func_code == 5 \|\| modbus.func_code == 6 \|\| modbus.func_code == 15 \|\| modbus.func_code == 16` | Isolates all coil and register write operations. |
| **Modbus Exception Responses** | `modbus.func_code >= 0x80` | Filters error responses returned by controllers. |
| **Specific Register Inspection** | `modbus.reference_num == 40001` | Traces access to a specific process register. |
| **All DNP3 Traffic** | `dnp3` | Isolates all Distributed Network Protocol frames. |
| **DNP3 Restart Commands** | `dnp3.al.func == 13 \|\| dnp3.al.func == 14` | Alerts on Cold Restart or Warm Restart dispatches. |
| **DNP3 Internal Device Trouble** | `dnp3.al.iin.rst == 1 \|\| dnp3.al.iin.dev_trouble == 1` | Flags controllers reporting hardware faults or restarts. |
| **OPC UA Discovery & Traffic** | `opcua` | Captures binary OPC UA sessions over TCP 4840. |
| **Insecure OPC UA Security** | `opcua.securityMode == 1` | Identifies sessions configured with SecurityMode `None`. |

---

## Module 4: Verified Free Learning Path (YouTube)
## الوحدة 4: مسار التعلم المجاني المعتمد (يوتيوب)

The following YouTube channels, lectures, and series are 100% free, publicly available, and verified for practical OT protocol and PCAP analysis:

القنوات والمحاضرات التالية مجانية بنسبة 100%، وموثقة ومتاحة للعامة على منصة YouTube، وتغطي تحليل بروتوكولات وحزم التكنولوجيا التشغيلية:

| Channel / القناة | Search Query / Playlist Title | Direct Channel URL | Core Technical Focus |
| :--- | :--- | :--- | :--- |
| **SANS ICS Security** | `SANS ICS Security Summit` / `ICS PCAP Analysis` | https://www.youtube.com/@SANSICSCybersecurity | In-depth technical presentations, ICS threat hunting, protocol dissection, and defensive engineering by industry leaders. |
| **Chris Greer** | `Wireshark Industrial Packet Analysis` / `TCP Deep Dive` | https://www.youtube.com/@ChrisGreer | Wire-level packet capture analysis, TCP transport troubleshooting, retransmissions, and custom Wireshark profiling. |
| **ICS Village** | `Defending Control Systems` / `Hack the Plant Tech Talks` | https://www.youtube.com/@ICSVillage | Real-world demonstrations of protocol manipulation, PLC vulnerability analysis, and hands-on capture reviews. |
| **OpenPLC Project** | `OpenPLC Overview and SCADA Addressing` | https://www.youtube.com/@ThiagoAlves | Understanding soft-PLC internal memory structures, Modbus mapping, and connecting physical I/O over industrial networks. |

---

## Module 5: Open Datasets & Official Documentation
## الوحدة 5: مجموعات البيانات المفتوحة والوثائق الرسمية

Practical mastery of OT security requires analyzing real-world operational packet captures. The following repositories provide open, verified ICS PCAPs:

يتطلب الإتقان العملي لأمن التكنولوجيا التشغيلية فحص عينات واقعية لحزم البيانات الصناعية. توفر المستودعات المفتوحة التالية ملفات PCAP موثقة للتدريب:

1. **CS3STHLM / 4SICS Industrial Security Captures:**
   - Real industrial capture datasets recorded during live industrial testbed attack and defense sessions (Modbus TCP, DNP3, IEC 60870-5-104, MMS).
   - Repository Access: https://www.netresec.com/?page=PCAP4SICS

2. **Automayt ICS-pcap Repository (Curated by Chris Sistrunk):**
   - Public GitHub archive of capture files for control protocols including Modbus, DNP3, IEC 61850, BACnet, and Ethernet/IP.
   - Access: https://github.com/automayt/ICS-pcap

3. **Wireshark Sample Captures Directory:**
   - Official Wireshark repository containing protocol-specific PCAP samples for Modbus, DNP3, and OPC UA.
   - Access: https://gitlab.com/wireshark/wireshark/-/wikis/SampleCaptures

4. **CISA ICS-CERT Technical Advisories & Guidance:**
   - Active industrial vulnerability assessments, protocol exploitation advisories, and architecture guidelines.
   - Access: https://www.cisa.gov/ics
