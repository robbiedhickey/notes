# Technology Strategy 2014-2018

This is reassesed every year, but this year represents a big change in vision. Put together by Shannon Ulmer, the CTO of TRTA. 

## Strategic Situation

### Organizational Structure

* Many applications and decentralized technology (150+ applications across five business segments + CTO)
* Consists of both internal and external factors that drive past decisions. 
* ~2000 technology employees across 54 sites in 9 countries

### Business Strategy
* Driving growth
* Focusing on scale, driven through platform and technology
* Innovation

### Most Significant External Factors

* Increased adoption of hosted / SAAS solutions
* Customers' expectation of integrated suites of products
* Threat of new competitors from adjacent markets
	* ERP / business mgmt - SAP, Oracle, Workday, Salesforce
	* Accounting - intuit, xero, myob
	* Media/information - Bloomberg, CCH
* Shift to mobile computing (phones, tables, phablets, watches)

## Mission, Values and Vision

### Core Values Inherited from Thomson Reuters

* **TR Values** - Trust, Partnership, Innovation, Performance
* **TR Technology Mission** - We unleash the magic of technology to power the decisions that change the world
* **T&A Accounting Technology Mission** - Build it once with engineering excellence.
* **T&A Technology Strategy** - 1) Platform Strategy 2) Engineering Excellence 3) Drive Innovation 

## Strategic Themes

### Platform-Based Approach

> Advocating for SOA

A platform-based approach to software development **is**:

* A shared architectural "playbook" for design
* re-usable components accessed via services (or APIs)
* commoditizes capabilities to independence, discrete functions that don't depend on other capabilities
* there isn't a user interface (products will use FEF and UX standards)
* necessary for an ecosystem
* both TR and TRTA technology assets and standards
* raising our game on engineering and architecture

A platform-based approach to software development **is not**:

* a mandate for a central team to control all shared assets
* a project where technology goes away and comes back with a platform
* an initiative to dilute features or to have segments share **products**
* a new business case for incremental funding
* just versions of a single **product**
* just a set of technical standards

In the past, the focus has been product oriented, not on platforms. Many cases where we have similar capabilities across many applications. 

#### Benefits of a Platform-Based Approach

* Improve quality and scalability
* Deliver tighter integration
* Efficient use of technology staff
	* Avoid duplication of effort
	* Free up staff to innovate
	* Achieve synergies across businesses
* Speed to market
* Attract / develop talent

#### Thomson Reuters Platform Architecture

Layered into three tiers:
 
1. Infrastructure Layer Infrastructure as a service. (Run on it) 
	1. Data Center Provided
	2. Standardized Hardware and Software Solutions 
2. Core Platform Platform as a service (Build on it)
	1. Corporate-wide, core platform components
		1. Cross-cutting, applicable to multiple business units
	2. BU-specific core platform components
3. Product Layer Software as a service. (Use it)
	1. Corporate-wide
	2. BU-specific

Early-look examples of synergies (not exhaustive):

Technical Capability | Potential Uses
-------------------- | --------------
Single Sign-on | Every App
Financial Balance Store | Global Indrect Compliance, Global Direct Compliance, Global Accounts Production, General Ledger Manager, Provision, OIT, Blue Moon
Workbook Calculator Engine (Kingfisher Core) | Global Accounts Production, Global Corporate Tax, Blue Moon, Velocity
Time & Billing | Blue Moon, Patagonia, Legal
Customer Portals | Blue Moon, Lonestar (new name for ONESOURCE platform), others to come
Accounting/Bookkeeping | Blue Moon, Patagonia
Audit Workpapers | Checkpoint Tools, Blue Moon
Tax Calendar | Indirect Tax, Patagonia, Blue Moon, ONESOURCE Income Tax (OIT)
eFiling | Patagonia, GoSystem, OIT, Blue Moon
Workflow | Blue Moon, Workflow Manager, Property Tax
Fixed Assets | Patagonia, Blue Moon
Payroll | Patagonia, Blue Moon

#### Goal: Decouple Capabilities from Products

Many of the capabilities we currently offer are embedded in our applications. It is a very strong product portfolio, but for the most part they are siloed. Enterprise services transcend products. Exposing capabilities also means we need to pay more attention to non-functional requirements: Performance & Scalability, Availability, Security, Flexibility, Disaster Recovery. Critical now that many BU's may depend on your services. 

#### TRTA Architecture Vision

We will target multiple browsers, devices, platforms and application integration (namely Office, Excel will continue to be key). 

Expose each core capability as a REST service. Services must have strong encapsulation, hide implementation details behind our interfaces. Will need to have multi-tenant datastores. In order to accomplish this, we need to have the concept of a **Universal Identity**. 

The [Cobalt Framework](https://thehub.thomsonreuters.com/groups/central-tech-cobalt-platform) is a general set of guidelines and best practices for this.

#### What Does a Platform Approach Mean For You?

* More words, less code. 
* KISS service interfaces.
	* if they are not, they won't be consumed
* Mitigate integration risk. 
* Higher stakes - automated & high quality tests.
* Understand the context of your work.
	* How does it fit into the wider architecture?
* If it smells - say so!
 
### 2014 Strategic Initiatives

* Agree on concept / principles for platform strategy including platform governance model
* Create a TRTA software architecture, design and development playbook for new SAAS dev
* Identify platform strategy blue chips and timeline
* Begin incorporating playbook guidance in active projects (Blue Moon, SMART already in progress) 
* Develop enabling technology to be able to use TR platforms in TRTA. 

#### Technology ELT Shared Objectives

They will support TRTA's platform strategy by:

* Supplying architecture resource to build the architectural "Playbook"
* Not building any redundant capabilities that already exist in TR
* Identifying 2 capabilities from your product set that can be abstracted as shared capabilities and commit to a implementation plan to deliver it via services.
* Identifying 2 existing capabilities in TR/TRTA that can be re-used in your product set and commit to an implementation plan to leverage them. 

### Engineering Excellence

What is our current level of engineering excellence? T&A has been and remains a fast growing business with high profit margins. We beat our competitors far more than they beat us. Financial success is directly related to the technology. Still, we need to increase the pace at which we improve our engineering excellence

#### Why Engineering Excellence?

It is foundational. It provides us a strong foundation for 1) demands and expectations of customers 2) business growth objectives 3) platform and architectural goals. The way we are working today will not scale to support these objectives. We need the platform strategy. Must start creating and consuming reusable capabilities. 

#### What is Engineering Excellence?

> "We are what we repeatedly do. Excellence, then is not an act but a habit." - Will Durant (after Aristotle)

If we focus too much on engineering excellence we fail, if we do not focus enough we fail. Must find a balance on this spectrum. 

Engineering excellence **is**:

* delivering non-functional requirements
* an occasional 'slow down' to improve something that has quick payback
	* must acknowledge that improving engineering abilities will temporarily slow us down. We also must be able to explain this.
* continuous and ongoing throughout your career
* our fiduciary responsibility
* fire prevention

Engineering excellence **is not**:

* license to become theoretical over engineer
* delivering less value to the customer
* a face-off with your business partners
* improving everything all at once

#### Examples of Engineering Excellence

* DRY, SOLID, YAGNI
* Security
* Agile / TDD
* TRAMS
* Sonar
* Profiling and Load Testing
* Disaster Recovery
* Automated Testing and Deployment
* Open Source software management

#### Engineering Excellence in 2014

* Tech Conferences! (Beyond The Edge)

**Target date:** Mid-September 2014

Location | Exec Sponsor | Key Contacts
--------|-------------|-------------
Ann Arbor | Brian Vroom | Don Bruey, George Cousineau
Carrollton | Tim Rendulic | Derek Lane, Kamalesh Donthula
NYC Metro | Stan Guzik | Lavinia Badea, Carl Brandt

* TRTA Engineering Excellence Award
	* Will be application based, must submit your team's work for approval
* **Your goals** - choose items appropriate for your group's situation

### Drive Innovation

> **Innovation:** Method for introducing something new or different. Often radically different. 

#### Types of innovation

* Product
* Operational
	* Profit model, processes (TDD), structure of organization
* Experience
	* how you service customer, sales channels, brand management, customer engagement

Operational and Experience are often cheaper to implement. 

#### Benefits of Innovation Process

* Improve customer experience
* Drive new sales and products
* Improve efficiency
* Provide competitive advantage
* Increase business agility
* Opportunities for employee growth
* Increased job satisfaction

#### TR Innovation Initiatives

* Innovation Champions
	* group of senior leaders that are looking for ways to incorporate and implement
* Innovation Network
	* HUB group for evangelists and discussion
* Catalyst Fund
	* provides money for people to put their innovative ideas to practice
* Senn Delany Culture Training
	* many aspects, about producing culture that is more supportive and open to innovative ideas
* MIT Media Lab Partnership
	* access and input to research they are doing (i.e. data visualization)
* Startup-Fund
	* seek money to partner with startups for mutual benefit. 


#### 2014 Innovation Strategic Initiatives

* TR Challenges
	* Crowd-sourcing internal 
* UX Certifications
	* way for teams to engage with UX group to get certified. Piloted with a couple of teams, including ONESOURCE Income Tax. Contact  [Bianka McGovern](https://thehub.thomsonreuters.com/people/0087286/content)
* FEF Training/Champions
	* way to accelerate knowledge sharing and capabilities of FEF
* EDGE Lab
	* A team to research and build proof of concepts for new technologies or processes.
	* Consist of 1 leader + 3-4 team members
	* Rotating membership of ~6 months each
	* Successful projects get spun back into the business
	* R&D!!
	* A way to recognize and reward individuals
* Emerging Technology Priority Matrix
	* matrix of Expected Benefit (low -> transformational) to Years to Adoption (Less than 2 years -> More than 10 years). Focus on top left hand corner
	* model for how to manage large-scale tech changes.
	* Primary focus in short term are:
		* Enterprise Architecture / Platform
		* Knowledge of Automation / Big Data
		* Modern front-end / HTML5/ Touch
		* User Experience

## Conclusion

The technology function of T&A exists to *unleash the magic of technology* and has developed a functional strategy to support the business' goals and objectives by *building it once with engineering excellence*.

## Q&A

**Q:** If this is to be a priority, why is the first thing we always here the delivery date? This immediately sets expectations and causes shortcuts to be taken. **A:** Must transition and shift effort. Goals will require more time and more allocation of resources. Challenge from a cultural perspective, but have support from the top. Battles will probably be fought along the way on a project-by-project basis.

**Q:** If a product has existing code that is not expecting to be updated, and there is a service created to replace that, will old products be changed to consume the new service? **A:** We will incrementally make changes of this nature. This sounds like a good opportunity to do so. Also consider "follow the money". If an old product is costing us a lot of money in support resources, it will be prioritized for consumption. 

**Q:** How do we plan to get consensus from so many people before beginning implementation? **A:** We have examples in other business units that have done a fantastic job, legal is a great example. Legal is decades ahead of us in terms of thinking about SOA and architecture. Starting point seems daunting, will require cultural changes, need to incorporate non-functionals. 

**Q:** How do I participate in the EDGE Lab? **A:** Tech ELT will nominate people to be considered for the EDGE Lab. 

**Q:** Does this strategy apply to desktop applications? **A:** This is a web application strategy. The question I would ask is when are you moving your desktop application to SAAS? The project should and will be moving this direction.

**Q:** Sometimes teams feel handicapped by the notion that a request doesn't provide enough business value. How will this be handled? **A:** This goes back to cultural changes that need to take place. All projects in flight right now were planned before the strategy was implemented. This will apply to future projects. We will be interrogating what services you are building? This will dictate the level of time paid to engineering excellence. For ongoing projects, compromises may need to take place.