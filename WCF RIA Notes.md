## Overview

### What is WCF RIA Services

* Goal is to simplify development of n-tier rich internet applications
	* accessed via the internet
	* rich client with real client-side business logic
* Business application focus
	* Accessing and handling data
	* Decouple resources, logic and presentation
		* data sources evolve
		* business ruels evolve
		* new types of clients over time

### Problems Addressed

* Addresses lack of a middle tier in many thin-client applications, connects middle tier to client across network boundaries
* Need to share some logic between the client and the server - evolves over time yet must stay in sync
* Asynchronous communication
	* overcome latency and disconnected scenarios. 
	* keeps UI from freezing for long-running operations
* Handling data retrieval and update across tiers
	* change tracking and concurrency issues
* Validation, errors
* Ensure secure access

### RIA Services' Answers

| Challenge | Solution |
| ----------|----------|
| Building a middle tier | DomainService (WCF) LINQ |
| Sharing logic between tiers | Design-time tools |
| Asynchronous Communication | WCF infrastructure, DomainContext |
| Handling data retrieval and update across tiers | LINQ Integration (IQueryable), WCF Infrastructure (Serialization), Silverlight Libraries |
| Cross-tier validation, error handling | Partial classes and metadata, Code sharing, Invoking Service operations directly, Silverlight INotifyDataErrorInfo |
| Security and access control | ASP.NET Membership Framework |

* DomainService
	* under covers implemented as WCF service, acts as data layer through LINQ
* DomainContext
	* client-side representation of the DomainService

### The WCF in WCF RIA Services

* Dlls, tools, services
	* RIA includes runtime APIs, design-time tools built into VS and services to ease development of Silverlight applications.
* Single logical application 
	* in reality split by network boundaries, but RIA abstracts that away. 
	* Client (UI/Application Logic) <=> Server (Application Logic/Data Access) - code sharing, communication managed by WCF
* WCF plumbing

![WCF Ria Services Diagram](http://i.imgur.com/PwTX37N.png)

All of these pieces are held together by a pattern prescribed by the framework. Thus, there are design guidelines for using RIA. 

### The built-in design pattern

Create a solution with a concept of a server project and a client project. 

![wcf ria design pattern](http://i.imgur.com/GaH1pJw.png)

When you generate a silverlight project with RIA services enabled, you will have a Web and App project. The Web project will need to define your data layer (EF, LINQ2SQL, etc). 

It will also need to define your Domain Service class, which derives from DomainService.

```csharp
// instructs tooling to generic DomainContext client-side code
[EnableClientAccess()]
public class BusinessDomainService : DomainService
{
	// this would be your EF repo or whatever you are using for data access
	private BusinessDataStore = new BusinessDataSource();

	public IEnumerable<Member> GetMembers()
	{
		return data.MemberList;
	}
}

public class Member
{
	// every model must have an identity property
	[Key]
	public int MemberId {get;set;}
	public string FirstName {get;set;}
	public string LastName {get;set;}
}
```

When you build, this will EnableClientAccess will cause the tooling to create a Generated_Code folder, which will include a *.Web.g.cs class that is generated from what we defined in the DomainService. If you inspect this file you will see the client-side DomainContext, as well as other helper classes needed by the framework. The DomainContext references an WCF svc file that was generated for us on the server. Our GetMembers() method should also be present as GetMembersQuery(). You will also find your Member model class in the output, derived from Entity. 

You will also see the ServiceContract classes needed for WCF plumbing generated. 

If you want to see your generated WCF service you can find it at the following location. All dots should be replaced with dashes:

> http://{baseUrl}/services/{project name}-{domain service class name}.svc


### Common Silverlight Scenarios

Now that our WCF services have been generated, let's look at a few scenarios. 

#### Querying

Note that the server DomainService is stateless while the client DomainContext is stateful!

Here is how you can wire up your client to use the DomainContext generated from the server side DomainService:

```csharp
public partial class MainPage : UserControl
{
	// client-side generated domain class
	BusinessDomainContext ctx = new BusinessDomainContext();

	public MainPage()
	{
		InitializeComponent();
		// project query
		EntityQuery<Member> query = ctx.GetMembersQuery().Where(m => !m.FirstName.Contains("M"));
		// LoadOperation is async
		LoadOperation<Member> lo = ctx.Load<Member>(query);
		// DataGrid in XAML
		MemberGrid.ItemsSource = lo.Entities;
	}
}
```

#### Changing Data

The DomainContext contains an EntityContainer class which contains all of the objects in our data model. This is what makes the client stateful. It also gives you change tracking.

We make changes to our local data, and can accept or rollback changes prior to submitting to server. When we are ready we can call SubmitChanges() on the DomainContext. The DomainService will then perform Authorization, validation and persistence. It will succeed or fail as a single unit of work.

This example assumes a grid on the left and a grid on the right. The left grid is initially loaded from the server and can be edited, and the right grid is loaded again from the server when the user clicks a load button. This will demonstrate the 'disconnect' between the client and server. 

```csharp
public partial class MainPage : UserControl
{
	// client-side generated domain class
	BusinessDomainContext ctx = new BusinessDomainContext();

	public MainPage()
	{
		InitializeComponent();
		EntityQuery<Member> query = ctx.GetMembersQuery();
		LoadOperation<Member> lo = ctx.Load<Member>(query);
		MemberGridLeft.ItemsSource = lo.Entities;
	}

	private void ButtonsEnabled(bool b)
	{
		btnAccept.IsEnabled = b;
		btn.Reject.IsEnabled = b;
	}

	private void btnAccept_Click(object sender, RoutedEventArgs e)
	{
		ctx.SubmitChanges();
		ButtonsEnabled(false);
	}

	private void btnReject_Click(object sender, RoutedEventArgs e)
	{
		ctx.RejectChanges();
		ButtonsEnabled(false);
	}

	private void btnLoad_Click(object sender, RoutedEventArgs e)
	{
		var ctx2 = new BusinessDomainContext();
		LoadOperation<Memeber> lo = ctx2.Load<Member>(ctx2.GetMembersQuery());
		MemberGridRight.ItemsSource = lo.Entities;
	}

	private void MemberGridLeft_RowEditEnded(object sender, DataGridRowEditEndedEventArgs e)
	{
		ButtonsEnabled(true);
	}
}
```
#### Validation

Common validation rules can be applied via data annotations to server side entity classes. Can also be applied via Metadata types. You can also create a custom validation class for a type. You can propogate these to the client by using *.shared.cs as the filename. 

```csharp
public class Member
{
	// every model must have an identity property
	[Key]
	public int MemberId {get;set;}
	public string FirstName {get;set;}
	
	[Required]
	public string LastName {get;set;}

	[Range(30, 90, ErrorMessage="Sorry, you are too old or too young.")]
	public int Age {get;set;}
}
```
When you compile these annotations will be included in the client-side data model. The data grid automatically knows how to display validation errors and takes care of that for you!

#### Securing Access

Framework provides an Authentication Domain Service which encapsulates authentication, authorization and user profile features. By default it relies on ASP.NET Membership provider but can be customized via extensibility points. 

Just like everything else, this is handled in an integrated manner on the client and the server. There is a User class on the client (proxy) and server. There is also a corresponding Authenitcation DomainContext class on the client. 

A WebContext class is also generated on the client. It provides access to the user class which holds user information received from the server. The AuthenticationService exposes methods to the client for authentication, and uses the AuthenticationDomainContext to proxy calls to the AuthenticationDomainService.  

Operations in the DomainService can be marked with the [RequiresAuthentication] attribute to incdicate that the current user must be authenticated. 

To enable authentication:

```xml
<!-- web.config -->
<configuration>
	<system.web>
		<authentication mode="Forms" />
	</system.web>
</configuration>
```

```csharp
[EnableClientAccess()]
public class BusinessDomainService : DomainService
{
	// this would be your EF repo or whatever you are using for data access
	private BusinessDataStore = new BusinessDataSource();

	[RequiresAuthentication]
	public IEnumerable<Member> GetMembers()
	{
		return data.MemberList;
	}
}
```

But how do you expose the authentication service to clients so they can login? Must create the AuthenticationDomainService. This will generate the client-side User which can be accessed through the WebContext class:

```csharp
[EnableClientAccess]
public class AuthenticationDomainService : AuthenticationBase<User>
{
	
}

// for user profile purposes
public class User: UserBase
{

}

public class MainPage : UserControl
{
	...

	private void btnLogin_Click(object sender, RoutedEventArgs e)
	{
		FormsAuthentication auth = new FormsAuthentication();
		auth.Login(new LoginParameters(tbUserName.Text, tbPassword.Text,
			delegate(LoginOperation op)
			{
				if(op.HasError)
				{
					tbStatus.Text = op.ErrorMessage;
					op.MarkErrorAsHandled();
				} 
				else
					tbStatus.Text = op.LoginSuccess ? "Logged in" : "Login failed";
				
			}, null);

		btnLogin.IsEnabled = false;
	}
}
```



## Exposing and Consuming Querying Services

## Updating Data

## Business Logic and Validation

## RIA Services and Visual Studio