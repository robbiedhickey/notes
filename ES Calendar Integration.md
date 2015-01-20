# ES/Calendar Integration 

Tracking document for integration planning and backlog management.

Get with Shawn Bolton on Sankar's monday meeting to transfer information. 

## Backlog Planning

These proposed user stories include items from both the OST and OIT side. For many of the OST stories, resources on the OIT side may need to be involved to transfer knowledge. 

* Entity Manager Dagger Integration (OST & OIT)
	1. Update MapSystem and MapType info in OSP DB with new Entity Manager items
		* TFS scripts: $/OneSourceTax/Visual Studio 2008/Dagger/Database/Control Data
	2. Extend OIT GWS instance to include the specific procedure for newly mapped items
* Entity Manager GenericWebService Implementation (OST)
	1. Stand up instance of GWS on Entity Manager
		* Template GWS code? Our instance has OIT specific code. Will EM simply remove the irrelevant parts?
	2. Leverage OITMapProcessor library in GWS instance (IContract implementation)
		* Will changes be required for EM (ex. connection strings)? The operations themselves should be generic and remain unchanged. 
	3. Create EMLibrary (another IContract implementation) to handle application specific map data
		* These are defined by the MapType records, which themselves define an operation contract that will return the application specific data for a source or target map
		* ex. in OIT - xml_GetEntitiesForMapper
	4. Create mapping schema in target DB (tables and procs)
		* TFS scripts: $/OneSourceTax/Visual Studio 2008/Dagger/Database/MapStorageSQLDB 
* Calendar Web Service Development (OST)
	1. Determine data required and payload format
	2. Development of PostDataToCal endpoint
* Estimated Payments Integration (OIT)
	1. Identify location(s) in ES code base where we will need to publish events to Calendar
	2. Based on Calendar service payload, ensure we have all necessary data that the contract requires
	3. Acquire or develop rest client for communication with Calendar service
	4. Introduce rest client into ES application and make calls based on information from step 1
* Dagger Deployment (OIT)
	1. Identify stakeholders
	2. Document process
	3. Cross-train OIT members
	4. Potentially automate or improve. 


## Standup Meeting Notes

* In calendar people create events based on a template
	* pick an entity and a year, then pick a template
	* Based on a template, there are due dates
* ex. of preliminary mapping "NJ ES 1120" (two digit Jurisdiction code, application, return type)


### 11/21

**Outstanding Items:**

1. OIT generation of DueDate
2. Solution for updating the OIT status
3. Timelines

**Realizations:**

* Processing Alerts should only show failed event updates
* Calendar cannot call OIT web service to update status

Mike O'Connor﻿ indicated that the Calendar service cannot have any knowledge of OIT and thus cannot update us of whether a specific event succeeded or failed. Calendar can instead expose an endpoint that would allow OIT to query the status. This could present scalability issues due to the batch nature of the process. Paul stated that as many as 200+ entities can be selected locking with larger clients during the 4th quarter. 

If we stick with a synchronous approach then this isn't really a problem, we can block the UI and calendar can return us the status of the failed events.

If the batch nature mandates an asynchronous process, then we need to come up with a different solution. Polling the calendar status endpoint for 200+ entities to get their status does not strike me as a good idea.

One proposal would be to include a unique identifier when we post to calendar that would represent the batch as a job. Then, the Calendar status endpoint could accept the jobId and report the status of the batch job. The payload could have an enumeration of statuses at the job level, something like:

* Complete - all events updated successfully
* Partial Complete - some events failed
* Processing - in progress 
* Not Found - jobId not found
* Error - something fatal happened, no events updated

The payload would need to return an array for the Partial Complete and Error statuses, which includes the entities that could not be updated. 

### 11/25

* include jonathan pollock for design discussion
* Create design discussion meeting for 12/1. Binod suggested 10:30-11:30
* Attempt to reschedule stand-up meeting for earlier in the day. Praveen and Atul suggested 7:30 but that most likely is not doable from Carrollton perspective as many do not arrive until between 8-9. 

### 12/8 - Binod Meeting

We do find out payment/due date in the voucher print application. Shawn should have a ticket assigned to him. 

Voucher print is just a data collection screen. It goes to a huge proc which actually does most of the work. Somewhere in the proc they are figuring out the due date. 

Check with Mike Roper - he should know where the FormRequestService lives. 

Binod is leaning towards asnychronous for the update since timeouts could be a realistic concern. In that case we will need to have a batch executable. 

### 12/11

* GWS/Mapper - Does it support unicode? 
	* The WebServiceProxy client submits payload as Content-Type: text/xml; charset=utf-8
	* Many of our string fields though are varchar instead of nvarchar so not sure
* GWS/Mapper - Where is the AutoMap stored?
* ES - What happens if the event update fails? Unlock and then attempt to lock it again?
	* Does that require cleanup?
	* Do we unlock the entities programatically?