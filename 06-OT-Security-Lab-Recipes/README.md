# OT Security Lab Recipes
# وصفات وتجارب مختبرات أمن التكنولوجيا التشغيلية

---

## Executive Summary / الملخص التنفيذي

Testing security controls, exploit payloads, or detection engineering rules directly against live industrial facilities (power grids, petrochemical refineries, water distribution plants) is strictly unacceptable due to the immediate risk of physical damage, environmental contamination, or loss of life. Rigorous, defensible OT cybersecurity engineering requires high-fidelity, safe, and reproducible testbed environments. This handbook provides end-to-end recipes for engineering Software-in-the-Loop (SITL) and Hardware-in-the-Loop (HITL) industrial laboratories utilizing entirely open-source and free platforms.

يُعد اختبار ضوابط الحماية الأمنية، أو حزم الاستغلال البرمجية، أو قواعد الاكتشاف مباشرة على المنشآت الصناعية الحية (شبكات الكهرباء، مصافي البتروكيماويات، ومحطات معالجة وتوزيع المياه) أمراً محظوراً تماماً لما يترتب عليه من مخاطر كارثية تشمل التدمير الفيزيائي للمعدات، والتلوث البيئي، والتهديد المباشر لحياة العاملين. يتطلب أمن التكنولوجيا التشغيلية الرصين بناء بيئات اختبارية ومختبرات هندسية آمنة وعالية الدقة وقابلة للتكرار. يقدم هذا الدليل إرشادات تطبيقية تفصيلية لتصميم وتجهيز مختبرات صناعية بنموذجي محاكاة البرمجيات (SITL) ومحاكاة العتاد المتصل (HITL) بالاعتماد على أدوات مفتوحة المصدر ومجانية بالكامل.

---

## Module 1: Lab Architecture & Typology
## الوحدة 1: معمارية وأنماط المختبرات الصناعية

### 1. Testbed Paradigms & Virtualization / نماذج الاختبار والافتراضية الهندسية
- **EN:**
  - **Software-in-the-Loop (SITL) vs. Hardware-in-the-Loop (HITL):**
    - *Software-in-the-Loop (SITL):* Entirely virtualized or containerized environment where physical processes, programmable logic controllers (PLCs), communication networks, and supervisory software run on commodity x86 hardware. Highly scalable, portable, cost-free, and ideal for detection engineering and CI/CD validation.
    - *Hardware-in-the-Loop (HITL):* Integrates physical industrial hardware (e.g., Siemens S7-1200, Schneider TM221, Allen-Bradley Micro800) coupled with virtualized or physical scale-model processes. Essential for firmware extraction, hardware reverse engineering, and low-level proprietary bus analysis.
  - **Virtualizing the Purdue Model:**
    - *Hypervisor Platform:* Type-1 hypervisors (Proxmox VE, VMware ESXi) or network simulation engines (EVE-NG, GNS3) installed on dedicated lab hardware.
    - *Virtual Network Segregation (pfSense / OPNsense):*
      - `VLAN 10 - Level 1 (Control Zone):` Hosting soft-PLCs (OpenPLC Runtime instances).
      - `VLAN 20 - Level 2 (Supervisory Zone):` Hosting virtual HMIs (FUXA / ScadaBR).
      - `VLAN 30 - Level 3 (Operations Management):` Hosting process historians, engineering workstations, and OT SIEM forwarders.
      - `VLAN 35 - Level 3.5 (Industrial DMZ):` Hosting reverse proxies, jump servers (Guacamole), and patch mirrors.
      - `VLAN 40 - Level 4 (Enterprise / Simulation Boundary):` External attacker/tester staging system (Kali Linux).
- **AR:**
  - **محاكاة البرمجيات (SITL) مقابل محاكاة العتاد المتصل (HITL):**
    - *محاكاة البرمجيات (SITL):* بيئة اختبار افتراضية بالكامل أو معتمدة على الحاويات، حيث تعمل المحاكاة الفيزيائية للعمليات، ومتحكمات الـ PLC، وشبكات الاتصال، والواجهات الإشرافية على خوادم تجارية قياسية (x86). تتميز بقابلية التوسع الفائقة، وسهولة النقل، وانعدام التكلفة المادية، مما يجعلها مثالية لتطوير قواعد الاكتشاف.
    - *محاكاة العتاد المتصل (HITL):* تدمج أجهزة تحكم صناعية فيزيائية حقيقية (مثل Siemens S7-1200 أو Schneider TM221) وتربطها بمحاكاة رقمية أو نماذج فيزيائية مصغرة. تعتبر ضرورية لأبحاث استخراج البرمجيات الثابتة (Firmware)، والهندسة العكسية للعتاد، وتحليل البروتوكولات المملوكة للموردين.
  - **بناء نموذج بيردو الافتراضي:**
    - *منصة التشغيل الافتراضي (Hypervisors):* استخدام منصات التشغيل من النوع الأول (Proxmox VE أو VMware ESXi) أو محاكيات الشبكات (EVE-NG وGNS3) على خوادم مخصصة للمختبر.
    - *تقسيم الشبكات الافتراضية عبر جدران الحماية (pfSense / OPNsense):*
      - `VLAN 10 - المستوى 1 (منطقة التحكم):` مخصصة لمتحكمات الـ PLC البرمجية (حاويات OpenPLC).
      - `VLAN 20 - المستوى 2 (المنطقة الإشرافية):` مخصصة لخوادم واجهات الإنسان والآلة (FUXA / ScadaBR).
      - `VLAN 30 - المستوى 3 (إدارة العمليات):` مخصصة لخوادم الأرشفة، ومحطات العمل الهندسية (EWS)، ومجمعات السجلات.
      - `VLAN 35 - المستوى 3.5 (المنطقة الصناعية منزوعة السلاح iDMZ):` مخصصة لخوادم العبور الآمنة (Apache Guacamole) وخوادم التحديثات.
      - `VLAN 40 - المستوى 4 (حدود المؤسسة / المهاجم):` مخصصة لمنصات اختبار الاختراق وتوليد الهجمات (Kali Linux).

### 2. Standard References / المراجع القياسية المعتمدة
- **Proxmox Virtual Environment Documentation:** https://pve.proxmox.com/pve-docs/
- **pfSense Community Edition Documentation:** https://docs.netgate.com/pfsense/en/latest/
- **EVE-NG Community Documentation:** https://www.eve-ng.net/index.php/documentation/

---

## Module 2: Process Simulation (The Physical Layer)
## الوحدة 2: محاكاة العمليات (الطبقة الفيزيائية)

### 1. Modeling Physical Dynamics / نمذجة الديناميكيات الفيزيائية
- **EN:**
  - **The Principle of Physics-First OT Security:** Defensible OT security requires that cyber anomalies manifest as observable, physical consequences (e.g., changes in pressure, fluid velocity, temperature, or chemical concentrations). A cyber-attack that merely changes bits without impacting physics is indistinguishable from synthetic noise.
  - **EPANET Engine for Water and Wastewater Systems:**
    - Developed by the United States Environmental Protection Agency (EPA), EPANET simulates the hydraulic and water quality behavior of pressurized pipe networks.
    - Simulates physical components: Pipes, nodes (junctions), pumps, control valves, and storage tanks.
    - Computes dynamic fluid pressures, friction head loss via Darcy-Weisbach or Hazen-Williams formulas, pump electrical energy consumption, and chemical decay kinetics over time.
  - **Cyber-Physical Coupling Pipeline:**
    - An intermediate Python daemon acts as the physical-to-digital transducer using libraries like `epanettools` or `WNTR` (Water Network Tool for Resilience).
    - Every simulation timestep (e.g., 1 second), the Python daemon executes one step of the physical hydraulics model.
    - The daemon reads physical variables (e.g., Tank 1 water level = 4.25 meters) and writes this value directly into the corresponding OpenPLC Holding Register (`%QW0` / Address `40001`).
    - The daemon reads the PLC's actuator outputs (e.g., Pump 1 run command in Coil `%QX0.0` / Address `00001`) and applies it as a physical state change to the EPANET pump actuator for the subsequent timestep.
- **AR:**
  - **مبدأ الأولوية الفيزيائية في أمن التكنولوجيا التشغيلية:** يتطلب أمن التكنولوجيا التشغيلية الحقيقي أن تنعكس الأنشطة السيبرانية المعادية في صورة تأثيرات فيزيائية ملموسة (مثل التغير في مستويات الضغط، سرعة تدفق السوائل، درجات الحرارة، أو تركيز المواد الكيميائية). فالهجوم السيبراني الذي لا يغير واقع العملية الفيزيائية لا يمكن تقييم خطورته التشغيلية بدقة.
  - **محرك EPANET لشبكات معالجة وتوزيع المياه:**
    - طورته وكالة حماية البيئة الأمريكية (EPA) لمحاكاة السلوك الهيدروليكي ونقاء المياه داخل شبكات الأنابيب المضغوطة.
    - يحاكي المكونات الفيزيائية: الأنابيب، العقد، المضخات، صمامات التحكم، وخزانات التخزين.
    - يحسب ديناميكيات ضغط السوائل، ومعدل فقدان الضغط بالاحتكاك وفق معادلات Darcy-Weisbach، واستهلاك المضخات للطاقة الكهربائية، وحركية تحلل المواد الكيميائية.
  - **مسار الربط السيبراني-الفيزيائي:**
    - يعمل برنامج وسيط بلغة بايثون كمحول للإشارات الفيزيائية إلى بيانات رقمية بالاعتماد على مكتبة `WNTR` (أداة مرونة شبكات المياه).
    - في كل خطوة زمنية من المحاكاة (مثل ثانية واحدة)، ينفذ الوسيط الحسابات الهيدروليكية للنظام.
    - يقرأ الوسيط القيم الفيزيائية (مثل: منسوب المياه في الخزان = 4.25 متر) ويكتبها مباشرة في سجل التخزين المخصص داخل OpenPLC (`%QW0` / العنوان `40001`).
    - يقرأ الوسيط مخرجات التحكم من الـ PLC (مثل: أمر تشغيل المضخة في الملف `%QX0.0` / العنوان `00001`) ويطبقه كحالة فيزيائية فعلية على مضخة EPANET في الدورة التالية.

### 2. Standard References / المراجع القياسية المعتمدة
- **US EPA EPANET 2.2 User Manual:** United States Environmental Protection Agency - https://www.epa.gov/water-research/epanet
- **WNTR (Water Network Tool for Resilience):** Sandia National Laboratories & US EPA - https://wntr.readthedocs.io/

---

## Module 3: Virtualizing the ICS Stack
## الوحدة 3: بناء وتجهيز مكدس أنظمة التحكم الافتراضي

### 1. OpenPLC & Supervisory HMI Integration / تكامل المتحكم البرمجي والواجهات الإشرافية
- **EN:**
  - **Deploying OpenPLC Runtime v3 (Level 1 Control):**
    - High-performance, open-source industrial controller executing standardized IEC 61131-3 logic on Linux, Docker, or Raspberry Pi.
    - Internal Driver Configuration: Exposes Modbus TCP server on port 502, DNP3 Outstation on port 20000, and OPC UA server on port 4840.
    - Memory Map Definition:
      - Digital Inputs: `%IX0.0` to `%IX0.7` (Mapped to Modbus Discrete Inputs).
      - Digital Outputs: `%QX0.0` to `%QX0.7` (Mapped to Modbus Coils).
      - Analog Inputs: `%IW0` to `%IW7` (Mapped to Modbus Input Registers).
      - Analog Outputs / Internal Setpoints: `%QW0` to `%QW7` (Mapped to Modbus Holding Registers).
  - **OpenPLC Editor (Logic Programming):**
    - Compiles Ladder Logic (LD) and Structured Text (ST) into native C++ binaries executed by the runtime.
    - Example Logic: Automated pump trip when high-level switch reaches 95% capacity, with manual override locking.
  - **Supervisory Interface Deployment (FUXA / ScadaBR - Level 2 HMI):**
    - Deploy FUXA as a modern, containerized, web-based SCADA interface operating over Node.js.
    - Communication Binding: Configure FUXA Modbus TCP driver targeting the OpenPLC runtime IP address on port 502.
    - UI Widget Association:
      - Tank level gauge element dynamically bound to `%QW0` (Holding Register `40001`).
      - High-pressure alarm indicator bound to `%IX0.1` (Discrete Input `10002`).
      - Pump start/stop momentary switch bound to `%QX0.0` (Coil `00001`).
- **AR:**
  - **تثبيت وتشغيل OpenPLC Runtime الإصدار 3 (المستوى 1 للتحكم):**
    - متحكم صناعي برمجي مفتوح المصدر ينفذ تعليمات منطق التحكم القياسية IEC 61131-3 على أنظمة لينكس أو حاويات Docker.
    - ضبط بروتوكولات الاتصال: يتيح خادم Modbus TCP عبر منفذ 502، ومحطة DNP3 عبر منفذ 20000، وخادم OPC UA عبر منفذ 4840.
    - خريطة الذاكرة الصناعية للمتحكم:
      - المدخلات الرقمية: من `%IX0.0` إلى `%IX0.7` (تطابق مدخلات مودباص المنفصلة).
      - المخرجات الرقمية: من `%QX0.0` إلى `%QX0.7` (تطابق ملفات مودباص الثنائية).
      - المدخلات التناظرية: من `%IW0` إلى `%IW7` (تطابق سجلات المدخلات).
      - المخرجات التناظرية ونقاط الضبط: من `%QW0` إلى `%QW7` (تطابق سجلات التخزين).
  - **محرر البرمجة OpenPLC Editor:**
    - يقوم بترجمة منطق السلم (LD) والنص المهيكل (ST) إلى برمجيات C++ تنفيذية تعمل مباشرة داخل محرك المتحكم.
    - نموذج للمنطق البرمجي: إيقاف تشغيل المضخة تلقائياً عند وصول منسوب الخزان إلى 95% مع تفعيل قفل التشغيل اليدوي.
  - **تجهيز الواجهة الإشرافية (FUXA / ScadaBR - المستوى 2):**
    - نشر منصة FUXA كواجهة HMI/SCADA حديثة معتمدة على الويب وتعمل ضمن بيئة Node.js.
    - ربط الاتصال الشبكي: إعداد مشغل بروتوكول Modbus TCP المدمج في FUXA ليتصل بعنوان IP الخاص بـ OpenPLC عبر المنفذ 502.
    - ربط العناصر الرسومية بذاكرة المتحكم:
      - مؤشر منسوب الخزان الرسومي مربوط بالسجل `%QW0` (سجل التخزين `40001`).
      - ضوء إنذار ارتفاع الضغط مربوط بالمدخل `%IX0.1` (المدخل الرقمي `10002`).
      - مفتاح تشغيل/إيقاف المضخة مربوط بالملف `%QX0.0` (الملف الثنائي `00001`).

### 2. Standard References / المراجع القياسية المعتمدة
- **OpenPLC Project Documentation & Source Code:** https://openplcproject.com/
- **FUXA SCADA/HMI GitHub Repository:** https://github.com/frangoteam/FUXA
- **IEC 61131-3:** Programmable controllers - Part 3: Programming languages.

---

## Module 4: Attack & Defense Tooling
## الوحدة 4: أدوات الهجوم والدفاع السيبراني للمختبر

### 1. Controlled Simulation Pipeline / مسار المحاكاة الهجومية والدفاعية المنضبطة
- **EN:**
  - **Passive Defensive Monitoring Pipeline (Malcolm / Security Onion):**
    - Configure a virtual SPAN (mirror port) on the pfSense firewall spanning all traffic traversing VLAN 10 (Level 1) and VLAN 20 (Level 2).
    - Connect the SPAN interface directly to the monitoring interface of the Malcolm suite (Zeek + Suricata + OpenSearch).
    - Verify that all operational Modbus reads and writes are continuously indexed in Zeek's `modbus.log` with zero frame alteration.
  - **Benign Baseline Traffic Generation:**
    - Execute a scheduled Python script simulating ordinary operator polling cycles from the HMI to the PLC:
      ```python
      # Baseline Polling: Read Holding Registers 0 to 5 every 1 second
      from pymodbus.client import ModbusTcpClient
      import time

      client = ModbusTcpClient('192.168.10.10', port=502)
      client.connect()
      while True:
          result = client.read_holding_registers(address=0, count=5, slave=1)
          time.sleep(1.0)
      ```
  - **Adversary Attack Simulation:**
    - *Scenario 1: Rogue Setpoint Alteration (Metasploit):*
      - Using `auxiliary/scanner/scada/modbusclient` to overwrite holding register `40001` (overriding safety temperature threshold from 70°C to 150°C).
    - *Scenario 2: Unauthorized Actuator Force (Custom Python Script):*
      - Injecting an unauthorized `FC05 (Force Single Coil)` to open the emergency drainage valve:
      ```python
      from pymodbus.client import ModbusTcpClient
      client = ModbusTcpClient('192.168.10.10', port=502)
      client.connect()
      # Force Coil 0 to True (Discharge tank)
      client.write_coil(address=0, value=True, slave=1)
      client.close()
      ```
    - *Verification:* Confirm immediate Suricata rule firing (`SEC-OT-POLICY: Modbus TCP FC05 Force Single Coil`) and observe physical water depletion in the EPANET hydraulic model.
- **AR:**
  - **مسار المراقبة الدفاعية السلبية (Malcolm / Security Onion):**
    - إعداد منفذ نسخ متطابق افتراضي (SPAN / Port Mirror) على جدار الحماية pfSense لنسخ كامل حركة البيانات بين شبكتي VLAN 10 وVLAN 20.
    - ربط منفذ النسخ مباشرة بواجهة الرصد المخصصة لمنصة Malcolm (التي تشغل محركات Zeek وSuricata وOpenSearch).
    - التأكد من أرشفة وفهرسة جميع معاملات قراءة وكتابة مودباص آلياً داخل سجلات Zeek (`modbus.log`) دون أي تعديل على الحزم.
  - **توليد حركة المرور القياسية الطبيعية:**
    - تشغيل سكربت بايثون مجدول لمحاكاة عمليات الاستعلام الدورية التي يجريها المشغل الطبيعي بين HMI وPLC:
      ```python
      # استعلام قياسي: قراءة سجلات التخزين من 0 إلى 5 كل ثانية واحدة
      from pymodbus.client import ModbusTcpClient
      import time

      client = ModbusTcpClient('192.168.10.10', port=502)
      client.connect()
      while True:
          result = client.read_holding_registers(address=0, count=5, slave=1)
          time.sleep(1.0)
      ```
  - **محاكاة الهجمات السيبرانية المعادية:**
    - *السيناريو 1: التلاعب غير المصرح به بنقاط الضبط (Metasploit):*
      - استخدام وحدة `auxiliary/scanner/scada/modbusclient` لكتابة قيمة جديدة في سجل التخزين `40001` (رفع حد حرارة الإيقاف الآمن من 70 إلى 150 درجة مئوية).
    - *السيناريو 2: فرض تشغيل المشغلات الفيزيائية عبر بايثون:*
      - حقن أمر غير مصرح به باستخدام `FC05 (كتابة ملف منفرد)` لفتح صمام التصريف الطارئ قسرياً:
      ```python
      from pymodbus.client import ModbusTcpClient
      client = ModbusTcpClient('192.168.10.10', port=502)
      client.connect()
      # تفعيل الملف رقم 0 قسرياً لتفريغ الخزان
      client.write_coil(address=0, value=True, slave=1)
      client.close()
      ```
    - *التحقق والتأكيد الدفاعي:* التحقق من إطلاق تنبيه فوري في نظام Suricata، ورصد التناقص الفيزيائي الحاد لمنسوب المياه في محاكي EPANET.

### 2. Standard References / المراجع القياسية المعتمدة
- **PyModbus Library Documentation:** https://pymodbus.readthedocs.io/
- **Metasploit Framework SCADA Modules:** https://www.metasploit.com/
- **Security Onion Documentation:** https://docs.securityonion.net/

---

## Module 5: Verified Free Learning Path (YouTube)
## الوحدة 5: مسار التعلم المجاني المعتمد (يوتيوب)

The following YouTube courses and engineering tutorials provide 100% free, verifiable instruction for constructing open-source OT security testbeds:

المصادر والدروس الهندسية التالية مجانية بنسبة 100% ومتاحة بالكامل على منصة YouTube لبناء مختبرات أمن التكنولوجيا التشغيلية:

| Channel / Organization | Search Query / Playlist Title | Direct Channel URL | Core Technical Focus |
| :--- | :--- | :--- | :--- |
| **Thiago Alves (OpenPLC)** | `OpenPLC Installation Guide` / `Connecting OpenPLC to SCADA` | https://www.youtube.com/@ThiagoAlves | Step-by-step setup of OpenPLC Runtime on Linux, Ladder Logic compilation, and Modbus TCP addressing. |
| **EPA / Water Modeling Tutorials** | `EPANET Simulation Basics` / `Water Network Modeling EPANET` | https://www.youtube.com/@WaterResearchFoundation | Hydraulic network modeling, defining nodes, pipes, headloss parameters, and running dynamic simulations. |
| **David Bombal & NetworkChuck** | `EVE-NG Complete Lab Setup` / `pfSense Firewall Configuration` | https://www.youtube.com/@DavidBombal | Virtualizing network topologies, configuring multi-VLAN firewalls, and setting up SPAN/mirror interfaces. |
| **SANS ICS Security** | `Building Low Cost ICS Labs` / `ICS Security Testbed Design` | https://www.youtube.com/@SANSICSCybersecurity | Architectural blueprints for hardware-in-the-loop and software-in-the-loop testbeds on personal workstations. |
| **HackerSploit** | `Security Onion 2 Setup and Deployment` | https://www.youtube.com/@HackerSploit | Installing and configuring network security monitoring sensors, Zeek pipelines, and Suricata rule management. |
