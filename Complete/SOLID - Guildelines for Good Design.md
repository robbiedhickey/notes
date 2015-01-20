> Presentation is given by Chris Breazeal as part of Beyond the Edge 2013.

## SOLID Overview

### History

Coined by Robert C. Martin from Agile Software Development, Patterns, Principles and Practices. The SOLID Principles weren't all devised by Robert Martin, sometimes referred to as Uncle Bob, but he gathered the concepts together. 

### SOLID Principles

SOLID is an acronym of acronyms, representing five engineering principles:

* **S**ingle responsibility Principle
* **O**pen Closed Principle
* **L**iskov Subsitution Principle
* **I**nterface Segregation Principle
* **D**ependency Inversion Principle

## Bad Design

The goal is to create a good design, but, what is good design? In general, it is a tough thing to agree upon. It might be easier to focus on what constitutes a bad design:

* Rigidity - hard to change
	* could cause cascading bugs
* Immobility - hard to reuse
	* often results in copy/paste
* Fragility - easy to break
	* breaks in unexpected ways in places we weren't expecting
* Viscosity - easier to hack than fix properly
	* difficult to understand the code
* Complexity - needless complexity
	* getting carried away with abstractions. Find a balance. 
* Repetition - duplicate code/bad structure
	* related to immobility
* Opacity - hard to understand
	* goes along with viscosity

### Consequences of Bad/No Design

* High Technical Debt
* Greater chance of product failure
* Difficult to maintain and evolve
* Cost
	* Support calls, bug fixes, etc. Hard to measure but affects resources and reputation as well.
* Code that is not fun to work on


### Overuse of Static Methods

Utility classes that have no cohesive responsibility and are difficult to test.  

### Fragile Base Classes

* Implementation inheritance can introduce fragility
	* Derived classes often depend on base class behaving a certain way
	* If you change the behavior of a base class method you may break the derived class
	* To top it off, the base class change is often an improvement

### Design Stamina Hypothesis

Graph that shows the long-term cost of no design. Y-Axis shows cumulative functionality, X-Axis shows time. Up-front, you can add more features more quickly with no design. But if you expect the lifetime of the product to be long, you are far better off having done good design up front. 

## Characteristics of Good Design

* Loose coupling - a change in one does not necessitate a change in another
	* coupling refers to the amount of interaction between two modules.
	* Tight coupling - a change in one causes a change in the other
* High cohesion - all methods are strongly related in terms of functionality
* Abstraction = Flexible
	* The less you know about how it works the better
	* Program in terms of the abstraction, not the concretions
* Abides by the SOLID principles

### Related Principles

* DRY - Don't Repeat Yourself
	* If you need to repeat then it's probably time to refactor
* YAGNI - You Ain't Gonna Need It
	* Don't build something now because you think you'll need it in the future
	* Often when the growth hits, you have implemented the feature wrong.
	* Now you are wasting time on QA and Documentation
* KISS - Keep It Simple Stupid
	* Don't overcomplicate
* Tell, Don't Ask
	* Ask the object that has the data to do the work for you.
	* Careful: if you are crossing a boundary/concern, then tell don't ask goes too far. 
* Separation of Concerns
	* Design principle for separating a computer program into distinct sections with distinct responsibilities. 
	* Often thought of in terms of layers
* Hollywood Principle
	* Don't call us, we'll call you

### How do the SOLID Principles Lead to Good Design?

* Design is about managing dependencies
	* Proper management of dependencies lead to good design
	* The SOLID Principles help you properly manage dependencies
	* Therefor, application of the SOLID principles leads to good design
* What is a dependency?
	* If you refer to something, you depend on it
		* The 'new' keyword, 3rd party libs, databases, configuration, static methods, system resources, etc.
	* When the things you depend on change, you must change
	* Dependencies tell you how changes are propagated through the system. 

## Single Responsibility Principle (SRP)

> Every class should have one and only one reason to change.

* Responsibility = reason to change
* Each method should also have only one reason to change
	* if your method has more than one if-statement or more than one loop, might need to refactor.
* Results in high cohesion
	* Classes are tightly focused
* Multiple small interfaces (ISP) can help to achieve SRP

Can result in many smaller classes and imposes a lot of structure. But the naming convention becomes easier when a class does one thing, and thus results in functionality that should be easier to find for new developers.

#### SRP Violation Example

```csharp
public class Transaction
{
	public void SaveToDatabase(){}
	public string GenerateHtml(){}
	public string Format(){}
	public void GenerateReport(){}
	public void NotifyCustomer(){}
	public void Validate(){}
}
```

This class has many reasons to change. Persistence, Output, Formatting, Reporting, Notification, Validation. 

#### How to Spot SRP Violations

Write down the purpose of a class. If the purpose has 'and' or 'or', then class has multiple responsibilities. 

ex. "It should download the CSV file from the FTP server **AND** should replace existing directories with new directories" 

* "And" shows multiple responsibilities
* "Or" is worse because it shows multiple unrelated responsibilities

#### Refactoring for SRP

Identify -> Extract -> Test -> Refactor -> Test -> Inject -> Test

#### When to Stop Refactoring for SRP

* Is it DRY?
* Does it have one responsibility?
* Does it depend on things that change less than it does?

## Open Closed Principle (OCP)

> Classes / Modules should be open for extension, closed for modification.

Came from the 1988 book *Object Oriented Software Construction* by Bertrand Meyer.

* Open for extension - the behavior of the module can be extended to meet new requirements
* Closed for modification - extending the behavior of a module does not result in changes to the source of the module. 

NOTE: "Extension" here means more than inheritance!

* You should never need to modify a base class to add a derived class
* Allows us to create systems whose behavior can be extended without modifying the core of the code
* If possible, adding an extension should not cause base class to be recompiled

#### How to Implement OCP

How can you change the behavior of a module without changing the module?

* Inheritance/polymorphism
* Strategy Pattern
* Template Method Pattern
	* Base class utilizes a generic algorithm with abstract methods injected into the algorithm. The template class will call them at the right time. 

```csharp
public double TotalPrice(IList<IProduct> products)
{
	return products.Sum(product => product.Price);
}
```
Since IProduct is an interface and polymorphism is being used, then this class can easily accommodate new types of products without having to be modified. Thus, it conforms to OCP. 

Consider this: as with any project, requirements change. New requirements:

* Carburetors are discounted 5%
* Batteries are discounted 20%

This seems like a simple tasks so it's assigned to the new junior developer, who produces the following:

```csharp
public double TotalPrice(IList<IProduct> products)
{
	var total = 0.0;
	foreach(var product in products)
	{
		if(product is Carburetor)
			total += (0.95 * product.Price);
		if(product is Battery)
			total += (0.8) * product.Price);
		else
			total += product.Price;
	}
	return total;
}
```
This is no longer closed for modification, new types will need to be added to this method. 

#### How to Fix

First, return the TotalPrice method back like it was. Then, introduce a new PricePolicy class that can be used to provide different pricing policies...

```csharp
public class PricePolicy : IPricePolicy
{
	private readonly double factor;
	public PricePolicy(double factor)
	{
		this.factor = factor;
	}
	public double Price(double price)
	{
		return price * factor;
	}
}
```
Now, inject the PricePolicy into the Product...

```csharp
// using the strategy pattern to make the Product class closed for modification
public class Product: IProduct
{
	private double price;
	private readonly IPricePolicy pricePolicy;
	public Product(double price, IPricePolicy pricePolicy)
	{
		this.price = price;
		this.pricePolicy = pricePolicy;
	}

	public double Price
	{
		get
		{
			return pricePolicy.Price(price);
		}
	}
}
```

#### Problems with OCP

* You can't predict the future
* It takes development time and effort to create the appropriate abstractions.
* There is no module that is natural to all contexts.

Sometimes it is better to do the simplest thing first. A good rule of thumb is to refactor to OCP the first time you have to change a class, and refactor the **right** way. 

Also consider the reality, you will never get to a point where all classes are closed to modification. 

## Liskov Substitution Principle (LSP)

> Subclasses must be substitutable for their base classes. 

You don't want your client to be surprised by your subclass. Objects should be replaceable with instances of their subtypes without altering the correctness of the program. 

* Is-A can fail here
* Is-A replaced by Is-Substitutable-For
* Non-substitutable code breaks polymorphism
* Caller of base class should not be surprised by subclass
* The guarantee of the LSP is that a subclass can always be used wherever its base class is used!

#### LSP Violation Example

```csharp
// custom stack class that extends List
public void SomeMethod()
{
	var stack = new CustomStack();
	stack.Push("A");
	AddTheX(stack);
}

// method expecting a List, mayble in another class
public void AddTheX(List list)
{
	list.AddHead("X");
}
```
Recall, the stack is a LIFO data structure. List has a number of extraneous methods such as AddHead, AddTail, Insert, Remove, Clear that have no meaning to a Stack!

Another violation:

```csharp
// in mathematics a square IS-A rectangle, 
// but does that relationship hold in code?

// What happens with this code?
// assuming square is a subclass of rectangle
var square = new Square();
square.Width(1);
square.Height(2);

// One approach would be to override the methods of the square class, 
// setting both Width and Height when either is set. 
public class Square: Rectangle
{
	public override double Width
	{
		set
		{
			base.Width = value;
			base.Height = value;
		}
	}

	public override double Height
	{
		set
		{
			base.Width = value;
			base.Height = value;
		}
	}
}

//However, this is unexpected behavior for the client, 
// who in a base method might be testing the area of a rectangle, 
// expecting their width/height to be different.
[Test]
public void ThenTheAreaIsCorrect()
{
	var square = new Square();
	AreaTester(square);
}
private void AreaTester(Rectangle rectangle)
{
	rectangle.Width = 5;
	rectangle.Height = 4;
	Assert.That(rectangle.Area, Is.Equalto(20));
}
```

This is obviously a problem, because the Rectangle expects any base class to behave the same way. This is a great example of IS-A vs IS-SUBSTITUTABLE-FOR and why IS-A can sometimes fail. 

* IS-A is about behavior
	* Is-A really means "behaves exactly like"
* LSP is one of the prime enablers of OCP
* "The true definition of a subtype is substitutable..." Bob Martin
* Whenever you're feeling the urge to check the subtype of an object, take a step back and re-think your approach

## Interface Segregation Principle (ISP)

> Clients should not be forced to depend on methods they do not use. (Fat Interfaces)

* Note relationship to SRP
* Abiding by this principle will 
	* minimize coupling
	* maximize cohesion

#### ISP Bad Smells

* Empty impls of interface methods
* Base class overrides do nothing but throw exceptions
* Changing an interface method affects classes that don't use that method

#### ISP Bad Example

```csharp
public interface IEmployee 
{
	void SubmitTimeSheet(TimeSheet timeSheet);
	void ApproveTimeSheet(TimeSheet timeSheet);
	InternetUsage GetInternetUsage(int employeeId);	
}

public class Grunt: IEmployee
{
	public void SubmitTimeSheet(TimeSheet timeSheet)
	{
		//this call is ok, call service to submit timeSheet
	}
	public void ApproveTimeSheet(TimeSheet timeSheet)
	{
		throw new NotImplementedException();
	}
	public InternetUsage GetInternetUsage(int employeeId)
	{
		throw new NotImplementedException();
	}
}
```
#### ISP Good Example
```csharp
public interface IEmployee 
{
	void SubmitTimeSheet(TimeSheet timeSheet);
}

public interface IManager : IEmployee
{
	void ApproveTimeSheet(TimeSheet timeSheet);
	InternetUsage GetInternetUsage(int employeeId);	
}

public class Grunt: IEmployee
{
	public void SubmitTimeSheet(TimeSheet timeSheet)
	{
		//call service to submit timeSheet
	}
}

public class Boss : IManager
{
	public void SubmitTimeSheet(TimeSheet timeSheet)
	{
		//call service to submit timeSheet
	}
	public void ApproveTimeSheet(TimeSheet timeSheet)
	{
		//call service to approve timesheet
	}
	public InternetUsage GetInternetUsage(int employeeId)
	{
		//call service to get internet usage
	}
}
```

## Dependency Inversion Principle (DIP)

> High level modules should not depend on low level modules, both should depend on abstractions. Abstractions should not depend on details, details should depend on abstractions. 

The modules that contain the high-level business rules should take precedence over, and be independent of, the modules that contain the implmementation details. The high-level modules own the abstractions.

* It is high-level policy modules that we want to be able to reuse
* Each upper-level layer declares an abstract interface for the services it eneds. The lower level layers are then realized from these abstract interfaces.
* The high-level policy owns the abstraction. 

**What problem does it solve?** High level, policy modules are no longer impacted by changes to low level modules. We want to code to abstractions, no "new"s is good news.

> "Indeed this inversion of dependencies is the hallmark of good object-oriented design. If its dependencies are inverted it has an OO design. If its dependencies are not inverted, it has a procedural design." - Bob Martin

#### DIP Bad Example

```csharp
public class Thermostat 
{
	private readonly Furnace _furnace;
	public Thermostat()
	{
		_furnace = new Furnace();	
	}
	public void Run()
	{
		if(ShouldBeOn()){_furnace.On();}
		if(ShouldBeOff(){_furnace.Off();}
	}	
}
```
Thermostat has a depdenency on Furnce. It has uses the constructor inline and has a tightly coupled dendency on the Furnace. The thermostat is not reusable. 

Is this code any better?

```csharp
public class Thermostat 
{
	private readonly Furnace _furnace;
	public Thermostat(Furnce furnace)
	{
		_furnace = furnace;	
	}
	public void Run()
	{
		if(ShouldBeOn()){_furnace.On();}
		if(ShouldBeOff(){_furnace.Off();}
	}	
}
```
You still have not made it any better, you are still dependent on the Furnace type. Your dependency is still not inverted because the interface does not belong to the policy. 



#### DIP Good Example

Instead, introduce an interface and make the high level module (thermostat) depend on it. 

```csharp
public interface IOnOffDevice
{
	void On();
	void Off();
}
public class Thermostat 
{
	private readonly IOnOffDevice _device;
	public Thermostat(IOnOffDevice device)
	{
		_device = device;	
	}
	public void Run()
	{
		if(ShouldBeOn()){_device.On();}
		if(ShouldBeOff(){_device.Off();}
	}	
}
```
Now, when the furnace changes, it no longer impacts the Thermostat. Someone else can change the implementation details of the On/Off device and thermostat is none the wiser. 

#### Dependency Injection

Dependency Injection is the act of supplying all objects that a module needs so that the module is no longer responsible for instantiating its own depdencies. 

Typically comes in three flavors:

* Constructor Injection
* Parameter Injection
* Property Injection

#### Difference between Dependency Injection and Dependency Inversion

Dependency injection does not care which way the dependencies flow (high-to-low or low-to-high). Dependency Inversion states that dependencies have to flow from high-to-low.

* Depdency Injection is a technique by which we achieve Dependency Inversion
* Dependency Injection deals more with Dependency Abstraction

#### Let's improve this code

```csharp
public IRepository Repository {get;set;}
public ExportJob()
{
	Repository = new DatabaseRepository();
}
public ExportJob(string path)
{
	Repository = new FileSystemRepository(path);
}
public void Run()
{
	//do something with repository
}
```
Separate responsibility:

```csharp
public class ExportJobInvoker
{
	public void Run()
	{
		var exportJob = new ExportJob();
		exportJob.Repository = new ExcelRepository();
		exportJob.Run();
	}
}
```
Improvements:

* Remove the "new"s
* Remove Parameter Injection since we won't change repositories mid-stream

```csharp
public class ExportJob
{
	public IRepository Repository {get;set;}
	public ExportJob(IRepository repository)
	{
		Repository = repository;
	}
	public void Run()
	{
		//do something with repository
	}
}

public class ExportJobInvoker
{
	public void Run()
	{
		// use the constructor injection
		var exportJob = new ExportJob(new ExcelRepository());
		exportJob.Run();
	}
}

```
