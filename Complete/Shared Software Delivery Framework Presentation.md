# Shared Software Delivery Framework Presentation

Presentation can be found [here](https://thehub.thomsonreuters.com/docs/DOC-794687).

What tools are people using across the organization across the SDLC? We have around 250 groups across the organization and that results in a lot of tools. It makes it different to integrate the tools, collaborate and report. 

The Shared Software Delivery Framework (SSDF) aims to address this. There is a nice diagram that represents it. It is a federated model because different services are run by different groups.

The eventual goal is to have continuous integration across all of these services. 

![SSDF](http://i.imgur.com/xZ1E5Zw.png)

* Requirements MGMT
	* TFS
	* Jira
* Task MGMT
	* TFS
	* Jira
* Source code MGMT
	* SAMI (Software Asset Management Integration) - investigating git
	* TFS
* Build 
	* PG CI Service (based on jenkins)
	* TFS
* Defect MGMT
	* TFS
	* Jira
	* TDM
* Test Automation
	* TEX, ATF, Selenium, Perfecto
* Deployment
	* TRAMS (Thomson Reuters Application Management Service) Mona
	* CA Lisa
	* Puppet
* OSS Compliance
	* Protex
	* OSSAT
* Load & Perf Testing
	* Perf Center, Shunra, JMeter


### Underlying all of this is the 'Unified Software-Delivery Information Set' (Key Data Model)

What is the essential underlying data for builds/deployments/etc?

Internal metrics can be seen at [delivery intelligence](http://dialpha.int.thomsonreuters.com/di/vis/home/index.html). Summary of all applications across divisions and their related application security scan data. Also has a build graph that associates checkin with the amount of time before it is built. 





