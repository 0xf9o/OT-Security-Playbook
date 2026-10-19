# Operational Technology (OT) Security Engineering Roadmap
# خريطة طريق هندسة أمن التكنولوجيا التشغيلية

---

## Executive Summary / الملخص التنفيذي

This document establishes a structured, rigorous, and vendor-agnostic progression path for transitioning from enterprise IT security into Operational Technology (OT) and Industrial Control Systems (ICS) security engineering. The curriculum emphasizes deterministic engineering environments, industrial networking protocols, threat landscape analysis, and defensive detection engineering.

تحدد هذه الوثيقة مساراً تدريبياً وهندسياً دقيقاً، مستقلاً عن تفضيلات الموردين التجاريين، للانتقال من أمن تقنية المعلومات المؤسسية (IT) إلى هندسة أمن التكنولوجيا التشغيلية (OT) وأنظمة التحكم الصناعي (ICS). يركز هذا المسار على البيئات الهندسية الحتمية، بروتوكولات الشبكات الصناعية، تحليل مشهد التهديدات السيبرانية-الفيزيائية، وهندسة اكتشاف الهجمات.

---

## Phase 0: The Convergence (Prerequisites)
## المرحلة 0: التقارب بين تقنية المعلومات والتقنية التشغيلية (المتطلبات الأساسية)

### 1. Phase Objective / الهدف من المرحلة
- **EN:** Establish foundational competency in classical enterprise networking, Linux/Windows operating system internals, and Security Operations Center (SOC) telemetry triage necessary to contextualize OT boundary controls.
- **AR:** تأسيس الكفاءة الجوهرية في هندسة شبكات المؤسسات التقليدية، وبنية أنظمة التشغيل (لينكس وويندوز)، ومبادئ مراكز العمليات الأمنية (SOC) اللازمة لإدارة ضوابط حواف شبكات التكنولوجيا التشغيلية.

### 2. Key Concepts to Master / المفاهيم الأساسية المستهدفة
- **EN:**
  - Layer 2 / Layer 3 network architecture: VLAN segregation, subnetting, spanning tree, unicast/multicast dynamics.
  - Deep packet inspection fundamentals using CLI tools (`tcpdump`, `tshark`) and GUI analyzers (`Wireshark`).
  - Windows internals relevant to enterprise interfaces: Active Directory, Kerberos, SMB, RPC, and DCOM.
  - Linux kernel fundamentals, process management, enterprise logging (`rsyslog`, `journald`), and network stack tuning.
  - Telemetry pipeline architectures: Syslog forwarding, SIEM ingestion schemas, and baseline anomaly detection.
- **AR:**
  - معماريات الطبقة الثانية والثالثة من نموذج OSI: عزل الشبكات المحلية الظاهرية (VLAN)، تقسيم الشبكات، بروتوكولات الشجرة الممتدة، وآليات الإرسال الفردي والجماعي.
  - التحليل العميق للحزم البرمجية باستخدام أدوات سطر الأوامر (`tcpdump`، `tshark`) وأدوات الواجهات الرسومية (`Wireshark`).
  - البنية الداخلية لأنظمة ويندوز المرتبطة بالواجهات المؤسسية: دليل الخادم النشط (Active Directory)، ومصادقة Kerberos، وبروتوكولات SMB وRPC وDCOM.
  - البنية التحتية لنظام لينكس، وإدارة العمليات، وإدارة سجلات الأحداث المؤسسية (`rsyslog`، `journald`)، وضبط مسار حزم الشبكة.
  - معماريات معالجة وتدفق البيانات الأمنية: إعادة توجيه السجلات عبر Syslog، ونماذج استيعاب أنظمة SIEM، ورصد الانحرافات عن الأنماط القياسية.

### 3. Recommended Free Resources & Milestones / الموارد المجانية وخطوات التنفيذ
| Resource / Resource Type | Focus Area | Source |
| :--- | :--- | :--- |
| Wireshark Sample Captures | Enterprise Packet Analysis | Wireshark Foundation Repository |
| CISA Cross-Sector Cybersecurity Performance Goals | Enterprise-to-Industrial Baselines | Cybersecurity and Infrastructure Security Agency (CISA) |
| MITRE ATT&CK for Enterprise | Threat Mapping & Telemetry Prioritization | MITRE Corporation |

---

## Phase 1: Industrial Machinery & ICS Basics
## المرحلة 1: الآلات الصناعية وأساسيات أنظمة التحكم الصناعي

### 1. Phase Objective / الهدف من المرحلة
- **EN:** Master the anatomy, operational physics, and architectural hierarchies of industrial production systems, including real-time control loops, field instrumentation, and the Purdue Enterprise Reference Architecture (PERA / ISA-95).
- **AR:** إتقان التشريح الهيكلي، والديناميكيات الفيزيائية التشغيلية، والتسلسلات الهرمية المعمارية لأنظمة الإنتاج الصناعي، بما يشمل حلقات التحكم في الوقت الحقيقي، أجهزة القياس الحقلية، ونموذج بيردو المرجعي (PERA / ISA-95).

### 2. Key Concepts to Master / المفاهيم الأساسية المستهدفة
- **EN:**
  - Control loop theory: Open-loop vs. Closed-loop systems, Proportional-Integral-Derivative (PID) controllers, feedback latency.
  - Field instrumentation: Sensors (analog 4-20mA, digital 24VDC), actuators, variable frequency drives (VFDs), relays, and I/O bus modules.
  - Core industrial compute hardware: Programmable Logic Controllers (PLCs), Remote Terminal Units (RTUs), and Distributed Control Systems (DCS).
  - The deterministic PLC execution cycle: Input Scan -> Program Execution -> Output Write -> Housekeeping/Diagnostics.
  - Supervisory components: Human-Machine Interfaces (HMIs), Engineering Workstations (EWS), and Process Historians.
  - Purdue Enterprise Reference Architecture (PERA) / ISA-95 functional level stratification:
    - Level 0: Physical process (Sensors, actuators).
    - Level 1: Basic control (PLCs, RTUs, safety controllers).
    - Level 2: Area supervisory control (Local HMIs, single-operator consoles).
    - Level 3: Site operations / Manufacturing Operations Management (MOM, Historians, EWS).
    - Industrial Demilitarized Zone (IDMZ / Level 3.5): Firewalls, jump hosts, reverse proxies.
    - Level 4/5: Enterprise business network and Internet boundary.
  - Basic Process Control Systems (BPCS) vs. Safety Instrumented Systems (SIS), Safety Integrity Levels (SIL 1 to SIL 4), and physical fail-safe modes.
- **AR:**
  - نظرية حلقات التحكم: الأنظمة المفتوحة والمغلقة، ومتحكمات التناسب والتكامل والتفاضل (PID)، وزمن استجابة التغذية الراجعة.
  - الأجهزة الميدانية وأدوات القياس: المستشعرات (الجهد التناظري 4-20mA، الرقمي 24VDC)، المشغلات الميكانيكية، محركات التردد المتغير (VFD)، ومرحلات التوصيل.
  - عتاد المعالجة الصناعية: أجهزة التحكم المنطقي القابلة للبرمجة (PLC)، وحدات الإرسال الطرفية (RTU)، وأنظمة التحكم الموزعة (DCS).
  - دورة التنفيذ الحتمية لمتحكمات PLC: قراءة المدخلات -> تنفيذ منطق التحكم -> تحديث المخرجات -> الفحوصات الذاتية والتشخيصية.
  - المكونات الإشرافية: واجهات الإنسان والآلة (HMI)، محطات العمل الهندسية (EWS)، وقواعد بيانات الأرشفة التاريخية (Historians).
  - مستويات نموذج بيردو المرجعي (PERA / ISA-95):
    - المستوى 0: العمليات الفيزيائية والمستشعرات والمشغلات.
    - المستوى 1: التحكم الأساسي (PLCs، RTUs، أنظمة التحكم الآمن).
    - المستوى 2: التحكم الإشرافي الموضعي (واجهات HMI الميدانية، محطات المشغلين).
    - المستوى 3: عمليات الموقع وإدارة التصنيع (Historians، MOM، محطات EWS).
    - المنطقة الصناعية منزوعة السلاح (IDMZ / Level 3.5): جدران الحماية، وخوادم العبور الآمنة، والوكلاء العكسيون.
    - المستوى 4/5: شبكة المؤسسة والأعمال والاتصال الخارجي.
  - التمييز بين أنظمة التحكم الأساسية (BPCS) وأنظمة الأمان الصناعي (SIS)، ومستويات تكامل الأمان (SIL 1 إلى SIL 4)، وحالات الإيقاف الآمن فيزيائياً.

### 3. Recommended Free Resources & Milestones / الموارد المجانية وخطوات التنفيذ
| Standard / Guide | Scope | Issuing Body |
| :--- | :--- | :--- |
| NIST Special Publication 800-82 Rev. 3 | Guide to Operational Technology (OT) Security | National Institute of Standards and Technology (NIST) |
| ISA/IEC 62443 Security Standards Overview | Security for Industrial Automation and Control Systems | International Society of Automation (ISA) / IEC |
| CISA ICS Training Curriculum | Web-Based Training Courses (101-201) | CISA Virtual Learning Portal (VLP) |

---

## Phase 2: Industrial Protocols
## المرحلة 2: بروتوكولات التكنولوجيا التشغيلية

### 1. Phase Objective / الهدف من المرحلة
- **EN:** Dissect the syntax, transport mechanisms, state transitions, and inherent vulnerabilities (cleartext transport, missing authentication, replay exposure) of dominant fieldbus and Ethernet industrial automation protocols.
- **AR:** فحص البنية اللغوية، وآليات النقل، وتحولات الحالات، ونقاط الضعف الهيكلية (نقل البيانات غير المشفر، غياب آليات المصادقة، والتعرض لهجمات إعادة الإرسال) في بروتوكولات الأتمتة الصناعية السلكية والإيثرنتية.

### 2. Key Concepts to Master / المفاهيم الأساسية المستهدفة
- **EN:**
  - Common operational limitations: Determinism requirements, payload size constraints, absence of session persistence.
  - **Modbus Protocol Family (Modbus RTU over RS-485 vs. Modbus TCP over Port 502):**
    - Modbus Application Protocol (MBAP) header: Transaction ID, Protocol ID (0x0000), Length, Unit Identifier.
    - Protocol Data Unit (PDU): Function Codes (FC01 Read Coils, FC03 Read Holding Registers, FC05 Write Single Coil, FC16/0x10 Write Multiple Registers).
    - Security deficiencies: Zero encryption, zero native authentication, unauthenticated command injection susceptibility.
  - **DNP3 (Distributed Network Protocol - IEEE 1815 over TCP/UDP Port 20000):**
    - Outstation and Master communications: Polling vs. Unsolicited Response models.
    - Internal Indications (IIN) status registers and object groups/variations.
    - DNP3 Secure Authentication version 5 (SAv5): Challenge-response mechanics, symmetric HMAC keys, operational adoption barriers.
  - **OPC Unified Architecture (OPC UA over TCP Port 4840):**
    - Transition from legacy OPC DA (DCOM-dependent) to modern OPC UA service-oriented architecture.
    - Address space structure: Nodes, NodeIDs, Attributes, and References.
    - Built-in security architecture: Transport layer profiles, X.509 digital certificates, Cryptographic suites (`Basic256Sha256`, `Aes128_Sha256_RsaOaep`), Sign-only vs. SignAndEncrypt modes.
- **AR:**
  - المحددات التشغيلية الشائعة: متطلبات الحتمية الزمنية، قيود أحجام الحمولات، والافتقار إلى استمرارية الجلسات المعتمدة.
  - **عائلة بروتوكولات مودباص (Modbus RTU عبر RS-485 مقابل Modbus TCP عبر منفذ 502):**
    - ترويسة بروتوكول تطبيق مودباص (MBAP Header): معرف المعاملة، معرف البروتوكول (0x0000)، حقل الطول، ومعرف الوحدة الطرفية.
    - وحدة بيانات البروتوكول (PDU): رموز الوظائف (FC01 قراءة الملفات، FC03 قراءة سجلات التخزين، FC05 كتابة ملف منفرد، FC16 كتابة سجلات متعددة).
    - الثغرات الهيكلية: انعدام التشفير بالكامل، غياب آليات المصادقة الأصلية، والقابلية لحقن الأوامر التحكمية غير المصرح بها.
  - **بروتوكول DNP3 (بروتوكول الشبكة الموزعة - معيار IEEE 1815 عبر المنفذ 20000):**
    - اتصالات المحطات الفرعية (Outstations) والمحطة الرئيسية (Master): نماذج الاستعلام الدوري مقابل الاستجابات غير المطلوبة.
    - سجلات مؤشرات الحالة الداخلية (IIN)، ومجموعات الكائنات وتفريعاتها.
    - آلية المصادقة الآمنة للبروتوكول (SAv5): التحدي والاستجابة، استخدام مفاتيح التوقيع الرقمي المتماثل (HMAC)، والتحديات التشغيلية للتطبيق.
  - **معمارية OPC UA (اتصالات المنصة المفتوحة الموحدة عبر المنفذ 4840):**
    - التحول من معمارية OPC DA القديمة (المعتمدة على DCOM) إلى معمارية OPC UA الحديثة الموجهة للخدمات.
    - هيكل فضاء العناوين: العقد (Nodes)، معرفات العقد (NodeIDs)، السمات، والمراجع الارتباطية.
    - نموذج الأمان المدمج: ملفات طبقة النقل، شهادات X.509 الرقمية، حزم التشفير القياسية، وأوضاع التوقيع فقط مقابل التوقيع والتشفير المتزامن.

### 3. Protocol Comparison Matrix / مصفوفة المقارنة الفنية للبروتوكولات
| Protocol | Default Port | Transport | Native Encryption | Native Authentication | Primary Sector |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Modbus TCP | 502 | TCP | No (Plaintext) | No | Manufacturing, Water, General Industrial |
| DNP3 | 20000 | TCP / UDP | No (Plaintext) | Optional (SAv5) | Electric Utilities, Water Distribution |
| OPC UA | 4840 | TCP | Yes (Configurable) | Yes (X.509 / User tokens) | Cross-industry, Process Automation |

---

## Phase 3: OT Threat Landscape & Detection
## المرحلة 3: مشهد التهديدات وهندسة الاكتشاف في التكنولوجيا التشغيلية

### 1. Phase Objective / الهدف من المرحلة
- **EN:** Analyze the mechanics of historically significant cyber-physical malware, map adversary techniques to the MITRE ATT&CK for ICS framework, and engineer passive, non-intrusive network detection rules.
- **AR:** تحليل الآليات التنفيذية للهجمات البرمجية ذات التأثير الفيزيائي تاريخياً، ومطابقة تقنيات المهاجمين مع إطار MITRE ATT&CK for ICS، وهندسة قواعد مراقبة سلبية غير متداخلة لاكتشاف الأنشطة المعادية.

### 2. Key Concepts to Master / المفاهيم الأساسية المستهدفة
- **EN:**
  - Case studies in cyber-physical disruption:
    - **Stuxnet (2010):** Frequency converter tampering, Siemens Step7 rootkits, man-in-the-middle against PLC process memory.
    - **BlackEnergy 3 / Industroyer (CrashOverride) (2015/2016):** Substation circuit breaker manipulation via IEC 60870-5-104, IEC 61850, and OPC DA.
    - **TRITON / TRISIS (2017):** Schneider Electric Triconex Safety Instrumented Systems (SIS) memory manipulation via proprietary TriStation protocol injection.
    - **INCONTROLLER / Pipedream (2022):** Modular framework targeting industrial controllers via CODESYS, Modbus, and Omron protocols.
  - MITRE ATT&CK for ICS TTP matrix: Initial Access, Execution, Persistence, Inhibit Response Function, Impair Process Control.
  - Passive industrial monitoring constraints: Prohibition of active vulnerability scanning (e.g., standard Nmap/Nessus) on operational PLCs to prevent equipment fault conditions.
  - Network TAP and SPAN port aggregation at Purdue Model Levels 1, 2, and 3.5.
  - Signature-based and behavioral detection using Zeek ICS protocol analyzers and Suricata rule crafting.
- **AR:**
  - دراسات حالة للحوادث التخريبية السيبرانية-الفيزيائية:
    - **Stuxnet (2010):** التلاعب بمغيرات التردد لمحركات الطرد المركزي، وبرمجيات التخفي على مستوى برمجيات Siemens Step7، وهجمات الوسيط ضد ذاكرة معالجة الـ PLC.
    - **BlackEnergy 3 / Industroyer (2015/2016):** التحكم غير المصرح به في قواطع الدوائر الكهربائية بمحطات التحويل باستخدام بروتوكولات IEC 60870-5-104 وIEC 61850 وOPC DA.
    - **TRITON / TRISIS (2017):** استهداف أنظمة الأمان والسلامة الصناعية (Schneider Electric Triconex SIS) عبر حقن تعليمات بروتوكول TriStation المملوك للمورد في ذاكرة النظام.
    - **INCONTROLLER / Pipedream (2022):** منصة هجومية نمطية تستهدف أجهزة التحكم الصناعية عبر استغلال بروتوكولات CODESYS ومودباص وأومرون.
  - مصفوفة التكتيكات والتقنيات (MITRE ATT&CK for ICS): النفاذ المبدئي، التنفيذ، التخفي، تعطيل وظائف الاستجابة، وإضعاف التحكم في العمليات.
  - محددات المراقبة السلبية في البيئات الصناعية: حظر الفحص النشط للثغرات على أجهزة التحكم المشغلة لتجنب توقف المعدات أو دخولها في حالات الخطأ الحرج (Fault States).
  - استخدام منافذ العكس (SPAN) وموزعات الإشارة السلبية (TAPs) على مستويات نموذج بيردو (المستوى 1 و2 و3.5).
  - الاكتشاف المعتمد على التواقيع والسلوكيات باستخدام محللات Zeek للبروتوكولات الصناعية، وتطوير قواعد اكتشاف متخصصة عبر Suricata.

### 3. Practical Detection Artifact / نموذج عملي لقاعدة اكتشاف (Suricata)
```suricata
# Detect unauthorized Modbus TCP write to multiple registers (FC 16 / 0x10) targeting safety/critical address space
alert tcp any any -> [192.168.10.0/24] 502 (
    msg:"SEC-OT-POLICY: Modbus TCP FC16 Write Multiple Registers to Level 1 Controller";
    flow:to_server,established;
    content:"|00 00|"; offset:2; depth:2; # Modbus Protocol ID
    byte_test:1,=,16,7;                   # Modbus Function Code 16 (0x10)
    threshold:type limit, track by_src, count 1, seconds 60;
    classtype:policy-violation;
    sid:1000001;
    rev:1;
)
```

---

## Phase 4: Labs & Hands-on
## المرحلة 4: المختبرات العملية والتطبيق التجريبي

### 1. Phase Objective / الهدف من المرحلة
- **EN:** Build local, containerized, and virtualized Software-in-the-Loop (SITL) industrial simulation testbeds to generate, capture, and inspect operational telemetries, control logic routines, and synthetic attacks.
- **AR:** بناء وتجهيز منصات اختبار افتراضية وحاويات برمجية معزولة (SITL) لمحاكاة الأنظمة الصناعية، بهدف توليد والتقاط وفحص بيانات القياس، شيفرات التحكم المنطقي، وسيناريوهات الهجوم المصطنعة.

### 2. Key Concepts to Master / المفاهيم الأساسية المستهدفة
- **EN:**
  - Deployment of open-source soft-PLCs: Configuring OpenPLC Runtime (v3) on Linux.
  - Ladder Logic and Structured Text engineering in compliance with IEC 61131-3.
  - Software-in-the-Loop (SITL) physical dynamics coupling using EPANET (water hydraulics and pipe modeling).
  - Virtual HMI integration: Connecting ScadaBR or FUXA to soft-PLCs over Modbus TCP.
  - Synthetic adversary generation: Using Python (`pymodbus`, `scapy`) to execute unauthorized telemetry injection, register spoofing, and rogue command dispatch.
  - Full-stack defensive capture pipeline: Generating `.pcap` files from testbed runs and validating Zeek OT parsers (`zeek-plugin-modbus`, `zeek-plugin-dnp3`).
- **AR:**
  - تثبيت وتشغيل متحكمات برمجية مفتوحة المصدر: إعداد منصة OpenPLC Runtime (الإصدار 3) على بيئة لينكس.
  - برمجة منطق التحكم باستخدام مخطط السلم (Ladder Logic) والنص المهيكل وفق المعيار الدولي IEC 61131-3.
  - ربط المحاكاة البرمجية بالديناميكيات الفيزيائية الواقعية باستخدام محاكي شبكات المياه والأنابيب (EPANET).
  - تكامل واجهات HMI الافتراضية: ربط منصات ScadaBR أو FUXA بالمتحكمات البرمجية عبر بروتوكول Modbus TCP.
  - توليد سيناريوهات الهجوم الاصطناعية: استخدام لغة بايثون ومكتبات (`pymodbus` و`scapy`) لحقن بيانات قياس مزيفة، والتلاعب بقيم السجلات، وإرسال أوامر غير مصرح بها.
  - بناء مسار فحص وتحليل دفاعي متكامل: تسجيل ملفات التحليل الشبكي (`.pcap`) أثناء تشغيل المنصة، واختبار كفاءة محللات Zeek الصناعية.

### 3. Open-Source Ecosystem Roadmap / بيئة العمل والأدوات المفتوحة الموصى بها
```text
+--------------------------------------------------------------+
|                    Supervisory Layer                         |
|         [ FUXA / ScadaBR Web HMI ] (Port 80/1880)            |
+--------------------------------------------------------------+
                                |
                   Modbus TCP / OPC UA (Port 502 / 4840)
                                |
+--------------------------------------------------------------+
|                     Control Layer                            |
|       [ OpenPLC Runtime v3 Engine (IEC 61131-3 Logic) ]      |
+--------------------------------------------------------------+
                                |
                   TCP Sockets / Shared Memory
                                |
+--------------------------------------------------------------+
|                 Physical Process Modeling                    |
|      [ EPANET / Python Physics Simulation Pipeline ]         |
+--------------------------------------------------------------+
```

| Tool | Functional Role | Project Source |
| :--- | :--- | :--- |
| OpenPLC Project | IEC 61131-3 Compliant Soft-PLC | openplcproject.com |
| FUXA | Web-based SCADA / HMI Engine | github.com/frangoteam/FUXA |
| EPANET | Hydraulic and Water Infrastructure Modeling | US Environmental Protection Agency (EPA) |
| GRFICSv2 | Graphical Realism Framework for ICS Cybersecurity | github.com/Fortiphyd/GRFICSv2 |
