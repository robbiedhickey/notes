## FEF Problem Statement
* Diverse tech stacks due to acquisitions
* Siloed development groups aren't aware of each other's efforts
* Shannon's 5 year Road Map aims to reduce the cost of development by implementing common platforms to 1) reduce diversity and 2) increase shared knowledge

https://thehub.thomsonreuters.com/docs/DOC-651087

Silos are being broken down by common platforms. Shannon Ulmer (T&A CTO) has outlined a [5 year roadmap](https://thehub.thomsonreuters.com/people/0066043/blog/2014/04/23/webcast-recording--trtas-5-year-technology-strategy-presentation) to break down these barriers and increase mindshare.

### Standardized Front-end Development. But How?

* Current web apps are tightly coupled with the back end. We need to decouple the back end to create a pure front-end platform
* Current efforts gives us the power to decouple front and back ends but...
* The next generation (web components) will be the real change and this is how we're preparing for it. Allows you to define and import custom HTML elements. Web components follows a lot of angular conventions, so we will be ready for it.
	* ex. [Polymer](http://www.polymer-project.org/?repost) and [Brick](https://hacks.mozilla.org/2013/08/introducing-brick-minimal-markup-web-components-for-faster-app-development/)

AngularJS has done the best job so far of being able to treat the browser as a true front-end/client. Contains necessary business logic on the client. 

TR is being pro-active this time around. We want to get ahead of trends before they start. We believe web components will be the future of web development and angular knowledge will position us for that moving forward. 

### Native Web Development (HTML/CSS/JAVACSRIPT)

Native Web Development allows us to drive down the cost of...

* Scale - huge development community
* Ubiquity - it runs everywhere
* Openness - Ability to view source, open coding, github, jsfiddle
	* we are going to solve very hard problems due to the sheer volume of people paying attention to these problems

### Creating a Shared Platform Together

If we get everyone on board, David believes that we can increase productivity by an order of magnitude, because the frameworks and patterns are so good and solve common problems. 

## What is the Front-End Framework?

* 3rd Party Libraries
* Recommend Best Practices
* Glue Code (80% libraries, 20% our code)
	* Custom angular directives, etc
	* Conventions for routing, unit testing, etc
	* Best practices for creating a grid
	* Seed/Bootstrap project
	* Reference applications
* Internal Community
	* Place to ask questions, learn and transfer knowledge

### Categories to Address

Boilerplate | Authoring Abstractions | Frameworks | Iteration Workflow | Performance Tuning | Build & Optimization | Deploy
----|----|----|----|----|----|---|
Templates | CSS PreProcessor | MVC | IDE | Profiling | Minifiers | VCS
| Javascript PreProcessor | Dependency Injection | Unit Testing | Tracing | Concatenation | Deployment Tools
| HTML Markdown | UI Controls | Regression Testing | | Optimizers | |
| | Look and Feel Standards | Test Runner | | |
| | DOM Manipulation | Browser Harness | | |
| | Security Frameworks | QA Lab | | |
| | Module/Package Mgmt | Linter | | |
| | Shims | Browser Dev Tools | | |

Learning & Community |
---------------|
Reference Implementation, Documenation, Books, Internal Community, Open Web Community, Blog Posts, Paid Support |

### Organization

Plan is to have a central nucleus of evangelists, that communicate with the business units. Two-way communication for migration upstream into the central repository. Plan is not to steal your team members.

## AngularJS

David recommends a presentation that ComponentOne gave on AngularJS: [Click Here](https://tlr.webex.com/tlr/lsr.php?RCID=cb8378cc805432176906749913df4682)

Angular concerns are bound together by the $scope, which allows the M-V-C pieces to share data. 

TR is looking to contribute to open source projects!

The TR grid is re-usable and takes columnDefinitions and gridData. It uses jquery DataTables on the backend, which does not support dynamic columns. The workaround for this is that we defer rendering of the grid until we have the data, and if the columns need to change, we destroy the grid and re-render it. 

Also have TR chart controls. Was not able to find angular directive map controls so we use [jVectorMap](http://jvectormap.com/) and [FlotChart](http://www.flotcharts.org/). David wrote a custom angular directive for the map in ~30 minutes. 

## Q&A

**Q:** Is this framework cross-browser compatible? **A:** Targeting back to IE8. 

**Q:** Who is using node.js? **A:** The BentoUI and Front-End Framework groups. Probably more. Unit-testing and linting is also executed via node on grunt. 

**Q:** Are we looking at the other control suites out there? **A:** The intention is to look at everything, but he gets something new on his radar every ~3 days. Primary differentiator will be angular support.

**Q:** What is the relationship between FEF and BentoUI? **A:** Both were created and report to Michael Henderson. Intention is for full integration with BentoUI components. Some complaints about ComboBox and other BentoUI components that were developed using pre-angular tech. Over time it is likely that they will combine into one framework. 

**Q:** What is BentoUI? **A:** Based out of Manhattan office, web designers and expert JS programmers. Creating a unified corporate look and feel. 

**Q:** If your team is still working on dated tech, how to get buy in? **A:** Set up a call with your management team. David will convince them!