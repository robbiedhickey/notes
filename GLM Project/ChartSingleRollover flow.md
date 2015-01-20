# ChartSingleRollover.asp

This is a rough outline of the steps that this page walks through for processing a chart rollover. 

## Validation 
Calls xml_GetTCCRolloverValidationResults, which given a chart id, determines if there are any invalid TCC records. Invalidity is determined by the TCC_Number having greater than 3 digits. If there are TCC records with more than 3 digits then we disable the rollover button. 

## Rollover Process

On click of the rollover button, page calls CreateRolloverXml() function, which builds an XML payload to hand off to the rollover process.

### CreateRolloverXml()

If the chart type is FTJ and ChartYear querystring variable = 0 then we call UpdateAutomicAdj() which sends the UPDATEAUTOADJ xml to chartprocess.asp. **This should not apply for us since we can reject the payload if they do not specify a year.**

If the chart type is any of the following: "FCA", "FBJ", "FRJ", "FMJ", "FCJ", "CTDM", "ICA", "SM3M", "SM3C", "FTJ", "IXRF", "MXRF", "PK1C", "PAAC", "DCA", "TCC", "SFXR", "FPK1","NAIC", we call  create an xml journal, process the xml journal, and call ProcessSingleChartRollover(). The xml journal functions are defined in XMLJournal.asa. 

If the chart type is "ARR" we call DoChartRollover()

Else we announce to the user that the rollover they are requesting will be coming soon. 

### XmlJournal Explanation

$/OnesourceIncomeTax/RS/independent/WIP/Documents/XmlJournalUsage.doc has explanation. After reading that, you will see the method that will eventually be called for this screen is spRollover_Charts.

### ProcessSingleChartRollover()

This calls SendXMLToGP() function which is defined in XmlJournal.asa. This function is explained in the XmlJournal document. The function will call spRollover_Charts to do the actual work. It then parses the return string to do error handling. 

If the return value is -1 then the proc was not called correctly and nothing executed, we do not have to do any further work.

If the return value is some other non zero value then we have cleanup to do. spDeleteCharts is called via the XmlJournal conventions. 

### spRollover_Charts

> Vamsi Kiran seems to have the most experience with it

spRollover_Charts accepts a number of parameters, and 4 of them do not have default values, they are:

1. Chart Id - IPCH_ROLLOVER_CHART_ID
	1. **Will be passed by GLM**
2. Chart Object Type - IPVC_OBJECT_TYPE
	1. **Can we infer this from the Chart_ID?**
3. User ID - IPVC_USER_ID
	1. **ONESOURCE ID. Is this the correct user id?**
4. From Year - IPSI_FROM_YEAR
	1. **Will be passed by GLM**

### spDeleteCharts

This has not been modified since the 2009 TFS migration (both the stored proc and the UI if branch that handles this case), and I question if the parameters line up anymore. The UI passes 5 parameters in the following order: ChartType, UserId, FromYr, OtherChartId, RSCS. The proc uses optional parameters, but the first 5 are: IPVC_DELETE_CHART_ID, IPVC_OBJECT_TYPE, IPVC_USER_ID, IPSI_FROM_YEAR, IPVC_OTHER_CHART_ID

Is it possible that the condition where the delete procedure would fail and execute has not happened in many years? Are there logs I can use to verify this?

## Follow up questions

* Do we need to handle every chart type (namely AAR)? Put another way: can any chart type be a 'master' chart of accounts?
	* **According to the three GWS procedures that GLM is calling (xml_GWS_GetPrimaryCOA, xml_GWS_GetPrimaryCharts, xml_GWS_GetPrimaryAccountsForChart), they all have where clauses with Chart_Type = 'FCA'.** 
	* In that case, can I hard-code FCA?
* How do we get the chart type? GLM has no knowledge of it unless we begin returning that as a field in our PrimaryCharts payload. 
* Where is the GenericWebService.asmx hosted? 
	* Also, I get configuration exceptions when running what is checked in locally. What is the preferred method for testing?
* Is spDeleteCharts still in use? If errors occur this is called, but it has not been modified since before the 2009 CS/RS migration and the parameters don't seem to line up anymore?
	* Are there logs that I can check for its use?
* Should I stick with the same approach of the GenericWebService if I'm making multiple calls? For example, I may need to make a validation call, and a call to retrieve the chart type, as well as the call to do the actual rollover. 
* GLM is passing us a ONESOURCE username for audit purposes, and our rollover proc requires an OIT userId. If null is passed an the rollover will fail. If you pass an empty string or invalid username, then the procs will assign the username as an empty string and log it to the log_message table. 
	* I was thinking that one way to get around this would be to default to the passed in userId rather than an empty string. We could even append '_GLM' or something to it. It seems like it would be more useful from an audit standpoint, unless an empty string has some inherent meaning. 
* Hosting?
	* Where is it hosted?
	* trouble hosting this service locally due to it targeting .NET 4 but having web.config references to .NET 3.5 which are now part of the machine.config file. http://stackoverflow.com/questions/3387322/iis7-deployment-duplicate-system-web-extensions-scripting-scriptresourcehandl
		* I can fix this with the suggestions in the SO post
	* Also, many of the DLL deps were not in the bin, I'm assuming the build process resolves these?
	* Might need to ask Matt about the deployment process. 


 