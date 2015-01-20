FederalTaxAdjustments is integrated with mapper, has to communicate with provision. This follows a synchronous model and we should be able to do the same for Calendar integration. 

ESActionItems are generally used when the user needs to accept or acknowledge. These are typically updates to tax laws that they can choose to accept or ignore. Binod believes process alerts should be read-only and should not be acknowledgeable or acceptable. 

For the synchronous update process - display a message to user in a message box. If successful we can indicate so. If there are errors then the message box would indicate to check the Processing Alerts screen for further information. This way we should not have to modify the UI to incorporate some sort of 'new' alert notification.

Filing Group is the root level object for locking. Every entity is associated to a filing group, even if they are only selecting one entity to lock. Raises the question - what are we sending to calendar? Will it need to include the FG/Entity/Jurisdiction/PayPeriod for each entity we are locking?

For logging purposes the key would be Filing Group -> Entity -> Jurisdiction -> PaymentPeriod. How should updates work? Are we making multiple calls for each entity, or is it batch? Does the filing group fail/succeed as a batch or will we have multiple success/failures on an entity per entity basis?

ESFilingGroupCalculationLog might be able to support logging. One idea is to have a details table that associates back to the ESFilingGroupCalculationLog. Another thought is that this is different enough that it might make sense to have completely separate schema - i.e. ESFilingGroupLockLog. This keeps the concepts clean. 

----------

For example of existing synchronous GWS usage, see ESFederalTaxAdjustmentsViewModel in the ESFederalTaxAdjustments project. This calls the DomainService.GetESTaxAdjustment method, which itself calls the ESFederalTaxAdjustmentsModel.GetESTaxAdjustment method. This method delegates the GWS call to the ProvisionsData.GetProvisionData method. 

This class is found in the following TFS location: $/OnesourceIncomeTax/RS/trunk/Build/dotNet/Reference Libraries/ProvisionsLibrary
