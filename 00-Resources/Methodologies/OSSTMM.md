# OSSTMM

## Open Source Security Testing Methodology Manual

[[OSSTMM-3.pdf]]

The **Open Source Security Testing Methodology Manual (OSSTMM)** is a security testing methodology developed by **ISECOM**. It is designed to measure **operational security (OpSec)** through structured and repeatable testing rather than relying on assumptions, best practices, or anecdotal evidence.

One of the main ideas behind OSSTMM is that security should be measured according to how a system actually operates. A configuration may look secure on paper, but that does not necessarily mean that the security controls work as intended when the system is being used. OSSTMM therefore focuses on testing the real behaviour of the environment and producing results that can be verified and measured. citeturn0search23turn0search25

## What OSSTMM Covers

OSSTMM is broader than a typical technical penetration testing methodology. It can be applied across several security channels, including:

- **Human** — People, procedures, and interactions.
    
- **Physical** — Physical locations, facilities, and physical controls.
    
- **Wireless** — Wireless communications and infrastructure.
    
- **Telecommunications** — Telecommunication systems and services.
    
- **Data Networks** — Network infrastructure, systems, and services.
    

This broad scope allows OSSTMM to be used for different types of security assessments rather than focusing exclusively on applications or network vulnerabilities. citeturn0search23turn0search3

## Operational Security

The concept of **Operational Security (OpSec)** is central to OSSTMM.

The methodology is concerned with whether the security controls that exist in an environment actually provide the protection they are expected to provide during normal operation.

This leads to an important distinction:

**Configured security ≠ operational security**

A firewall rule, access control, security policy, or authentication mechanism may exist, but the important question is whether it actually prevents the interaction or exposure it is supposed to prevent.

OSSTMM therefore attempts to verify security through testing instead of simply checking whether a particular control has been configured.

## Testing Instead of Assumptions

OSSTMM places strong emphasis on obtaining **facts from testing**.

For example, instead of assuming that a service is protected because a firewall is configured to restrict access, the tester can verify how the service behaves when different interactions are attempted.

The methodology is designed to produce results that are:

- **Consistent**
    
- **Repeatable**
    
- **Measurable**
    
- **Based on observed results**
    
- **Actionable**
    

This makes the methodology useful when the goal is to understand how effective security controls are in the real environment. citeturn0search24turn0search0

## Scope, Vectors and Attack Surface

OSSTMM places considerable importance on clearly defining what is being tested.

The **scope** represents the security environment being assessed, while a **vector** represents the perspective or path through which interaction with an asset can occur.

The result of testing can then be used to understand the **attack surface**. In OSSTMM terms, the attack surface is related to the unprotected portion of the defined scope from a particular vector.

This is useful because attack surface is not simply a list of open ports or vulnerabilities. It depends on what can actually be interacted with, accessed, or influenced from the perspective being tested.

## The Testing Process

OSSTMM does not reduce security testing to running a scanner and collecting its output. The methodology describes a process in which the tester first develops an understanding of the target, performs testing, analyses the results, and derives measurable information from those results.

A simplified representation is:

**Understand → Test → Observe → Analyse → Measure**

The methodology also distinguishes between **normal operations** and deliberately testing beyond that normal baseline. By comparing the two, the analyst can better understand how the environment behaves and where its controls provide effective separation or protection. citeturn0search0

## Test Types

OSSTMM defines different test types depending on how much information is available to the tester and how the test is performed.

These include:

- **Blind**
    
- **Double Blind**
    
- **Gray Box**
    
- **Double Gray Box**
    
- **Tandem**
    
- **Reversal**
    

The choice of test type affects what the tester knows and what aspects of the security process can be evaluated. It should therefore be defined as part of the assessment rather than chosen arbitrarily.

## The Four-Point Process

A significant part of the OSSTMM approach is the analysis of both direct and indirect information obtained during testing.

The methodology describes a process that involves understanding normal operations, actively testing those operations, analysing the resulting information, correlating the observations, identifying errors, and deriving measurements.

This matters because a security assessment should not only ask:

> “Can I perform this action?”

It should also consider what happened as a result, what controls were involved, what information was exposed, and whether the observed behaviour matches the intended security posture.

## Operational Security Metrics

OSSTMM uses its own approach to measuring operational security. Rather than relying exclusively on generic severity ratings, it attempts to quantify security based on the results of testing.

One important concept is the **RAV (Rav)**, which is used within the OSSTMM metrics model to help represent the relationship between the different elements observed during an assessment.

The methodology also provides formulas and metrics for analysing areas such as:

- Attack surface.
    
- Controls.
    
- Limitations.
    
- Operational security.
    
- Actual security.
    

The purpose is to turn test observations into measurable information that can be compared and used for decision-making. citeturn0search0

## Controls

OSSTMM does not simply assume that a security control works because it exists.

Controls should be tested to determine whether they provide the expected protection during actual operation. This can include examining authentication, identification, logging, access controls, alarms, and other mechanisms depending on the channel being assessed.

For example, if a system is supposed to record interactions for accountability, the assessment can verify whether those interactions are actually identified and logged correctly.

This is one of the practical differences between checking a configuration and testing operational security.

## Reporting

The results of an OSSTMM assessment should be based on **verified information obtained during the test**.

The methodology includes the **STAR (Security Test Audit Report)** concept for reporting security test results. Reporting should provide useful information about what was tested, what was observed, and what the results mean for the security of the environment. citeturn0search3

An important principle is to document **what was not tested** as well. A security assessment cannot claim to represent an environment accurately if significant parts of the defined scope were never assessed.

## OSSTMM and Penetration Testing

OSSTMM can be used for penetration testing, ethical hacking, vulnerability assessments, red teaming, blue teaming, and other types of security assessments. However, it is broader than a conventional penetration testing workflow. citeturn0search24

For example:

**PTES** is mainly concerned with structuring a penetration testing engagement.

**NIST SP 800-115** provides guidance for technical security testing and assessment.

**OWASP WSTG** focuses specifically on web application security testing.

**OSSTMM** takes a broader view and is concerned with measuring operational security across different channels.

Because of this, OSSTMM can complement more specialised testing methodologies rather than necessarily replacing them.

## Practical Example

Imagine an organisation has a network service protected by several security controls. A basic assessment might verify that the firewall is configured correctly and that authentication is enabled.

An OSSTMM-style assessment goes further. The tester is interested in how the service behaves during actual interaction, whether the controls provide the expected separation, what information can be obtained, and whether unexpected paths around those controls exist.

The important question becomes:

**Does the security mechanism actually work under operational conditions?**

rather than simply:

**Is the security mechanism configured?**

## Key Concepts

When studying OSSTMM, a few concepts are particularly important:

**Operational Security (OpSec)** — Security as it actually functions during operation.

**Scope** — The environment and assets included in the assessment.

**Vector** — The perspective or means through which interaction with the scope is tested.

**Attack Surface** — The portion of the scope that is exposed or insufficiently protected from a defined vector.

**Controls** — Measures intended to provide protection or reduce unwanted interaction.

**RAV** — A metric used by OSSTMM to quantify aspects of operational security.

**STAR** — Security Test Audit Report used for presenting OSSTMM assessment results.

## In Practice

The main lesson from OSSTMM is that security should be **tested and measured, not assumed**.

A security control is valuable because of the protection it provides in operation, not simply because a policy says it should exist or a configuration indicates that it is enabled.

For a security professional, OSSTMM is therefore useful as a methodology for looking at an environment as an operational system and asking whether its controls actually provide the intended level of security.

**Observe the operation → Test the controls → Verify the result → Measure the security → Report the facts**