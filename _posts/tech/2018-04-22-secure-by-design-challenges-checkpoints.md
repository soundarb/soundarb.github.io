---
layout: post
title: Secure by Design - Challenges and Checkpoints
category: tech
comments: true
excerpt: Design to build from the ground up to secure
tags: null
---

I wanted to write this article for a long time and have been preparing for the last two months to publish. Finally it's here for you.

*Secure by design is an approach to software engineering that the software has been designed to build from the ground up to secure*.

An application determined to be of risk to mission critical data will require a thorough security component during its design phase, in development and implementation, and into maintenance. Every data asset must be examined and its confidentiality, criticality and vulnerability assessed. It is crucial to develop security procedures that are appropriate to each assets criticality and vulnerability.

<figure>
  <img src="{{ site.url }}/images/blog/secure-by-design-challenges-checkpoints.jpg">
  <figcaption>Security is almost always an overhead, either in cost or performance but inevitable</figcaption>
</figure>

The following disciplines will determine the level of security offered and identify any gaps in security controls. For each security discipline, it must be ensured that the application meets the listed ‘Checkpoints’ either natively or through use of extensions and additional libraries.

## Authentication

### Challenge

Authentication is a first line of defense. The application must determine if the user/entity/server/program is what it claims to be. The most common form of authentication is the user id and password. Authentication policies, processes, and logging must be designed, developed, and documented to assure that application keeps unauthorized users from accessing the solution.

### Checkpoints

- Account Lockout - Failed logins should trigger auto-lockout after a determined number of attempts   
- Password Management - Strong policies and procedures (Expiry, Forgot Password, Recovery, Encrypted Storage, One-way Hashing – MD6 (Merkle-Damgard), SHA3 (Secure Hashing Algorithm))
- Secured Socket Layer – SSL communication
- Multi-factor Authentication

## Authorization and Access Control

### Challenge

After authentication, the application must determine the user role and what the user can access and modify. Access control determines where a user can connect from; what time they can connect, and the type of encryption required for communication. Application security strategy (e.g., Role-based Access Control or RBAC) must be built to protect the entire application access workflow. RBAC will ensure the use of roles, credentials, sensitivity labels, and entitlements.

### Checkpoints

- Least Privilege Model - If a user role will not be modifying data, then the role should not be given any opportunity to edit, delete, or add data to the database
- Secured access to critical resources
- No direct resource URLs
- OAUTH2 – Open Authorization
- Policy-Based Access Control

**Policy Administration Point (PAP)** – Point which manages policies

**Policy Decision Point (PDP)** – Point which evaluates and issues authorization decisions

**Policy Enforcement Point (PEP)** – Point which intercepts user’s access request to a resource and enforces PDP’s decision

**Policy Information Point (PIP)** – Point which provides external information to a PDP, such as LDAP attribute information

## Session management

### Challenge

A common vulnerability of web applications is caused by not protecting account credentials and session tokens. Types of session attacks are interception, prediction, brute-force, and fixation. In each attack, an unauthorized user can hijack a session and assume the valid user’s identity. Encrypting sessions is effective against interception; randomly assigned session ids protect against prediction; long key spaces render brute-force attack less successful, and forcing assignment and frequent regeneration of session ids make fixation less problematic.

### Checkpoints

- Session Management Strategy - Invalidate and cleanup session after use; complicated session generation; no sensitive data storage in session; no transient data storage in session
- Session Fixation Attack
- Partition for memory-to-memory sessions
- No implicit trust policy
- Monitoring session memory usage

## Input Sanitization and Validation

### Challenge

The attacker bypasses security mechanisms by adding malicious code to open parameters in an application. An open parameter could be a URL, QueryString, Header, Cookie, Form Field, or a Hidden Field. It is any parameter that does not assure that the data entered is data that would normally be expected. For example, if the parameter is a date field, and the input “injected” into it is a script file, then the attacker has been successful in finding and using an open parameter. Well-written code would discard the script.

### Checkpoints

- Constrain input – decide what is allowed in the field
- Validate data – type, length, format, and range
- Reject “known bad” input – do not rely only on this as it assumes the developer knows everything that could possibly be malicious
- Sanitize input – stripping a null; proper escaping; HTML or URL encoding to wrap data
- Input data canonicalization

## Cross-site Scripting Tool (XSS)

### Challenge

Cross-Site Scripting takes advantage of a “violation of trust” between a user accessing a known and trusted site and an attacker. When a web application creates output from user input without validating the data, the output can include malicious code. An attacker looks for instances in code where there is no validation and inserts the attack at that point. The output that the user receives may well carry malicious code. The user may receive a "click here" message, and trusting the "known site," abide by the hacker’s desire. The result is directed at the end user rather than the application's infrastructure. This could transmit confidential data to an outside site. It can result in program installations or disclosure of end user files.

### Checkpoints

- All code must be reviewed for input variables that result in output and have no validation included
- Configure the acceptable data lists to validate all headers, cookies, query strings, form fields, and hidden fields accepting inputs
- List of acceptable values for each field. Replace below characters respectively to defeat XSS

Replace < with &lt
Replace > with &gt
Replace ( with &#40
Replace ) with &#41
Replace # with &#35
Replace & with &#38

## Command Injection Flaws

### Challenge

Command injection flaws allow attackers to relay malicious code through the web application to another system. The malicious code can include whole scripts. SQL injection is the most prevalent. SQL injection attacks specifically to a parameter that passes through to a SQL database allowing an attacker to modify, erase, copy, or corrupt an entire database. SQL Injections can take the following forms: Authorization (Authentication), Select Statement, Insert, and SQL Server Stored Procedures.

### Checkpoints

- Examine/review all parameters for calls to external sources
- Build filters that verify only expected data is included
- Prepend and append a quote to all user inputs
- Restricted user access to required SQL stored procedures
- No shell commands or system calls in the code
- Block and timeout session for unacceptable data

## Buffer Overflows

### Challenge

The buffer overflow attack involves sending large amounts of data that exceed the quantities expected by the application within a given field. Such attacks cause the application to abandon its normal behavior and begin executing commands on behalf of the attacker. Attackers find buffer overflow vulnerabilities by searching for system calls and functions that do not restrict the length and type of input. This can be done manually or electronically with a code inspection tool. The attacker can also run a brute force attack against the program to find vulnerabilities in the code. Once the attacker finds a vulnerability, custom code is inserted that does not crash the system, rather instructs it to execute other commands or programs as the attackers desire.

### Checkpoints

- Examine/review all the code that accepts input from users via an HTTP request
- Log and reject the inappropriate data
- Constrain the text allowed limit and specific data type in free form fields
- Routine check the code to ensure secured design

## Error / Exception Handling

### Challenge

Errors / Exceptions are inevitable. Errors can be caused by user, programs, or perhaps they are errors between two systems. During development and testing an effort is made to identify all potential errors and appropriate error messages are developed for the end user. There will also be errors that are unanticipated. The application must have protocols for these errors as well. Left “unhandled,” the administrator will have no idea that an error has occurred. The procedure for handling the unanticipated needs to include what the error was, when it occurred, and where it occurred.

### Checkpoints

- Determine appropriate error/exception to be logged and triggered
- Log unhandled exceptions including time, date, user id, error code
- Handle complete list of exceptions/errors
- Error due to program failure, fail close the system; block unauthorized user access for OS

## File I/O

### Challenge

Handling input/output is one of the most common situations for the developers. If a developer is writing a real world application, they have to write a considerable amount of code to handle I/O regardless of the programming language or platform they’re going to use. Challenges are listed below:
- Reflected file download
- Native interoperability code which doesn’t validate its parameters enough
- De-serialize from untrusted data; Instantiating arbitrary types when no whitelist
- Any form of file IO, especially file data stream penetration
- Dynamic loading of file libraries; File path injection

### Checkpoints

- Validate file extensions
- No file storage in web directory
- Whitelist Approach (Access Control List)
- Reject file path injection
- Auto-close file I/O streams
- Logger for all I/O operations

## Logging

### Challenge

Logging is crucial to an organization’s ability to track unauthorized access and to determine if any access attempt was successful. Logs provide individual accountability. They are vital to reconstruction of events leading to a program failure. Log as much as possible. Logs are often required in any legal proceedings.

### Checkpoints

- Synchronize app server and syslog server to a time server
- Log entry (date and time, Initiating process/program/function, process owner, description)
- Authentication and Authorization events
- Administrative activities
- Modification of data characteristics (permissions, location, data type)
- Log file archive
- Restricted log file access

## Concurrency

### Challenge

Accessing the shared resource is a major issue with concurrency operations. Some common challenges are:
- Insecure parallel access to shared resource
- Shared resource locks
- Race condition
- Spurious functional wake up in loop
- Thread safety and liveness
- Recursive mutex

### Checkpoints

- Use mutex objects to provide safe and secure access to shared resource
- Protect mutex instances/objects
- Release locks on exceptional conditions
- Insert a non-bit-field between any two-bit-fields to prevent data races
- Predefined order to avoid deadlock
- Wrap functions to prevent spurious wake up in loop
- Consistent object states to ensure Thread-safety
- Uninterrupted execution to ensure Liveness
- No lock for non-recursive mutex

## Cryptography

### Challenge

Confidential or critical data should be encrypted. Assure that all elements of encryption are securely stored. Encryption schemas' should be developed by a commercial company rather than developed internally. Encryption is fairly easy to add to an application, but it is often not done correctly. Some common challenges are:
- Insecure storage of keys, certificates, and passwords
- Improper storage of secrets in memory
- Poor source of randomness
- Poor choice of algorithm or internally developed algorithms
- Failure to encrypt critical data
- Attempt to invent a new encryption algorithm
- Failure to include support for encryption key changes and maintenance procedures

### Checkpoints

- Encryption schemes to critical and vulnerable information
- Examine how keys, passwords, secrets are stored, processed, loaded, and cleared from memory
- High quality cryptographic algorithms (Triple DES, RSA, Blowfish, Twofish, AES)
- High degree algorithm randomness

## Remote Administration

### Challenge

Remote administration is restricted in a secured environment. As this is not often possible, it is necessary to design a secure system for remote connections to the server.

### Checkpoints

- Strict administrative approach
- Access Control List (Whitelisting)
- VPN, Auth Tokens, Certificates
- IP Filtering

## Web Application and Server Configuration

Challenge : Out of the box, servers are loaded with vulnerabilities. They must be patched before web services are installed. All default settings should be reviewed; and unnecessary services deleted or disabled.

### Checkpoints

- Restrict directory traversal by configuring server disks to separate OS and web server
- Least privilege mode for access provisioning
- No default admin/user accounts; no guest accounts
- Least interpretable error messages
- No self-signed SSL certificates
- Secure all server communication ports
- Routine security maintenance

## Vulnerabilities

### Challenge

A vulnerability is a weakness which allows an attacker to reduce a system's information assurance. Vulnerability is the intersection of three elements: a system susceptibility or flaw, attacker’s access to the flaw, and attacker’s capability to exploit the flaw. Some common challenges for software trends:
- No Bloated Operating Systems - Deep OS integration runs counter to principle of compartmentalization; Attackers can compromise the OS as a side effect of exploiting a deeply integrated app.
- Evolution of Components and Objects - The art of composing components into a coherent system while maintaining security is extremely difficult and poorly understood, making component-based software subject to exploitation. Widely shared but broken components are a big problem.
- Rise of Mobile Code - The wide range of technology for VMs (tiny processors / complicated app servers) does not fit all from a security perspective. Much to improve on determining the reasonable security mechanism for resource constrained devices.
- Normalized Distributed Computing - Complexity is the friend of software exploit. Distributed systems often make the job of exploiting software easier.
- Proliferation of Embedded Systems - Although PDAs (Personal Digital Assistant) haven't been greatly exploited, their glaring lack of software security make them prime targets for future attacks.
- Wireless Networks (Mass adoption) - Wireless networking has a pervasive, negative impact on security because it breaks down physical barriers. The breaking of the WEP encryption algorithm and the reemergence of ARP cache-poisoning attacks are some examples.
- Change in Payment Models - Protecting digital content is an unsolved and technically unsolvable problem. Despite the availability of digital rights management solutions, software exploits allow attackers to steal intellectual property with impunity.

### Checkpoints
- Fire Brigade Approach (Incidental response to exploits)
- Policy-Based Access Control for critical/high risk assets
- Prioritize threat / risk (fix the highest risk)
- Centralized risk analysis
- Vulnerability metrics
- Least Privilege Model to reduce surface attack

## Conclusion
Vulnerabilities in open source are particularly attractive to attackers. The ubiquity of the affected components provides a target-rich environment; the vulnerabilities are publicly disclosed (often with sample exploits); and without a traditional support model, users are typically unaware of new updates and vulnerabilities. It’s obvious that it is impossible to defend against a threat that you don’t know exists.
