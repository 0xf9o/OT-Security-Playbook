<div align="center">

<!-- Title Panel -->
<table width="100%" style="border-collapse: collapse; border: 1px solid #30363d; border-radius: 6px; background-color: #0d1117;">
  <tr>
    <td style="padding: 20px;">
      <h1 style="margin: 0; color: #e3b341; text-transform: uppercase;">[ DOCS ] OT Security Playbook</h1>
      <p style="margin: 10px 0 0 0; color: #8b949e; font-size: 1.1em;">
        <b>Bridging Physical Machinery with Cyber Resilience.</b><br>
        A comprehensive, open-source reference for Industrial Control Systems (ICS) and Operational Technology (OT) Security.
      </p>
    </td>
  </tr>
</table>
</div>

<br>

## 📖 About This Playbook / عن هذا المرجع

**[ EN ]**
The transition from traditional IT security to Operational Technology (OT) security is steep. This playbook is engineered to bridge that gap. Built by an engineer who started with wrenches and mechanical maintenance before diving into network packets and detection engineering, this repository serves as a structured, pragmatic guide to securing industrial environments. It avoids theoretical fluff and focuses on what works in the field: understanding the Purdue Model, dissecting industrial protocols (Modbus, DNP3, OPC UA), and building robust Detection Engineering rules without disrupting physical processes.

**[ AR ]**
الانتقال من أمن المعلومات التقليدي (IT) إلى أمن التقنيات التشغيلية (OT) ليس بالأمر السهل. تم بناء هذا المرجع لسد هذه الفجوة. تمت كتابة هذا الدليل بواسطة مهندس بدأ مسيرته بين المعدات الميكانيكية قبل أن ينتقل لتحليل الشبكات وهندسة الكشف، ليقدم لك دليلاً عملياً ومنهجياً لتأمين البيئات الصناعية. هذا المرجع يبتعد عن الحشو النظري ويركز على التطبيق الفعلي: فهم نموذج بيردو (Purdue Model)، تحليل البروتوكولات الصناعية، وبناء قواعد كشف دقيقة لا تتسبب في تعطيل العمليات الفيزيائية الحرجة.

---

## 🎯 Target Audience / الفئة المستهدفة

**[ EN ]**
*   **IT Security Professionals & SOC Analysts:** Looking to understand how to monitor and secure Level 1 and Level 2 industrial assets.
*   **Automation & Control Engineers:** Seeking to secure their PLCs, HMIs, and SCADA systems against cyber threats.
*   **Cybersecurity Students:** Who want a structured, free, and verifiable roadmap into the highly demanded ICS/OT security field.

**[ AR ]**
*   **محللو مركز العمليات الأمنية (SOC) ومختصو الـ IT:** الراغبون في فهم كيفية مراقبة وتأمين الأصول الصناعية.
*   **مهندسو التحكم والأتمتة:** الباحثون عن طرق تأمين أجهزة PLC و HMI وأنظمة SCADA من التهديدات السيبرانية.
*   **طلاب الأمن السيبراني:** الباحثون عن خريطة طريق منظمة، مجانية، وموثوقة لدخول مجال أمن الـ OT عالي الطلب.

---

## 🧭 Navigation / خريطة المرجع

<div align="center">
<table width="100%" style="border-collapse: separate; border-spacing: 15px;">
  <tr>
    <!-- Section 1 -->
    <td width="50%" valign="top" style="border: 1px solid #30363d; border-top: 3px solid #58a6ff; border-radius: 6px; padding: 15px; background-color: #0d1117;">
      <h3 style="margin-top: 0; color: #58a6ff;">01. Foundations & Roadmaps</h3>
      <p style="color: #8b949e; font-size: 0.9em; margin-bottom: 10px;">أساسيات الشبكات وخريطة طريق الدخول للمجال.</p>
      <ul style="color: #8b949e; padding-left: 20px;">
        <li><a href="./01-Cybersecurity-Foundations" style="color: #c9d1d9; text-decoration: none;">Cybersecurity Foundations</a></li>
        <li><a href="./02-OT-Security-Roadmap" style="color: #c9d1d9; text-decoration: none;">OT Security Roadmap</a></li>
      </ul>
    </td>
    <!-- Section 2 -->
    <td width="50%" valign="top" style="border: 1px solid #30363d; border-top: 3px solid #3fb950; border-radius: 6px; padding: 15px; background-color: #0d1117;">
      <h3 style="margin-top: 0; color: #3fb950;">02. Architecture & Protocols</h3>
      <p style="color: #8b949e; font-size: 0.9em; margin-bottom: 10px;">نموذج بيردو، المعماريات، وتحليل البروتوكولات الصناعية.</p>
      <ul style="color: #8b949e; padding-left: 20px;">
        <li><a href="./03-Industrial-Protocol-Handbook" style="color: #c9d1d9; text-decoration: none;">Industrial Protocol Handbook</a></li>
        <li><a href="./04-ICS-Architecture-Handbook" style="color: #c9d1d9; text-decoration: none;">ICS Architecture Handbook</a></li>
      </ul>
    </td>
  </tr>
  <tr>
    <!-- Section 3 -->
    <td width="50%" valign="top" style="border: 1px solid #30363d; border-top: 3px solid #bc8cff; border-radius: 6px; padding: 15px; background-color: #0d1117;">
      <h3 style="margin-top: 0; color: #bc8cff;">03. Detection & Labs</h3>
      <p style="color: #8b949e; font-size: 0.9em; margin-bottom: 10px;">هندسة الكشف (Zeek/Suricata) وبناء مختبرات المحاكاة.</p>
      <ul style="color: #8b949e; padding-left: 20px;">
        <li><a href="./05-OT-Detection-Engineering" style="color: #c9d1d9; text-decoration: none;">OT Detection Engineering</a></li>
        <li><a href="./06-OT-Security-Lab-Recipes" style="color: #c9d1d9; text-decoration: none;">OT Security Lab Recipes</a></li>
      </ul>
    </td>
    <!-- Section 4 -->
    <td width="50%" valign="top" style="border: 1px solid #30363d; border-top: 3px solid #e3b341; border-radius: 6px; padding: 15px; background-color: #0d1117;">
      <h3 style="margin-top: 0; color: #e3b341;">04. Operations & Tooling</h3>
      <p style="color: #8b949e; font-size: 0.9em; margin-bottom: 10px;">دليل أدوات الفحص والمراقبة السلبية والمفتوحة المصدر.</p>
      <ul style="color: #8b949e; padding-left: 20px;">
        <li><a href="./07-OT-Security-Toolbook" style="color: #c9d1d9; text-decoration: none;">OT Security Toolbook</a></li>
      </ul>
    </td>
  </tr>
</table>
</div>

---

## 🤝 Maintainer / المشرف على المرجع

Built and maintained by **0xf9o (Faisal Alnumani)**. 
*   **LinkedIn:** [Faisal Alnumani](https://www.linkedin.com/in/faisal-alumani)
*   **X:** [@0xF9o](https://x.com/0xF9o)
