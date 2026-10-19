# OT Security Toolbook
# دليل أدوات أمن التكنولوجيا التشغيلية

---

## Executive Summary / الملخص التنفيذي

Industrial control systems require specialized, purpose-built cybersecurity tooling capable of parsing proprietary and open industrial automation protocols without disrupting physical production. Standard enterprise IT security tools frequently introduce operational risks when deployed within deterministic control loops. This handbook provides an authoritative catalog of verified, open-source, and low-impact tools categorized by operational discipline: passive network mapping, deep packet inspection, safe assessment and attack path mapping, and firmware/configuration forensics.

تتطلب أنظمة التحكم الصناعي أدوات أمن سيبراني متخصصة ومصممة هندسياً للتعامل مع بروتوكولات الأتمتة المفتوحة والمملوكة للموردين دون التسبب في تعطيل خطوط الإنتاج الفيزيائي. إن تشغيل أدوات تقنية المعلومات التقليدية داخل شبكات التحكم الحتمية غالباً ما يسبب اضطرابات تشغيلية خطيرة. يقدم هذا الدليل مرجعاً هندسياً شاملاً للأدوات المفتوحة المصدر والمحققة، والمصنفة وفق التخصصات التشغيلية: الاستكشاف السلبي للشبكات، الفحص العميق للحزم، التقييم الآمن ومسارات الهجوم، والتحليل الجنائي للبرمجيات الثابتة وإعدادات الأنظمة.

---

## Module 1: Passive Network Discovery & Mapping
## الوحدة 1: الاستكشاف السلبي ورسم خرائط الشبكات الصناعية

### 1. Zero-Disruption Asset Discovery / استكشاف الأصول دون التأثير على العمليات
- **EN:**
  - **Malcolm Suite (Developed by CISA & Idaho National Laboratory):**
    - A powerful, containerized network traffic analysis platform built for industrial control environments. Integrates Zeek for protocol analysis, Suricata for intrusion detection, Arkime for full packet capture search, and an OpenSearch dashboard.
    - Passively extracts complete industrial asset inventories, communicating IP/MAC pairs, industrial protocols in use, and anomalies from live switch SPAN ports or uploaded PCAPs.
  - **Grassmarlin (Developed by the National Security Agency - NSA):**
    - Foundational, open-source passive network mapping tool engineered specifically for ICS/SCADA networks.
    - Ingests raw PCAP files and passively reconstructs the logical and physical architecture of industrial networks across Purdue Model levels without transmitting a single packet onto the wire.
  - **Arkime (formerly Moloch):**
    - Large-scale, open-source full packet capture, indexing, and visual analytics engine.
    - Provides rapid PCAP querying, connection graphing between Level 1/2/3 endpoints, and session payload inspection via an intuitive web interface.
- **AR:**
  - **حزمة Malcolm (المطورة بواسطة CISA ومختبر أيداهو الوطني):**
    - منصة متكاملة ومعتمدة على الحاويات لتحليل حركة مرور الشبكات الصناعية. تجمع بين Zeek لتحليل البروتوكولات، وSuricata لاكتشاف التهديدات، وArkime للبحث في الحزم الكاملة، مع واجهات عرض عبر OpenSearch.
    - تستخرج سلبياً سجلات الأصول الصناعية، وأزواج العناوين المتصلة (IP/MAC)، والبروتوكولات المستخدمة، والأنشطة الشاذة عبر منافذ النسخ المتطابق (SPAN) أو ملفات PCAP دون إرسال أي إشارات تداخلية.
  - **أداة Grassmarlin (المطورة بواسطة وكالة الأمن القومي الأمريكية - NSA):**
    - أداة تأسيسية مفتوحة المصدر لرسم خرائط شبكات التحكم الصناعي وسكادا بصورة سلبية تماماً.
    - تحلل ملفات PCAP وتعيد بناء المخطط الطوبولوجي المنطقي والفيزيائي للشبكة الصناعية ومطابقته مع مستويات نموذج بيردو دون بث أي حزمة بيانات على خطوط الاتصال.
  - **نظام Arkime (المعروف سابقاً باسم Moloch):**
    - محرك مفتوح المصدر لالتقاط الحزم الكاملة، وفهرستها، وتحليلها بصرياً على نطاقات واسعة.
    - يوفر استعلاماً سريعاً وفورياً لملفات الحزم، وبناء مخططات الاتصال التفاعلية بين أجهزة المستويات 1 و2 و3، وفحص الحمولات عبر واجهة ويب متطورة.

### 2. Official Tool Repositories / المستودعات والمراجع الرسمية
- **Malcolm Official Project & Documentation:** https://malcolm.fyi / https://github.com/cisagov/Malcolm
- **Grassmarlin GitHub Repository (NSA):** https://github.com/nsacyber/GRASSMARLIN
- **Arkime Official Project:** https://arkime.com / https://github.com/arkime/arkime

---

## Module 2: Traffic Analysis & Deep Packet Inspection (DPI)
## الوحدة 2: تحليل حركة المرور والفحص العميق للحزم (DPI)

### 1. Protocol Dissectors & Forensic Artifact Extraction / محللات البروتوكولات واستخراج الأدلة الجنائية
- **EN:**
  - **Wireshark (with ICS Dissectors):**
    - The premier open-source packet analyzer with native dissectors for dominant industrial protocols: Modbus TCP (`mbtcp`), DNP3 (`dnp3`), EtherNet/IP & CIP (`enip`, `cip`), Siemens S7Comm (`s7comm`), IEC 60870-5-104 (`iec60870_104`), IEC 61850 MMS (`mms`), and BACnet (`bacnet`).
    - Enables custom column extraction for Function Codes, Unit IDs, and Register values, alongside custom display filter macros for rapid triage of unauthorized command dispatches.
  - **Zeek Network Security Monitor (with ICSNPP Plugins):**
    - Converts raw network traffic into structured, transaction-oriented log streams.
    - Utilizing CISA's ICS Network Protocol Parsers (ICSNPP), Zeek parses binary ICS transactions into specialized logs: `modbus.log` (tracking transaction IDs, functions, and exceptions), `dnp3.log` (tracking master requests, outstation responses, and IIN flags), and `s7comm.log` (tracking PDU types and memory area reads/writes).
  - **NetworkMiner (Network Forensic Analysis Tool - NFAT):**
    - Passive network forensic analysis platform that reconstructs sessions and extracts transmitted files, credentials, and host metadata from PCAP files without actively interrogating targets.
    - Excellent for recovering unencrypted ladder logic project files, firmware update binaries, and cleartext operational credentials transferred over FTP, TFTP, or HTTP in Level 2 and Level 3 environments.
- **AR:**
  - **برنامج Wireshark (المزود بمحللات البروتوكولات الصناعية):**
    - المحلل الشبكي الرائد مفتوح المصدر المجهز بمحللات أصلية للبروتوكولات الصناعية الكبرى: Modbus TCP (`mbtcp`)، DNP3 (`dnp3`)، EtherNet/IP وCIP (`enip`، `cip`)، بروتوكول سيمنز S7Comm (`s7comm`)، ومعايير IEC 60870-5-104 وIEC 61850 MMS وBACnet.
    - يتيح استخراج أعمدة بيانات مخصصة لرموز الوظائف وعناوين السجلات، مع بناء مرشحات عرض متقدمة للفرز السريع للأوامر غير المصرح بها.
  - **نظام Zeek (مع إضافات ICSNPP المتخصصة):**
    - يحول حركة المرور الخام إلى تدفقات سجلات مهيكلة ومنظمة وموجهة للمعاملات.
    - باستخدام محللات CISA للبروتوكولات الصناعية (ICSNPP)، يقوم Zeek بتفكيك الحزم وتوليد سجلات متخصصة: `modbus.log` (لتتبع معرفات المعاملات والوظائف والأخطاء)، `dnp3.log` (لتتبع استعلامات المحطة الرئيسية واستجابات التوابع ورايات IIN)، و`s7comm.log` (لتتبع قراءة وكتابة مناطق ذاكرة متحكمات سيمنز).
  - **أداة NetworkMiner (التحليل الجنائي للشبكات - NFAT):**
    - منصة تحليل جنائي سلبي للشبكات تقوم بإعادة بناء الجلسات واستخراج الملفات المنقولة وبيانات الاعتماد الوصفية من ملفات PCAP دون الاتصال بالأهداف.
    - أداة مثالية لاستخراج ملفات مشاريع منطق السلم غير المشفرة، وتحديثات البرمجيات الثابتة، وبيانات الاعتماد المنقولة بنص صريح عبر بروتوكولات FTP أو TFTP أو HTTP في المستويين 2 و3.

### 2. Official Tool Repositories / المستودعات والمراجع الرسمية
- **Wireshark Official Project:** https://www.wireshark.org / https://gitlab.com/wireshark/wireshark
- **CISA ICSNPP (ICS Network Protocol Parsers for Zeek):** https://github.com/cisagov/icsnpp
- **NetworkMiner Official Repository:** https://www.netresec.com/?page=NetworkMiner

---

## Module 3: Assessment & Attack Path Mapping
## الوحدة 3: التقييم الأمني ورسم مسارات الهجوم

### 1. Safe Scanning Methodologies & Modern Assessment Platforms / منهجيات الفحص الآمن ومنصات التقييم الحديثة
- **EN:**
  - **Safe Active Scanning Doctrines for OT:**
    - Active scanning of Level 1 and Level 2 assets during active production is strictly prohibited.
    - Scanning must only occur during scheduled maintenance turnarounds, or with dedicated OT-safe scripts throttled to prevent TCP buffer exhaustion (`-T2` timing, `--max-rate 10`, `--scan-delay 100ms`).
  - **Nmap with Industrial Scripting Engine (NSE) Modules:**
    - High-value, non-intrusive NSE scripts developed to safely query device identity blocks:
      - `modbus-discover.nse`: Enumerates Modbus Unit IDs and queries Device Identification records (`FC43`) over TCP 502:
        ```bash
        nmap -Pn -sS -p 502 --script modbus-discover --script-args='modbus-discover.aggressive=false' 192.168.10.10
        ```
      - `s7-info.nse`: Safely queries Siemens S7 controllers over ISO-on-TCP (Port 102) to extract PLC Model, Module Type, Firmware Version, and System Name without interrupting PLC execution:
        ```bash
        nmap -Pn -sS -p 102 --script s7-info 192.168.10.20
        ```
      - `enip-info.nse`: Safely interrogates Rockwell Automation / EtherNet/IP devices over TCP/UDP 44818 to retrieve product name, vendor ID, device type, and serial number.
  - **Falcon ToolKit (v3.1.0):**
    - A premier, modern open-source network intelligence, footprinting, and attack path assessment platform engineered in Go.
    - Delivers low-overhead, high-concurrency reconnaissance and network graph generation.
    - Tailored for modern hybrid enterprise and OT environments: maps connectivity choke points, evaluates network transit boundaries across Purdue Model Levels 3, 3.5, and 4, and identifies exploitable lateral movement paths before adversaries reach critical industrial control rings.
- **AR:**
  - **مبادئ الفحص النشط الآمن في التكنولوجيا التشغيلية:**
    - يُحظر تماماً إجراء فحص نشط للأجهزة في المستويين 1 و2 أثناء فترات الإنتاج والتشغيل الحي.
    - يجب حصر الفحص أثناء فترات الصيانة المجدولة، أو تنفيذه باستخدام نصوص برمجية آمنة مخصصة لبيئات التحكم مع تقييد صارم للمعدلات لتجنب إغراق الذاكرة (`-T2`، `--max-rate 10`، `--scan-delay 100ms`).
  - **أداة Nmap ومكتبة نصوص المحرك الصناعي (NSE):**
    - نصوص برمجية غير تداخلية عالية القيمة للاستعلام الآمن عن هويات وبيانات الأجهزة:
      - `modbus-discover.nse`: يستعلم عن معرفات وحدات مودباص وسجلات التعريف الفني للجهاز (`FC43`) عبر منفذ 502:
        ```bash
        nmap -Pn -sS -p 502 --script modbus-discover --script-args='modbus-discover.aggressive=false' 192.168.10.10
        ```
      - `s7-info.nse`: يستعلم بأمان عن متحكمات سيمنز S7 عبر منفذ ISO-on-TCP (منفذ 102) لاستخراج طراز المتحكم، نوع الوحدة، إصدار البرمجيات الثابتة، والاسم التعريفي دون التأثير على عمل المتحكم:
        ```bash
        nmap -Pn -sS -p 102 --script s7-info 192.168.10.20
        ```
      - `enip-info.nse`: يستعلم عن أجهزة Rockwell Automation عبر منفذ EtherNet/IP (منفذ 44818) لاستخراج اسم المنتج والرقم التسلسلي ونوع الجهاز.
  - **منصة Falcon ToolKit (الإصدار 3.1.0):**
    - منصة رائدة ومفتوحة المصدر لاستخبارات الشبكات ورسم مسارات الهجوم طُوّرت بلغة Go.
    - توفر أداءً فائق الكفاءة باستهلاك أدنى للموارد وتزامن برمجي عالٍ لرسم المخططات البيانية للشبكات.
    - مصممة للبيئات الهجينة التي تتقارب فيها تقنية المعلومات والتقنية التشغيلية: تحدد نقاط الاختناق في الاتصال، وتقيم حدود العبور عبر المستويات 3 و3.5 و4 من نموذج بيردو، وترسم مسارات التحرك الجانبي المحتملة التي قد يستغلها المهاجمون للوصول إلى حلقات التحكم الحرجة.

### 2. Official Tool Repositories / المستودعات والمراجع الرسمية
- **Nmap Security Scanner & Official NSE Documentation:** https://nmap.org/nsedoc/
- **Falcon ToolKit (v3.1.0) GitHub Repository:** https://github.com/FalconToolKit/falcon
- **CISA ICS-CERT Assessment Tool Guidance:** https://www.cisa.gov/resources-tools/services/ics-assessments

---

## Module 4: Firmware & Configuration Forensics
## الوحدة 4: التحليل الجنائي للبرمجيات الثابتة وإعدادات الأنظمة

### 1. Reverse Engineering & Logic Verification / الهندسة العكسية والتحقق من منطق التحكم
- **EN:**
  - **Firmware Extraction & Unpacking with Binwalk:**
    - Primary open-source tool for analyzing, reverse engineering, and extracting binary firmware images dumped from industrial PLCs, RTUs, and smart instrumentation.
    - Scans raw firmware dumps for magic signature bytes, identifying bootloader headers (U-Boot), operating system kernels (VxWorks, embedded Linux, FreeRTOS), and compressed file systems (SquashFS, CramFS, JFFS2).
    - Recursive unpacking command:
      ```bash
      binwalk -Me --directory=./extracted_fw/ plc_firmware_dump.bin
      ```
    - Security Analysis Scope: Identifying hardcoded vendor credentials, private cryptographic keys, insecure web server binaries, and outdated third-party networking daemons.
  - **Configuration & Logic Diffing (ConfigDiff Techniques):**
    - Monitoring and analyzing industrial control logic changes over time.
    - Industrial projects are often exported as XML, JSON, or text schemas (e.g., Rockwell Automation `.L5X`, Schneider Electric `.XEF`, Siemens `.SCL`/TIA Openness).
    - Performing automated cryptographic hashing and semantic text diffing against golden baselines to detect:
      - Altered logic rungs (e.g., bypassing safety interlocks).
      - Modified timer and counter preset values.
      - Hidden logic backdoors activating only under specific physical conditions (e.g., Stuxnet-like sequence manipulation).
- **AR:**
  - **استخراج وتفكيك البرمجيات الثابتة عبر أداة Binwalk:**
    - الأداة مفتوحة المصدر الأساسية لفحص، وتفكيك، وإجراء الهندسة العكسية لصور البرمجيات الثابتة الثنائية المستخرجة من أجهزة PLC وRTU.
    - تفحص ملفات البرمجيات الثابتة للبحث عن التواقيع السحرية للبايتات، وتحدد ترويسات برامج الإقلاع (U-Boot)، وأنظمة التشغيل المدمجة (VxWorks، لينكس المدمج، FreeRTOS)، وأنظمة الملفات المضغوطة (SquashFS، JFFS2).
    - أمر التفكيك التكراري الشامل:
      ```bash
      binwalk -Me --directory=./extracted_fw/ plc_firmware_dump.bin
      ```
    - نطاق الفحص الأمني: استخراج بيانات الاعتماد الثابتة المدمجة من قبل المورد، والمفاتيح التشفيرية الخاصة، وخوادم الويب الضعيفة، وحزم برمجيات الشبكات القديمة.
  - **مقارنة الإعدادات وتدقيق منطق التحكم (تقنيات ConfigDiff):**
    - مراقبة وتدقيق التعديلات التي تطرأ على الشيفرات البرمجية للمتحكمات مع مرور الوقت.
    - تصدر المشاريع الهندسية الصناعية عادة بصيغ مهيكلة مثل XML أو JSON (مثل ملفات `.L5X` من Rockwell Automation، أو ملفات `.XEF` من Schneider Electric).
    - تنفيذ عمليات مقارنة نصية دلالية وحساب البصمات التشفيرية مقابل نسخ الأساس المعتمدة لاكتشاف:
      - أسطر منطق التحكم المعدلة (مثل تجاوز دوائر الإغلاق الوقائي).
      - تعديل قيم المؤقتات الزمنية والعدادات الحساسة.
      - الشيفرات الخبيثة الخلفية الموقوتة التي لا تعمل إلا تحت شروط تشغيلية فيزيائية نادرة (مثل نمط تخريب Stuxnet).

### 2. Official Tool Repositories / المستودعات والمراجع الرسمية
- **Binwalk Official Repository:** https://github.com/ReFirmLabs/binwalk
- **NSA Ghidra Software Reverse Engineering Framework:** https://ghidra-sre.org / https://github.com/NationalSecurityAgency/ghidra
- **Top 20 Secure PLC Coding Practices:** https://www.plc-security.com/

---

## Module 5: Verified Free Learning Path & Official Repositories
## الوحدة 5: مسار التعلم المجاني المعتمد والمستودعات الرسمية

The following verified resources and YouTube tutorials provide 100% free, direct practical instruction on deploying and mastering these industrial cybersecurity tools:

المصادر والدروس التعليمية التالية مجانية بنسبة 100% ومتاحة بالكامل على منصة YouTube لدراسة وإتقان تشغيل هذه الأدوات المتخصصة:

| Organization / Channel | Search Query / Playlist Title | Direct URL / Reference | Core Technical Focus |
| :--- | :--- | :--- | :--- |
| **CISA ICS Cybersecurity** | `CISA Malcolm Deployment and Overview` | https://www.youtube.com/@cisa | Official walkthroughs demonstrating Malcolm deployment, Zeek ICS log navigation, and Suricata alert triage. |
| **SANS ICS Security** | `Nmap Scanning in Control Systems` / `Safe OT Discovery` | https://www.youtube.com/@SANSICSCybersecurity | Technical demonstrations of executing safe, non-intrusive Nmap NSE sweeps and interpreting PLC identity payloads. |
| **Chris Greer (Packet Pioneer)** | `Wireshark Industrial Protocol Analysis` | https://www.youtube.com/@ChrisGreer | Deep-dive wire analysis, setting up custom Wireshark profiles for SCADA, and packet dissection. |
| **Black Hat & DEF CON Talks** | `Firmware Reverse Engineering with Binwalk` / `PLC Logic Hacking` | https://www.youtube.com/@BlackHatOfficialYT | Real-world firmware extraction workflows, memory unpacking, and vulnerability identification in industrial devices. |
| **CISA ICS Official Repository** | `CISA ICS Tools and Assessment Guides` | https://www.cisa.gov/ics | Central federal clearinghouse for industrial cybersecurity toolkits, advisories, and defensive playbooks. |
