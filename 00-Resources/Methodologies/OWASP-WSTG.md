# OWASP Web Security Testing Guide (WSTG)

The **OWASP Web Security Testing Guide (WSTG)** is a comprehensive guide for testing the security of web applications and web services. It provides a structured collection of testing techniques that can be used throughout a web penetration test, helping testers systematically evaluate different parts of an application instead of relying only on vulnerability scanners.

The WSTG is organised around different areas of web security testing, including **information gathering, configuration and deployment management, identity management, authentication, authorisation, session management, input validation, error handling, weak cryptography, business logic, client-side testing, and API testing**. Each category contains specific tests designed to identify common security weaknesses.

## Information Gathering

The testing process begins with understanding the application's attack surface. Information gathering includes identifying application entry points, technologies, frameworks, server components, and other information that can help determine where further testing should be focused.

This stage is important because effective web testing depends on understanding how the application is structured and which components are exposed to the tester.

## Configuration and Deployment Management

This section focuses on weaknesses in the application's configuration and deployment environment. Testing may include identifying default files, administrative interfaces, unnecessary functionality, exposed configuration information, insecure HTTP methods, and other deployment-related issues.

Misconfigurations can expose sensitive functionality or information even when the application's core functionality is otherwise secure.

## Identity Management

Identity management testing evaluates how the application handles users and accounts. This includes testing user registration, account provisioning, account enumeration, and other mechanisms related to the application's identity model.

The objective is to determine whether the application properly controls how identities are created, managed, and exposed.

## Authentication

Authentication testing focuses on how users prove their identity to the application. This includes evaluating login mechanisms, password policies, authentication bypasses, remember-me functionality, multi-factor authentication, and other authentication-related controls.

Weak authentication mechanisms can allow attackers to gain access to accounts without valid credentials.

## Authorisation

Authorisation testing determines whether authenticated users can access resources and functionality they are actually permitted to use. This includes testing for **horizontal and vertical privilege escalation**, insecure direct object references, forced browsing, and other access control weaknesses.

A secure authentication system does not guarantee secure authorisation. An application must also verify permissions for every protected resource and action.

## Session Management

Session management testing examines how the application creates, maintains, and terminates user sessions. This includes testing session identifiers, cookies, session fixation, session expiration, logout functionality, and session invalidation.

Weak session management can allow attackers to hijack or abuse authenticated sessions.

## Input Validation

Input validation testing focuses on how applications process user-controlled input. Testers attempt to determine whether malicious or unexpected input can alter application behaviour or reach sensitive backend functionality.

This area includes vulnerabilities such as **SQL injection, command injection, cross-site scripting (XSS), XML-related attacks, and other injection-based weaknesses**.

## Error Handling

Error handling testing evaluates how an application behaves when unexpected conditions occur. Poorly handled errors can disclose information about internal systems, application logic, database structures, file paths, or debugging information.

Error messages should provide enough information for legitimate users and administrators without exposing unnecessary technical details to attackers.

## Weak Cryptography

This section focuses on the use of cryptography within the application. Testing can include identifying weak algorithms, insecure configurations, improper key management, exposed sensitive data, and other cryptographic weaknesses.

The goal is to determine whether sensitive information is adequately protected both during transmission and when stored.

## Business Logic Testing

Business logic testing focuses on vulnerabilities caused by flaws in the way an application implements its intended functionality. These weaknesses may not be detected by traditional automated scanners because the application can behave correctly from a technical perspective while still allowing an attacker to abuse its intended workflow.

Examples include bypassing business rules, manipulating transaction processes, abusing functionality, or performing actions in an unintended order.

## Client-Side Testing

Client-side testing examines security mechanisms implemented in the user's browser or other client-side components. This includes testing JavaScript functionality, DOM-based vulnerabilities, client-side validation, browser storage, cross-origin behaviour, and other client-side security controls.

Client-side controls should not be treated as a replacement for server-side validation and authorisation.

## API Testing

Modern web applications frequently expose functionality through APIs. The WSTG therefore includes testing considerations for web services and APIs, including authentication, authorisation, input handling, error handling, and the exposure of sensitive functionality or data.

API testing is particularly important because APIs may expose functionality that is not directly visible through the application's normal user interface.

## WSTG and Web Penetration Testing

The WSTG is particularly useful during the **web application testing phase of a penetration test**. It provides a structured checklist of areas that should be considered and helps testers maintain consistency across different assessments.

It can be combined with a broader penetration testing methodology such as **PTES**. PTES can provide the overall engagement structure, while the WSTG provides detailed guidance for the web application testing itself.

For example:

**PTES**  
→ Defines the overall penetration testing lifecycle.

**OWASP WSTG**  
→ Guides the detailed security testing of the web application.

**MITRE ATT&CK**  
→ Can be used to map relevant adversary techniques where appropriate.

## Practical Mental Model

A useful way to think about the WSTG is:

**Discover → Understand → Test → Validate → Document**

First understand the application's attack surface and architecture. Then systematically test authentication, authorisation, sessions, input handling, business logic, client-side functionality, APIs, and other relevant areas.

The WSTG should be treated as a **testing guide rather than an automated checklist**. A penetration tester still needs to apply context, manual testing, technical knowledge, and professional judgement to determine which tests are relevant and how deeply they should be performed.

## Key Takeaway

The **OWASP Web Security Testing Guide** provides a structured methodology for assessing web applications and web services. Its main value is the breadth and organisation of its testing areas, helping penetration testers systematically identify security weaknesses across the entire web application attack surface.

**Understand the application → Map the attack surface → Test each relevant security area → Validate vulnerabilities → Document the findings.**