# PTES

## Penetration Testing Execution Standard

The **Penetration Testing Execution Standard (PTES)** is a framework for conducting penetration tests in a structured and consistent way. It defines the main stages of a penetration testing engagement, from the initial agreement with the client to the final reporting.

The purpose of PTES is not to provide a list of commands or specific tools. Instead, it provides a common structure that helps penetration testers understand what needs to happen during an engagement and ensures that important parts of the assessment are not overlooked.

The PTES lifecycle is divided into seven main phases:

**Pre-engagement Interactions → Intelligence Gathering → Threat Modeling → Vulnerability Analysis → Exploitation → Post-Exploitation → Reporting**

---

## 1. Pre-engagement Interactions

The engagement starts before any technical testing takes place.

During **Pre-engagement Interactions**, the tester and the client establish what the assessment is supposed to achieve and how it will be performed. This is where the scope, objectives, expectations, limitations, and rules of the engagement are defined.

Important topics include:

- Scope and target systems.
    
- Objectives of the penetration test.
    
- Testing timeframe.
    
- Communication channels.
    
- Rules of engagement.
    
- Authorised testing activities.
    
- Systems or actions that are out of scope.
    
- Handling of sensitive information.
    
- Reporting requirements.
    

This phase is particularly important from a legal and operational perspective. Having permission to test one system does not automatically give permission to test everything connected to it.

A well-defined scope gives the tester a clear boundary and gives the client a clear understanding of what the assessment will and will not cover.

---

## 2. Intelligence Gathering

Once the engagement has been defined, the tester starts gathering information about the target.

**Intelligence Gathering** is about understanding the environment before attempting to exploit it. The information collected can come from both public sources and direct interaction with the target, depending on the scope.

Information may include:

- Domains and subdomains.
    
- IP addresses and network ranges.
    
- DNS information.
    
- Technologies and software.
    
- Publicly exposed services.
    
- Email addresses and usernames.
    
- Organisational information.
    
- Application functionality.
    
- Network architecture.
    

The amount of information available at this stage can have a major influence on the rest of the assessment.

For example, discovering that a target uses a particular web technology may lead the tester towards specific areas of vulnerability analysis. Similarly, identifying an exposed service can provide a starting point for further enumeration.

The goal is to build a useful picture of the target rather than simply collecting as much information as possible.

---

## 3. Threat Modeling

**Threat Modeling** connects the information gathered about the target with realistic attack scenarios.

Instead of treating every possible vulnerability as equally important, the tester considers what an attacker would actually want to achieve and which assets are most valuable.

This can involve identifying:

- Important assets.
    
- Potential threat actors.
    
- Valuable data.
    
- Entry points.
    
- Trust relationships.
    
- Possible attack paths.
    
- Business objectives.
    
- Security controls that may prevent or limit an attack.
    

Threat modeling helps answer questions such as:

> What would an attacker target first?

> What would happen if they gained access?

> Which systems could provide access to more valuable resources?

This phase gives the penetration test direction. It helps the tester prioritise testing based on the actual environment instead of blindly checking every possible vulnerability.

---

## 4. Vulnerability Analysis

The next stage is **Vulnerability Analysis**.

Here, the tester examines the attack surface and identifies weaknesses that could potentially be exploited. This involves more than simply running automated vulnerability scanners.

Vulnerability analysis can include:

- Service enumeration.
    
- Application testing.
    
- Configuration analysis.
    
- Vulnerability scanning.
    
- Manual testing.
    
- Version and software identification.
    
- Authentication testing.
    
- Access control testing.
    
- Vulnerability research.
    

The results need to be analysed and validated.

A scanner might report that a particular service is vulnerable, but the tester needs to determine whether the vulnerability actually exists in the target environment and whether it can realistically be exploited.

The output of this phase should therefore be a set of **validated attack opportunities** that can be considered during exploitation.

---

## 5. Exploitation

**Exploitation** is where the tester attempts to use identified vulnerabilities to gain access or demonstrate their real impact.

The purpose is not simply to obtain a shell at any cost. Exploitation should demonstrate what an attacker could realistically achieve while remaining within the rules of the engagement.

Depending on the target, exploitation could result in:

- Unauthorised access.
    
- Remote code execution.
    
- Access to sensitive information.
    
- Account compromise.
    
- Privilege escalation.
    
- Access to internal systems.
    
- Bypassing security controls.
    

The tester should keep track of how access was obtained and what was possible from that position.

For example:

**Vulnerability → Exploitation → Initial Access → Further Enumeration**

This creates a connection between individual vulnerabilities and the larger attack path.

---

## 6. Post-Exploitation

Obtaining initial access is often only the beginning.

During **Post-Exploitation**, the tester determines what can be achieved from a compromised position and how far an attacker could potentially move through the environment.

Activities can include:

- Situational awareness.
    
- Identifying the compromised system.
    
- Identifying users and privileges.
    
- Discovering other systems.
    
- Searching for credentials.
    
- Identifying trust relationships.
    
- Privilege escalation.
    
- Lateral movement.
    
- Accessing additional resources.
    

The goal is to understand the **value of the compromised position**.

For example, compromising a low-privileged account on one workstation may initially appear to be a limited finding. However, if that account can be used to obtain credentials for an administrator or access another internal system, the overall impact becomes much greater.

Post-exploitation is therefore useful for demonstrating attack chains rather than looking at vulnerabilities in isolation.

---

## 7. Reporting

The final phase is **Reporting**.

A penetration test is only useful if its results can be understood and acted upon. Reporting turns the technical work performed during the engagement into information that the organisation can use to improve its security.

A report should clearly explain:

- What was tested.
    
- What was discovered.
    
- How vulnerabilities were validated.
    
- What evidence was obtained.
    
- What systems were affected.
    
- What the potential impact is.
    
- How the vulnerabilities should be remediated.
    

The report normally needs to serve two different audiences.

A **technical audience** needs enough detail to understand and reproduce the findings and perform remediation.

Management and other **non-technical stakeholders** need a clear understanding of the overall risk, business impact, and priorities.

Good reporting therefore focuses on the significance of the findings rather than simply listing every command or tool used during the assessment.

---

## The PTES Lifecycle

The seven phases can be viewed as one continuous process:

```
Pre-engagement
      ↓
Intelligence Gathering
      ↓
Threat Modeling
      ↓
Vulnerability Analysis
      ↓
Exploitation
      ↓
Post-Exploitation
      ↓
Reporting
```

The phases are connected. Information discovered during reconnaissance can influence threat modeling, threat modeling can influence vulnerability analysis, and successful exploitation can reveal new attack paths that require additional testing.

A penetration test is therefore not necessarily a perfectly linear process. Testers may move between phases as new information is discovered.

---

## PTES in Practice

Consider a web application assessment.

The tester first agrees on the scope and rules of the engagement. During intelligence gathering, they identify the application's domains, technologies, functionality, and exposed endpoints.

Threat modeling then helps determine which parts of the application are most important. Vulnerability analysis can identify issues such as broken access controls, weak authentication, or injection vulnerabilities.

If a vulnerability can be safely exploited, the tester validates its impact. After obtaining access, post-exploitation activities can determine whether the compromised account provides access to additional functionality or systems.

Finally, the complete attack path is documented in the report, including evidence, impact, and remediation recommendations.

The important point is that PTES connects these activities into a **single penetration testing process**.

---

## PTES and Other Frameworks

PTES should not be confused with frameworks that have different purposes.

**NIST SP 800-115** provides broader guidance for technical information security testing and assessment.

**OSSTMM** focuses heavily on measuring operational security through structured testing.

**OWASP WSTG** provides detailed guidance specifically for web application security testing.

**ISSAF** provides a structured approach to information systems security assessment.

**MITRE ATT&CK** documents adversary tactics and techniques and can be used to map attacker behaviour.

PTES is primarily concerned with **how a penetration testing engagement is organised and executed from beginning to end**.

In a real engagement, these resources can be combined. For example, PTES can provide the overall engagement structure while OWASP WSTG supplies detailed web testing guidance and MITRE ATT&CK is used to map relevant adversary techniques.

---

## Key Takeaway

PTES provides a practical structure for carrying out a penetration test:

**Define the engagement → Understand the target → Model threats → Find weaknesses → Exploit them → Determine the real impact → Report the results**

The value of PTES is not in following the seven phases mechanically. It is in providing a consistent way to think about a penetration test as a complete engagement, from the initial agreement with the client to the final delivery of actionable security findings.