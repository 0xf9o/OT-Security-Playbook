# ICS Architecture Handbook
# دليل بنية ومعمارية أنظمة التحكم الصناعي

---

## Executive Summary / الملخص التنفيذي

This handbook provides an authoritative architectural reference for Industrial Control Systems (ICS) and Operational Technology (OT) networks. It details the structural segregation mandated by the Purdue Enterprise Reference Architecture (PERA), technical classifications and functional roles of critical OT hardware, the architecture of the Industrial Demilitarized Zone (iDMZ / Level 3.5), and methodologies for establishing defensible Zones and Conduits in accordance with international standards.

يقدم هذا الدليل مرجعاً هندسياً ومعمارياً شاملاً لشبكات أنظمة التحكم الصناعي (ICS) والتكنولوجيا التشغيلية (OT). يوضح الدليل آليات العزل الهيكلي المفروضة وفق نموذج بيردو المرجعي للمؤسسات (PERA)، والتصنيفات التقنية والأدوار الوظيفية لعتاد التحكم الصناعي الحرج، ومعمارية المنطقة الصناعية منزوعة السلاح (iDMZ / المستوى 3.5)، بالإضافة إلى منهجيات بناء المناطق والقنوات الأمنية (Zones and Conduits) بما يتوافق مع المعايير الدولية المعتمدة.

---

## Module 1: The Purdue Enterprise Reference Architecture (PERA)
## الوحدة 1: نموذج بيردو المرجعي للمؤسسات (PERA)

### 1. Structural Layer Stratification / التقسيم الطبقي الهيكلي
- **EN:**
  - **Origin & Purpose:** Developed under the Purdue University Consortium and codified within ISA-95 / IEC 62264, PERA establishes an engineering hierarchy separating high-level enterprise business logic from real-time physical processes.
  - **Level 0 - Physical Process:** The physical instrumentation layer where chemical, mechanical, and electrical processes occur. Contains sensors (thermocouples, pressure transmitters, flowmeters), actuators, variable frequency drives (VFDs), control valves, and electrical switchgear. Communication relies on hardwired analog (4-20mA, 0-10V), digital signals (24VDC), or fieldbuses (PROFIBUS-DP, Modbus RTU, Foundation Fieldbus).
  - **Level 1 - Basic Control:** The direct process interfacing and automation layer. Contains Programmable Logic Controllers (PLCs), Remote Terminal Units (RTUs), Intelligent Electronic Devices (IEDs), and Safety Instrumented Systems (SIS). These devices execute deterministic, sub-second scan cycles to directly read Level 0 inputs, execute programmed control algorithms, and drive Level 0 outputs.
  - **Level 2 - Area Supervisory Control:** The operational interface layer. Contains local Human-Machine Interfaces (HMIs), supervisory control systems, engineering consoles, and alarm management servers. Operators interact with Level 2 systems to monitor live production dynamics, review trend telemetry, and inject operational setpoints into Level 1 controllers.
  - **Level 3 - Site Manufacturing Operations & Control (MOM/MES):** The site-wide manufacturing management layer. Hosts process historians, Manufacturing Execution Systems (MES), advanced process control (APC) platforms, Engineering Workstations (EWS) for logic reprogramming, laboratory information management systems (LIMS), and site-dedicated OT active directory infrastructure.
  - **Level 3.5 - Industrial Demilitarized Zone (iDMZ):** The security perimeter boundary separating enterprise IT networks from operational industrial networks. No direct routing or trust relationships are permitted across this boundary.
  - **Level 4 - Site Business Planning & Logistics:** The enterprise IT layer managing plant production scheduling, supply chain management, and material tracking. Hosts Enterprise Resource Planning (ERP) systems, corporate email, file servers, and corporate business intelligence infrastructure.
  - **Level 5 - Enterprise Network & External Boundaries:** The global corporate WAN, data centers, cloud analytics, and Internet gateways.
- **AR:**
  - **النشأة والغرض الهيكلي:** طُوّر النموذج من خلال تحالف جامعة بيردو وصيغ معيارياً ضمن مواصفات ISA-95 / IEC 62264، ليحدد تسلسلاً هرمياً هندسياً يفصل بين المعالجات التجارية للمؤسسة والعمليات الفيزيائية في الوقت الحقيقي.
  - **المستوى 0 - العمليات الفيزيائية (Physical Process):** طبقة القياس والأجهزة الحقلية حيث تجري العمليات الكيميائية والميكانيكية والكهربائية. تضم أجهزة الاستشعار (المزدوجات الحرارية، أجهزة قياس الضغط والتدفق)، المشغلات الميكانيكية، محركات التردد المتغير (VFD)، وصمامات التحكم. تعتمد الاتصالات على إشارات تناظرية سلكية (4-20mA)، إشارات رقمية (24VDC)، أو شبكات الحقل (PROFIBUS، Modbus RTU).
  - **المستوى 1 - التحكم الأساسي (Basic Control):** طبقة الأتمتة المباشرة المرتبطة بالعملية. تضم أجهزة التحكم المنطقي القابلة للبرمجة (PLC)، وحدات الإرسال الطرفية (RTU)، الأجهزة الإلكترونية الذكية (IED)، وأنظمة السلامة الصناعية (SIS). تنفذ هذه الأجهزة دورات مسح حتمية تقاس بأجزاء من الثانية لقراءة مدخلات المستوى 0 وتنفيذ خوارزميات التحكم وتحديث المخرجات.
  - **المستوى 2 - التحكم الإشرافي الميداني (Area Supervisory Control):** طبقة واجهة التشغيل الميداني. تضم واجهات الإنسان والآلة (HMI) المحلية، أنظمة التحكم الإشرافي، محطات المشغلين، وخوادم إدارة الإنذارات. يتفاعل المشغلون مع هذا المستوى لمراقبة مؤشرات الإنتاج الحية، متابعة المخططات البيانية، وإدخال نقاط الضبط (Setpoints) إلى أجهزة المستوى 1.
  - **المستوى 3 - إدارة وعمليات التصنيع في الموقع (MOM/MES):** طبقة إدارة الإنتاج الشاملة للمصنع. تضم خوادم الأرشفة التاريخية (Historians)، أنظمة تنفيذ التصنيع (MES)، محطات العمل الهندسية (EWS) لتعديل منطق التحكم، أنظمة إدارة معلومات المختبرات (LIMS)، وخوادم إدارة النطاق المخصصة لشبكة التكنولوجيا التشغيلية.
  - **المستوى 3.5 - المنطقة الصناعية منزوعة السلاح (iDMZ):** الحدود الأمنية الفاصلة بين شبكات تقنية المعلومات للمؤسسة والشبكات الصناعية التشغيلية. يُحظر التوجيه المباشر لحزم البيانات أو بناء علاقات ثقة عبر هذه الحدود.
  - **المستوى 4 - تخطيط الأعمال واللوجستيات (Site Business Planning):** طبقة تقنية المعلومات المؤسسية التي تدير جداول الإنتاج وسلاسل الإمداد. تضم أنظمة تخطيط موارد المؤسسات (ERP)، البريد الإلكتروني، وخوادم الملفات المؤسسية.
  - **المستوى 5 - شبكة المؤسسة والحدود الخارجية (Enterprise Network):** شبكة المؤسسة الواسعة (WAN)، مراكز البيانات السحابية، بوابات الاتصال بالإنترنت، وخدمات التحليل المركزية.

### 2. Standard References / المراجع القياسية المعتمدة
- **ANSI/ISA-95.00.01 (IEC 62264-1):** Enterprise-Control System Integration - Part 1: Models and Terminology.
- **NIST SP 800-82 Rev. 3:** Guide to Operational Technology (OT) Security (Section 2: Overview of Operational Technology).
- **IEC 62443-1-1:** Security for Industrial Automation and Control Systems - Terminology, Concepts, and Models.

---

## Module 2: Core ICS Components
## الوحدة 2: المكونات الأساسية لأنظمة التحكم الصناعي

### 1. Component Deep-Dive & Per-Level Mapping / التحليل الفني للمكونات ومواقعها الهيكلية
- **EN:**
  - **Programmable Logic Controllers (PLCs) - Level 1:**
    - Hardened, microprocessor-based real-time controllers engineered to withstand industrial extremes (temperature, electromagnetic interference, physical vibration).
    - Architecture: Central Processing Unit (CPU), power supply, communication modules, and backplane bus hosting digital/analog I/O cards.
    - Execution Model: Deterministic scan cycle (Input Read -> Logic Execution -> Output Write -> Housekeeping).
    - Programming Languages (IEC 61131-3): Ladder Diagram (LD), Structured Text (ST), Function Block Diagram (FBD), Instruction List (IL), and Sequential Function Chart (SFC).
  - **Human-Machine Interfaces (HMIs) - Level 2 / Level 3:**
    - Graphical software interfaces running on embedded hardware or industrial PCs (IPC). Translates raw PLC register telemetry into visual process graphics, trend graphs, and alarm notification interfaces.
    - Risk Profile: HMIs hold write access to PLC holding registers. Compromise of an HMI gives adversaries direct authority to dispatch rogue setpoints or manipulate alarm thresholds.
  - **Supervisory Control and Data Acquisition (SCADA) - Levels 2 through 3 (Wide-Area):**
    - Distributed software architecture engineered for geographically dispersed infrastructure (oil and gas pipelines, municipal water systems, electrical transmission grids).
    - Architecture: Central SCADA servers (Master Terminal Units - MTUs) communicating over Wide Area Networks (WAN, satellite, cellular, radio) with remote RTUs and PLCs. Relies heavily on asynchronous protocols (DNP3, Modbus).
  - **Distributed Control Systems (DCS) - Levels 1 through 3 (Localized Process):**
    - Highly integrated control architecture engineered for localized, high-speed, continuous manufacturing plants (oil refineries, petrochemical complexes, pharmaceutical manufacturing, thermal power generation).
    - Distinction from SCADA/PLC: The DCS features a single unified engineering database across all controllers, HMIs, and historians. Control logic is distributed across redundant autonomous controllers located throughout the facility, rather than centralized.
- **AR:**
  - **أجهزة التحكم المنطقي القابلة للبرمجة (PLCs) - المستوى 1:**
    - أجهزة حاسوبية صناعية مصلبة تعمل في الوقت الحقيقي ومصممة لتحمل الظروف القاسية (الحرارة، التداخل الكهرومغناطيسي، والاهتزازات الميكانيكية).
    - المعمارية: وحدة معالجة مركزية (CPU)، وحدة إمداد بالطاقة، وحدات اتصال شبكي، وناقل اتصال داخلي يربط بطاقات الإدخال والإخراج الرقمية والتناظرية.
    - نموذج التنفيذ: دورة مسح حتمية دقيقة (قراءة المدخلات -> تنفيذ المنطق البرمجي -> تحديث المخرجات -> الفحوصات الذاتية).
    - لغات البرمجة المعتمدة (IEC 61131-3): مخطط السلم (LD)، النص المهيكل (ST)، مخطط الكتل الوظيفية (FBD)، قائمة التعليمات (IL)، ومخطط الوظائف المتسلسل (SFC).
  - **واجهات الإنسان والآلة (HMIs) - المستوى 2 / المستوى 3:**
    - واجهات رسومية تشغيلية تعمل على عتاد مدمج أو حواسيب صناعية (IPC). تقوم بترجمة بيانات السجلات الخام من أجهزة PLC إلى رسومات تفاعلية، مخططات بيانية، ونوافذ لعرض الإنذارات.
    - تقييم المخاطر: تمتلك واجهات HMI صلاحيات كتابة مباشرة في سجلات الـ PLC. يمنح اختراقها للمهاجمين قدرة كاملة على تغيير نقاط الضبط وإسكات الإنذارات التشغيلية.
  - **أنظمة التحكم الإشرافي وتحصيل البيانات (SCADA) - المستويات من 2 إلى 3 (واسعة النطاق):**
    - معمارية برمجية مصممة للبنى التحتية الممتدة جغرافياً (خطوط أنابيب النفط والغاز، شبكات توزيع المياه، وشبكات نقل الكهرباء).
    - المعمارية: خوادم سكادا المركزية (وحدات التحكم الطرفية الرئيسية - MTU) تتصل عبر شبكات واسعة النطاق (WAN، أقمار صناعية، أجهزة لاسلكية) مع أجهزة RTU وPLC البعيدة، وتعتمد بشكل أساسي على بروتوكولات مثل DNP3 ومودباص.
  - **أنظمة التحكم الموزعة (DCS) - المستويات من 1 إلى 3 (العمليات المتصلة في الموقع):**
    - معمارية تحكم متكاملة مخصصة لبيئات التصنيع المستمر والمعقدة في موقع واحد (مصافي التكرير، المجمعات البتروكيماوية، ومحطات توليد الكهرباء).
    - الفارق الجوهري عن SCADA وPLC: تمتلك أنظمة DCS قاعدة بيانات هندسية موحدة لكافة المتحكمات وواجهات العرض وسجلات الأرشفة، وتوزع منطق التحكم عبر متحكمات مستقلة ذاتية التشغيل ومجهزة بنماذج تكرار وحماية ضد الأعطال (Redundancy).

### 2. Component Comparison Matrix / مصفوفة المقارنة الفنية للمكونات
| Component / المكون | Purdue Level | Deployment Scope | Redundancy Architecture | Determinism Requirement |
| :--- | :--- | :--- | :--- | :--- |
| **PLC** | Level 1 | Machine / Skid Control | Dual-controller / Hot-Standby | Hard Real-Time (<10ms) |
| **HMI** | Level 2 / 3 | Local Console / Control Room | Client-Server Redundancy | Soft Real-Time (100-500ms) |
| **DCS** | Levels 1, 2, 3 | Facility-Wide Continuous Plant | Native Redundant Controllers & Bus | Hard Real-Time at L1 (<50ms) |
| **SCADA** | Levels 2, 3 | Geographically Dispersed | Clustered MTU Servers | Soft Real-Time (Seconds/Minutes) |

---

## Module 3: The Industrial DMZ (iDMZ) & Segmentation
## الوحدة 3: المنطقة الصناعية منزوعة السلاح (iDMZ) وتقسيم الشبكات

### 1. Architecture of Level 3.5 / معمارية المستوى 3.5
- **EN:**
  - **Core Defensive Doctrine:** The iDMZ provides absolute isolation between the enterprise network (Level 4/5) and the industrial production network (Levels 0 through 3). Direct Layer 3 routing between enterprise IT and OT is strictly prohibited.
  - **Dual-Firewall Topology:** Implements two physical firewalls from distinct vendors:
    - *External Firewall:* Positioned between Level 4 and Level 3.5. Governed by enterprise IT security policy.
    - *Internal Firewall:* Positioned between Level 3.5 and Level 3. Governed by OT engineering security policy.
    - Vendor diversity ensures that a vulnerability or zero-day compromise of the external firewall does not grant automatic lateral traversal through the internal firewall.
  - **Identity & Trust Segregation:** OT Active Directory must exist as a completely independent, isolated forest. No forest-level or external trusts may be established between the corporate Active Directory and the OT Active Directory.
  - **Core iDMZ Infrastructure Services:**
    - *Secure Bastion / Jump Servers:* Administrative access from IT into OT must terminate in the iDMZ via dedicated Jump Hosts enforcing Multi-Factor Authentication (MFA), protocol breaking (e.g., HTTPS-to-RDP), session recording, and credential vaulting.
    - *Historian Mirroring:* The Level 3 Process Historian pushes compressed operational telemetry *outward* to an iDMZ Historian replica. Enterprise business applications (ERP/BI) query only the iDMZ replica, preventing IT hosts from querying the Level 3 database.
    - *Patch & Antivirus Mirroring:* Local patch repository servers (e.g., WSUS proxy) and Antivirus definition mirrors stage updates in the iDMZ before controlled distribution into Level 3.
  - **Unidirectional Gateways (Data Diodes):** Hardware-enforced physical optical isolation (a transmitter LED connected via single-strand fiber to a receiver photodiode). Physically prevents any electrical or optical feedback from IT to OT, allowing strictly one-way egress telemetry.
  - **IT/OT Convergence Risks:** Dual-homed servers bridging Level 3 and Level 4 without firewall controls, maintenance laptops directly connected to PLCs, unmanaged cellular modems on remote skids, and cloud-connected IIoT sensors bypassing PERA boundaries.
- **AR:**
  - **العقيدة الدفاعية الأساسية:** توفر منطقة iDMZ عزلاً تاماً بين شبكة المؤسسة (المستوى 4/5) وشبكة الإنتاج الصناعي (المستويات من 0 إلى 3). يُحظر بشكل قاطع التوجيه المباشر لحزم البيانات (Layer 3 Routing) بين بيئات IT وOT.
  - **طوبولوجيا جدران الحماية المزدوجة (Dual-Firewall):** تعتمد على نشر جداري حماية فيزيائيين من مصنعين مختلفين:
    - *جدار الحماية الخارجي:* يفصل بين المستوى 4 والمستوى 3.5، ويخضع لسياسات أمن تقنية المعلومات المؤسسية.
    - *جدار الحماية الداخلي:* يفصل بين المستوى 3.5 والمستوى 3، ويخضع لسياسات وضوابط هندسة التكنولوجيا التشغيلية.
    - تنوع المصنعين يضمن أن استغلال ثغرة أمنية أو ثغرة يوم الصفر في الجدار الخارجي لا يتيح للمهاجم النفاذ المباشر إلى الشبكة الصناعية.
  - **عزل الهوية وعلاقات الثقة:** يجب أن تكون خوادم دليل المستخدم النشط (Active Directory) لبيئة OT مستقلة ومعزولة تماماً في غابة (Forest) منفصلة، ويُمنع بناء أي علاقات ثقة مع خوادم الدليل المؤسسي في IT.
  - **الخدمات الأساسية داخل منطقة iDMZ:**
    - *خوادم العبور الآمنة (Jump Hosts):* يجب أن تنتهي جلسات الوصول الإداري القادمة من IT داخل منطقة iDMZ عبر خوادم عبور تفرض المصادقة متعددة العوامل (MFA)، كسر وتغيير البروتوكول، وتسجيل كامل للجلسات.
    - *خوادم الأرشفة المتطابقة (Historian Replication):* يقوم خادم الأرشفة في المستوى 3 بدفع البيانات باتجاه الخارج إلى خادم أرشفة وسيط داخل iDMZ. تتصل تطبيقات المؤسسة (ERP) بالخادم الوسيط فقط لمنع أي استعلامات مباشرة باتجاه المستوى 3.
    - *خوادم التحديثات وبرامج الحماية:* خوادم محلية وسيطة لاستقبال تحديثات الأنظمة وتعاريف مكافحة الفيروسات وتوزيعها المنضبط على أجهزة المستوى 3.
  - **بوابات النقل أحادية الاتجاه (Data Diodes):** أجهزة أمنية تعتمد على عزل فيزيائي ضوئي (صمام ثنائي باعث للضوء متصل بليف ضوئي أحادي مع مستقبل ضوئي). تمنع فيزيائياً أي مسار رجعي لنقل البيانات من IT إلى OT، وتسمح فقط بإرسال البيانات نحو الخارج.
  - **مخاطر تقارب IT/OT:** الخوادم المزودة ببطاقات شبكة مزدوجة (Dual-Homed) التي تصل بين المستويين 3 و4 مباشرة دون جدران حماية، حواسيب الصيانة المحمولة المتصلة مباشرة بالمتحكمات، أجهزة المودم الخلوي غير المصرح بها، وحساسات إنترنت الأشياء الصناعي المتصلة بالسحابة متجاوزة ضوابط بيردو.

### 2. Standard References / المراجع القياسية المعتمدة
- **IEC 62443-3-2:** Security for Industrial Automation and Control Systems - Security Risk Assessment and System Design (Zones and Conduits).
- **NIST SP 800-82 Rev. 3:** Section 5 (Network Architecture and the Industrial DMZ).
- **CISA ICS-CERT:** Recommended Practice - Improving Industrial Control System Cybersecurity with Defense-in-Depth Strategies.

---

## Module 4: Verified Free Learning Path (YouTube)
## الوحدة 4: مسار التعلم المجاني المعتمد (يوتيوب)

The following YouTube courses and technical sessions provide 100% free, verifiable instruction on industrial architectures and the Purdue Model:

المصادر التالية مجانية بنسبة 100% ومتاحة بالكامل على منصة YouTube لدراسة معماريات أنظمة التحكم ونموذج بيردو:

| Channel / Organization | Search Query / Playlist Title | Direct Channel URL | Core Technical Focus |
| :--- | :--- | :--- | :--- |
| **CISA (Cybersecurity & Infrastructure Security Agency)** | `CISA ICS Training Series` / `ICS Security Architecture` | https://www.youtube.com/@cisa | Official government training covering ICS architecture, perimeters, threat vectors, and defensive strategies. |
| **SANS ICS Security** | `Purdue Model Architecture` / `Defensible ICS Architectures` | https://www.youtube.com/@SANSICSCybersecurity | Deep-dive engineering presentations on PERA segmentation, iDMZ engineering, and network conduit inspection. |
| **RealPars** | `What is SCADA?` / `PLC vs DCS Explained` | https://www.youtube.com/@RealPars | High-definition industrial automation tutorials explaining real-world hardware, fieldbus layouts, and control loops. |
| **CS3STHLM - Cyber Security for Cyber Physical Systems** | `CS3STHLM Industrial Architecture and Defense` | https://www.youtube.com/@CS3STHLM | International summit presentations on real-world industrial architecture failures and defensible network segmentation. |

---

## Module 5: Official Documentation & Standards
## الوحدة 5: الوثائق والمعايير الرسمية وأطر العمل التنظيمية

Industrial network architectures must strictly adhere to the following internationally validated framework standards:

يجب أن تتوافق تصاميم شبكات التحكم الصناعي بدقة مع المعايير والأطر الدولية التالية:

1. **IEC 62443-3-2: Security Risk Assessment and System Design:**
   - Establishes the authoritative methodology for segmenting industrial assets into logical **Zones** sharing common security requirements, interconnected via monitored **Conduits**.
   - Access: https://www.isa.org/standards-and-publications/isa-standards/isa-iec-62443-series

2. **IEC 62443-3-3: System Security Requirements and Security Levels:**
   - Defines technical foundational requirements (FRs) including access control, use control, data integrity, data confidentiality, and system availability across defined zones.
   - Access: https://webstore.iec.ch/publication/60155

3. **NIST SP 800-82 Rev. 3 - Guide to Operational Technology (OT) Security:**
   - Comprehensive federal guideline on firewalls, iDMZ design, remote access architecture, and legacy control integration.
   - Access: https://csrc.nist.gov/publications/detail/sp/800-82/rev-3/final

4. **NIST Special Publication 800-160 Vol. 2 Rev. 1:**
   - Developing Cyber-Resilient Systems: A Systems Security Engineering Approach applied to industrial operational environments.
   - Access: https://csrc.nist.gov/publications/detail/sp/800-160/vol-2/rev-1/final
