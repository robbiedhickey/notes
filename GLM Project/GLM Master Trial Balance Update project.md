# GLM Master Trial Balance Update project

GLM has provided us with a new proxy, and the only breaking change I see is the GetAllMasterTrialBalancesByYear. Pagination was added and the signature has changed from:

```csharp
// from
public string GetAllMasterTrialBalancesByYear(int year)

// to
public string GetAllMasterTrialBalancesByYear(int year, int page, int pageSize)

```

The proxy class will need to be updated in the following locations, broken down by concern:

### LIB 
* $/OnesourceIncomeTax/RS/trunk/Build/dotNet/Services/GLMWebServiceCaller
	* no dependencies in project

### TEST
* $/OnesourceIncomeTax/RS/trunk/Build/dotNet/Services/IncomeTaxWebServiceTestLib/IncomeTaxWebServiceTestLib/Proxies
	* will break $/OnesourceIncomeTax/RS/trunk/Build/dotNet/Services/IncomeTaxWebServiceTestLib/IncomeTaxWebServiceTestCaller
* $/OnesourceIncomeTax/RS/trunk/Build/dotNet/Services/Test/TestGLMWebServices/TestGLMWebServices
	* will break same project
* $/OnesourceIncomeTax/RS/trunk/Build/dotNet/Services/WebServiceTestHarness/WebServiceTestHarnessLib/Proxies
	* will break $/OnesourceIncomeTax/RS/trunk/Build/dotNet/Services/WebServiceTestHarness/WebServiceTestHarnessCaller

### APPLICATION

These are not the proxies but the consuming application that needs to be updated. There is a reference to the proxy method in a generated code class in $/OnesourceIncomeTax/RS/trunk/Build/dotNet/Silverlight/GLMUpdate/GLMUpdate/Generated_Code but I'm not quite sure how this works yet.

* $/OnesourceIncomeTax/RS/trunk/Build/dotNet/Silverlight/GLMUpdate/GLMUpdate.Web/Services
* $/OnesourceIncomeTax/RS/trunk/Build/dotNet/Silverlight/GLMUpdate/GLMUpdate/ViewModels

Once this is done we will get a better idea of what dependencies we are breaking.

### Adding Pagination Resources

Pagination is not built into the flexgrid by default. This link shows an example of using a PagedCollectionView as a datasource. This link also includes an technique called 'stealth paging' which is basically like infinite scroll, and wouldn't require changes to the UI. Stealth Paging seems to rely on being able to pass a start and end index for the collection, but the GLM only provides us with pageSize and pageNumber variables. So in order to implement stealth paging, I think we would have to continually append to our collection. Hmm. 

* http://our.componentone.com/groups/topic/is-there-any-paging-available-in-flexgrid-for-silverlight/

This link has a non-component one specific implementation using PagedCollectionView

* http://www.codeproject.com/Articles/270972/Paging-Data-from-the-Server-with-Silverlight

MSDN documentation for PagedCollectionView using DataPager

* http://msdn.microsoft.com/en-us/library/system.windows.data.pagedcollectionview%28VS.95%29.aspx

ComponentOne example of stealth paging using DataGrid. Is it adaptable to FlexGrid?

* http://demos.componentone.com/silverlight/ControlExplorer/#DataGrid/Stealth%20Paging

ComponentOne example of stealth paging using FlexGrid for WPF. Maybe similar enough to Silverlight?

* http://our.componentone.com/2013/05/20/stealth-paging-with-c1flexgrid-wpf/

### Examples of Silverlight Pagination in code base

* OneSourceIncomeTax\RS\trunk\Build\dotNet\OneSourceIncomeTaxWeb\StateTIApp
*  OneSourceIncomeTax\RS\trunk\Build\dotNet\Reference Libraries\Silverlight\TASShared\TASShared\PatternsFramework\PagedEntityCollectionView.cs
*  OneSourceIncomeTax\RS\trunk\Build\dotNet\Reference Libraries\Silverlight\TASShared\TASShared\PatternsFramework\PagedViewModelBase.cs
*  OneSourceIncomeTax\RS\trunk\Build\dotNet\Silverlight\EFileStatus\EFileStatus\ViewModel\EFileViewModel.cs
*  OneSourceIncomeTax\RS\trunk\Build\dotNet\Silverlight\EstimatedPayments\ESAuditTrail\ESAuditTrail\ViewModels\ESAuditTrailViewModel.cs

And more. Just search for uses of PagedCollectionView



