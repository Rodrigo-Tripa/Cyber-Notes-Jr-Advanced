# MITRE ATT&CK

## Adversarial Tactics, Techniques, and Common Knowledge

**MITRE ATT&CK** is a globally accessible knowledge base that documents real-world adversary behaviour. It organises the techniques and procedures commonly used by attackers and provides a common language for describing how an adversary operates during a compromise.

Unlike penetration testing methodologies such as **PTES**, **NIST SP 800-115**, or **OSSTMM**, ATT&CK does not define a complete penetration testing process. Instead, it is primarily used to understand, classify, and map **adversary behaviour**.

## How ATT&CK Is Structured

The core of ATT&CK is organised around three important concepts:

**Tactics → Techniques → Procedures**

### Tactics

**Tactics** represent the adversary's **goals** or the reason behind an action.

Examples include:

- **Reconnaissance** — Gathering information about a target.
    
- **Resource Development** — Obtaining or preparing resources required for an operation.
    
- **Initial Access** — Gaining an initial foothold in a target environment.
    
- **Execution** — Running malicious or attacker-controlled code.
    
- **Persistence** — Maintaining access to a compromised environment.
    
- **Privilege Escalation** — Obtaining higher levels of permissions.
    
- **Defense Evasion** — Avoiding or bypassing security controls.
    
- **Credential Access** — Obtaining credentials or authentication material.
    
- **Discovery** — Learning about the compromised environment.
    
- **Lateral Movement** — Moving from one system to another.
    
- **Collection** — Gathering information of interest.
    
- **Command and Control** — Establishing communication with compromised systems.
    
- **Exfiltration** — Removing collected information from the environment.
    
- **Impact** — Manipulating, disrupting, or destroying systems or data.

A tactic therefore answers:

> **Why is the adversary performing this action?**

## Techniques

**Techniques** describe **how** an adversary attempts to achieve a tactical objective.

For example, under the **Credential Access** tactic, an attacker may use techniques involving credential dumping, password stores, or other methods of obtaining authentication material.

Techniques are identified using ATT&CK IDs such as:

`T1003` — OS Credential Dumping

Some techniques also contain **sub-techniques**, which provide a more specific classification of the behaviour.

For example:

`T1003.001` — LSASS Memory

This hierarchy allows ATT&CK to describe attacker behaviour at different levels of detail.

A technique therefore answers:

> **How is the adversary achieving the objective?**

## Procedures

**Procedures** describe examples of how a technique has been used in practice. They provide concrete context about the behaviour associated with a technique, including examples associated with known threat actors, malware, or other software.

This creates a useful relationship:

**Tactic → Technique → Procedure**

For example:

**Credential Access**  
→ **OS Credential Dumping (T1003)**  
→ **LSASS Memory (T1003.001)**  
→ Specific real-world procedures used to obtain credentials from LSASS.

This allows a security professional to move from a high-level objective to a specific technique and finally to real-world examples.

## ATT&CK Matrices

ATT&CK presents techniques within **matrices**, where tactics form the high-level categories and techniques are organised underneath them.

Different ATT&CK domains exist for different environments. The most commonly encountered is **Enterprise ATT&CK**, which focuses on adversary behaviour against enterprise environments, including Windows, Linux, macOS, cloud platforms, networks, containers, and other technologies.

Other ATT&CK knowledge bases cover areas such as:

- **Mobile** — Adversary behaviour targeting mobile devices.
    
- **ICS** — Adversary behaviour targeting industrial control systems.

The matrix makes it possible to visualise an attack across multiple stages and understand how different techniques relate to an adversary's objectives.

## ATT&CK in Penetration Testing

ATT&CK can be useful during penetration testing and red team engagements because it provides a standard way to describe attacker behaviour.

A tester can map actions performed during an engagement to ATT&CK techniques. For example, activities involving credential access, discovery, privilege escalation, or lateral movement can be mapped to their corresponding ATT&CK techniques.

This can make reports more useful because the organisation can understand not only **what vulnerability was exploited**, but also **what adversary behaviour the activity represents**.

For example:

**Finding:** Weak administrative credentials  
↓  
**Result:** Valid account obtained  
↓  
**ATT&CK mapping:** Valid Accounts  
↓  
**Potential impact:** Enables authenticated access and potentially further lateral movement

ATT&CK can therefore complement a penetration testing methodology rather than replace it.

## ATT&CK for Detection and Defence

ATT&CK is also heavily used from the defensive perspective. Security teams can map their detection capabilities against known adversary techniques and identify gaps in monitoring.

For example, if an organisation knows that a particular technique is relevant to its environment, it can investigate whether its security controls are capable of detecting that behaviour.

This creates a relationship between:

**Adversary behaviour → Technique → Detection → Security control**

Security teams can use this information to improve:

- Detection engineering.
    
- SIEM rules.
    
- Endpoint monitoring.
    
- Threat hunting.
    
- Incident response.
    
- Security validation.
    
- Defensive coverage.

## ATT&CK vs Penetration Testing Methodologies

It is important to understand the distinction between ATT&CK and traditional penetration testing methodologies.

**PTES** provides a structured approach for conducting a penetration test.

**NIST SP 800-115** provides guidance for planning and performing technical security assessments.

**OSSTMM** provides a methodology for structured security testing.

**OWASP WSTG** provides specialised guidance for web application security testing.

**MITRE ATT&CK** provides a knowledge base for understanding and categorising adversary tactics and techniques.

Therefore, a penetration tester might use **PTES** to structure an engagement while using **ATT&CK** to map relevant attacker techniques performed during the engagement.

## Example Attack Chain

Consider a simplified attack against a Windows environment:

**Initial Access**  
→ The attacker obtains access through a compromised account.

**Execution**  
→ Commands are executed on the compromised system.

**Discovery**  
→ The attacker enumerates users, hosts, and network information.

**Credential Access**  
→ Authentication material is targeted.

**Privilege Escalation**  
→ The attacker obtains higher privileges.

**Lateral Movement**  
→ Access is extended to another system.

Each action can potentially be mapped to an ATT&CK technique. The resulting mapping provides a structured representation of the attacker's behaviour.

## Key Takeaway

MITRE ATT&CK is best understood as a **common language for describing adversary behaviour**.

The central relationship is:

**Tactics = Why**  
**Techniques = How**  
**Procedures = How it was done in practice**

For penetration testers, ATT&CK is particularly useful for mapping attack activity, communicating findings, designing realistic attack simulations, and understanding how individual techniques fit into a broader attack chain.

It should therefore be viewed as a **complementary framework**, rather than a replacement for a penetration testing methodology.