## Dagger Deployment Overview

While OIT owns the Dagger code-base, we are not in control of the release process for this module. We will need to coordinate with the ONESOURCE Workflow Tools team to propagate changes to this module into DEV/PD/SAT/PRODUCTION. 

The deployment package for code will need to mimic the structure of the application found in $/WorkflowTools/Dev/LoneStar/OneSourceTax. This is the hierarchy for our release packages, assuming a release number of 2014.005:

<pre>
└───2014.005
    └───Web
        └───OneSourceTax
            ├───App_Code
            │   └───Dagger
            │           DaggerSystemService.svc.cs
            │           IDaggerSystemService.cs
            │           Reference.cs
            │
            ├───Bin
            │       DaggerXmlBroker.dll
            │
            └───ClientBin
                    DaggerUI.xap
</pre>

There is a deployment script that will build this hierarchy for you, but it is subject to change. 

### Code Deployment Process

1. Make any necessary changes to the code base and check them in. It may also be necessary to wait for the TFS build to check in the latest binaries.
2. Get latest if TFS updated the binaries
3. Execute the BuildDaggerWebDeploymentFolder.bat file in a VS Command Prompt. It takes one parameter for the Release Number (ex. 2014.005) that it requires to build the folder hierarchy for the deployment package. 
4. Go into the Package directory and copy the {{Release Number}} folder into the following location: $/WorkflowTools/CodeFreeze/OIT/
5. Check the newly created folder into TFS
6. Send an email to the following distribution group for the deployment request. Include the environments and timelines: TRTAWorkflowTools.AppSupportandReleaseTeam@thomsonreuters.com 

### Database Deployment Process

Data only changes are considered data-fixes for the WorkflowTools team and so follow a different process. 

1. Make any necessary changes to the Control Data in $/OneSourceTax/Visual Studio 2008/Dagger/Database/Control Data
2. Attach the change to an email and send it to the following distribution group for the deployment request. Include the environments and timelines: TRTAWorkflowTools.AppSupportandReleaseTeam@thomsonreuters.com  


### Contacts

If you have any questions, our primary contacts on the WorkflowTools team are:

* Kuruba, Vikram (Tax&Accounting) <vikram.kuruba@thomsonreuters.com> - Code changes
* Mukesh, Abhinav (Tax&Accounting) <abhinav.mukesh@thomsonreuters.com> - Code changes
* Vanderlip, CJ (Tax&Accounting) <christopher.vanderlip@thomsonreuters.com> - Database changes
* Setty, Srinivasulu <srinivasulu.thirumalasetty@thomsonreuters.com> - Database changes