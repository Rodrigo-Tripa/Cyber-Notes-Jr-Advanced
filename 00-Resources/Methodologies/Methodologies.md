# Penetration Testing Methodologies

This directory contains notes on some of the most relevant methodologies and frameworks used in penetration testing, security assessments, web security testing, and adversary emulation.

These frameworks do not all serve the same purpose. Some describe how to organise and execute a penetration test, while others focus on a specific type of security assessment or provide a way to describe adversary behaviour.

## Methodologies

### PTES — Penetration Testing Execution Standard

[[PTES]]

PTES provides a structured approach to a penetration testing engagement, covering the process from the initial agreement with the client through reconnaissance, threat modelling, vulnerability analysis, exploitation, post-exploitation, and reporting.

**Best suited for:** Structuring an end-to-end penetration test.

---

### NIST SP 800-115 — Technical Guide to Information Security Testing and Assessment

[[NIST-SP-800-115]]

NIST SP 800-115 provides guidance for planning and conducting technical information security testing and assessments. It covers assessment planning, examination, interviewing, technical testing, analysis, and reporting.

**Best suited for:** Formal technical security testing and assessment.

---

### OSSTMM — Open Source Security Testing Methodology Manual

[[OSSTMM]]

OSSTMM provides a methodology for testing and measuring operational security. It takes a broader view of security and covers areas such as human, physical, wireless, telecommunications, and data network security.

**Best suited for:** Broad operational security assessments and measurable security testing.

---

### ISSAF — Information Systems Security Assessment Framework

[[ISSAF]]

ISSAF provides structured guidance for assessing the security of information systems and infrastructure. It covers activities such as information gathering, vulnerability assessment, penetration testing, analysis, and reporting.

**Best suited for:** Structured information systems and infrastructure security assessments.

---

### OWASP WSTG — Web Security Testing Guide

[[OWASP-WSTG]]

The **OWASP Web Security Testing Guide (WSTG)** provides detailed guidance for testing web applications and web services. It covers areas such as information gathering, configuration and deployment, authentication, authorisation, session management, input validation, business logic, client-side testing, and API security.

**Best suited for:** Web application and web service security testing.

---

### MITRE ATT&CK

[[MITRE-ATTCK]]

MITRE ATT&CK is a knowledge base that documents real-world adversary tactics, techniques, and procedures. Unlike PTES, NIST, or OSSTMM, it is not a complete penetration testing methodology. Instead, it provides a common language for describing and mapping adversary behaviour.

**Best suited for:** Adversary emulation, red teaming, threat-informed testing, detection engineering, and mapping attack techniques.

---

## Choosing a Framework

There is no single framework that is appropriate for every security assessment. The choice depends on the **target, objectives, scope, and type of engagement**.

A simple way to approach the decision is:

|Situation|Relevant Framework|
|---|---|
|General penetration test|**PTES**|
|Technical security assessment|**NIST SP 800-115**|
|Operational security assessment|**OSSTMM**|
|Information systems assessment|**ISSAF**|
|Web application testing|**OWASP WSTG**|
|Adversary behaviour and attack techniques|**MITRE ATT&CK**|

These frameworks can also be used together. A penetration tester might use **PTES** to structure an engagement, **OWASP WSTG** to guide detailed testing of a web application, and **MITRE ATT&CK** to map relevant adversary techniques.

The important skill is not memorising which framework is considered "best". It is understanding **what each framework is designed for and selecting the one that matches the assessment objectives**.

## Quick Comparison

|Framework|Main Focus|Type|
|---|---|---|
|**PTES**|Penetration testing lifecycle|Methodology|
|**NIST SP 800-115**|Technical security testing|Assessment guide|
|**OSSTMM**|Operational security measurement|Methodology|
|**ISSAF**|Information systems security assessment|Assessment framework|
|**OWASP WSTG**|Web application security|Testing guide|
|**MITRE ATT&CK**|Adversary tactics and techniques|Knowledge base / framework|

## How They Fit Together

These resources can be viewed as complementary rather than competing methodologies.

A real engagement might look like:

**PTES**  
→ Defines the overall penetration testing process.

**OWASP WSTG**  
→ Provides detailed techniques for testing the web application.

**MITRE ATT&CK**  
→ Maps relevant attacker behaviour and techniques.

**NIST SP 800-115 / ISSAF / OSSTMM**  
→ Can provide additional guidance depending on the assessment requirements and methodology being followed.

The framework provides the structure; the tester still needs to apply professional judgement and select the appropriate techniques for the environment.

---

## Related Notes

- [Pentesting Methodology](01-Pentesting/Pentesting-Methodology.md)
    
- [Rules of Engagement](01-Pentesting/Rules-of-Engagement.md)
    
- [Reconnaissance](01-Pentesting/Reconnaissance.md)
    
- [Vulnerability Assessment](01-Pentesting/Vulnerability-Assessment.md)
    
- [Exploitation](01-Pentesting/Exploitation.md)
    
- [Post-Exploitation](01-Pentesting/Post-Exploitation.md)
    
- [Reporting](01-Pentesting/Reporting.md)