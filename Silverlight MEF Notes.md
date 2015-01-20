## MEF Introduction

> http://channel9.msdn.com/Blogs/mtaulty/MEF--Silverlight-4-Beta-Part-1-Introduction

A library that allows development of loosely coupled components (think plugins) via Import/Export. Allows you to compose applications with different methods for discovering components at runtime. 

### Components

Capable of imports/exports and provides some level of abstraction between components.

To understand the idea of exports, think of building a class library. When you compile that assembly into a dll, you are effectively exporting the types for consumption by another library or application. Fundamentally, the consuming application is importing the components that were exported by your class library. In this case, the CLR has full control over how the types are discovered and instantiated. 

What if you wanted more control over that process? For example, load a type over HTTP? MEF gives you the ability to have more granular control over imports and exports. 

**The class library approach for imports/exports:**

```csharp
// WordProcessorLibrary.dll
namespace WordProcessorLibrary
{
	public class WordProcessor
	{
		public WordProcessor()
		{
			theEngine = new DefaultTextEngine();
		}

		private DefaultTextEngine theEngine;
	}
}

// TextEngineLIbrary.dll
namespace TextEngineLibrary
{
	public class DefaultTextEngine
	{

	}
}
```
The MEF approach for imports/exports:

```csharp
namespace WordProcessorLibrary
{
	[Export]
	public class WordProcessor
	{
		[Import]
		public DefaultTextEngine TextEngine {get;set;}
	}
}

namespace TextEngineLibrary
{
	// Where can you apply this?
	// Who ran the constructor?
	// Are instances shared/what is the creation policy?
	[Export]
	public class DefaultTextEngine
	{
		public string Name
		{
			get { return "default"; }
		}

		public void ProcessText(string text)
		{
		
		}
	}
}

namespace App
{
	public partial class MainPage: UserControl
	{
		// Where can I use this? Properties? 
		// What about cardinality of these plugins?
		// Can I import derived types?
		[Import]
		public WordProcessor WordProcessor { get;set; }

		public MainPage()
		{
			InitializeComponent();
			
			// What did this do?
			// How can we take more control over how things are imported?
			// How does MEF discover types?
			PartInitializer.SatisfyImports(this);
		}

		private void OnButtonClick(object sender, RoutedEventArgs e)
		{

		}
	}
}
```

Notice how we didn't have to specify where these imports were coming from. MEF resolves the dependencies and hands you an instance. 

In order to use MEF you need to reference the following assemblies:

1. System.ComponentModel.Composition.dll
2. System.ComponentModel.Composition.Initialization.dll

## MEF Imports and Exports

> http://channel9.msdn.com/blogs/mtaulty/mef--silverlight-4-beta-part-2-imports--exports

What things can we control when we use these Import/Export attributes?

### Imports

* We can import types to both properties and fields, but the protection level must be Public due to the way reflection works in Silverlight.
* Can also use the [ImportingConstructor] attribute on ctor

### Exports

* We can export types and properties/fields.
	* if we are exporting a property, MEF will take care of instantiating the type to resolve our dependency for us
* We can also Export on methods to export the returned value
	* when you use methods you will have to have a property with Func<TResult>, you can no longer just use a standard property

Here we show an example of breaking the concrete dependency between WordProcessor and TextEngine.

```csharp
namespace WordProcessorLibrary
{
	[Export]
	public class WordProcessor
	{
		[Import]
		public ITextEngine TextEngine {get;set;}
	}
}

namespace TextEngineLibrary
{
	public interface ITextEngine
	{
		string Name {get;}
		void ProcessText(string text);
	}

	[Export(typeof(ITextEngine)]
	public class TextEngine : ITextEngine
	{
		public string Name
		{
			get { return "default"; }
		}

		public void ProcessText(string text)
		{
		
		}
	}
}

namespace App
{
	public partial class MainPage: UserControl
	{
		[Import]
		public WordProcessor WordProcessor { get;set; }

		public MainPage()
		{
			InitializeComponent();
			PartInitializer.SatisfyImports(this);
		}

		private void OnButtonClick(object sender, RoutedEventArgs e)
		{

		}
	}
}
```

### Objection Creation Lifecycle

By default, when we call PartInitializer.SatsifyImports(this), MEF will initialize our types. 

We can do lazy initialization by wrapping the exportable type in Lazy<T> :

```csharp
public partial class MainPage: UserControl
{
	[Import]
	public Lazy<WordProcessor> WordProcessor { get;set; }

	public MainPage()
	{
		InitializeComponent();
		PartInitializer.SatisfyImports(this);
	}

	private void OnButtonClick(object sender, RoutedEventArgs e)
	{
		// now initializes here instead of on SatisfyImports
		var wp = WordProcessor.Value;
	}
}
```

We can also use the PartCreator<T> to accomplish lazy initialization. This also allows you to create multiple instances if necessary:

```csharp
public partial class MainPage: UserControl
{
	[Import]
	public PartCreator<WordProcessor> Creator { get;set; }

	public MainPage()
	{
		InitializeComponent();
		PartInitializer.SatisfyImports(this);
	}

	private void OnButtonClick(object sender, RoutedEventArgs e)
	{
		var wp1 = Creator.CreatePart().ExportedValue;
		var wp2 = Creator.CreatePart().ExportedValue;
		var wp3 = Creator.CreatePart().ExportedValue;
	}
}
```

What happens if you have multiple imports of the same type? By default, one instance will be created and handed to you each time you Import. You can change this by specifying a PartCreationPolicy of NonShared:

```csharp
[Export]
[PartCreationPolicy(CreationPolicy.NonShared)]
public class WordProcessor
{
	[Import]
	public ITextEngine TextEngine {get;set;}
}
```
You can also require a specific type of creation policy on the Imports.

If MEF cannot find an export then the SatisfyImports method will throw an exception. If this is not desired, you can specify AllowDefaults on the Import:

```csharp
[Export]
public class WordProcessor
{
	[Import(AllowDefault=true)]
	public ITextEngine TextEngine {get;set;}
}
```

What if you have two exportable types? Assume we have many ITextEngine implementations. You can use ImportMany to get all registered exports:

```csharp
[Export]
public class WordProcessor
{
	[ImportMany]
	public IEnumerable<ITextEngine> TextEngine {get;set;}
}
```

If you aren't trying to import multiple, and want to select the 'correct' export, you can define an Export Name:

```csharp
[Export("Regular", typeof(ITextEngine)]
public class TextEngine : ITextEngine {}


[Export("Fancy", typeof(ITextEngine)]
public class FancyTextEngine : ITextEngine {}

[Export]
public class WordProcessor
{
	[Import("Fancy")]
	public ITextEngine TextEngine {get;set;}
}
```

Now imagine you are importing many, but you don't want to use the names. You can also use the ExportMetadata attribute. This requires the signature to be of type IEnumerable<Lazy<TExport,Dictionary<string,object>>>:

```csharp
[Export(typeof(ITextEngine)]
[ExportMetadata("ishighres", false)]
public class TextEngine : ITextEngine {}


[Export(typeof(ITextEngine)]
[ExportMetadata("ishighres", true)]
public class FancyTextEngine : ITextEngine {}

[Export]
public class WordProcessor
{
	[ImportMany]
	public IEnumerable<Lazy<ITextEngine, 
		Dictionary<string,object>>> TextEngines {get;set;}

	public void ProcessUsingHighRes()
	{
		foreach(var engine in TextEngines)
		{
			if (engine.Metadata.ContainsKey("ishighres")
				&& (bool) engine.Metadata["ishighres"])
			{
				engine.Value.ProcessText();
			}
			
		}
	}
}
```

If this feels kludgy and you don't like the magic strings, you can also create your own type for exporting metadata:

```csharp
[MetadataAttribute]
[AttributeUsage(AttributeTargets.Class, AllowMultiple=false)]
public class TextEngineExportAttribute : ExportAttribute
{
	public TextEngineExportAttribute() : base(typeof(ITextEngine)) {}

	public bool IsHighRes {get;set;}
}

public interface ITextEngineMetadata
{
	bool IsHighRes {get;}
}

[Export(typeof(ITextEngine)]
[TextEngineExport(IsHighRes=false)]
public class TextEngine : ITextEngine {}


[Export(typeof(ITextEngine)]
[TextEngineExport(IsHighRes=true)]
public class FancyTextEngine : ITextEngine {}

[Export]
public class WordProcessor
{
	[ImportMany]
	public IEnumerable<Lazy<ITextEngine, ITextEngineMetadata>> TextEngines {get;set;}

	public void ProcessUsingHighRes()
	{
		foreach(var engine in TextEngines)
		{
			if (engine.Metadata.IsHighRes)
			{
				engine.Value.ProcessText();
			}
			
		}
	}
}
```

## Catalogs

How is MEF finding/locating/resolving the types that are in play? How to have a runtime dependency instead of explicitly referencing assemblies?

PartInitializer can find types that are located in the XAP. It uses the CompositionContainer to accomplish this. There is a default/implicit container that allows this basic functionality. If you want to have custom ability (to say load dlls from a plugin directory) you will need to create your own CompositionContainer. One way of populating the container is to use the TypeCatalog: 

```csharp
public partial class MainPage: UserControl
{
	[Import]
	public IWordProcessor WordProcessor { get;set; }

	public MainPage()
	{
		InitializeComponent();
		InitializeContainer();
		PartInitializer.SatisfyImports(this);
	}

	void InitializeContainer()
	{
		var catalog = new TypeCatalog(typeof(WordProcessor), typeof(TextEngine));
		var container = new CompositionContainer(catalog);
		CompositionHost.InitializeContainer(container);
	}

	private void OnButtonClick(object sender, RoutedEventArgs e)
	{
		WordProcessor.ProcessText("");
	}
}
```
But this doesn't actually buy us anything, the default container could resolve these types for us. In the real world, you could use a configuration file for example. There is also an AssemblyCatalog, and the same idea holds there:

```csharp
public partial class MainPage: UserControl
{
	[Import]
	public IWordProcessor WordProcessor { get;set; }

	public MainPage()
	{
		InitializeComponent();
		InitializeContainer();
		PartInitializer.SatisfyImports(this);
	}

	void InitializeContainer()
	{
		var catalog1 = new AssemblyCatalog(typeof(WordProcessor).Assembly);
		var catalog2 = new AssemblyCatalog(typeof(TextEngine).Assembly);
		var catalog = new AggregateCatalog(catalog1, catalog2);
		var container = new CompositionContainer(catalog);
		CompositionHost.InitializeContainer(container);
	}

	private void OnButtonClick(object sender, RoutedEventArgs e)
	{
		WordProcessor.ProcessText("");
	}
}
```

With the AggregateCatalog, you can alter it after you construct it. This allows dynamically loaded types and really highlights some powerful possibilities. If a container changes, then MEF is smart enough to know it needs to recompose the types. 

Here is an example of what, by default, MEF is doing under the covers:

```csharp
public partial class MainPage: UserControl
{
	[Import]
	public IWordProcessor WordProcessor { get;set; }

	public MainPage()
	{
		InitializeComponent();
		InitializeContainer();
		PartInitializer.SatisfyImports(this);

	}

	void InitializeContainer()
	{
		var catalog = new AggregateCatalog();
		foreach(var assemblyPart in Deployment.Current.Parts)
		{
			Stream stream = Application.GetResourceStream(
				new Uri(assemblyPart.Source), UriKindRelative).Stream;

			var assembly = assemblyPart.Load(stream);
			catalog.Add(new AssemblyCatalog(assembly));
		}
		var container = new CompositionContainer(catalog);
		CompositionHost.InitializeContainer(container);
	}

	private void OnButtonClick(object sender, RoutedEventArgs e)
	{
		WordProcessor.ProcessText("");
	}
}
```
All of these Catalogs we discussed derive from ComposablePartCatalog, which is an abstract class. You can use this to create your own custom catalogs. 

## Recomposition

