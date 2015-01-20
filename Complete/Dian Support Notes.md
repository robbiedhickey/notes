
# Dian Support Notes

* Rotating pager among 14 people, keep it for a week
* We receive emails and have 30 minutes to respond
* Use the TTA_HEAT application to log support information and feedback
* First thing you do is click 'acknowledge' to let support know you have responded. Within 30 minutes you must acknowledge!
	* after 30 minutes the director is notified

There are many accounts within the application, we have a TTAX account. Those accounts have unlimited rights. 

Once you login to TTAX you can select the user's account that pertains to the ticket. Not al clients will show up here (confidentiality). 

Can check to see if product has expired?

We have a backdoor to query production data, but do not have direct acesss to environment. The Maintenance/support screens has a query window. You need to prepend 'SELECT 1' to all queries. 

Use the Binder Usage Report ( http://rswiki.int.thomson.com/Binder%20Usage%20Report.ashx ) if they want an audit trail. If they want to see deleted binders ( http://rswiki.int.thomson.com/Deleted-Binders-Report.ashx )  

Our wiki is http://rswiki.int.thomson.com/InSource%20RS%20Main.ashx 

When you are done with the support ticket, click the stoplight icon and categorize the ticket. You can also attach documents.

## 04423954

Was not able to see + sign for years underneath binder. Usually this is due to an expired license. It can also be a permissions issue. But all of these are correct.

In this case it turns out that under Home -> 'Display Options' the user had deselected National Binders. This prevented them from being displayed.

This case should not have been escalated and should have been handled in support specialist.

## 04426435

User cannot compute TAS data. When they click the compute button nothing happens. 

First - we need a ticket to return a better error message. Nothing is dispalyed ot the user.

We need to use Undercover to troubleshoot this.

Dian used his MGMT account to log into web server. There are 4 web servers in production, can use the help screen to determine which instance your session is running on. Once you find the name, you can use RSWiki to look up the fully qualified name. 

You will use this name to look up error messages in Undercover. First checks errors, then checks warnings. 

In the view source of the screen there is a binderID that you can use to prune the logs (Use Find dialog to filter). 

This one indicates that the proc_GetBindersForOSATransferAndSetPending failed, but there is no error message. OSA is another product outside of OIT. This is an integration problem, we are pulling data from outside OIT. 

Dian opened the code and searched for the proc name. It is called from ComputeClass.asa. It turns out that the first record returned was null. So the proc did not return valid data, not actually an error. 

We need to run the proc in SSMS to see what is going on. 

It turns out they are missing a Mapset record for 2013. The error message should probably be more clear about this.

If 'Update Organizer with ONESOURCE State Appointment' checkbox is checked, then this could occur. This means we can let the client know that they need to create a Mapset record. 

