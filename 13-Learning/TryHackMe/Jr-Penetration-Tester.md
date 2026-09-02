# Jr Penetration Tester

## Introduction

The **Jr Penetration Tester** path on TryHackMe is designed to develop the fundamental skills required to begin working in penetration testing.

Throughout the path, different areas of offensive security are covered, including networking, reconnaissance, enumeration, vulnerability assessment, exploitation, web application security, Windows and Linux systems, Active Directory, and other techniques commonly used during penetration tests.

The goal is to progressively develop a practical methodology for assessing targets, identifying attack surfaces, validating vulnerabilities, and documenting findings.

[TryHackMe — Jr Penetration Tester](https://tryhackme.com/path/outline/jrpenetrationtester)

---

# 1. Penetration Testing Foundations

This module introduces the fundamental concepts of penetration testing through guided engagements and the methodologies used to structure professional security assessments.

It covers the penetration testing process from reconnaissance through exploitation using both a web application and vulnerable infrastructure. The module also introduces the **Cyber Kill Chain** and several **penetration testing frameworks**, providing the foundation for understanding how real-world penetration tests are structured.

[TryHackMe — Penetration Testing Foundations](https://tryhackme.com/module/penetration-testing-foundations2)

## Guided Pentest: Web

[TryHackMe — Guided Pentest: Web](https://tryhackme.com/room/guidedpentestweb)

### Content

This room introduces a practical web application penetration test, following the process from initial reconnaissance and enumeration through vulnerability discovery and exploitation. The engagement focuses on understanding how a penetration tester maps a web application's attack surface, identifies interesting functionality, and uses the information gathered during reconnaissance to guide further testing.

The assessment covers several common web application weaknesses, including **Insecure Direct Object References (IDOR)**, weaknesses in **password reset functionality**, and insecure **file upload mechanisms**. These vulnerabilities demonstrate the importance of testing both application functionality and access controls rather than relying only on automated scanning.

The room ultimately demonstrates how multiple vulnerabilities can be chained together to escalate the impact of an attack, progressing from information disclosure and account compromise to access to administrative functionality and **Remote Code Execution (RCE)**. This highlights the importance of understanding the complete attack chain rather than analysing vulnerabilities in isolation.

## Guided Pentest: Infrastructure

[TryHackMe — Guided Pentest: Infrastructure](https://tryhackme.com/room/guidedpentestinfrastructure)

### Content

This room introduces the methodology behind an infrastructure penetration test, covering the process from initial enumeration to exploitation, privilege escalation, and reporting. The assessment focuses on a Linux host and demonstrates how a penetration tester combines scanning, service identification, vulnerability research, exploitation, and post-exploitation enumeration to compromise a target.

The room uses **Nmap** for service enumeration and demonstrates how the results can be analysed to identify vulnerable software and potential exploits. After gaining initial access through a vulnerable service, the assessment continues from inside the target, where local Linux files and configurations are enumerated to identify a path to privilege escalation.

The final stage introduces the importance of **penetration testing reports**, showing how findings should be documented with their impact, exploitation evidence, and remediation recommendations. The overall engagement demonstrates the complete workflow of an infrastructure pentest: **enumeration → vulnerability analysis → initial access → privilege escalation → reporting**.

## Dive Into Pentesting

[TryHackMe — Dive Into Pentesting](https://tryhackme.com/room/diveintopentesting)

### Content

This room introduces the core concepts and methodology behind penetration testing, focusing on how authorised security testing differs from malicious hacking. It covers the main areas of web application and network penetration testing, as well as the relationship between **vulnerabilities, threats, and risk**, including the concept of `Vulnerability × Threat = Risk` and the risk management cycle.

The room also explores why vulnerabilities exist, including human assumptions, software bugs, system complexity, excessive customisation, and technical or design weaknesses. It emphasises the mindset required from an effective penetration tester, particularly attention to detail, contextual thinking, avoiding excessive reliance on automated tools, and maintaining good notes and evidence throughout an engagement.

Finally, the room covers the importance of **ethics, permission, and trust** during penetration tests. Professional testing requires proper authorisation, defined scope, responsible communication, effective time management, and professional conduct to ensure that security testing benefits the organisation without causing unnecessary impact.

## Cyber Kill Chain

[TryHackMe — Cyber Kill Chain](https://tryhackme.com/room/cyberkillchain)

### Content

This room introduces the **Cyber Kill Chain**, a framework developed by Lockheed Martin to describe the different stages of a cyber attack. It breaks an attack into seven stages: **Reconnaissance, Weaponization, Delivery, Exploitation, Installation, Command and Control (C2), and Actions on Objectives**. Understanding these stages helps security professionals analyse how an attack progresses from initial preparation to achieving its final objective.

The room demonstrates how the Cyber Kill Chain can be applied to real-world attacks and how defenders can use it to identify opportunities to detect, disrupt, or prevent an attack at different stages. It also highlights that attackers do not necessarily follow every stage in a strict sequence, making the framework a useful model for understanding attacker behaviour rather than a rigid procedure.

Understanding the Cyber Kill Chain is useful during penetration testing because it provides a structured way to think about an attack from both the **attacker's and defender's perspective**. It helps identify where security controls can be bypassed, where detection is possible, and how individual actions contribute to the overall attack lifecycle.

## Penetration Testing Frameworks

[TryHackMe — Penetration Testing Frameworks](https://tryhackme.com/room/penetrationtestingframeworks)

### Content

This room introduces penetration testing frameworks and explains how they provide a structured methodology for conducting security assessments, from planning and scoping through exploitation, reporting, and remediation validation. Using a recognised framework improves thoroughness, consistency, compliance, and communication during an engagement, while also helping testers avoid unstructured testing and activities outside the agreed scope.

The room introduces several major frameworks and methodologies, including **OSSTMM**, **OWASP Web Security Testing Guide (WSTG)**, **NIST SP 800-115**, **PTES**, and **ISSAF**. It also introduces **MITRE ATT&CK** as a complementary knowledge base for mapping adversary tactics and techniques, alongside other frameworks such as WASC, CSA Cloud Controls Matrix, OWASP MASTG, PCI DSS Penetration Testing Guidelines, and CBEST.

The final task provides a practical exercise where different testing scenarios are presented and the appropriate framework must be selected for each situation. This reinforces the main concept of the room: **the most suitable penetration testing framework depends on the type, scope, and objectives of the engagement**.