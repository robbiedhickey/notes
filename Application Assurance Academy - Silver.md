## Fundamentals of Application Security

Reasons to be concerned about security:

* Loss of revenue
* Damage to brand or corporate image
* Regulatory compliance
* Customer security requirements
* Liability for security breaches resulting from defective software
* Overall risk for your business
* Competitive pressures

#### Defining Threat Terminology

* Asset - resource of value. can very by perspective
* Threat - undesired event that has potential to cause harm. Could compromise asset. May or may not be malicious
* Vulnerability - weakness in a system or security control that could be used to harm an asset. 
* Attack - a malicious action performed on an asset utilizing a(n) vulnerability to realize a threat
* Countermeasure - addresses a vulnerability to counteract a risk. 
* Security Control - process or policy put in place to reduce threats to an acceptable level

#### How are Threats Evaluated? STRIDE

* Spoofing
* Tampering
* Repudiation - ability of users to deny that they performed an action
* Information Disclosure - unwanted exposure of private data
* Denial of Service
* Elevation of Privilege

#### Risk Management Techniques
s
* Reduce
* Transfer
* Avoid
* Accept

#### Understanding the Attacker

They are categorized by: 

* Skillset
	* Infiltration potential, damage potential
	* Script Kiddies
		* young and inexperienced, low infil bug high dmg
	* Hackers
		* curiousity. med infil, low dmg
	* Crackers/Cyber warfare operatives
		* most malicious and dangerous
* Motivation
	* White hats
		* break systems for betterment of security
	* Black hats
		* exploit software for personal gain
	* Gray hats
		* may bounce between
* Origin
	* Internal 
		* attacks of choice, leverage insider knowledge, highly targeted
	* External

### Challenging Security Assumptions

#### Misconceptions

* The OS takes care of security for me
	* only 15% of reported vulns are OS vulns
* I am covered by a good patch strategy
	* often vuln during the window of vulnerability, time between vuln been reported and vuln patched. 
* The firewall, anti-virus, and IDS on the network will do enough to keep me safe
	* 92% are in software not in network
* My team will find security vulns during functional testing
	* just doesn't happen
* I'm using Java or .NET, therefore, I'm secure.
	* not a cure-all, XSS, SQL Injection, all still relevant
* I use crypto, therefore, I'm secure.
	* man in the middle, etc. 

#### The changing attack profile

Attacks are now 70% at the application level, not system or network.

#### Firewall Limitations

Firewalls developed with two things in mind: access control and protocol integrity. They are just a basic defense. 

* Cannot reach into data files to recognize if something is malicious
* HTTP traffic is perceived as friendly but can harbor many types of attacks
	* allows attackers to pass through firewall
* Assume that apps are free of vulnerabilities

#### Client-side security

* Client-side security is an oxymoron
* can act as usability aid, but should never be relied on for security
* can be circumvented, reverse-engineered and manipulated

#### Functional Testing vs Security Testing

* Security Testing involves verifying that application does not do what it is not supposed to
* it includes applying inputs and verifying that no bad things occur
* security testers ask "what is the software not supposed to do?"

#### Security Tools

##### Static Analysis Tools

Will not ensure your software is secure. Will only identify some vulns. Cannot find design-level flaws or authorization errors. Cannot account for external mitigating controls. False positives!

##### Dynamic Analysis Tools

Discover vulns that can only happen during runtime. Cannot find business logic problems. 

##### Application Vulnerability Scanners

Check against known attack vectors, find some. They only scan web apps, they return false positives. 

#### Internal Threats

29% of attacks are conducted by insiders (both intentional and unintentional). 

### OWASP - Open Web Application Security Project

Top 10:

1. Injection
2. Cross-Site Scripting (XSS)
	1. Persistent
	2. Reflective (non-persistent)
	3. DOM-based
3. Broken Authentication and Session Management
	1. Session Hijacking
4. Insecure Direct Object Reference
	1. Directory traversals
		1. may attempt to escape app directory to access root, parent directory, or symmbolic links
5. Cross-Site Request Forgery (CSRF)
	1. append challenge tokens to prevent this
6. Security Misconfiguration
	1. missing patches
	2. misconfigured or disabled security features
	3. default accounts
	4. unnecessary/unused services or features
7. Insecure Cryptographic Storage
	1. poorly implemented cryptography
8. Failure to Restrict URL Access
	1. bypass website security by accessing files directly instead of following links
		2. do not allow anonymous read access. define ACL for what types should be viewable. log files should not be accessible, etc
9. Insufficient Transport Layer Protection
	1. Not using SSL/TLS/IPSec
10. Unvalidated Redirects and Forwards
	1. results in bypassed authorization checks. Must validate referrer header. 
	2. use the table indirection technique. Effectively a map of integers to locations, that way you don't include the redirect URL as a query string param. 

Threats from Earlier OWASP:

1. Malicious File Execution
	1. Malware
	2. never expose file identifiers to user input
	3. never use unfiltered user input to craft a filename
	4. Don't allow user input to create server-side script or include files
2. Information Leakage and Improper Error Handling
	1. Can provide information that can be used to attack the system. Don't identify platform or implementation details

### Security Principles

Security is a process and is expensive to retrofit. Incorporate it early, it will improve security and be cheaper. Security is not a feature or add-on. 

#### Layered Security / Defense in depth

Using several solutions at multiple levels. Makes it more difficult and time consuming for an attacker. 

#### Segmentation

* Segment data from logic
* Segment data by privilege
* Segment application from environment

#### Structural Security

Foundation of an application's architecture. Ex a hardened server with unused features and services removed. 

#### Principle of Least Privilege

User should be assigned fewest privileges consistent with their assigned duties and functions

#### Default Deny

All access should be denied unless explicitly permitted. It is easier to use a white list than to maintain a blacklist. Default to a deny stance

#### Input / Data Validation

Can lead to system crashes, DB manipulation/corruption. Do not assume that another layer or module will sanitize data. Do not trust input data. 

#### Test Everything

It is important to test all code, these things are easy to overlook. 

### Security Goals and Controls

#### Error and Exception Handling Considerations

Do not volunteer too much information. 

#### Monitoring / Logging

Recording of application events for later auditing. Also leverage an IDS. Note that logs themselves may be a target of an attacker. 

### Security in the SDLC


## SDLC Gap Analysis & Remediation

### Security Engineering Overview

* Reduces the likelihood of vulnerabilities in design and code
* Requires executive commitment
* Requires ongoing process improvement
* Requires education and training
* Requires tools and automation
* Requires incentives and consequences

Must identify the right checkpoints in your process. 

#### What it entails?

* Integrating security into dev life cycle
	* apply upfront security design, secure coding practices, and testing for security throughout your dev life cycle
* Identifying your objectives
* Knowing your threats
* Using an iterative approach. 

### GAP Analysis and Remediation Planning

1. Identify Goals and Objectives
	* ex. ISO standards, Microsoft SDL, etc.
2. Assess the Existing Development Process
3. Create a Roadmap
4. Implement the Roadmap

#### Identifying Goals and Gaps

Drivers for process improvement

* Regulatory compliance
* Customer requirements
* Standards compliance
* Risk reduction

You must define measurable goals. 

* Create checkpoints and track progress over time
* Justify investment in improving your development life cycle
* Determine if investment should be increased or reduced

#### Assessing the Existing Process

The key areas that you should consider when assessing your existing development process include:

* Security Training
* Security Policies
* Organizational Capabilities
* Security Activities

##### SDLC Process Assessment

1. Review Org Structure and Team Roles
2. Analyze Policies and Standards Requirements
3. Analyze and Aggregate Data
4. Refine via Focused Interviews (usually team leads)
5. Create Gap Analysis Report with Recommendations

##### Security Activities in Requirements and Design phase

* Developing security requirements objectives
* Use of design best practices
* Threat modeling
* Security architecture and design reviews

##### Security Activities in the Implementation Phase

* Use of development best practices
* Security code reviews

##### Security Activities in the Verification Phase

* Abuse case definition
* Penetration testing

##### Security Activities in the Release and Response Phase
 
* Security deployment review
* Security attack response
* Patching processes

#### Creating a Remediation Roadmap

* Use your goals and key risks to analyze the results of your analysis and prioritize the areas most in need of augmentation based on practical and proven IT risk and cost/benefit considerations.
* Conduct a stakeholder strategy and planning workshop to review the major software risk management strategies (avoid, transfer, accept, and remediate) and attach the appropriate control options to each identified threat or risk category.
* Create your software risk remediation roadmap, which will be the basis of specific subsequent security improvement initiatives.

##### Recommendations: Technical, Policy Change and Training

**Technical**:

* Update IDE to the latest version. (additional cost)
* Use Visual Studio Code Analysis. (free)
* Use compiler options to improve security. (free)
* Deploy Fortify for static analysis. (additional cost)
* Deploy PC Lint for static analysis. (free)
* Improve access control and monitoring for source code access. (free)

**Policy Change**:

* Each application development team should appoint a security champion or representative.
* The security champion will be responsible for driving security and ensuring compliance with application security best practices within the team and when interacting with other teams.
* The CSO will call regular meetings to discuss security issues encountered by each team and review issues that have been logged during the SDLC.
* Each team will start to analyze security statistics such as:
	* The number of security issues handled.
	* The number of times the Incident Response Plan has been used.
	* How issues have been resolved.

**Training**:

* Security awareness training for all staff
* Application security fundamentals training for development staff
* Architecture and risk analysis training for architects
* Creating secure code Java training for developers
* Penetration test training for the QA team

##### Creating an Activity Matrix

For example, some application might be in maintenance mode, so there is no need to Design Reviews, Defining objectives, etc. 

##### Creating a Training Matrix

Identifies which members of a team should take which courses, which are optional, etc. 

#### Implementing a Remediation Roadmap

Typical implementations include:

* Training courses that cover security design, development, and testing best practices; or specific tools
* Threat modeling conducted earlier in the SDLC
* More-frequent, iterative code reviews
* Rolling out your secure development best practices

Sequencing is critical:

* Explain the initiative to your team members
* Provide training
* Build a knowledge base of best practices, and develop checklists to use during reviews
* Work with security champions; develop them as mentors for intermediate and advanced topics that will be rolled out at later stages

### Resources

[TeamMentor](https://teammentor.net/teamMentor) - Secure development guidance system

Books: Hacking: The Art of Exploitation, The Art of Software Security Assessment, Security Engineering: A Guide to Building Dependable Distributed Systems, Writing Secure Code, 19 Deadly Sins of Software Security, The Security Development Lifecycle, How to Break Software Security, How to Break Web Software

[OWASP TeamMentor](http://owasp.teammentor.net/teamMentor) - includes top 10 security topics


## How to Create Application Security Design Requirements

The Application Security Management (ASM) model consists of three phases:

1. Panic scramble (15%) - realize we have to do something. Often occurs during compliance audit or data breach. Buy tools. 
2. Pit of Despair (15-60%) - realize they don't know how to use their new tools. Get discouraged. 
3. Security as Core Business Principle (25%) - they 'get' it. 

Most companies invest in tools before training, which is a common problem. 

Only 6% of companies have a SAST (Static Analysis Security Tool) integrated into the SDLC. 

Only 11% of organizations conducted both security awareness and technical appsec training. 

### Adopting a Secure SDLC

* Requirements and Analysis -> Security objectives
* Architecture and Design -> Design Guidelines for Security, Threat Modeling, A&D Review for Security
* Development -> Code Review for Security
* Testing -> Security Testing
* Deployment -> Deployment Review for Security

### Key Security Engineering Activities

* **Identify security objectives** to understand your key security issues and scenarios.
* **Apply security design guidelines** to avoid common security design mistakes and learn from past vulnerabilities.
* **Conduct security architecture and design reviews** to identify security problems that can have multiplier effect in later phases of development.
* **Create threat models** to identify threats, attacks, vulnerabilities, and countermeasures.
* **Perform security code reviews and penetration tests** to uncover vulnerabilities during development and in deployment.
* **Conduct security deployment reviews** to ensure that configuration and deployment problems are found before your application is placed into production.

### Defining Software Security During Requirements and Design

According to IDC, it is 15 times more expensive to fix a defect in the testing phase than in the design phase. 

#### Know Your Security Goals and Objectives

Use your security goals to:

* Filter the set of applicable design guidelines.
* Guide threat modeling.
* Scope and guide architecture and design reviews.
* Help set code review objectives.
* Guide security test planning and execution.
* Guide deployment reviews.

#### The Discovery Process

Use a question drive approach.

##### Tangible assets to protect

* Are there user accounts and passwords to protect?
* Is there confidential user information (such as credit card numbers) that needs to be protected?
* Is there sensitive intellectual property that needs to be protected?
* Can this system be used as a conduit to access other corporate assets that need to be protected?

##### Intangible assets to protect

* Are there corporate values that could be compromised by an attack on this system?
* Is there potential for an attack that may be embarrassing, although not otherwise damaging?

##### Compliance Requirements

* Are there corporate security policies that must be adhered to?
* Is there security legislation you must comply with?
* Is there privacy legislation you must comply with?
* Are there standards you must adhere to?
* Are there constraints forced upon you by your deployment environment?

##### Quality of Service Requirements

* Are there specific availability requirements you must meet?
* Are there specific performance requirements you must meet?

#### The Matrix Approach

Object/Subject role matrix. ex Admin can create a user, reader cannot.

### Applying Security Design Guidelines

#### Potential Problems due to Bad Design

* **Input/Data Validation** - Insertion of malicious strings in UI or public APIs. These attacks include command execution, cross-site scripting (XSS), SQL injection, and buffer overflows. Results can range from information disclosure to elevation of privilege and arbitrary code execution.
* **Authentication** - Identity spoofing, password cracking, elevation of privileges, and unauthorized access.
* **Authorization** - Access to confidential or restricted data, data tampering, and execution of unauthorized operations.
* **Configuration Management** - Unauthorized access to administration interfaces, ability to update configuration data, and unauthorized access to user accounts and account profiles.
* **Sensitive Data** - Confidential information disclosure and data tampering.
* **Cryptography** - Access to confidential data or account credentials, or both.
* **Exception Management** - Denial of service and disclosure of sensitive system-level details.
* **Auditing and Logging** - Failure to spot the signs of intrusion, inability to prove a user's actions, and difficulties in problem diagnosis.

#### Design Guidelines - Best Practices

* **Input/Data Validation** - Do not trust input; consider centralized input validation. Do not rely on client-side validation. Be careful with canonicalization issues. Constrain, reject, and sanitize input. Validate for type, length, format, and range.
* **Authentication** - Use strong passwords. Support password expiration periods and account disablement. Do not store credentials (use one-way hashes with salt). Encrypt communication channels to protect authentication tokens.
* **Authorization** - Use least-privileged accounts. Consider authorization granularity. Enforce separation of privileges. Restrict user access to system-level resources.
* **Configuration Management** - Use least privileged process and service accounts. Do not store credentials in clear text. Use strong authentication and authorization on administration interfaces. Do not use the Local Security Authority (LSA). Secure the communication channel for remote administration.
* **Sensitive Data** - Avoid storing secrets. Encrypt sensitive data over the wire. Secure the communication channel. Provide strong access controls for sensitive data stores.
* **Cryptography** - Do not develop your own. Use proven and tested platform features. Keep unencrypted data close to the algorithm. Use the right algorithm and key size. Avoid key management (use DPAPI). Cycle your keys periodically. Store keys in a restricted location.
* **Exception Management** - Use structured exception handling. Do not reveal sensitive application implementation details. Do not log private data such as passwords. Consider a centralized exception-management framework.
* **Auditing and Logging** - Identify malicious behavior. Know what good traffic looks like. Audit and log activity through all of the application tiers. Secure access to log files. Back up and regularly analyze log files.

### Threat Modeling

Important not to confuse threats with vulnerabilities. 

* **A Threat** is what an attacker might try to do to a protected resource in the system.
* **A Vulnerability** is a specific way that a threat is exploitable, based on an unmitigated attack path.

#### Benefits of Threat Modeling

Threat modeling helps you understand application risk, shape your application design to meet your security objectives, identify where more resources are required to reduce risks, and weigh security decisions against other design goals.

In addition, you can use threat modeling to identify specific considerations such as:

* Legal requirements: SOX, GLBA, HIPAA, SB 1386, and more on the horizon
* Safety requirements
* Contractual requirements or customer needs

#### Threat Modeling Steps

1. Identify Security Objectives
2. Application Overview - itemize important characteristics and actors
3. Decompose Application
4. Identify Threats
5. Identify Vulnerabilities

Progressively refine your Threat Model by repeatedly doing steps 2-5. 

Input | Step | Output
-----|-----|------
Business Requirements, Security Policies, Compliance Requirements | Step 1: Identify Security Objectives | Key Security Objectives
Deployment Diagrams, Use Cases, Functional Specs | Step 2: Create an Application Overview | Whiteboard-style diagram with end-to-end deployment scenario, key scenarios, roles, tech, application security mechanisms
Deployment diagrams, use cases, functional specifications, data flow diagrams | Step 3: decompose your app | trust boundaries, entry points, exit points, data flows
Common threats | Step 4: identify threats | threat list
Common Vulnerabilities | Step 5: identify vulnerabilities | vulnerability list

#### Mitigating your threats

* Redesign and eliminate
* Use a mitigation technique
	* attack tree, with mitigation actions and expected results
* Accept the risk in accordance with your organization’s risk tolerance

#### Threat Modeling - Best Practices

Best Practice | Description | Example
-------------|--------------|---------
Enumerate assets of value | Understand what assets matter to you as a business as well as what assets may matter to an attacker | site login credentials
Consider threats | For each asset, consider the potential threats that could impact the asset. Consider the confidentiality, integration and availability (CIA) for each asset. | Login credentials could be stolen or modified. The login service could be denied to legitimate users
Brainstorm attacks | For each threat, consider what attacks could realize the threat | Network sniffing, man-in-the-middle
Identify Conditions | Identify conditions under which attack could succeed | Clear-text login credentials sent over a public network. 

Threat Profiles can serve as a basis for security test planning. A threat profile includes:

* Assets of value
* Threats that could compromise those assets
* Attacks that could realize the threats
* Key conditions that must be met for each attack to be successful

Test plan should focus on testing the key attack conditions in order to prove or disprove threats to your system. This ensures that you are testing the areas where the difficulty of attack is lowest and the impact is highest.

### Performing Architecture and Design Reviews

The goals of performing an architecture and design review are to:

* Analyze application architecture and design from a security perspective.
* Expose high-risk design decisions that have been made.

#### Example Checklist

* All entry points and trust boundaries are identified by the design
* Input validation is applied whenever input is received from outside the current trust boundary
* The design assumes that user input is malicious
* Centralized input validation is used where appropriate
* The input validation strategy that the application adopted is modular and consistent
* The validation approach is to constrain, reject, and then sanitize input
* Data is validated for type, length, format, range
* The design addresses potential canonicalization issues
* Input file names and file paths are avoided where possible
* The design addresses potential SQL injection issues
* The design addresses potential cross site scripting issues
* The design does not rely on client-side validation
* The design applies defense in depth to the input validation strategy providing input validation across tiers. 

#### Techniques

Question driven approach is preferred on the following:

1. Review Deployment and Infrastructure
	1. Host, Network
2. Security Frame
	1. Input validation, authentication, authorization, etc
3. Layer-by-layer
	1. Presentation, Business, Data

### Reducing Software Risks

What is your application's attack surface? Entry points:

* User interfaces
* Web services
* Direct database access
* Network channels
* Pipes
* Files
* APIs

Goal is to reduce attack surface. A malicious user will:

* Identify entry points.
* Exploit the entry point or the backend exposed behind it.
* Try to deny legitimate users the access to these entry points.

Reducing the surface is not a matter of flipping a switch, principle of least privilege:

Higher Attack Surface | Lower Attack Surface
--------|---------
Executing by default | off by default
Open socket | closed socket
anonymous access | authenticated access
constantly on | intermittently on
administrative access | user access
internet access | local subnet access
system privileges | not system privileges
large codebase | small codebase
weak acl | strong acl


## How to Create an Application Security Threat Model

### Thread Modeling and the Security Development Life Cycle

**SDL (Security Development Lifecycle)**

* Complete the Threat Models
* File Bugs
* Document Mitigations
* Conduct Threat Model Reviews

**Threat Modeling Process**

1. Vision
2. Model
3. Identify Threats
4. Mitigate
5. Validate -> Model

**Who Threat Models**

* Developers - Involved during the Model, Mitigate, and Validate phases. 
* Testers - Involved during the Model, Identify Threats, Mitigate and Validate phases
* Program Managers - Involved throughout the process and typically drive it.
* Architects - Involved in the Vision phase where their background on the application domain helps identify general threats and issues.
* Security Advisors - Involved mainly during the Validate phase, but also participates during the Mitigate phase. 

**What to Threat Model**

All products and services should be threat modeled. The objective is to model the product as a whole, including:

* Security-relevant features.
* Privacy-related features.
* Features where a failure would result in a security concern.
* Features that interact with external or un-trusted processes.

**When to Threat Model**

Threat modeling provides the most value during the design phase.

* It is much harder to change direction in later phases.
* Threat modeling early gives a greater chance for eliminating threats by design and eases the process of the Final Security Review.
* Threat modeling late often requires awkward mitigations to be introduced and these mitigations must be tested and maintained, risking schedule slips and customer pain.

### Performing the Threat modeling steps

**The Vision Step**

* Defining Guarantees
* Developing Scenarios
* Identifying Security Features and Properties

**The Model Step**

* Create a Diagram
	* Common is the Data Flow Diagram
* Diagrams should include
	* Processes
	* Data Flows
	* Data Stores
	* External Entities
	* Trust Boundaries

![Data Flow Diagram](http://i.imgur.com/c5QFDFt.png)

**The Identify Threats Step**

The STRIDE model:

* Spoofing - Spoofing threats involve an adversary creating and exploiting confusion about the identity of someone or something
* Tampering - Tampering threats involve an adversary modifying data, usually as it flows across a network, in memory, or on disk.
* Repudiation - Repudiation threats involve an adversary who performs an action and then plausibly denies having performed the action. 
* Information Disclosure - Information disclosure involves data that can be accessed by someone who should not have access.
* Denial of Service - Denial of Service threats involve denying legitimate users access to a system or component.
* Elevation of Privilege - Elevation of Privilege threats involve a user or component being able to access data or programs for which they are not authorized.

Threat Trees:

![](http://i.imgur.com/v2HkVXU.png)

**The Mitigation Step**

* Redesign features
* Choose standard mitigations

Standard Mitigation Techniques:

* Spoofing
	* Basic/Digest/Cookie/Kerberos auth
	* PKI systems
	* IPSec
* Tampering
	* ACLs
	* Digital Signatures
* Repudiation
	* Security logging and auditing
	* Digital signatures
	* Secure time stamps
* Information Disclosure
	* verify encryption and ACLs
* Denial of Service
	* ACLs, Filtering, Quotas, Authorization, High availability design
* Elevation of privilege
	* ACLs, group or role membership, privilege ownership, permissions, input validation

Recognize when to accept risk:

* Critical
	* Remote elevation of privilege by anonymous users
	* Remote execution of arbitrary code by anonymous users
* Important
	* Remote DDOS that can be easily exploited by anon users
	* Remote elevation of privileges by authenticated users
	* Remote execution of arbitrary code by authenticated users
	* Information disclosure that allows attackers to obtain information from anywhere in the system
	* Spoofing a specific computer or user
* Moderate
	* Denial of service that requires extensive efforts by anonymous or authenticated users
	* Information disclosure that allows attackers to obtain information from known locations in the system.
	* Spoofing of a random computer or user.
	* Permanent modification of any user data or data used in making security decisions in a specific scenario.
* Low
	* Information disclosure that exposes random data to an attacker.
	* Temporary modification of data in a specific scenario.

**The Validate Step**

* Validating the Model
	* Will need to be updated for two reasons: product changes or knowledge about threat changes.
	* Design Change Request (DCR) Process
* Validating the Threats
* Validating the Mitigations
* Validating Dependencies and Assumptions

### Creating the Threat Model Document

**Required Sections:**

* Threat model information
	* The threat model information section contains general information about the document and the project. The information within this section should be high level and brief.
* Diagrams
	* Diagrams are the core of threat models. As visual aids, diagrams assist in understanding the product's architecture and identifying potential threats. 
* Threats and mitigations
	* The threats and mitigations section consists of a series of tables with one threat and mitigation per table
* Dependencies
	* The goal of the dependencies section is to enumerate your product's dependencies, explain why they are necessary, and to provide any security notes that were produced while selecting them.
* Assumptions
	* The purpose of the assumptions section is to identify any security assumptions that have been made while gathering requirements and creating diagrams. Keeping track of security assumptions provides additional information for determining whether a threat has been successfully mitigated.
* External security notes
	* External security notes are necessary for customers to know how to use a product safely. Use this field to record anything you think will help any external evaluator to understand the threat models. 

**Optional Sections:**

* Scenarios
	* The scenarios section is an optional place to enumerate important security scenarios. 
* Protected resources or assets
	* There are two ways in which this optional section can be used. Protected resources are aspects of the system whose protection is critical, such as the SAM or the TPM keys. Assets are things which an attacker has reason to steal or deny access to, such as a customer database, source code of your application, or network connectivity.

**Threat Modeling Tools**

* Microsoft SDL Threat Modeling Tool
	* http://msdn.microsoft.com/en-us/security/dd206731.aspx
	* Suitable for product groups, the Threat Modeling Tool aids teams in the creation of their products' threat models. It organizes relevant entry points, assets, trust levels, DFDs, threats, threat trees, and vulnerabilities into an easy-to-use tree-based view. Threats identified through this tool are enumerated using STRIDE.
* Microsoft Threat Analysis and Modeling Tools
	* http://www.microsoft.com/en-us/download/details.aspx?id=14719
	* Targeted at teams who do not have security experts on hand, The Microsoft Application Security Threat Analysis & Modeling tool provides a three-phase process to threat model an application:
		* Define the application context.
		* Model threats on top of the application context.
		* Measure the risk that is associated with each threat. 


## Fundamentals of Secure Architecture

### State of the Industry

**Quality Triangle - Software Quality has three aspects:**

* Feature Richness
* Time to market (TTM)
* Total cost of ownership (TCO)

### Learn from Past Mistakes

Keep in mind that security training is never complete:

* You should hold refresher courses often.
* Attend conferences and trade shows.
* Don’t forget to train your new hires and interns.
* Designate a security champion.

### Information Security Tenets

**CIA Triad**

* Confidentiality
	* Privacy of an asset
	* Achieved by limiting information access to authorized users
	* Requires:
		* Authentication and Access Controls
	* Privacy = Confidentiality + Anonymity
	* Graham-Leach Bliley addresses this 
* Integrity
	* Data and system can only be modified by authorized users
	* Achieved by ensuring the trustworthiness of resources and authenticating entities and data origin points
	* Sarbanes-Oxley (SOX) addresses this
* Availability
	* Reliable access to system resources
	* Requires:
		* Resistance to DOS attacks
		* Component (hardware and software) redunduancy
		* Fault-tolerant and failure-tolerant software

**Achieving Security Goals**

Confidentiality is primarily achieved by encryption and access control mechanisms:

* Limiting data to intended users
* Protecting data in transit

Integrity is often achieved by *digital signatures*:

* They assure that messages/documents remain untampered with throughout their lifetime.
* As a byproduct, non-repudiation is an added bonus.

The price of availability is eternal vigilance through passive and proactive protection of information assets.

## OWASP Top 10 - Threats and Mitigations

### Threat 1: Injection

In effect, it is user inputs getting interpreted as commands rather than data. 

They are high risk because they require little effort to learn, discover and execute. They also expose the most sensitive parts of the web application. Developers often fail to defend against these attacks. 

SQL Injection is by far the most common injection attack. It is a serious threat because tool automation is now prevalent. 

**Advanced SQL Injection**

* Unions - By using the union operator, an attacker can pull data from other tables and execute multiple SQL statements
* Errors - By intentionally producing errors, an attacker can discover tables and field types or gather other valuable information.
* Stored Procedures - Many sp's are available (including sys procs) if not properly secured
* Timing - even if the web app does not return data, attackers can create delays based on conditions to be tested. 

**Prevention**

Stored Procedures, Parameterization and input validation. 

**Additional Mitigation Techniques:**

* Blacklist validation
* Never return raw error messages to browser
* Perform all queries with minimal privileges necessary
* Harden OS
* don't use SELECT *
* Use limit clause in SQL statements that return fixed number of results
* Use consistent character sets and be explicit
* Normalize file paths and check if it is in bounds of app

### Threat 2: Broken Authentication and Session Management

Attacker exploits flaws to impersonate users.

Usually result of:

* Poor session control and isolation
* Weak password recovery and account management functions
* Inadequately secured transmission or storage of user credentials

Some examples:

* Forgotten password functionality
	* can identify valid user names
* Relying on an IP address for a session
* Having inadequate timeouts for inactive sessions


### Threat 3: Cross-Site Scripting (XSS)

XSS is a form of injection attack. Can occur when an app accepts user input without validation, and uses the input to generate output. Allows attacker to exploit other users. 

**An attacker can use XSS to:**

* Access and modify the structure, appearance and behavior of browser content.
* Redirect the user to a site that contains malicious content
* Execute client-side scripts, exploiting the users' trust of your web app
* Perform actions on your site on behalf of the client
* Access sensitive session and cookie information
* Access private and sensitive client information
* Spy on all actions the client performs

**Stored XSS vs Reflected XSS:**

In a stored XSS attack, also known as a persistent attack, an attacker sends malicious input that is stored in the application’s database. The malicious input is then displayed as a normal part of the site’s content.

In a reflected XSS attack, also known as a non-persistent attack, the attacker gets the user to send malicious input to a particular URL—for example, by sending the user email with a link to click. By clicking the link, the user sends the malicious input to the web application. The application then uses that input to generate a response.

**Preventing XSS:**

* Input Validation
	* Whitelist validation, only allow known, approved inputs
	* Blacklist validation, block data matching specific set of malicious code
* Output sanitizing
	* Escape all untrusted data using the method most appropriate for the type of output
	* Explicitly define character encoding and output mime types

### Threat 4: Insecure Direct Object References

An application directly refers to an object such as a filename, user account or database record. Even with extensive input validation, this can potentially allow attackers to predict or guess other objects that they may not be authorized to access, leading to exposure of sensitive system and user information.

When designing a web application, it is easy to mistakenly assume that users cannot access content that they cannot see. Failure to control access to this content is an (often unintentional) attempt at security through obscurity.

The Insecure Direct Object References threat reflects flaws in system design where access to sensitive data or assets is not fully protected, and data objects are exposed by the application. This exposure occurs when developers assume that users always adhere to the basic rules of the application.

**Identifying IDOR**

Difficult to identify with automated tools. 

To protect against the threat:

* Perform a code review
* Do a walkthrough of the website
* Verify that the object is not directly accessible based on common information

Common implementation areas where these objects could be exposed include URLs and links, hidden variables, drop-down list boxes and JS Arrays.

**Exploiting IDOR**

1. Identify a potentially exploitable direct object reference
2. Experiment with how to exploit the flaw and bypass any input validation that might exist
3. Predict or guess other object names to access

**Preventing IDOR**

Can get around this by using indirect object references for server side objects/files/data, need to maintain a hash table per-session. For example, instead of using the name of a command line utility directly, you would use an obscure hash. 

This isn't enough, must also verify user authorization.

### Threat 5: Security Misconfiguration

Although an application’s code is vital to its security, the platform it runs on is also very important. The misconfiguration of security-related settings within operating systems, web services, and databases contribute to your application’s attack surface.

**Defending the OS**

* Minimalist installs
* Strictly limit user accounts and disable or rename default accounts
* Establish strong password policies
* Use a packet filter or firewall to restrict access and isolate the machine on the network. 
* Frequently patch system and applications
* Set file and directory permissions to least privilege required
* Review OS settings that can improve system security
* Proper system auditing and log file management
* Avoid installing SDK and debugging tools
* Install anti-virus and other security software as appropriate
* Consider using a hardening guide
* Ensure server is physically secure. 

**Defending the Web Server**

* Install only the modules or services necessary for your application.
* Use appropriate file and directory permissions to strictly control access to web content directories.
* Disable directory browsing.
* Review web server settings that can improve platform security.
* Remove default, demo, backup, temporary, and other directories not appropriate for a production server.
* Remove, rename, or restrict IP address access to administrative directories.
* Disable or reconfigure error reporting features so that users never see detailed error messages.
* Disable or block HTTP methods not needed for your application.
* Modify server headers to not reveal server platform and version.
* Review script interpreter and application framework settings to ensure that proper limits and security settings are in place.
* Consider using a hardening guide or tool appropriate for your web server and application framework.
* Ensure that the server is physically secure.

**Defening the Database**

* Remove or disable unnecessary database features or services.
* Strictly limit user accounts and disable or rename default accounts.
* Use a packet filter or firewall to tightly restrict access to database ports.
* Remove any demo, testing, training, and all other databases not necessary for the web application.
* Carefully configure user roles and permissions to strictly limit access for web application accounts. Never use DBA, root, or system accounts for general database access.
* Consider using a hardening guide or tool appropriate for your database platform.
* Disable stored procedures that are not required for the application.
* Ensure that the server is physically secure.

**Defense in depth: Other Strategies**

* Regularly audit the full system configuration.
* Use software to perform regular vulnerability scanning of the web server.
* Where possible, manage system configuration settings with version control software.
* Deploy intrusion detection systems to identify any overlooked misconfiguration.
* Monitor search engines to identify changes made to your web application and identify possible information leaks.
* Utilize log analysis or event management software to identify unusual system activity.

### Threat 6: Sensitive Data Exposure

Comprised of two steps:

1. Insecure Cryptographic Storage
	* Use symmetric encryption algorithms such as AES to securely store data. Requires a specific key or password to retrieve the data.
	* Useing one-way hashing algorithms such as 
	* 
	* -256 to store hashes used to verify user passwords
2. Insufficient Transport Layer Protection

**Common Cryptographic Storage Mistakes**

* Failure to encrypt sensitive data.
* Using homegrown algorithms.
* Using weak, out-of-date algorithms.
* Using insufficient key lengths.
* Using weak random number generation.
* Failure to use salt with password hashes.
* Poor key management.

**Selecting Cryptographic Algorithms**

Appropriate algorithms and minimum key lengths change over time and may differ. You should always be aware of the minimum acceptable standards and stay well ahead of them.

SHA1 and MD5 are ALWAYS inadequate and should be phased out.

**Generating Random Numbers**

* Algorithm based RNGs
	* Seemingly random, but given a starting point (the seed), the algorithm always produces the same numbers in the same sequence. 
* Weak RNGs
	* Use factors such as current time to make seed less predictable. Attacker can often narrow time variable down enough to determine the seed
* Strong RNGs
	* Use both internal and external data. Internal is key here (free memory, mouse movement, etc). Allows RNG to simulate randomness to a greater degree. 

**Increasing security**

Use salts when generating password hashes, regardless of Cryptographic algorithm in use. 

Key derivation functions increase the size and entropy of a password before hashing to accomplish the following:

* Hinder brute-force attacks by increasing the cost in both CPU cycles and memory overhead.
* Increase the bit length and entropy of short passwords.
* Reduce exposure to crypto-analytic and timing attacks by working from keys of standard length and high entropy.
* Add additional salt and optionally a master key or password.

**Key Management Best Practices**

* Do not hard-code encryption keys in your application
* Plan file system permissions carefully to protect files that contain encryption keys
* Store encryption keys outside of the web content directories if possible
* Build the application to support periodic key changes and establish regular schedules for changing keys
* Do not include encryption keys in backups.

**Insufficient Transport Layer Protection**

Use SSL and TLS. Both provide a layer of security across an insecure network. 

You may not be providing complete transport layer protection if:

1. HTTPS is not used to protect all areas of your website
2. You include non-https resources on https pages
3. Weak Cryptographic algorithms are used. 
4. Session cookies are not properly marked as secure
5. Web server's SSL certificate is not valid for any reason

**Best practices for Transport Layer Protection**

* Use TLS on the initial login page
* Once a user is authenticated, only allow TLS on all pages
* Use TLS even for internal network communications
* Never mix encrypted and non-encrypted content on the same page
* Use the secure flag on all authentication cookies
* Never pass sensitive information or session tokens in URL
* Use cache-control headers to prevent persistent caching of sensitive content
* use HTTPS
* Disable support for TLS re-negotiations
* Do not use cipher suites with a symmetric encryption algorithm smaller than 128 bits
* For maximum security, use RSA or DSA authentication with Diffie-Hellman key agreement
* Use vulnerability scanners to identify and assess all TLS implementations

**Best practices for handling SSL/TLS certificates**

* Never use self-signed certificates because they provide little security over non-encrypted communications and provide a false sense of security.
* Do not use X.509 certificates with an RSA or DSA key less than 2048 bits.
* Do not use X.509 certificates signed using MD5 hash, due to known collision attacks.
* Store private keys in a secure location, never on the server itself.
* Keep certificates up-to-date and include all applicable domain names so that the user is never presented with a certificate error.
* Use extended validation (EV) certificates if appropriate for your organization.

### Threat 7: Missing Function Level Access Control

Most applications have at least some requirements for restricting access to sensitive application functions. Insufficient access controls could allow attackers to access administrative features, AJAX endpoints, or other unprotected files in your web content directories. Examples of attacks might include path traversal, directory browsing, file and directory enumeration, and unauthorized file access.

The greatest risk of these vulnerabilities is exposure of sensitive user data or information that could facilitate other attacks.

Common examples of failing to restrict URL access include failure to:

* Prevent directories from listing contents when no default document exists.
* Authenticate AJAX or other API requests properly.
* Restrict access to administrative and maintenance features.
* Protect log files, backups, test files, hidden files, or temporary files.
* Map file extensions to mime types to control how they are handled.
* Check for directory traversal when accessing files based on user input.
* Restrict access to folders on FTP servers.

**URL Authorization Strategies**

* Clearly separate public, private, and administrative content by placing them in separate physical directories.
* Deny access to all protected pages and sensitive functions by default. Then, check user permissions to explicitly grant access.
* Consider role-based security to define clear boundaries and check user roles before allowing access to functions, data, files, URLs, and services.
* Supplement security boundaries through user-based access control and file system permissions.
* Centralize authorization functions and policy management. Avoid hard-coded policy enforcement in individual modules.
* When making authorization decisions based on workflow or other state, always consider unexpected state conditions that might occur.
* Always make authorization decisions on the server side, never in client-side code.

**File and Directory Enumeration**

It is important to identify unprotected and potentially vulnerable files by:

* Manually reviewing web content directories for unneeded, sensitive, or out-of-date files.
* Running automated vulnerability scanners on your web server to identify vulnerabilities that an attacker might also be able to find.
* Reviewing web logs and web log statistics to identify possible abuse of unidentified vulnerabilities.
* Employing a web application firewall or intrusion detection system to block or at least identify unknown vulnerabilities.

### Threat 8: Cross-Site Request Forgery (CSRF)

A technique that uses XSS, browser flaws, social engineering and other methods to cause a user to perform an undesired action in their current authenticated user context.

Rather than steal your password, attackers use CSRF to get you to take action for them. 

**Preventing CSRF**

* Using **double-submitted cookies** is a simple technique in which you include session tokens in hidden form fields. When a user submits a form, the session token in the form must batch the token in their cookie. Otherwise the form submission fails.
* **Per-request nonces** provide an even stronger safeguard by generating a random one-time token, or nonce, in every form sent to the user.

### Threat 9: Using Components with Known Vulnerabilities

Exploiting known flaws in components is particularly attractive to attackers because:

* Attackers generally learn about flaws at the same time as everyone else, giving them a window of attack before systems are updated or patched. Some sites will never get patched.
* Exploit code is often widely available shortly after a flaw is made public.
* With flaws in commonly used components, attackers can easily automate massive attacks and even compile lists of vulnerable sites through search engines.
* Open-source applications are easy for attackers to review and find unpublished vulnerabilities.
* Some open source projects, although popular, may not have the development resources or may be too new to have been fully reviewed for security flaws. (On the other hand, some open source projects receive significant public review, and therefore few flaws escape unnoticed.)

**Auditing External Components**

* Carefully consider the risks of using any third-party components. Only use those produced by reputable developers and that have received extensive security review.
* Maintain lists or repositories of components approved for use in your organization.
* Identify all third-party components in existing applications and their versions, including any child components.
* Subscribe to notifications for new and updated versions of all components you use and keep all components up-to-date.
* Use both manual and automated reviews to identify security flaws in any components you implement, whenever possible.
* Be aware of and document configuration and other implementation details necessary for a component’s security.
* Prevent your application from displaying names and version numbers of included components, if possible.

### Threat 10: Unvalidated Redirects and Forwards

User believes they are clicking a known or trusted URL, but may be redirected to a malicious site.

**Finding Vulnerable Components**

Components that redirect based on any user input, including less obvious input sources such as HTTP headers, cookie contents, hidden form fields, reverse DNS lookups, or other data that a user can modify.

* Perform deep and creative review of all code
	* File with keywords such as redirect, go and link
	* URL parameters such as URL, ReturnURL and redirect

**Redirect Exploits**

* Pages that include iframe or content pop-up windows based on user input
* Pages that grab and include remote content based on user input
* Login pages that redirect back to where a user was before logging in

**Protecting Redirects**

* Whitelists
* Lookup tables
* Intermediate Warning Pages
* Nonces and Message Authentication Codes (MACs)
* Application Firewalls and Intrusion Detection Systems

### OWASP 2007 Top Ten

**Old:**

* Malicious File Execution
	* Developers sometimes use or concatenate potentially hostile user input with file or stream functions
	* Use strict input validation and carefully planned file permissions
* Information Leaking and Improper Error Handling
	* Poorly configured apps can leak information about their configuration and structure.
	* Never reveal too much information if errors occur. 
	* Save debugging information in a separate internally accessible location

**New:**

* Security Misconfiguration

## Introduction to Security Tools and Technologies

### Virtual Private Network (VPN)

Private network constructed across a public network. Encrypts data before sending and decrypts on the receiving end. 

**Benefits**:

* Extended Geographic Connectivity
* Simplified Network Topology
* Lower operation cost compared to traditional WAN

**VPN Types:**

* Hardware-based
	* Usually use encrypting routers
	* Easy-to-use, nearest thing to "plug and play" encryption
	* Provides the highest throughput of all VPN systems.
	* Generally the most expensive
	* Think: Cisco, Checkpoint
* Firewall-based
	* Take advantage of the firewall's security mechanisms, including restricting access to the internal network
	* Perform network address translation
	* Satisfy requirements for strong authentication
	* Provide real-time alarms and extensive logging
* Software-based 
	* Idea when both endpoints of the VPN are not controlled by the same organization
	* Performance requirements should be modest
	* Disadvantages:
		* Generally harder to manage
		* Require familiarity with the host OS, the application and the appropriate security mechanisms
		* May require changes to routing tables and network addressing schemes
		* Since data is encrypted end-to-end, hardware-based content filtering will not work

### Firewalls

Attempt to secure the network by reducing the number of services that are exposed to the internet. Most services do not need to be publicly accessible. 

There are hardware and software based firewalls. Some routers include a firewall.

| Operating System | Firewall Examples|
|------------------|------------------|
| Unix | ipfw, ipchains, pf |
| HP-UX | ipfilter |
| Windows 7, OSX, Linux | Built-in Firewall |

Firewalls are usually deployed in two phases - an internet facing one with maximum restrictions and an internal one to protect the intranet from attacks:

Internet -> Firewall -> DMZ (Web server, Email server, Proxies, etc) -> Firewall -> Intranet

Packet-filtering Firewalls are used to filter packets based on a combination of packet features. For example, drop all packets with destination port 23 (telnet).

**Firewall Rules:**

Example using ipfw:

* /sbin/ipfw add deny tcp from cracker.evil.org to wolf.tambov.su telnet

**Firewall Utility:**

You can also restrict access based on inbound/outbund rules. For example, why would a web server telnet out? Can disable outbound telnet traffic. 

**Firewall Limitations:**

They allow most common traffic to pass through. You cannot just disable HTTP traffic which is a common attack vector.

Remember that a firewall:

* Is only the first line of perimeter defense
* Is NOT a complete security solution
* Cannot prevent:
	* Complexity mismanagement
	* Input validation attacks
	* SQL injection
	* HTTP session hijacking
	* Impersonation
	* Other app vulnerabilities

### Intrusion Detection Systems (IDS)

**IDS Types:**

* Network-based IDS 
	* can detect intrusions - ex. Network Flight Recorder, Snort
	* Listens to all traffic on segment, must reside on target net
	* Has throughput limitations
* Host-based IDS 
	* can detect as well as prevent intrusions - ex. AIDE, LIDS
	* resides on host, uses CPU and disk cycles
	* Proves real-time alerts
* Log-based IDS
	* Reviews syslog or SNMP
	* Not real-time, mostly a forensics tool
* Target Monitoring IDS
	* Watches the OS, lives on box, watches files
	* Scheduled runs, near real-time

An application-based IDS currently doesn't exist. eEye's SecureIIS might be a precursor, but has been proven flawed already. AZN-API is a useful new direction for authorization issues.

They limit anonymity but lessen privacy. They also limit the timeframe of attacks by alerting stakeholders about intrusions.

Generally IDS is located in the DMZ or behind the first firewall.

**Limitations:**

* They need to be monitored
* Problems "tuning" sensitivity:
	* too verbose? not sensitive enough? 
	* not enough eyes to monitor all systems?
* Crying wolf
	* False IDS emergencies can frustrate sysadmins, who may eventually ignore IDS alerts completely

### Cryptography

Cryptography is good for specific classes of security problems, but it may make some problems even worse such as:

* Denial-of-service attacks
* Distributed denial-of-service
* Command injection against web servers
* Viruses like Melissa
* Some attacks against DNS servers

**Goals of Cryptography:**

* Information Confidentiality
	* Ability to share confidential information over an insecure channel
* Information Integrity
	* Ability to verify that information has not been modified. 
* Authentication and Non-Repudiation
	* Ability to verify the identity of the author of information and to bind information to its author

**Cryptographic Algorithms**:

* Symmetric Key
	* Most common for transferring confidential data over the network. Examples of symmetric key algorithm include Triple DES and AES. Triple DES is outdated and no longer recommended; you should use AES.
	* Process:
		* The sender and the receiver share the same secret key (K)
		* The plain text message (m) is encrypted using the secret key to obtain the ciphertext K(m)
		* The ciphertext is decrypted using the secret key to recover the plain text message
                              K(K(m)) = m
	* Primary challenge is key distribution
		* Key Distribution Centers
		* Key exchange algorithms
		* Pre-sharing keys
* Public Key
	* Used for authentication and digital signatures; for example, RSA.
	* Process:
		* The sender encrypts the message using the receiver’s public key (KRed)
		* The receiver decrypts the ciphertext using his private key (KBlue): KBlue(KRed(m)) = m
	* Important to protect the private key
	* How to authenticate and authorize a public key?
		* Certificate Authority
			* A PKI is a system for a trusted third party to vouch for the authenticity of a public key.
			* A PKI includes one or more Certificate Authorities which issue certificates that bind public keys to particular individuals or organizations.
		* Digital Signatures
			* Advantage over symmetric key cryptography:
				* No risk to a shared private key
				* Management of public keys is easier and safer than symmetric private keys
			* However, it is computationally expensive to encrypt long messages
				* Solution: Compute the one-way hash (for example, MD5) value of the message and digitally sign it with the private key
				* This solves multiple problems: message integrity, authentication, and non-repudiation are guaranteed
* Hash algorithms
	* Used to verify the integrity of files and network payloads; for example, MD5 and SHA-1.

**RSA Asymmetric Encryption Algorithm**

* Asymmetric/public key encryption
* Encryption and digital signing
* Block cipher
* Variable-length key
* Hard to break because it is difficult to factor large numbers
* Various cryptographic mechanisms are derived from RSA

In RSA, you have a private key, which is a set of two separate numbers, and your public key is these two numbers multiplied together. You can multiply those two numbers together quickly and easily, and then, send that larger number―that’s your public key. It’s difficult to determine which two numbers are the factors of that larger number.

**Secret Key vs Public Key Cryptography**

|Secret Key | Public Key |
|-----------|------------|
|Requires parties to maintain a shared secret | Does not require parties to maintain a shared secret|
|Logistics of key exchange is a problem| No key exchange problems|
|Parties who mistrust each other may not way to have a shared secret key|No issue of trust since there is no shared secret|
|Computationally cheap|Computationally expensive|

**Cryptographic Hash Functions**

Function takes a long string (the message) and computes a smaller fixed-length string, which is the message digest.

A cryptographic hash function must have the following properties:

* Given the message digest, it should be computationally impossible to determine the message in a time period shorter than a brute-force mechanism.
* Given a message m1, it should be computationally unfeasible to find a message m2 such that both have the same message digest. (No collisions.)
* Small changes in the message should result in large differences in the message digest.

MD5 and SHA-1 are examples of hash functions

**Limitations:**

Weak encryption provides a false sense of security. Examples:

* Poorly seeded random number generators
* Weak Cryptographic algorithms, such as MD5
* Keys that are too short, for example, WEP 40-bit key

As good as it is, it can only mitigate a subset of security attacks:

* Spoofing
	* Use authentication to guarantee the identity of the sender
* Information Disclosure
	* Encrypt data to make sure it isn’t obtained by an untrusted user
* Tampering
	* Use tamper-protected hashing techniques or signatures to ensure that data hasn’t been modified

**Common Mistakes**

* Believing that simply adding cryptography will secure your implementation.
* Using bad random number generation.
* Mismanaging key information.
* Writing a custom Cryptographic implementation.

For effective use of cryptography, you need to be careful with random data. 

* Most common random number generators are not really random – don’t use rand()
* Using faulty random number generators can weaken your encryption algorithm. If encryption has 256 bits of entropy and the random number generator has only 64 bit entropy, then your encryption algorithm is only 64 bit strong.
* Random data is especially crucial for key generation because if an attacker can guess your key then the encryption is useless.

Use a random number generator with the following features:

* High degree of entropy
* Generates a seed from many variables including unique hardware configuration and state
* Mathematically very difficult to guess next number in sequence

**Key Management:**

You can use CryptGenKey on Windows and /dev/random on Unix systems to make key generation more robust.

Common Mistakes:

* Hard-coding key values anywhere in the source, including resource files
* Failure to protect key data in memory or when persisted to a file
* Using key data too often in your application
* Generating the key from a weak password
* Being careless with private/symmetric keys

Key Management Goals:

* Users must be able to securely obtain a key pair.
* There must be a way to look up other people's public keys.
* There must be a way to publicize one's own key.
* There must be a way to publicize lost/stolen keys and make everyone aware.
* Users must be able to store their private keys securely.
* Keys need to be valid only until a specified expiration date.

**Tips:**

* Reduce time in memory
* Ensure secrets are not paged to disk
* Never store secrets in plain text anywhere

**Attacks against Crypto-applications**

* Brute-force
* Cryptanalysis or frequency analysis: succeeded in breaking substitution ciphers
* Differential cryptanalysis: succeeded in breaking DES
* Other attacks:
   –   Target insecure random number generators
   –   Key-management attacks
   –   Design attacks
   –   Implementation attacks
   –   Attacking the weakest link: users

### Security Tools

**Hardening and Lockdown Tools**

Windows:

* MS Baseline Security Analyzer
   –   Improves the security management process
   –   Detects commonly found security misconfigurations and security updates missing on computer systems
* HFNetChk
   –   Microsoft Network Security Hotfix checker
   –   Command-line tool used to centrally monitor security patches
* IIS Lockdown tool
   –   http://www.microsoft.com/technet/security/tools/locktool.mspx
   –   Free download
   –   Restricts different misconfigurations of IIS, HTTP, FTP, SMTP, and NNTP services
* HiSecWeb security template
   –   Reduces the attack surface for Web deployments

Linux:

* Bastille is a hardening and lockdown tool
	* Proactively configures the system for increased security
	* Can be used to assess a system’s current hardened state
	* Webmin
       – Web-based tool used for system administration
       – Helps setup user accounts, file sharing, and DNS configurations

**Security Patch Currency Analysis and Update Tools**

Security patch currency analysis and update tools analyze the file sets and patches installed on a system and recommend security actions to take. In simple terms, these products help users to understand how to update and patch their systems.

Examples:

* Up2date: RedHat Linux patch update tool
* Windows Update (WSUS): MS update tool that ships with windows

**Vulnerability Assessment and Remediation Tools**

These tools typically apply the following five steps in fixing these vulnerabilities:

* Discovery of systems
* Vulnerability assessment
* Vulnerability review
* Vulnerability remediation
* Ongoing vulnerability management

### Anti-virus Technologies

**Scanners**

Scanners check for viruses by analyzing virus signatures. 

Using Heuristics:

* Help detect unknown viruses
   –   Looks at characteristics of a file and determines probability of being infected
   –   Can find and stop some new viruses from executing

* Use a point system
   –   Certain actions get a certain amount of points
   –   If enough points accumulated, the scanner raises an alarm

**Integrity Checkers**

* Monitors the system changes.
* Scans the disk and records a unique “signature” for all files and partitions. The "signature" is usually a hash digest.
* Alerts the user when certain changes are made.
* Shows what damage has been done by a virus.
* Detects unknown viruses.
* Checking host integrity.
* Monitoring configuration changes.
* Detecting system break-ins.

**Safe Computing:**

* Remove media from the disk drive when you shut down or restart the computer.
* Be suspicious of email attachments from unknown sources.
* Verify that attachments have been sent by the professed author of the email. Newer viruses can send email messages that appear to be from people you know.
* Do not set your email program to "auto-run" attachments or auto preview.
* Back up your data frequently. Keep the (write protected) media in a safe place—preferably in a different location than your computer.

### Access Control Technologies

**Physical Access Controls:**

* Network Segregation
* Perimeter Security
* Security Guards
* Badge Systems
* Biometric Access Controls
* Closed Circuit TV Monitoring
* Sensors and Alarms

**Technical (Logical) Access Controls:**

* Access Control Software
* Passwords
* Smart Cards
* Encryption
* Authentication
* Authorization

### Secure Devices or Tokens

**Tokens and Smart Cards:**

A token:

* Is an object that authenticates its possessor.
* Can significantly improve the security of your system without affecting its usability.
* Can be a smart card or something that is integrated in your system.
* Must be unique.
* Is not foolproof since it may be lost or stolen.

A smart card is a type of token. It can be used to compute the response to a challenge, or it could be used for encryption. A smart card can also be used as a random number generator for enabling secure access.

### Biometrics

You can authenticate a user based on something they are. Biometrics is the statistical measurement of biological data.

**Biometric Identifiers:**

Biometric identifiers are the identifying characteristics that can be measured for an individual. In order to be effective, these biometric identifiers must be able to uniquely authenticate a user.

Biometric identifiers need to be:

* Universal
* Unique
* Stable
* Quantifiable

Considerations for biometric identifiers include:

* Performance
* Acceptability
* Resistance to forgery

**Classification of Biometric Methods:**

Static biometric methods include:

* Fingerprint scan
* Retinal scan
* Iris scan
* Hand geometry

Dynamic biometric methods include:

* Signature recognition
* Speaker recognition

**Biometric System Architecture:**

The biometric authentication process includes the following procedures:

* Data acquisition: Measures the biometric identifier
* Feature extraction: Normalizes data into a form that can be analyzed
* Matching: Compares the normalized data to known identifiers for users that can be authenticated
* Decision: Determines if the user is authentic
* Storage: Stores the data for future use

**Bypassing Biometric Tests**

The Japanese test was invented by Tsutomu Matsumoto, a Japanese cryptographer working at Yokohama national university. For this test, he: 

* Researched 11 state-of-the-art fingerprint sensors
* Used 2 different processes to make artificial, gummy fingers
   –   From live finger
   –   From latent fingerprint 
* Gummy fingers fooled fingerprint sensors 80% of the time.

The first test in Germany used:

* 11 biometric sensors
* 9 fingerprint sensors
   –   Reactivate latent fingerprints (optical and capacitive sensors)
   –   Apply latex finger (thermal sensor)
* 1 face recognition system
   –   Download/upload biometric reference data from/to hard disk
   –   Only weak or no life detection

The second test in Germany bypassed iris recognition. In this test, researchers printed a high-resolution picture of the iris of an enrolled person and cut out the pupil to bypass "life verification tests." The system accepted the artificial iris.

### Authentication Methods

You can use various authentication methods to confirm the identity of your users.

These methods include:

* Terminal Access Controller Access Control System (TACACS+)
	* Is a centralized security protocol that validates users to a network access server (NAS) or a router.
	* Allows a remote access server to communicate with an authentication server to determine if a user has access to the network.
	* Uses TCP for reliable connections between clients and servers.
	* Uses centralized communication, which is easier to manage but also more susceptible to Denial-of-Service (DOS) attacks. 
	* Is an open source product, so variations exist and not all implementations are the same.
* Remote Authentication Dial In User Service (RADIUS)
	* The RADIUS authentication method provides authentication, authorization, and accounting (AAA) for network access. It is commonly used for embedded network devices such as routers, modem servers, and switches. It provides some protection against packet sniffing.
	* Drawbacks and Vulnerabilities:
		* Not flexible with changing protocols.
		* Authenticates with MD5 hash—shared secret vulnerability.
		* Can be vulnerable to a man in the middle attack if the authenticator is compromised or predictable.
* Lightweight Directory Access Protocol (LDAP)
	* The LDAP authentication method supports the following authentication types:
		* Anonymous: Allows anonymous users access to a limited set of resources
		* Simple: LDAP server is sent the fully qualified domain name of the client (user) and the client's clear-text password 
		* Simple authentication and security layer (SASL): Uses the challenge-response protocol
* Pluggable Authentication Modules(PAM)
	* The PAM authentication method combines various low-level authentication modules in a single high-level application programming interface (API).
	* This method allows development of programs that are independent of the authentication scheme. You can attach the authentication plug-ins at runtime.
	* Hundreds of pluggable authentication modules exist that use different methods. 
* Challenge and response systems
	* Challenge and response systems authenticate using both “something you have” and “something you know.”
	* Unencrypted passwords are used by these systems from the time of entry until accepted by the host.
	* Challenge and Response Systems:
		* Create a pseudo one-time password system.
		* Are most appropriate for host-to-host communications.
		* Provide authentication and identification, and eliminate replay problems because the password changes with each communication.
	* To use these systems effectively to reduce security threads:
		* Encryption keys should be changed regularly.
		* Algorithms should be changed occasionally.

## Fundamentals of Security Testing

### Introducing Security Testing

The Security Development Life Cycle (SDL) introduced at Microsoft in 2002 has raised the bar. Vulnerability finders have to work harder to penetrate Microsoft applications, and the focus of the security research community is now shifting to those vendors and platforms once considered to be secure.

Well-engineered security testing plays a key role in the effectiveness of the SDL. Vulnerabilities found during the SDL have less chance of damaging customer confidence in Microsoft, and removing critical flaws before shipping leaves attackers with fewer reasons to focus on Microsoft.

Performing security testing during the verification phase of the SDL allows the product team to:

* Perform adequate penetration testing activities on new and legacy code.
* Verify that the application is adequately reviewed for security vulnerabilities.
* Ensure that threats against the application are properly mitigated.
* Document the evidence indicating that customers can trust the application.
* Perform a security push for those legacy components that were not covered by the SDL.

**Overview of the SDL Requirements for Security Testing:**

Verification:

* Review and Update Threat Models
* Re-evaluate your project's attack surface
* Satisfy the AppVerifier Testing Requirements
* Perform Fuzz Testing on Specific Input Parameters

### Adopting the Security Testing Mindset

**Difference between Functional Testing and Security Testing**

* Identifying helpful sources of information for test planning. 
	* A security test engineer must rely on information supplied by the application itself and must learn to extract that information from the application and its I/O interfaces. Functional testing involves testing an application under realistic user scenarios, while in security testing the test engineer frequently deals with scenarios that may not be realistic for the common user. Often, exploitation scenarios cannot be derived from an application’s technical specification, but anticipating these scenarios is vital when planning for security testing.
* Choosing an appropriate set of inputs. 
	* During security testing the test engineer has to consider from where the inputs are coming, how much trust can be placed in an input source, and what could go wrong if this trust is misplaced. During functional testing the test engineer will most commonly use expected inputs derived from use cases, whereas during security testing the test engineer is required to use unexpected and malicious inputs that may expose a security problem.
* Automating security tests. 
	* Interfaces that are not visible to the human eye are most interesting for security testing. In order to be viewed, such interfaces often require the use of specialized tools. Security test engineers must be familiar with these tools and their limitations. During functional testing it is simpler to automate a testing process, as results for a specific test are easily verified. Establishing whether a security defect has been triggered during security testing is much harder. 
* Deciding when testing is complete. 
	* Choosing when to ship a product is a business decision, and a security test engineer’s job is to provide data to assist the people who must make that business decision. When carrying out functional testing, it is straightforward to determine when testing is complete, because with the help of use cases it is clear how much testing has been done. When carrying out security testing, however, deciding when to stop is more complicated because the test engineer needs to help to quantify the risk and decide whether enough testing has been done to declare the product "safe".

**Four Main Information Sources for Security Testing:**

* The User Interface
	* The user interface is the first place to investigate when planning security testing. The information on the screen is primarily directed at legitimate users, but it is important to remember that hackers are users as well, and they can learn quite a bit from this information.
	* The interface may disclose information about how the application works and provide clues about the kind of resources or information the application is relying on, which can help the attacker.
	* Additionally, because user-interface input often has a counterpart in remote input, security bugs uncovered through testing the user interface should be investigated to determine whether they can be triggered remotely. The reason for this is that security issues that can be leveraged by remote attackers usually introduce a much bigger risk than locally exploitable security flaws.
* Error messages and outputs
	* Error messages are a special concern when it comes to planning security testing. When testing the security of an application, testers should leverage information provided by error messages to drive their testing. One obvious example is for determining the set of invalid inputs to test a given feature with. Many applications output error messages to their users to inform them on what the expected format of input is. This can provide very useful information to the tester on the possible assumptions that developers made on the type of data that are expected through a given input vector.
* Abuse cases
	* Whereas functional test engineers consider use cases, security test engineers must consider abuse cases. When conducting security testing, the goal is not to make the software work for users, rather it is to find out if it can stand up to abusers. Abuse cases are often outlined in a threat model. 
* Knowledge of how the system is NOT supposed to work
	* Another vital source of information is a test engineer’s own experience. A security test engineer has to be able to think about what the system is NOT supposed to do; this is exactly the opposite of what is done in functional testing.

**Considering Inputs with a Security mindset:**

Entry Points of the Application:

* OS/Runtime Environment
* User Interface
* External Resources
* File System

Categorizing Dangerous Inputs:

* Long strings and buffers
* Format strings
* Numeric boundaries
* Scripts
* Code
* OS commands
* "Control" characters
* Error codes
* Return values

**Automating Security Testing**

Important because:

* Larger coverage. 
	* Coverage refers to the percentage of security-critical functions that are exercised by different inputs during a security test. Security testing tools help achieve a substantially larger coverage.
* Regression testing. 
	* Automated security testing allows regression testing to be performed easily. During regression testing an application is tested for newly introduced bugs, usually by re-running previous tests and checking whether previously fixed faults have re-emerged.
* Efficiency. 
	* Automated security tests are more efficient than manual testing. Automated tests are repeatable with no human intervention, as opposed to manual testing, which is far more time-consuming. Automation allows for more targeted manual testing as they disclose areas of potential weakness in various execution, trust, and I/O boundaries, which means the test engineer need not try to find all weaknesses manually, but can focus on key areas and provide feedback to threat modeling and other phases of SDL.

Generating the input:

In order to generate inputs that are dangerous to your application, a list detailing all possible inputs must be created. This list might be generated in real time as the software is being tested, manually or by using a fuzz test automation suite, or it may already be available to you from other security tests applied to similar products. If the latter, you must keep in mind that often a list is not cross-applicable, and you may need to modify it or create your own list. 

**Defining Stopping Criteria for Testing:**

Quantifying the risk, to do so you must ask:

* Was the process which was established to prevent and detect security flaws fully implemented?
* How many unmitigated vulnerabilities are currently extant?
* Is additional testing necessary? 
* What can we do to make this product safer?

Assigning Severity:

* Critical
	* Remote, anonymous user escalation of privilege
	* Arbitrary code execution
* Important
	* DoS (low bandwidth attack, blue-screen, or long duration)
	* Local elevation of privilege
	* Information disclosure with privacy implications
	* Tampering with user data
	* Spoofing a user or computer

Collecting the proper Metrics:

* How well did the team:
	* Create required documentation?
	* Adhere to best practices?
	* Use all appropriate tools?
	* Respond to problems that arose?

* How well did the testing team test:
	* Untrusted interfaces?
	* Untrusted resources?
	* Untrusted protocols?
	* Untrusted files?

* How well did the testing team cover the:
	* Threat model?
	* Set of possible attacks that apply to the application?

### Categorizing Security Vulnerabilities

**Environment Vulnerabilities**

* Configuration Files
* Temporary Files
* External Binaries
* Environment Variables
* Registry

**Input Vulnerabilities**

Malicious input/injection.

**Data and Logic Vulnerabilities**

Linked to the way a program behaves or makes decisions. 

* Test APIs
* Hard-coded accounts 

### Testing for Environment Vulnerabilities

**Attacking Dependent Binaries**

Given that applications typically depend on external components, the degree of trust placed on such components directly impacts how secure an application is.

* The Fault
	* When using external binaries, it is highly recommended to check return values and error codes. In addition, binary-level authentication checks such as hash or digital signature verifications will ensure that the correct dependent binaries are used.
* Testing
	* When conducting testing, ensure that dependent binaries are identified. Use design documentation and any appropriate tools, such as fault injection tools, as aids in performing this task. Once dependent binaries have been identified, it is important to prioritize these binaries according to their roles in the application’s security model. 
* Failure Symptoms
	* The display of error messages during testing is an early indication that the application checks return values that are produced when a library is loaded. However, error messages should be carefully examined as the application could be incorrectly identifying the problem.
	* The lack of error messages is a sign that the application does not check return values that are produced when a library is loaded. If the application fails to recognize that a library has not been loaded successfully or that it contains rogue code, force the application to use features from the DLL in question. In this case, an application crash may be an appropriate defense for the application.

Example: 

In 2005, a vulnerability was uncovered in the MySQL database server software. This vulnerability laid in the way the database server loaded user defined functions. User defined functions allow MySQL users to extend the default function set by creating native code libraries that the database server can load and call. MySQL failed to adequately validate user input that specified which library to load before calling the LoadLibraryEx function. 

Two vulnerabilities arose as a result of this behavior. First, it was possible for a remote attacker with access to the database to specify a non-existent library for MySQL to load. The result was that the server hanged and required for an error dialog to be acknowledged before recovering. Second, and more seriously, it was possible to specify an existing library that implemented native functions with names as expected by MySQL and cause the server to completely crash. 

**Tampering with Dependent Data Stores**

Applications typically accept input from various sources, such as files and data stores. These inputs constitute entry points into an application and might expose it to malicious data.

* The Fault
	* Applications read and write static data as part of their normal operation. This static data might be read from files, cookies, the registry, or other places. If an application reads compromised data, there is a risk that it might be exploited. It is crucial for the development team to consider how much trust they can place in data that the application reads. For instance, if the application is reading a configuration file that defines security settings for the application and its ACLs allow non-administrative users to modify it, then the application’s security can be lowered by an attacker. It is crucial to determine to what extent each source of data can be trusted and how it can be manipulated by unauthorized users.
* Testing
	* Points of interaction between the application and its dependent data stores should be identified. A fault injection tool can aid in this process by allowing enumeration of such points. Pay attention to which data the application reads from static data stores such as files, the registry, and cookies. Consider for what purposes the read data is being used and identify any sensitive data. Imagine exploitation scenarios that could result if the data is altered and see whether it is possible to carry them out.
* Failure Symptoms
	* The goal of the test is to determine whether or not it is possible for a user to perform an action that he or she should not be able to. This can occur when a user with a certain access level is able to perform actions that belong to a higher access level—that is, when elevation of privilege is possible. An example of this would be a user being able to manipulate a setting that controls his or her access level to the application so that he gains administrator-level access. However, the failure might not manifest itself in such an obvious way. Another example would be a user modifying application settings in such a way that data designed to be inaccessible to that user becomes accessible. This would be the case, for example, if an application kept pointers to the location of user data which could be changed by the user to point to different users’ data.

**Identifying Shared Assets**

When two or more applications share assets such as memory or files then there is a potential for security defects to be introduced. These kinds of defects stem from applications not following consistent and secure rules for the handling of shared resources.

* The Fault
	* When two or more applications share files or memory, they must all obey a common set of rules for accessing and modifying these shared assets. If an asset is shared by applications with different privileges or is visible by different users, then rules for deleting, marking as no longer in use, or otherwise expiring these assets at the right time also have to be considered. If access to the shared assets does not follow proper rules then an attacker might be able to manipulate the different applications to perform unauthorized reads or writes on the shared data.
* Testing
	* Identify shared assets for testing by locating: 
		* Shared synchronized objects, such as events, semaphores, mutexes, and lock files created by one application and/or shared by several applications. 
		* Communication channels, such as sockets, pipes, and shared memory.
	* You can use tools such as WinObj from SysInternals to identify named resources and objects that might be shared by the application. 
* Failure Symptoms
	* Common failure symptoms that could be observed when modifying the state of shared synchronization objects include:
		* One of the applications entering a deadlock and becoming unresponsive.
		* Operations performed by one or more applications appearing in an unexpected order.
	* When modifying shared communication channels, such as shared memory, shared files, or pipes, check for either of the following:
		* An application crashing because it receives unexpected input from the communication channel.
		* An application waiting indefinitely on some input from the communication channel.
	* Also verify that the applications still run securely when you change permissions and properties of the shared assets.

Example:

TrustPort is a major producer of software solutions for secure communication and reliable data protection. In 2009 it was found that one of their Windows products installed its own program files with insecure permissions (Everyone - Full Control). Because of this lack of oversight a local unprivileged attacker could replace some files (including executable files of Trustport services) by malicious files and execute arbitrary code with SYSTEM privileges.

In this example, program files are a shared asset between the different users of the system and must be protected so that one malicious user can not exploit the trust of the other users by replacing program files.

**Applying Environment Stress**

An application’s environment is beyond its control. However, the way an application reacts to problematic events caused by its environment is under its control. Unusual events, such as low memory conditions, might pose a threat to the application if they were not considered during its design and implementation. Handling problematic events is not always easy, as some of them might be very difficult or even impossible to recover from.

* The Fault
	* Handling these kinds of problematic and unusual events may pose a danger to applications that have weak error checking on the availability of resources or that trust the integrity of the data that they are reading. Developers often fail to consider memory, file system, and network errors. This can manifest itself in the following ways:
		* The application allocates memory but fails to check for the return value, thus assuming that more memory is available.
		* The application blindly reads network data, assuming that it will come in an expected, well-formed structure.
		* The application writes to the file system and expects the write to be successful.
* Testing
	* It is hard to simulate environment failures without instrumentation. API shims, which replace one API with another, can be used to intercept calls to library functions and make them return unexpected values. Fuzz testing and fault injection tools, which can be implemented on the basis of such shims, facilitate the testing for such failures. 
	* Fuzz testing tools can be used to modify environment parameters with a large number of different values. Fault injection tools, through intercepting calls to functions that manage the application’s environment, can simulate unlikely events in that environment.
* Failure Symptoms
	* If the application is properly checking for resource availability, the user will probably receive an error message. For example, if an application requests more memory from the operating system and none is available, then it should signal this condition to the user and in many cases abort execution. 
	* Watch for catch-all exception handlers as these might be a sign that the application can be terminated prematurely or that it might leave its environment in an insecure state. If the same error message is displayed for different erroneous conditions, this could be an indication that the same error-handling code is used for all of them.

### Testing for Input Vulnerabilities

**Bypassing Input Checks**

Software should accept only good input and should reject all bad input. However, this is often difficult to implement correctly. Part of the reason for this difficulty is that even classifying good and bad input might be problematic because of issues such as localization.

* The Fault
	* Input that could result in the security of the software being compromised must be filtered or rejected by an application. In general, developers have to choose between using a block-list or an allow-list. 
* Testing
	* When testing for input bypasses you should watch for error messages related to bad input as they indicate points where the application is performing input checking. These are points that should be tested to verify that it is not possible to allow dangerous data access into the application.
* Failure Symptoms
	* If the application hangs or fails because of any of the inputs you submit, then a security defect might have been found. While a crash might be a sign that exploitation is possible, it might also be a result of a defensive measure in the application.
	* If you are testing a client-server application disable the client-side input checking and verify that the same checks are performed by the server. If they are not, then it is possible that the application is vulnerable to malicious input. However, even if client-side and server-side validation are not identical this does not necessarily mean that the application is vulnerable—the client might just be implementing a more user-friendly validation.

Example:

The Microsoft Anti-Cross Site Scripting Library is a security library that aids in the detection and elimination of Cross Site Scripting attacks. In January 2012, security vulnerability was discovered  in the library. The library did not properly filter HTML code from user-supplied input, which could allow a remote user to be able to exploit a target application that uses the library to cause arbitrary scripting code to be executed by the target user's browser. Due to the vulnerability, the code will originate from the application using the Microsoft Anti-Cross Site Scripting Library software and will run in the security context of that site and be able to access the target user's cookies and access data recently submitted by the target user via web form to the site, or take actions on the site acting as the target user.

**Injecting Long Strings**

If a user submits more data to an application than can be fitted within the data structure that is destined to hold it, the application’s security might be compromised. The submitted data might be executed if the application allows the data structure to overflow, or the data might be truncated in a dangerous way if the application attempts to limit its size.

* The Fault
	* Most applications make use of fixed-size data structures. An attacker can attempt to take advantage of this fact by submitting data that is too large to fit within the data structure reserved for it by the application. When confronted with this, the application has to either truncate the data or completely reject it. 
	* If data is truncated this may have security consequences. The data may no longer make sense, and later it may be interpreted in a dangerous way. If the application fails to restrict the size of data that is being inserted into a fixed-size data structure, it might crash as a result of memory corruption or an exception being launched. In the case of native code, memory can be corrupted, and an attacker might be able to exploit a buffer overflow condition. While it might be hard to prove that exploitation of memory corruption vulnerabilities is possible, you should err on the side of caution and assume that it is.
* Testing
	* To conduct testing you should identify points where the application accepts input from untrusted interfaces. A threat model of the application can be of great help in obtaining this information. Files and network entry points are especially interesting as they may be accessible to a large number of potential attackers. 
	* Fuzz testing tools should be used to generate large volumes of data to test for overflows. It is essential to understand the formats and protocols being fuzzed and to identify likely points of danger. Consider overflow vulnerabilities caused directly by the use of dangerous memory and string manipulation functions as well as by integer overflows. Typical danger points are data fields that have accompanying size fields and/or type fields, and data fields of varying size that rely on an "end-of-data" marker. When varying size fields, you should strategically consider integer boundary limits: values close to them are more likely to cause integer overflows.
* Failure Symptoms
	* As a result of overly long input being received, an application may crash, indicating that internal data was corrupted and then accessed. Even if the application does not crash, memory might still be overwritten. In this case, the challenge is then to determine whether memory has indeed been overwritten, and if so, to identify where it has been overwritten.
	* Both situations might or might not be exploitable. You should use a debugger to view sensitive stack and heap locations and then determine whether the inserted data can reach them.

**Injecting Executable Content**

An application might pass input it receives, either directly or after transforming it, to other systems it interacts with. If an application passes along malicious input, then the security of downstream systems might be put at risk. 

* The Fault
	* An application should distinguish between data it receives and executable content it submits to other systems. A failure to do so might result in the data and the executable content becoming insecurely mingled, so that the data is interpreted as executable content. For example, if externally controlled input makes its way into the parameters of an SQL query for a database, it might be possible for an attacker to manipulate the query and modify the database operation that is performed, or even to perform an entirely different operation.
* Testing
	* To identify which inputs are suitable for testing, you need to determine which downstream systems process the application’s output and understand how that processing takes place. Craft input that should not make it into those systems, but that would be processed by them if it did. Then ensure that the application does not pass it along. Lists of dangerous inputs should be used to drive testing, as the number of potentially dangerous inputs can be large.
	* If you see error messages you should attempt to change your input in order to bypass the checking that is being performed by the application. Make a note of any information that might be displayed in an error message about what is wrong with the input, as this can be used to adjust the input in an attempt to bypass checking. For example, if you are attempting to insert a series of characters that the application might be blocking, try submitting them one by one to discover exactly how the input checking is being done.
	* Some of the common categories of input that should be tested include:
		* SQL queries being passed to a database (SQL injection).
		* Script code being passed to a scripting engine or Web browser (cross-site scripting).
		* Commands being passed to an operating system or to a command-line interface (command injection). 
* Failure Symptoms
	* In general, injection attacks are launched with a specific goal, such as gaining access to an account or writing a file to a user’s hard drive. Once the attack has been performed, check to see if the goal has been achieved.
	* The content you inject might not have the expected outcome in the target system, or it might not reach it intact. As a result, you should monitor for symptoms that indicate that the executable made its way to the system you are targeting. The nature of the input being submitted as well as the type of system being targeted will dictate the kinds of symptoms that you should look for.
	* If you are attempting to inject commands into the operating system, monitor for processes being spawned and observe whether their parent is the process you are targeting or a related one.
	* If you are attempting to inject SQL into a database, monitor its log files for indications of invalid queries or permission-related errors that might be triggered by the insufficient level of access the application has to the database.
	* If you are attempting to inject JavaScript code into an application to reach a browser, monitor the JavaScript error log in the browser for indications of invalid JavaScript.

**Corrupting Network Packets**

Historically, developers have tended to trust network input because they see it as being generated by programs and out of the direct control of a user. However, programs can be made malicious, can have their output intercepted, or can be replaced with malicious versions, and dangerous network traffic might make its way to the application.

* The Fault
	* When development teams create programs that process network data they might:
		* Forget to consider corrupt packets, or they might not consider all the ways in which a packet can be corrupted. A typical cause of this is focusing on an application’s speed, rather than on its security. As a result, insufficient validation of inputs may be performed by the application.
		* Expect too much of the networking layer. If the underlying protocols that are supporting the application are not well understood then the developers might make false assumptions. For example, they might trust that data returned by a DNS query is always authentic, when, in fact, it may not be so.
* Testing
	* You should identify all network interfaces that the application is using. A port scanner can aid in this task, but make sure that it is not blocked by a firewall and that it is able to reach all the ports that the application might have open. You can also look for calls to networking functions that open ports and receive data from the network by using tools that intercept calls to these types of functions, such as fault injection tools.
	* Fuzzing tools are very efficient at generating corrupted packets. If the network protocol relies on size fields to determine how much data there is in different fields, you should attempt to cause a mismatch between these fields. In this way, you can establish whether an application allocates an incorrect amount of memory for data that it is receiving as a result of this mismatch. You should also attempt to set size fields to integer boundaries in order to identify integer overflow problems. Overly long packets, even for protocols that do not make use of size fields, should be transmitted, as these might result in buffer overflows.
* Failure Symptoms
	* Crashes and hangs might occur when bad data is being processed and should always be investigated. Crashes are an indication that the application might be vulnerable to a buffer overflow. Hangs are also important indicators, as they might be a sign that the application is vulnerable to a denial of service attack.
	* If the application does not crash, memory might still be corrupted, or the application might enter an insecure state. You should monitor outputs such as the screen and log files for signs of failure or error.

Example:

Windows Vista SP2, Windows 7, and Windows Server 2008 suffered from a vulnerability that would allow an attacker to remotely cause a denial of service.

The TCPIP.SYS driver, which is used by Windows to process incoming IP packets, does not properly implement URL-based QoS, which allows remote attackers to cause a denial of service (reboot) via a crafted URL to a web server. This vulnerability was caused by the way that the Windows TCP/IP stack processes ICMP messages and handles URLs in memory when URL-based QoS is enabled.

Fuzzing can excel at finding these types of bugs by exposing implicit assumptions and shedding light on poorly understood trust boundaries.

### Testing for Data and Logic Vulnerabilities

**Monitoring Outputs for Information Disclosure**

Usability and security are often at odds. Developers want to provide useful information to legitimate users but in so doing, they might end up giving assistance to attackers.

* The Fault
	* Usability has been a concern longer than security has been, and, as a result, development teams often give it priority. Information displayed by an application might give an attacker insight into its working, or it may be used to identify areas of potential weakness. 
	* Information given by an application might be obviously useful to an attacker, but in some cases its usefulness might not be so apparent. For example, an application might display an error message such as "Invalid login attempt" when an invalid user name is entered, but display a slightly different one, such as "Login failed", when an invalid password is entered. This would allow an attacker to ascertain valid user names for the application. 
	* Under abnormal conditions the application might also display information that is useful to an attacker in the form of an error message. A common example of this is that of a Web site that propagates an error message from the database to the user. These kinds of messages would not show up in normal use, so they could be missed by developers and testers.
* Testing
	* You should identify all outputs of the application, such as files, the UI, the registry, the event log, and debug logs, whilst paying special attention to error messages. Watch for information that could help an attacker. It is useful to identify such outputs in a threat model so that you know what you are looking for before you begin testing.
* Failure Symptoms
	* You should monitor for information that would help an attacker directly compromise security or that could be used to direct further attacks at the application. Information that would qualify includes:
		* Stack traces that disclose usage of external resources, other systems, or OS environment. A stack trace that indicates a failure in writing to a file would tell an attacker that his input is reaching the file system.
		* Disclosed secrets such as passwords and encryption keys.
		* Identifiers, addresses, or names of external systems that the application depends upon. As an example, consider a stack trace that indicates a network failure and shows the address of the system that the application was attempting to connect to.
	* Some of the common places where failures are found include:
		* Error messages. Make sure you read them carefully.
		* Dialog boxes during installation and configuration.
		* Registry and files. Do not forget about temporary files.
		* Event log, debug logs, and log files.

**Exposing Authentication Data**

Applications that accept or hold confidential data from different sources need to distinguish between those sources in order to ensure that the confidential data cannot be inspected or altered. In this case, applications might authenticate trusted parties such as machines, components, and users. If weak authentication is used it might be possible to spoof data to the application.

* The Fault
	* When a trust relationship exists between an application and another resource, be it a remote server, another local application, or a database, the application’s designers have to decide how to establish that trust. The degree of trust granted to a particular resource should be related to the amount of checking done to verify authenticity of the data coming from that resource. If an application trusts forgeable data without properly authenticating then it might be vulnerable to spoofing attacks, in which data seems to be from one source but in reality is from another.
	* Sometimes trust is extended solely on the basis of identification, and no authentication is performed. While identification and authentication are related, they are not the same. Identification involves the user providing his or her identity, while authentication involves the user giving proof of this claimed identity. If identification only is performed on a piece of data, then spoofing it simply requires changing it to a different value.
* Testing
	* Determine trust boundaries and the features that authenticate that trust. Having a threat model for the application can assist you in enumerating the different trust boundaries of the application.
	* Identify data that passes those boundaries and ascertain how its source is determined. This might involve IP addresses, MAC addresses, cryptographic certificates, or digital signatures.
	* Modify these identifiers so that they use a different address or become invalid and check whether the authentication routine notices.
* Failure Symptoms
	* The objective is to determine whether the application will accept data or commands from a source whose origin has been spoofed.
	* If access is denied, this is a sign that the application is doing the right thing. Modifying data may cause the application to crash, meaning that a denial of service attack is possible. It might or might not be possible to leverage the crash to further attack the application. If you are able to authenticate with faked credentials, then there is a security defect.

Example: 

JBoss Operations Network is a Java EE-based network administration and management platform for the development, testing, deployment, and monitoring of the application lifecycle.

In February 2012, it was discovered that the system does not properly verify security tokens allowing a remote agent to spoof the identity of an approved agent and hijack the approved agent's session and steal the target agent's security token. The remote user can obtain potentially sensitive data from the target server. It was also found that the platform sometimes allowed agent registration to succeed when the registration request did not include a security token. This feature was designed to add convenience but allowed a remote attacker to spoof the identity of an approved agent and pass a null security token.

**Identifying Hardcoded Secrets**

As a means to facilitate testing, code and data might be introduced into the software during the development process. Applications connecting to external resources normally need to authenticate themselves, and the development team might opt to hardcode the credentials for doing so into the application. Failure to remove any of these secrets from release builds of the software might compromise security if an attacker is able to identify them.

* The Fault
	* Complex applications are often difficult to test by making use solely of APIs intended for normal users. As a result, test APIs that bypass normal authentication checks and/or hardcoded accounts are often used by testers and testing code. Naturally, these APIs and accounts should be removed before the application ships externally. However, they may be left behind for a number of reasons. For example, the testing APIs can become so integrated into the product that removing them would have a significant impact on the shipping date. Hardcoded accounts might provide an open backdoor to different privilege levels within the application. Test APIs might also provide this level of access or provide access to internal data that can be leveraged to launch an attack.
	* When applications connect to external resources authentication is normally necessary. This typically involves relying on a user name and password pair or a cryptographic key. If the user name and password pair or the key are embedded in the application, then an attacker might be able to recover this information.
* Testing
	* To check for hardcoded accounts and test APIs:
		* Pay attention to how test automation authenticates to an application and extracts execution data. Take note of these access points.
		* Scan release builds to ensure the absence of those access points.
		* Attempt to run test automation on release build to see if it fails as it should.
		* Monitor for other code and data that should be purged from the release build and keep track of it, verifying that it does not make it to the release builds.
	* To test for hardcoded user name and password pairs or keys:
		* Run the strings tool on native code to obtain a list of all the text strings in the application’s binary and then look for any suspicious strings.
		* Run the ildasm disassembler on managed code to obtain the Intermediate Language (IL) for the code and then search for ldstr instructions to find text strings that might be suspicious.
* Failure Symptoms
	* When you run test automation on the release build any steps that depend on hardcoded accounts or test APIs should not work. If they do run then you have identified a security defect.
	* Any string that is defined near words such as "key", "password", "secret", "private", "passphrase", "crypt" or similar, in native or IL code, is a strong indication that a user name and password pair, or a key, is hardcoded in the application. Strings that are encoded in common formats, such as base 64 encoding, are also indications that data could be hidden in the application.

**Varying Execution Logic**

Different execution routes through an application might result in outcomes that are not expected by the developers. Seemingly benign features might be abused to launch an attack against the application.

* The Fault
	* Development teams often expect users to use a product in the same way they would use it. In so doing, they may fail to consider the implications of a user combining operations in a different order. If a user takes a different route through the application, defenses may or may not work, as a code path that might not have been considered during development is being followed.
	* Modern software is complex and has many different features and code paths. As a result, it is possible that some of its logic might be leveraged to carry out unexpected scenarios. In particular, functionality that is intended to protect the software might fall into this category. Consider, for example, an application that locks out users if they enter their password incorrectly three times. While this protects users’ accounts from being brute forced, it also enables an attacker to lock out all of the applications’ users by submitting three failed password attempts for each of their user names.
* Testing
	* To test for potentially dangerous routes through the software:
		* Identify the goal of an attack—for example, gaining access to a feature of the application, installing a virus, or jumping past a particular Web page.
		* Think of all the paths you might take to reach the identified goal.
		* Test the expected path first and break it into specific, distinct steps.
		* Try to skip steps, replace steps, or create variation in the inputs used at the different steps.
	* To test for potentially dangerous functionality:
		* List the features of the application and brainstorm which ones could be misused, and in what ways.
		* Carefully test any operations if you are not sure what their outcome should be and verify that they cannot be leveraged to attack the application.
* Failure Symptoms
	* You should define the goals you are trying to achieve by varying the execution logic and abusing the software’s functionality. If any of these goals can be achieved then a security defect has been found.


### Fuzz Testing Tools

Fuzz testing is a testing technique in which large volumes of random or arbitrary input are provided to a program. Security weaknesses are uncovered by monitoring the application for obvious failure signals, such as a crash or a hang, or by monitoring the application’s outputs or internal state. Although traditional fuzzing refers to black-box fuzzing, in which malformed data is sent without verification as to which code paths will be hit and which will not, white-box fuzzing can also be performed. In this case, the malformed data is sent after having modified the software configuration and the fuzzed data to ensure that all target code paths will be hit.

Fuzz testing has numerous advantages compared to other testing techniques:

* Automation. 
	* Malformed data passed to the input are generated automatically and do not require testers to write multiple test cases.
* Volume. 
	* Because fuzz testing is automated, it allows a very high volume of testing to be performed with many variations and passes.
* Productivity. 
	* Fuzz testing usually uncovers many bugs, most of them reliability related and many of which are potential security vulnerabilities.
* Reproducibility. 
	* The malformed data that caused the application to fail can be saved and the test case can easily be reproduced.
* Inherent threat modeling. 
	* Fuzz testing can anticipate and help prevent attacks because attackers themselves use fuzz testing to identify potentially exploitable vulnerabilities. 

Even though fuzz testing is a very efficient testing technique in uncovering vulnerabilities caused by access violation and buffer overruns, it suffers from the following limitations:

* Functional defects are not easily detected because the application’s response and possibly its internal state have to be monitored while the testing is being done. Some functional defects, such as a lack of authorization controls, are easier to identify, however, because the application will process data that should have been discarded as a result of the authorization check.
* If inputs are not carefully chosen then insufficient code paths could be exercised. As a result, potentially vulnerable code paths could be missed.
* If the application requires authentication to be performed before accepting data that could exploit it, the fuzzer needs to be able to authenticate before sending data.

**Fuzz testing approaches**

A fuzzer can rely either on mutation or generation, depending on how test data is created. Different levels of understanding of the data format, on the other hand, define how "dumb" or "smart" the fuzzer is.

Generated vs Mutated:

The technique of creating input by modifying existing valid data is called mutation. The fuzzer starts with samples of valid data for a protocol or file format and continuously modifies them to generate random data. This approach can leverage existing tools and files, allowing a fuzzer to be quickly constructed. As the existing data is in a valid format, the fuzzer gains a certain level of format awareness from the outset.

When input is not derived from existing valid data, it is said to be generated. Choosing this approach requires more work to be done in understanding the protocol specification or file format. Fuzzers of this type might be quite generic, allowing the tester to construct a template for the data format and defining which parts should be fuzzed and how.

Dumb vs Smart:

The level of understanding a fuzzer has for a given data format can vary across a broad spectrum. At the lower end of the spectrum, a generation-based fuzzer might simply generate completely random data, without any format awareness at all. At the higher end, a generation-based fuzzer might understand where it is located in the execution path of the target application and will be capable of moving forward to reach desired states. This latter level of understanding is necessary to fuzz test for vulnerabilities that, for example, can only be found after authentication, or after an initial handshake.

Smarter fuzzers are capable of navigating more code paths in the application and therefore have the potential to find more bugs. Naturally, they take more effort to write and to use.

**Fuzz testing Targets**

Fuzz testing can be targeted at a range of different types of applications. Applications might expect input locally, such as through files or through command line arguments, or remotely, through network packets. Server-side network interfaces and file parsers are traditional targets for fuzzing, but nowadays much fuzzing is performed on client-side applications such as browser scripting engines, ActiveX controls, and browser plug-ins. The reason for this is that these client-side interfaces parse input from many different untrusted sources and are also easy for attackers to target.

When considering fuzz testing of an application, you should identify interfaces that receive input across its trust boundaries. Ideally, you will be able to do this by using the threat model that was created for the application. Interfaces that sit in trust boundaries pose significant risk to the application and should be prioritized. You should also consider which interfaces are more prone to have security defects. A complex file format parser would be a likely candidate, for example, because many mistakes can be made in its implementation.

**Fuzz Testing Tools**

Numerous fuzz testing tools exist, so before you decide to develop your own fuzzer you should investigate whether there is an existing tool that suits your needs or can be easily adapted. There are numerous fuzzing frameworks (such as SPIKE and Peach) which you should consider using if you need to create your own fuzzer. They can provide you with a lot of the underlying functionality needed for a fuzzer.

### Fault Injection Tools

Fault injection is the process of simulating failures in an application. It is particularly efficient because it allows for the simulation of extraordinary conditions that developers and designers might not have considered. Depriving the application of disk space and prohibiting an external library from loading are examples of fault injection. There are two types of fault injection:

* Run-time fault injection. 
	* Tools are used to get between the application and the OS to control responses from system calls and deny resources such as memory, disk space, files, and libraries. 
* Source-based fault injection. 
	* Source statements are modified so that internal data can be set to values that cause exception conditions to evaluate to true. Source-based fault injection requires access to the source code.