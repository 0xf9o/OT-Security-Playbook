# OT Detection Engineering
# هندسة اكتشاف التهديدات في التكنولوجيا التشغيلية

---

## Executive Summary / الملخص التنفيذي

Detection engineering in Operational Technology (OT) and Industrial Control Systems (ICS) demands a fundamentally different methodology than enterprise IT Security Operations Centers (SOCs). While enterprise detection relies heavily on host agents and tolerates high alert volumes, industrial environments require non-intrusive, zero-impact monitoring strategies that prioritize deterministic physical stability. This handbook establishes the operational architecture for passive Network Security Monitoring (NSM), multi-source telemetry aggregation, rule development (Suricata, Sigma, YARA), and threat mapping to the MITRE ATT&CK for ICS framework.

تتطلب هندسة اكتشاف التهديدات في التكنولوجيا التشغيلية (OT) وأنظمة التحكم الصناعي (ICS) منهجية تختلف جذرياً عن مراكز العمليات الأمنية المؤسسية (IT SOC). فبينما يعتمد اكتشاف التهديدات التقليدي على برمجيات العميل المضيف وتوليد تنبيهات متعددة، تفرض البيئات الصناعية استراتيجيات مراقبة غير تداخلية ذات تأثير صفري تضمن استقرار العمليات الفيزيائية الحتمية. يحدد هذا الدليل المعمارية التشغيلية للمراقبة السلبية للشبكة (NSM)، وتجميع البيانات الأمنية متعددة المصادر، وتطوير قواعد الاكتشاف (Suricata وSigma وYARA)، ومطابقة التهديدات مع إطار عمل MITRE ATT&CK for ICS.

---

## Module 1: OT Detection Fundamentals
## الوحدة 1: أساسيات اكتشاف التهديدات في البيئات الصناعية

### 1. Passive Monitoring vs. Active Scanning / المراقبة السلبية مقابل الفحص النشط
- **EN:**
  - **The Perils of Active Probing in Level 1 & Level 2:** Legacy Programmable Logic Controllers (PLCs), Remote Terminal Units (RTUs), and Safety Instrumented Systems (SIS) utilize lightweight, fragile TCP/IP micro-stacks running on resource-constrained real-time operating systems (RTOS). Active vulnerability scanning (e.g., unthrottled Nmap, Nessus, OpenVAS) floods controller network buffers, causing CPU starvation, fatal memory allocation exceptions, and watchdog timer timeouts. This directly causes controllers to enter a Hardware Fault state, de-energizing output modules, tripping industrial processes, and threatening physical safety.
  - **Passive Network Security Monitoring (NSM) as the Gold Standard:** Industrial detection must be 100% passive at Levels 0, 1, and 2. Telemetry is collected via hardware Test Access Points (TAPs) or Switched Port Analyzer (SPAN/mirror) ports. Sensor network interfaces operate in promiscuous mode with zero assigned IP address, electrically or logically precluding any packet injection into the monitored medium.
  - **The Deterministic Advantage:** Industrial networks are static and deterministic:
    - Host populations and MAC-IP pairs remain fixed for months or years.
    - Polling frequencies and communication paths follow strict, repetitive cycles.
    - Function codes, register bounds, and command sets are strictly bounded by engineering configurations.
    - *Detection Axiom:* In a deterministic network, any unbaselined transaction (a new IP, an abnormal function code, an off-hours logic write) represents a high-fidelity anomaly with minimal false-positive noise.
- **AR:**
  - **مخاطر الفحص النشط في المستويين 1 و2:** تعتمد أجهزة التحكم المنطقي القابلة للبرمجة (PLC) ووحدات الإرسال الطرفية (RTU) وأنظمة السلامة (SIS) القديمة على مكدسات شبكية مصغرة وهشة تعمل ضمن أنظمة تشغيل الوقت الحقيقي (RTOS) محدودة الموارد. يؤدي الفحص النشط للثغرات (مثل أدوات Nmap أو Nessus غير المقيدة) إلى إغراق الذاكرة المؤقتة للمتحكمات، وتوقف المعالجات، واستنفاد الذاكرة، وإطلاق مؤقتات الحراسة (Watchdog Timers). ينتج عن ذلك انتقال المتحكم إلى حالة الخطأ الحرج (Fault State)، مما يؤدي إلى قطع الإشارات عن المشغلات الفيزيائية وتوقف العمليات الصناعية وتهديد السلامة المادية.
  - **المراقبة السلبية للشبكة (NSM) كمعيار هندسي أساسي:** يجب أن تكون المراقبة في المستويين 1 و2 سلبية بنسبة 100%. يتم التقاط حركة المرور عبر موزعات الإشارة السلبية (TAPs) أو منافذ النسخ المتطابق (SPAN). تعمل بطاقات الشبكة في مجسات الرصد في الوضع الشامل (Promiscuous Mode) دون تعيين أي عنوان IP، مما يمنع فيزيائياً ومنطقياً حقن أي حزم بيانات داخل وسيط النقل الصناعي.
  - **ميزة الحتمية التشغيلية:** تتسم الشبكات الصناعية بالثبات والحتمية التامة:
    - عناوين الأجهزة المضيفة وثنائيات MAC-IP تظل ثابتة لشهور أو سنوات.
    - معدلات الاستعلام ومسارات الاتصال تتبع دورات متكررة ودقيقة.
    - رموز الوظائف ونطاقات السجلات وأوامر التحكم محددة ومقيدة بالتصاميم الهندسية.
    - *قاعدة الاكتشاف الذهبية:* في الشبكات الحتمية، يمثل أي سلوك خارج النمط القياسي (ظهور عنوان IP جديد، رمز وظيفة غير معتاد، أو كتابة منطق برمجي خارج ساعات الصيانة) تنبيهاً أمنياً عالي الدقة ومنخفض الإنذارات الكاذبة.

### 2. Standard References / المراجع القياسية المعتمدة
- **NIST SP 800-82 Rev. 3:** Section 6.2 (OT Monitoring and Incident Detection).
- **CISA ICS Recommended Practices:** Passive Network Security Monitoring in Control Systems.
- **ISA/IEC 62443-3-3:** System Security Requirements and Security Levels (Requirement SR 6.2: Continuous Security Monitoring).

---

## Module 2: Telemetry & Log Sources
## الوحدة 2: مصادر البيانات والقياس الأمني عن بُعد

### 1. Multi-Tier Telemetry Architecture / معمارية جمع القياسات متعددة الطبقات
- **EN:**
  - **Network Wire Telemetry (Full Packet Capture & Metadata):**
    - Raw PCAP captures captured at critical conduits (Level 1/2 boundaries and Level 3/3.5 transition points).
    - Session flow telemetry (NetFlow v9 / IPFIX) tracking bandwidth and connection durations.
  - **Dedicated Open-Source OT NSM Platforms:**
    - **Zeek (Bro) with ICS Protocol Analyzers:** High-performance network security monitor that parses binary industrial wire transactions into structured logs (`modbus.log`, `dnp3.log`, `enip.log`, `bacnet.log`). Extracts function codes, register addresses, exception codes, and unit identifiers without altering packets.
    - **Malcolm Suite (Developed by CISA / Idaho National Laboratory):** An open-source, containerized network traffic analysis platform integrating Zeek, Suricata, Arkime, Logstash, and OpenSearch with pre-configured parsers for industrial control protocols.
  - **Endpoint Telemetry for Level 2 & Level 3 Assets (HMIs, EWS, Historians):**
    - **Windows Security Event Logs:** Event ID 4624 (Successful Logon), Event ID 4625 (Failed Logon), Event ID 4672 (Special Privileges Assigned), and Event ID 7045 (New Service Installed).
    - **Microsoft Sysmon (System Monitor):**
      - *Event ID 1 (Process Creation):* Detects unauthorized command-line execution, staging of exploitation scripts (e.g., Python scripts, `plink.exe`, `s7cli`), and engineering tool invocation.
      - *Event ID 3 (Network Connection):* Identifies unexpected outbound connections originating from HMIs or Engineering Workstations.
      - *Event ID 7 (Image Loaded):* Detects DLL hijacking and unauthorized driver loading into industrial engineering software.
      - *Event ID 11 (File Create):* Monitors file creation in PLC logic project directories (e.g., `.s7p`, `.acd`, `.apa`, `.project`).
  - **Perimeter & Infrastructure Logs:** Industrial firewall connection states, VPN gateway session terminations, and jump server (bastion host) session video and keystroke recordings at Level 3.5.
- **AR:**
  - **بيانات الشبكة السلكية (الحزم الكاملة والبيانات الوصفية):**
    - التقاط الحزم الكاملة (PCAP) عند القنوات الحيوية (بين المستويات 1 و2 وعند نقاط الانتقال في المستوى 3 و3.5).
    - بيانات تدفق الجلسات (NetFlow v9 / IPFIX) لتتبع أحجام البيانات ومدد الاتصال.
  - **المنصات مفتوحة المصدر المتخصصة في مراقبة شبكات التكنولوجيا التشغيلية:**
    - **نظام Zeek مع محللات البروتوكولات الصناعية:** مراقب شبكي متقدم يقوم بتفكيك المعاملات الثنائية للبروتوكولات وتحويلها إلى سجلات مهيكلة ومفصلة (`modbus.log`، `dnp3.log`، `enip.log`). يستخرج رموز الوظائف وعناوين السجلات واستجابات الأخطاء دون التأثير على حركة البيانات.
    - **حزمة Malcolm (المطورة بواسطة CISA ومختبر أيداهو الوطني):** منصة مفتوحة المصدر مبنية على الحاويات تجمع بين Zeek وSuricata وArkime وLogstash وOpenSearch، ومجهزة بمحللات مهيأة مسبقاً لحركة مرور أنظمة التحكم الصناعي.
  - **سجلات نقاط النهاية للمستويين 2 و3 (واجهات HMI، محطات العمل الهندسية EWS، وخوادم الأرشفة):**
    - **سجلات أمان ويندوز:** الحدث 4624 (تسجيل دخول ناجح)، الحدث 4625 (فشل تسجيل الدخول)، الحدث 4672 (منح امتيازات خاصة)، والحدث 7045 (تثبيت خدمة جديدة).
    - **أداة Microsoft Sysmon المتقدمة:**
      - *الحدث 1 (إنشاء العمليات):* رصد تشغيل أوامر سطر الأوامر غير المصرح بها، واستدعاء نصوص الاستغلال البرمجية (مثل بايثون و`plink.exe`)، وتشغيل برمجيات الهندسة.
      - *الحدث 3 (اتصالات الشبكة):* رصد الاتصالات الخارجية غير المعتادة الصادرة من واجهات HMI أو محطات الهندسة EWS.
      - *الحدث 7 (تحميل البرمجيات والملفات التنفيذية):* اكتشاف هجمات اختطاف ملفات DLL وتحميل برامج التشغيل غير الموثقة في البرامج الهندسية.
      - *الحدث 11 (إنشاء الملفات):* تدقيق ومراقبة حفظ ملفات المشاريع البرمجية لمنطق التحكم (مثل امتدادات `.s7p` و`.acd` و`.apa`).
  - **سجلات الحدود والبنية التحتية:** سجلات جدران الحماية الصناعية، جلسات بوابات VPN، والتسجيل الكامل لجلسات خوادم العبور (Jump Hosts) عند المستوى 3.5.

### 2. Standard References / المراجع القياسية المعتمدة
- **CISA Malcolm Repository & Documentation:** https://malcolm.fyi
- **Zeek ICS Protocol Parsers Documentation:** https://github.com/cisagov/icsnpp
- **Microsoft Sysmon Documentation & Baseline Schemes:** Microsoft Learn - Sysinternals Suite.

---

## Module 3: Rule Development (Suricata, Sigma, YARA)
## الوحدة 3: تطوير قواعد الاكتشاف (Suricata وSigma وYARA)

### 1. Practical Detection Rule Implementations / النماذج الهندسية العملية لقواعد الاكتشاف

#### A. Network Signatures (Suricata) / قواعد كشف الشبكة (Suricata)
- **EN:** The following rules detect unauthorized commands targeting Level 1 controllers across industrial conduits:
- **AR:** تكتشف القواعد التالية الأوامر غير المصرح بها الموجهة لأجهزة التحكم في المستوى 1 عبر القنوات الشبكية:

```suricata
# Rule 1: Detect Modbus TCP FC16 (Write Multiple Holding Registers) targeting Controller Subnet
alert tcp any any -> [192.168.10.0/24] 502 (
    msg:"SEC-OT-POLICY: Modbus TCP FC16 Write Multiple Holding Registers (Potential Setpoint Alteration)";
    flow:to_server,established;
    content:"|00 00|"; offset:2; depth:2; # Modbus Protocol Identifier
    byte_test:1,=,16,7;                   # Modbus Function Code 16 (0x10)
    threshold:type limit, track by_src, count 1, seconds 60;
    classtype:policy-violation;
    sid:2000001;
    rev:1;
)

# Rule 2: Detect Modbus TCP FC05 (Force Single Coil / Actuator Override)
alert tcp any any -> [192.168.10.0/24] 502 (
    msg:"SEC-OT-POLICY: Modbus TCP FC05 Force Single Coil (Actuator Override)";
    flow:to_server,established;
    content:"|00 00|"; offset:2; depth:2; # Modbus Protocol Identifier
    byte_test:1,=,5,7;                    # Modbus Function Code 5 (0x05)
    threshold:type limit, track by_src, count 1, seconds 30;
    classtype:policy-violation;
    sid:2000002;
    rev:1;
)
```

#### B. Host Behavioral Rules (Sigma) / قواعد السلوك للأنظمة المضيفة (Sigma)
- **EN:** Detects unauthorized executions of PLC logic compilers or command-line download utilities on Engineering Workstations:
- **AR:** تكتشف التشغيل غير المصرح به لمجمعات الشيفرات البرمجية للمتحكمات أو أدوات التحميل عبر سطر الأوامر على محطات العمل الهندسية:

```yaml
title: Unauthorized PLC Programming Utility Execution
id: 5b4d7c81-42e1-4c12-8e67-d867c29e2011
status: production
description: Detects command-line execution of industrial engineering utilities from unexpected parent processes or non-engineering user accounts.
references:
    - https://attack.mitre.org/techniques/T0843/
author: OT Security Playbook
logsource:
    category: process_creation
    product: windows
detection:
    selection:
        Image|endswith:
            - '\Step7.exe'
            - '\LogixDesigner.exe'
            - '\OpenPLC_Editor.exe'
            - '\plink.exe'
        CommandLine|contains:
            - '--download'
            - '--stop'
            - '-upload'
    filter_authorized_users:
        User|contains: 'OT_ENGINEER_'
    condition: selection and not filter_authorized_users
fields:
    - CommandLine
    - ParentImage
    - User
falsepositives:
    - Scheduled maintenance logic downloads by authorized contractors.
level: high
tags:
    - attack.execution
    - attack.t0843 # Program Download
```

#### C. Binary & Malware Signatures (YARA) / تواقيع البرمجيات الخبيثة (YARA)
- **EN:** Identifies strings and memory footprints associated with industrial malware frameworks targeting OPC DA or industrial protocols (e.g., Industroyer / CrashOverride patterns):
- **AR:** تكشف التراكيب النصية وبصمات الذاكرة المرتبطة بمنصات البرمجيات الخبيثة الصناعية التي تستهدف معايير OPC DA أو البروتوكولات الصناعية:

```yara
rule MALW_ICS_Industroyer_OPC_DA_Strings {
    meta:
        description = "Detects strings associated with industrial malware targeting OPC DA interfaces to manipulate process data"
        author = "OT Security Playbook"
        reference = "https://attack.mitre.org/software/S0604/"
        date = "2026-10-19"
        threat_level = "Critical"
    strings:
        $opc_str1 = "OPC.ServerList" ascii wide nocase
        $opc_str2 = "IOPCServer" ascii wide
        $opc_str3 = "IOPCItemMgt" ascii wide
        $cmd_stop = "StopSystem" ascii wide
        $cmd_kill = "ForceTerminate" ascii wide
    condition:
        uint16(0) == 0x5A4D and (2 of ($opc_str*) and 1 of ($cmd_*))
}
```

---

## Module 4: Verified Free Learning Path (YouTube)
## الوحدة 4: مسار التعلم المجاني المعتمد (يوتيوب)

The following courses and conference sessions are 100% free, publicly accessible, and verified for OT Detection Engineering:

المصادر والمحاضرات التالية مجانية بنسبة 100% ومتاحة بالكامل على منصة YouTube لدراسة وتطبيق هندسة اكتشاف التهديدات في بيئات OT:

| Channel / Organization | Search Query / Playlist Title | Direct Channel URL | Core Technical Focus |
| :--- | :--- | :--- | :--- |
| **SANS ICS Security** | `OT Threat Hunting` / `Detection Engineering for Control Systems` | https://www.youtube.com/@SANSICSCybersecurity | Threat hunting methodologies in deterministic networks, baseline anomaly modeling, and ICS protocol triage. |
| **The Zeek Project** | `Zeek in Industrial Control Systems` / `Zeek ICS Parsers` | https://www.youtube.com/@TheZeekProject | Configuring Zeek for Modbus, DNP3, and Ethernet/IP; script development for custom ICS protocol anomaly extraction. |
| **OISF (Suricata)** | `Suricata User Conference` / `Writing Suricata Rules for OT` | https://www.youtube.com/@OISFSuricata | Advanced rule syntax, byte extraction, flow state inspection, and tuning alerts for high-throughput industrial links. |
| **Black Hat & DEF CON ICS Village** | `ICS Detection Engineering Case Studies` | https://www.youtube.com/@ICSVillage | Real-world demonstrations of malware execution, protocol abuse, and defensive sensor engineering. |

---

## Module 5: Official Documentation & MITRE ATT&CK for ICS
## الوحدة 5: الوثائق والمعايير الرسمية وإطار MITRE ATT&CK for ICS

### 1. Framework Architecture & Detection Mapping / معمارية إطار العمل ومطابقة قواعد الاكتشاف
- **EN:**
  - **Structure of MITRE ATT&CK for ICS:** A publicly accessible, curated knowledge base of adversary behaviors observed in real-world attacks against industrial control systems. Tactics represent the adversary's technical goals; techniques represent the precise means utilized.
  - **Core Tactical Categories Relevant to Detection:**
    - *Initial Access (TA0108):* Wireless compromise, external remote services, supply chain compromise.
    - *Execution (TA0104):* Command-line interfaces, graphical user interfaces, native API manipulation.
    - *Persistence (TA0102):* Hardcoded credentials, project file infection, unauthorized system services.
    - *Evasion (TA0103):* Controller indicator suppression, spoofed reporting, rootkit execution.
    - *Inhibit Response Function (TA0107):* Blocking alarm channels, disabling emergency stop (E-Stop) logic, denial-of-service against safety systems.
    - *Impair Process Control (TA0106):* Unauthorized modification of control logic, setpoint tampering, unauthorized device reset.
    - *Impact (TA0105):* Denial of control, loss of safety, physical damage to equipment.
  - **Operational Mapping Methodology:** Every developed detection rule (Suricata, Sigma, YARA) must explicitly tag corresponding ATT&CK for ICS technique IDs (e.g., `T0843 - Program Download`, `T0855 - Unauthorized Command Message`, `T0836 - Modify Parameter`) to enable continuous defensive posture scoring and coverage matrix evaluation.
- **AR:**
  - **بنية إطار عمل MITRE ATT&CK for ICS:** قاعدة معمارية عامة وموثقة توثق سلوكيات وتكتيكات المهاجمين المرصودة في الهجمات الواقعية ضد أنظمة التحكم الصناعي. تمثل التكتيكات الأهداف الهندسية للمهاجم، بينما تمثل التقنيات الوسائل التنفيذية الدقيقة.
  - **التصنيفات التكتيكية الأساسية المرتبطة بهندسة الاكتشاف:**
    - *النفاذ المبدئي (TA0108):* اختراق الشبكات اللاسلكية، استغلال خدمات الوصول عن بُعد، واختراق سلاسل الإمداد.
    - *التنفيذ (TA0104):* تشغيل أوامر سطر الأوامر، استغلال الواجهات الرسومية، والتعامل المباشر مع واجهات البرمجة التطبيقية (APIs).
    - *الاستمرارية (TA0102):* بيانات الاعتماد الثابتة، تلويث ملفات المشاريع الهندسية، وإنشاء خدمات نظام خفية.
    - *التخفي (TA0103):* حجب مؤشرات التنبيه بالمتحكمات، تزييف تقارير القياس، وتثبيت برمجيات التخفي العميقة (Rootkits).
    - *تعطيل وظائف الاستجابة (TA0107):* حظر قنوات نقل الإنذارات، تعطيل دوائر الإيقاف الطارئ (E-Stop)، وهجمات حجب الخدمة ضد أنظمة السلامة.
    - *إضعاف التحكم في العمليات (TA0106):* التعديل غير المصرح به لمنطق التحكم، التلاعب بنقاط الضبط التشغيلية، وإعادة تشغيل الأجهزة قسرياً.
    - *التأثير المادي (TA0105):* فقدان القدرة على التحكم، فقدان السلامة التشغيلية، والتدمير الميكانيكي للمعدات.
  - **منهجية المطابقة التشغيلية للقواعد:** يجب ربط وتوسيم كل قاعدة اكتشاف يتم تطويرها (Suricata أو Sigma أو YARA) برقم التقنية المطابق في إطار العمل (مثل: `T0843 - تحميل البرامج للمتحكمات`، `T0855 - رسائل الأوامر غير المصرح بها`، `T0836 - تعديل المعاملات التشغيلية`) لقياس التغطية الدفاعية وتقييم الفجوات الأمنية باستمرار.

### 2. Standard References / المراجع القياسية المعتمدة
- **MITRE ATT&CK for ICS Matrix:** https://attack.mitre.org/matrices/ics/
- **MITRE ATT&CK for ICS Techniques & Mitigations Repository:** https://attack.mitre.org/techniques/ics/
- **CISA ICS-CERT Defense-in-Depth and Detection Strategies:** https://www.cisa.gov/ics
- **Sigma Detection Rule Specification:** https://github.com/SigmaHQ/sigma
