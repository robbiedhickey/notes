# Silverlight 4 Core Notes

## Introduction

### What is Silverlight

Browser plugin for building rich web applications

* HTML Integration
* HD Video
* Vector Animation

Filtering/Sorting/Browsing is all done on the client in many applications. 

It brings the .NET Programming model to client-side applications. Javascript is also supported, but most developers use the .NET model.

Silverlight is also a multi-platform and multi-browser plugin. 

### Versions and Platform

V1 was a rendering engine for vector graphics, bit maps, HD video, etc. It relied on javascript and the DOM as its programming model. It focused on graphics and video, and offered little to developers. 

V2 introduced a trimmed down version of the .NET framework, so developers could now use any language supported by the CLR for Silverlight development. First 'Full' version of Silverlight

V3 built on that, fleshing out of DataBinding and control features, as well as improved deployment. Added support for 'Out of browser applications', which is basically a desktop deployment model. Similar to click-once but reduces the friction considerably. Also added 3D support.

V4 added LOB enhancements: printing, form controls (data binding validation, annotations, WCF RIA Services), COM automation (control MS Office within Silverlight). COM was also the first feature that was platform specific. Also added support for MEF, right-click, performance, multi-touch, toaster notifications, trusted apps, Webcam & mic, custom window chrome (for out of browser apps). 

* Windows
	* Requires WinXP Service Pack 2+
	* Won't run at all on ME and Windows 2000 has limited support
	* IE6+ with a hotfix
	* Firefox 2-3
	* Chrome 4
* Mac (Intel only)
	* Firefox
	* Safari
* Linux (Moonlight, gradually)
	* Standalone
	* Firefox
* Mobile (eventually)
	* This never came to pass except for windows phone

### Getting Started

What you need?

* HTML Page
	* must contain a suitable tag to inject the silverlight plugin
	* optional detection/install UI for fallback
* Compiled Silverlight Application
	* XAP file consisting of XAML and .NET code.

Project generates a silverlight class library and a web hosting project for debugging. 

When you wire up event handlers in XAML, notice that they are not registered in the code behind, this is because XAML compilation subscribes the events for us. You can also create them by hand in the code behind. 

When you compile a silverlight application, the build automatically copies the output XAP file into the ClientBin of the web host project. You will notice that the <object> tag includes a param that points its 'Source' to the ClientBin/*.xap file.

### XAML

Short for Extensible Application Markup Language. It is XML based. The elements can correspond to UI elements, or how to confiure the element. They can also define the structure and layout of the UI. Elements in XAML correspond to objects in .NET. 

Silverlight XAML is a Subset of WPF Xaml. This is not strictly true, they are very similar but each have features that the other does not. 

#### XAML and Code Behind

Each Xaml file has a code behind. 

Special Directives:

* x:Name - this attribute corresponds to the name of the object in the code behind.
* x:Class - this attribute corresponds to the name of the class for the code behind.  

Directives live in a separate XML namespace to make the differences between them and other attributes clear. 

#### XAML and Javascript

Silverlight provides Javascript wrappers for any XAML element. This is often used for downloading splash/UI and replacing the default animation. 

The simplest way to create a wrapper is to use the name of a javascript function for the event handler. If the code behind does not have a corresponding method, the javascript wrapper will be created. 

```xml
<Rectangle MouseLeftButtonDown="OnRectMouseDown" />
```

```javascript
function OnRectMouseDown(sender, eventArgs) {
	sender.Fill = "Red";
	var x = eventArgs.GetPosition(rootCanvas).x;
	var y = eventArgs.GetPosition(rootCanvas).y;
	...
}
```

### Comparing Silverlight with WPF

* intended for different scenarios (web vs desktop)
* Some WPF features not in Silverlight
	* Full hardware acceleration
	* Drawing types
* Less powerful features in Silverlight
	* Layout, Resources, Templates, Styles, Flow Text
* Silverlight features not in WPF
	* Browser integration, VideoBrush, Deep Zoom

## Getting Started w/ Visual Studio 2010

Out of the box only supports Silverlight 3, but 4 is supported via an optional download. You can still target multiple versions. 

### Silverlight Project Templates

* Silverlight Application
	* executable - builds a runnable program
* Silverlight Class Library
	* builds a library designed to be incorporated into other applications
* Silverlight Business Application
	* similar to Navigation application, but also adds features around WCF RIA services
	* See RIA Services course for more information
* Silverlight Navigation Application
	* similar to ordinary silverlight app, includes some additional generated code for navigation between silverlight apps, allows it to behave more like a website.
	* contains Assets/Styles.xaml which define style for navigation bar
	* contains Views/*.xaml with additional screens to navigate to as targets
	* Allows user to user browser back/forward buttons as though it was a typical web page. Silverlight uses the # technique to accomplish this. 
* WCF RIA Services Class Library
	* see RIA services course for more information
* Silverlight Unit Test Application
	* write unit tests that can be run/executed in the browser using MSTest. 

#### Tips

* Note that if you decide to not create the web host project, visual studio will automatically generate an HTML page for you in the bin folder for debugging purposes. 
* Use the Document Outline window for large XAML files. 
* You can also use the breadcrumb bar at the buttom of the XAML viewer to navigate directly to an element, and you even get a preview. 
* The ClientBin build/copy is managed in the project properties screen(s) for the web project.
	* Right-Click -> Properties -> Silverlight Applications -> Shows a list of all silverlight applications associated with the project
	* Useful, allows you to add additional silverlight apps later. 
	* You can also choose to output to something other the ClientBin.
* WCF RIA Services configuration
	* you can also add this later if you didn't select it during project creation, but this lives in the project properties of the silverlight library. 
	* Properties -> WCF RIA Services Link Dropdown -> Select web project
	* This will enable the code generation features. 

#### Testing

Silverlight comes with its own unit test framework built on top of MSTest. When you run a silverlight unit test project, your results will be displayed in the browser. The results are displayed an a tree view along with some information about each test. The test look just like any other MSTest class:

```csharp
[TestClass]
public class Tests
{
	[TestMethod]
	public void TwoPlusTwoIsFour()
	{
		Assert.AreEqual(4, Calculator.Add(2,2));
	}
}
```

## Layout

### Fixed Layout

* Canvas
	* Canvas.Left and Canvas.Top positioning
	* Derives from Panel class

Supports Attachable properties

```xml
<Ellipse Canvas.Left="10" Canvas.Top="12.5" Width="80" Height="80" Fill="Orange" />
```

Can also set these properties in code:

```csharp
Canvas.SetLeft(myEllipse, 10);
Canvas.SetTop(myEllipse, 12.5);
```

### Dynamic Layout

* Adaptable to
	* quantity of information
	* size of items
	* space available

Silverlight offers two panels with dynamic layout strategies:

#### StackPanel

Arranges elements in a vertical or horizontal stack. You can mix elements in a stack (buttons and ellipses). You can also nest StackPanels. The height of the nested stack is determined by the height of the tallest parent element

```xml
<StackPanel>
    <Border Background="SkyBlue" BorderBrush="Black" BorderThickness="1">
      <TextBlock Foreground="Black" FontSize="12">Stacked Item #1</TextBlock>
    </Border>
    <Border Width="400" Background="CadetBlue" BorderBrush="Black" BorderThickness="1">
      <Button Text="Click"></Button>
    </Border>
</StackPanel>
```

#### Grid

Arranges elements in a table-like fashion. Each row and column can use a different sizing strategy.

* Column and row sizing:
	* Automatic 
	* Fixed
	* Proportional
* Column and rows are zero based
* 1* or 2* indicates that they share whatever space is left over by the specified magnitude. So, 2* gets twice as much space as 1*.

Elements can span cells. You have a fine level of control over this with the margin property. As you drag around the design surface, the margin properties will be calculated for you.

A grid can also take over most of the responsibilites of a StackPanel (One column with all rows set to automatic height will behave the same as a StackPanel). The main reason StackPanel exists because it doesn't need to know how many rows it has up front. It will just keep getting taller. 

```xml
<Grid x:Name="LayoutRoot" Background="White">
	<Grid.ColumnDefinitions>
		<ColumnDefinition Width="130" />
		<!---->
		<ColumnDefinition Width="1*" />
		<ColumnDefinition Width="2*" />
	</Grid.ColumnDefinitions>
	<Grid.RowDefinitions>
		<RowDefinition Height="Auto" />
		<RowDefinition Height="50" />
	</Grid.RowDefinitions>

	<TextBlock Grid.Column="0" Grid.Row="0" Text="Content" />
	<TextBlock Grid.Column="0" Grid.Row="2" Grid.ColumnSpan="2" Text="Content />
</Grid>
```

### Layout Properties and Concepts

| Common Layout Properties |
|--------------------------|
| Margin |
| Padding |
| HorizontalAlignment/VerticalAligment |
| HorizontalContentAlignment/VerticalContentAlignment |
| Min/MaxWidth |
| Min/MaxHeight |
| Width, Height |

#### Margin

* Space around edges
* Silverlight allocates a 'layout slot' for an element, and the margin dictates its position within the slot. 

Note that the order of numbers is different from CSS: Left, Top, Right, Bottom
```xml
<Button margin="20" />
<!-- Horizontal, Vertical -->
<Button margin="20,30" />
<!-- Left, Top, Right, Bottom -->
<Button margin="20,30,40,50" />
```
#### Padding

* Similar to margin but on the inside
* Space between boundary and content

Padding obeys the same rules as Margin in terms of order.

#### Alignment

* VerticalAlignment
	* Top
	* Center
	* Bottom
	* Stretch (default)
* HorizontalAlignment
	* Left
	* Center
	* Right
	* Stretch (default)

#### Constained vs Unconstrained

A constrained layout is where the width is imposed by a container.

An unconstrained layout is where the elements size to the content (example, a vertical stack panel)

Alignment properties can switch you from constrained to unconstrained, or vice-versa. 

#### GridSplitter

Allows user to determine the size of a row or cell in a grid. To use you simply:

* Add to a cell
* Set Direction
* Resize columns/rows when dragged

```xml
...
<ListBox Grid.Row="1" Grid.Column="0">
	...
</Listbox>
<TextBox Grid.Row="1" Grid.Column="1" />
<Border Grid.Row="2" Grid.ColumnSpan="2">
	<TextBlock Text="Ready" />
</Border>
<!-- Notice that it shares the column with the listbox and is oriented to the right -->
<sdk:GridSplitter Grid.Column="0" Grid.Row="1" HorizontalAlignment="Right" />
```
#### ScrollViewer

Can wrap any element. It must be a single child element, but it can always be a panel which itself wraps more elements. The viewer expects to be in a constrained layout context because the whole reason for its existence is to display variable sized content. 

```xml
<ScrollViwer HorizontalScrollbarVisibility="Auto" VerticalScrollbarVisibility="Auto">
	<Image ... />
</ScrollViewer>
```
#### Border

* Handy for extra Padding and Margin
* Also draws border around stuff
* Round edges with CornerRadius

```xml
<Border BorderBrush="Blue" BorderThickness="10" Margin="5" CornerRadius="80">
	<Ellipse Fill="Green" Width="40" Height="70" />
</Border>
```
### Full Screen Mode

* Plug-in can run full-screen
* Security Constraints
	* Can only set in input-related event handler
	* Do not apply to out of browser apps with elevated trust
	* Always displays message "Press ESC to exit full screen mode"

```csharp
using System.Windows.Interop;

public class MainPage : UserControl
{
	...

	// toggle full screen
	Content content = Application.Current.Host.Content;
	content.IsFullScreen = !content.IsFullScreen;

	...

	// do something when full screen changes
	Content content = Application.Current.Host.Content;
	content.FullScreenChanged += OnFullScreenChanged;

	void OnFullScreenChanged(object sender, EventArgs e)
	{
		...
	}
}

```

### Silverlight and HTML Layout

* Silverlight root element constrained by HTML/CSS
	* afterall, it all resides in an <object> tag. 
	* Plugin cannot size to content
* Default project templates fills browser window
	* Xaml has fixed size

The default CSS you get with the VS2010 project includes height 100% on body. 

This is why it is often a good idea to have relative dimensions in your silverlight applications. If you used fixed dimensions, be aware that the plugin still is constrained by what the initial settings are for the CSS. Basically, CSS always wins if there is a conflict. 

By default, overlapping content is not supported, silverlight will hide anything that overlaps with the plugin surface area. If you want to overlap, must enable 'Windowless' mode and set the z-index. If you want HTML event handlers to still run, the content must be on top of the silverlight content. 

## Input Handling

* Mouse
* Keyboard
* Touch
	* Raw input only (no gestures) on non-mobile applications. You will have to write your own code to recognize gestures.
	* Windows phone has some gesture support.

### Mouse

#### Mouse Input Events

Note that the position detection is not by the bounding box, but by the actual element. So an ellipse will only trigger the event once the mouse actually moves inside the border of the ellipse. If your ellipse does not have a fill, then you will only get mouse movement events when your mouse is on the border! The way around this is to set a fill and make it transparent. 

You can also use the **IsHitTestVisible** property to prevent events from bubbling!

| Movement |
|----------|
| MouseMove |
| MouseEnter |
| MouseLeave |
| LostMouseCapture |

Button events can also be captured

| Button |
|----------|
| MouseLeftButtonDown |
| MouseLeftButtonUp |
| MouseRightButtonDown |
| MouseRightButtonUp |

As well as mouse wheel events

| Wheel |
|----------|
| MouseWheel |

All of these event handlers follow the .NET pattern of handler(sender, eventArgs). 

* MouseEventArgs
	* MouseWheelEventArgs
	* MouseButtonEventArgs

The base class provides the GetPosition method that we saw earlier. The reason that it is a method is because position is relative in Silverlight - you must specify an element. 

#### Event Bubbling

Some input events do not need to be handled by the element from which they originated. 

* Events bubble up XAML tree
	* note that you can handle an event and not set the Handled property to true, and this will allow the event to continue propagating up the tree. 
	* allows you to not specify handlers for primitive elements. 
	* similar to DOM event bubbling, but silverlight will stop at the top of the silverlight tree -- they will not propagate to the DOM.
* Origin indicated by OriginalSouce property of EventArgs
	* Note that the sender corresponds to the element on which the handler was attached - if you want to get the element that triggered the event - check the OriginalSource property on the MouseEventArgs
	* ex. Tree of Canvas -> Many Ellipses. Attach mouse click event handler to Canvas. Sender will be Canvas, not clicked ellipse. 
* Set Handled=true to halt bubbling
* No tunneling
* Intrinsic events only -- bubbling mechanism isn't publicly available. Cannot write your own custom events that bubble. 
	* Can cause unexpected behavior - events specific to some microsoft provided control may not bubble. Example - the Button click event does not bubble. 
	* Intrinsic elements are 'special' and can do things that your code can't. You could not write your own version of the Ellipse class. 
	* Only low-level input events bubble - not those defined by controls. 
	* Raises another point - not everything is a control. Only those that have some essential interactive behavior are controls. Ellipses and Rectangles are not controls. 

#### Mouse Capture

* Mouse can move out of element
	* What if we still want events?
	* ex. slider control - annoying if moving outside of the control would stop the slider.
* Capture mouse events
	* when you capture a mouse event, other elements will not be notified until you release the capture. 
* Should Release capture when done
	* May get early LostMouseCapture if something outside of your application intercepts the mouse. 

#### Right Mouse Button

* New in Silverlight 4
* Old versions would show the Silverlight dialog. Still the default. Have to handle RightMouseButtonDown.
* Quirk: you cannot just handle the MouseUp event. If you do not mark the MouseDown event as handled, the MouseUp will never run. 
* No built-in context menu
	* April 2010 Silverlight Toolkit provides one

#### Mouse Wheel

* Platform-specific oddities
	* NPAPI (Netscape plugin API) limitations - mouse wheel didn't exist when it was created
	* No Mac support
* HTML DOM may work
	* Not out of browser compatible

### Touch

By default touch inputs are handled via the typical mouse events. This may present usability issues.

* Touch input as mouse substitute
* Touch-aware code
	* can handle multitouch 
	* Specialist touch-based interaction (ex inertial scrolling) 

### Keyboard

* KeyUp and KeyDown events
	* KeyEventArgs.Key indicates which key was pressed
		* Multiple platforms? Only those which are platform independent will be returned
		* A mac specific key will return Unknown. 
	* KeyEventArgs.PlatformKeyCode lets you identify a platform specific key if you don't care about portability
* Focus
	* plugin model makes this slightly messy (plugin must have focus)
	* only Control-derived classes can receive focus

### Controls

> These are NOT controls: Grids, Panels, Rectangles, Ellipses, Borders, TextBlocks. 

* Textual input with TextBox
	* Globalized
	* Accessible - works with accessibility APIs
	* Data Bindable
* Button
	* Accessible
	* Commands

### Commands

Rule of thumb - your code behind should have as little code as possible. One way to accomplish this is to use commands. Largely this allows you to avoid normal event handling in favor of commands. 

* ICommand
	* used with buttons or context menu items. Can respond to user input.
	* Represents an invokable action via an object
	* Lets you connect UI to code through data binding alone. 
	* Single abstraction for execution and enabled/disabled. 
* Surprisingly, nothing in silverlight actually implements ICommand. 
	* Silverlight only consumes ICommand
	* Button, MenuItem

## Applications, Resources and Deployment

Common issues around creating and deploying applications and the resources they depend on. 

### Loading the Application

* HTML <object> tag
	* Compiled silverlight content
	* Splash/download UI
	* Error handling
	* Fallback content

Object tag is very flexible and not just for plugins - can use for images and html as well. 

```html
<object id="slPlugin" width="100%" height="100%"
		type="application/x-silverlight-2"
		data="data:application/x-silverlight-2,">
	<param name="source" value="ClientBin/SlContext.xap" />
	<param name="onerror" value="onSilverlightError" />
	<param name="minRuntimeVersion" value="4.0.50401.0" />
	<param name="autoUpgrade" value="true" />

	<!-- fallback html -->
	<a href="..." style="text-decoration:none;">
		<img src="..." />
	</a>
</object>
```
Alternative to using the object tag is Silverlight.js. 

```html
<script type="text/javascript" src="silverlight.js"></script>

<div id="slPlaceholder"></div>

<script type="text/javascript">
	Silverlight.createObject("ClientBin/SlContext.xap",
		document.getElementById("slPlaceholder"),
		"slControl",
		{width:"500", height: "500", version: "4.0" },
		{onError: MyErrorHandler}
</script>
```
* Benefits
	* Allows more flexibility when silverlight is not installed
		* Silverlight.isInstalled helper
	* Can choose content at runtime
	* Historical interest: solved activation patent legal fiasco
* Disadvantages
	* More completed
	* Extra file to download
	* Moving target (changed several times in its history)

#### Download UI

Silverlight provides an animation while your application downloads, this can grow significantly as the complexity of your app increases, and depending on what kind of media/resources you incorporate. 

You can replace this with your own! 

* Defined as XAML
* Set SplashScreenSource <param>
* Optionally handle onSourceDownloadProgressChanged
	* must use V1 programming model - javascript

```html
<script type="text/xaml" id="xamlSplashContent">
	<Canvas>
		<TextBlock x:Name="percentText"></TextBlock>
	</Canvas>
</script>

...

<script type="text/javascript">
	function updateDownload(sender, args) {
		var xamlContent = sender.getHost().content;
		var percentText = xamlContent.findName("percentText");
		var percentage = Math.floor(args.progress * 100);
		percentText.Text = percentage + "%";
	}
</script>

...

<object ...>
	...
	<param name="splashScreenSource" value="#xamlSplashContent" />
	<param name="onSourceDownloadProgressChanged" value="updateDownload" />
	...
</object>
```

### Xap files

* Just a ZIP file
* Convenient for silverlight to have a common extension
* ZIPs are often blocked by firewalls, so it makes sense to have a separate type

Inside the XAP

* Main DLL
* Dependent DLLs
* Resources
* Manifest (describing the content, which DLL contains entry point)

If you change the extension to ZIP and open in extraction utility, you will see these files. Images are embedded into the application DLL. If you use a decompiler you will see that they are embedded as Resource streams in the Resources folder.

Files are added as resource to the XAP if their build action is set to 'Resource'. You can also set these things to 'Content' if you want the files to be separate in the XAP. 

### Binary resources

#### Application Library Caching

* Package shared assemblies separately
	* Exploits browser caching
	* The price is that on startup you will have to fetch all dependent assemblies. 
* Controlled through appmanifest.xaml in the ExternalParts section
* Can also enable this in Silverlight project properties

```xml
<Deployment EntryPoitnAssembly="UriStyles" EntryPointType="UriStles.App" RuntimeVersion="3.0.40624.0">
	<Deployment.Parts>
		<AssemblyPart x:Name="UriStyles" Source="UriStyles.dll" />
	</Deployment.Parts
	<Deployment.ExternalParts>
		<ExtensionPart Source="System.Windows.Controls.Data.zip" />
	</Deployment.ExternalParts>
</Deployment>
```

#### Resources: XAP vs 'Loose'

* Images, Media, etc in Silverlight project ends up in XAP:
	* Application self-contained
	* Availability guaranteed once application starts
* Can add to web site instead:
	* Faster Startup
	* Easier to change

#### Resources and URIS

* Image and MediaElement refer to resources

| URI Style | Location | 
| ----------|----------|
| /img.jpg | in Xap (falls back to site, relative to xap location |
| img.jpg | in DLL as embedded resource (also falls back to site) |
| /AssemblyName;component/img.jpg | In DLL as embedded resource, specified completely |
| http://example.com/app/img.jpg | Specified Location |

Prefer the third form, the first two have relative reference problems if your xaml files are housed in application folders. 

### Xaml resources

* Object resources
	* Data templates
	* Control templates
	* Brushes, shapes

All UserControls have a Resources property. It is a dictionary, so must specify a key for each element. These resources would be available anywhere inside that user control. Recall that the Main App.xaml is a user control, so you can define global resources there:

```xml
<Application.Resources>
	<SolidColorBrush x:Key="psGreen" Color="Green" />
	<SolidColorBrush x:Key="psOrange" Color="Orange" />
</Application.Resources>

...
<!-- then you can use it anywhere. this is the data binding syntax -->
<Canvas>
	<Rectangle Width="120" Height="120"
		Canvas.Top="7" Canvas.Left="31"
		Fill="{StaticResource psGreen}" />
</Canvas>
```

#### Resource Merging

Helps to avoid XAML bloat, allows you to export things like templates to an application scope and modularize your XAML. To do this, you would split your resources into separate files. For example:

LogoBrushes.xaml

```xml
<ResourceDictionary ...>
	<SolidColorBrush x:Key="psGreen" Color="Green" />
	<SolidColorBrush x:Key="psOrange" Color="Orange" />
</ResourceDictionary>
```

These ResourceDictionary files contain XAML but no code behind. Merging these resource files can be done in App.xaml with the following syntax:

```xml
<Application.Resources>
	<ResourceDictionary>
		<ResourceDictionary.MergedDictionaries>
			<ResourceDictionary Source="LogoBrushes.xaml" />	
		</ResourceDictionary.MergedDictionaries>
	</ResourceDictionary>
</Application.Resources>
``` 

### Application class

* Entry point of Silverlight Applications

```csharp
public partial class App : Application
{
	public App()
	{
		this.Startup += AppStartup;
		this.Exit += Application_Exit;
		this.UnhandledException += Application_UnhandledException;
		InitializeComponent();
	}

	private void AppStartup(object sender, StartupEventArgs e)
	{
		// load the main control
		this.RootVisual = new Page();
	}
	
	...
}
```
You cannot change the RootVisual, it can be set only once.

Note that if you don't handle exception and mark them as such, then it will crash your application.

### Application Extension Services

Sometimes it is useful to have services available at an application scope. You can do this by adding properties to the App class, but silverlight offers a more compositional approach. 

* Services available at application scope
	* Application.ApplicationLifetimeObjects
* Implements IApplicationService, and optionally IApplicationLifetimeAware interfaces
	* before silverlight starts up, before it raises the Startup event, it walks through all IApplicationService and calls the IApplicationService.StartService method and IApplicationLifetimeAware.Starting method.
	* Silverlight then raises Application.Startup event
	* IApplicationLifetimeAware.Started
	* Once app exits
	* Application.Exit Event
	* IApplicationLifetimeAware.Exited
	* IApplicationService.StopService

## Out of Browser Applications

Skipping - not relevant for immediate purposes.

## File Access

Describes how to access files on the end-user's computer

### File Access Options

* OpenFileDialog and SaveFileDialog
	* user selects file
	* path not available
* FileStream, StreamWriter
	* user not involved
	* trusted applications only
* Isolated Storage
	* User not involved
	* Do not need elevated trust
	* Private per-XAP store in user's AppData
	* Cannot depend on info being persisted
	* Limited amount of storage

### File Dialogs

#### SaveFileDialog

```csharp
var save = new SaveFileDialog();
save.Filter = "Text Files (*.txt)|*.txt|All Files (*.*)|*.*";
save.DefaultExt = "*.txt";
if(save.ShowDialog() == true)
{
	using(Stream saveStream = save.OpenFile())
	using(var w = new StreamWriter(saveStream))
	{
		w.Write(contentTextBox.Text);
	}
}
```

Note that while debugging, you have to set a breakpoint after ShowDialog() otherwise silverlight thinks the dialog was not user initiated.

#### OpenFileDialog

```csharp
var open = new OpenFileDialog();
if(open.ShowDialog() == true)
{
	using(Stream openStream = open.File.OpenRead())
	using(var r = new StreamReader(openStream))
	{
		contentTextBox.Text = r.ReadToEnd();
	}
}
```
This also supports selecting multiple files. If we need more, we use the Files property instead of File. 

### System.IO

#### FileStream

Similar to file access with full .NET framework. 

* Requires elevated trust (requires OOB)
* Only certain folders accessible 
	* Documents, Music, Pictures, and Videos
	* Environment.GetFolderPath
* Use Directory and DirectoryInfo

### Isolated Storage

* Private user-specific storage
	* string or binary data
* Store per application or per-site
	* the key is the URL from which the XAP file is downloaded
	* Independent of hosting of web page
* 1MB default limit
	* can prompt user to increase
* Cross browser support
	* like having cookies shared across browsers

#### Writing to Isolated Storage

```csharp
try
{
	// obtain isolated storage for the user
	using(IsolatedStorageFile isoStore = 
			IsolatedStorageFile.GetUserStoreForApplication())
	{
		// create new file
		using(IsolatedStorageFileStream isoStream = 
				new IsolatedStorageFileStream("UserData.txt", FileMode.Create, isoStore))
		{
			// write content to file
			using(StreamWriter writer = new StreamWriter(isoStream))
			{
				// write a line to the file
				writer.WriteLine("Wrote to isolated storage at " + DateTime.Now);
			}
		}
	}
}
catch(Exception ex)
{
	...
}
```

#### Reading to Isolated Storage

```csharp
try
{
	// obtain isolated storage for the user
	using(IsolatedStorageFile isoStore = 
			IsolatedStorageFile.GetUserStoreForApplication())
	{
		// open existing file
		using(IsolatedStorageFileStream isoStream = 
				new IsolatedStorageFileStream("UserData.txt", FileMode.Open, isoStore))
		{
			// read content from file
			using(StreamReader reader = new StreamReader(isoStream))
			{
				// read a line from the file
				string s = reader.ReadLine();
			}
		}
	}
}
catch(Exception ex)
{
	...
}
```

#### Deleteing Isolated Storage Files

```csharp
try
{
	// obtain isolated storage for the user
	using(IsolatedStorageFile isoStore = 
			IsolatedStorageFile.GetUserStoreForApplication())
	{
		isoStore.DeleteFile("UserData.txt");
	}
}
catch(Exception ex)
{
	...
}
```

#### Increasing your Quota

* Can request an increase of store quota
	* must be in response to a user initiated event (like a button click)

```csharp
try
{
	// obtain isolated storage for the user
	using(IsolatedStorageFile isoStore = 
			IsolatedStorageFile.GetUserStoreForApplication())
	{
		var spaceToAdd = 500000;
		var curAvail = store.AvailableFreeSpace;
		if(curAvail < spaceToAdd)
		{
			if(!store.IncreaseQuotaTo(store.Quote + spaceToAdd))
				messageTextBlock.Text = "Increase rejected";
			else
				messageTextBlock.Text = "Increase approved";
		}
		else
			messageTextBlock.Text = curAvail.ToString() + " bytes are still available";
	}
}
catch(IsolatedStorageException ex)
{
	...
}
```

## Data Binding

The assumption for a long time was that a data source was a database, no longer. Silverlight doesn't even include ADO.NET so it is not possible for silverlight code to directly communicate with a database.

Purpose of data binding is to connect features of user interface to objects. Objects could mean IEnumerable<T>, JSON or XML.

### Binding

**Binding Expressions** using markup extensions. This reads as "Create a binding object and set its path property to the string Surname, and let that binding object manage the Text property of the TextBox. "Text" is the target and "Surname" is the source. 

```xml
<TextBox Text="{Binding Path=Surname}" />
```

How does silverlight know what object the source property comes from? Data contexts!

### Data contexts

* Multiple targets, one source

First, define the Data Source
```csharp
namespace BindingExample
{
	public class Person
	{
		public string GivenName {get;set;}
		public string Surname {get;set;}
		public double Age {get;set;}
	}
}
```
Second, instantiate the Data Source and set the DataContext of your UserControl

```csharp
namespace BindingExample
{
	public partial class MainPage: UserControl
	{
		Person src = new Person { GivenName = "Ian", Surname = "Griffiths", Age = 36.9 };

		public MainPage()
		{
			InitializeComponent();
			//set context
			this.DataContext = src;
		}
	}
}
```

All user interface elements have a DataContext. They are inherited properties, meaning one that cascades down the user interface tree. So setting this for a Panel would cascade down through every element in the panel. 

Finally, use the binding in the XAML

```csharp
...
<TextBox Text="{Binding Path="GivenName}" />
<TextBox Text="{Binding Path="Surname}" />
<TextBox Text="{Binding Path="Age}" />
...
```

**NOTE** that this is an example of one-way binding! You can change this by setting the Binding's Mode property to TwoWay:

```csharp
...
<TextBox Text="{Binding Path="GivenName, Mode=TwoWay}" />
<TextBox Text="{Binding Path="Surname, Mode=TwoWay}" />
<TextBox Text="{Binding Path="Age, Mode=TwoWay}" />
...
```

#### Binding Updates

What if the data source changes? The UI can be updated if the data source raises change notification events. This can be done by implementing INotifyPropertyChanged:
```csharp
namespace BindingExample
{
	public class Person : INotifyPropertyChanged
	{
		public string GivenName {get;set;}
		public string Surname {get;set;}

		private double _age;
		public double Age 
		{
			get { return _age; }
			set
			{
				if(value != _age)
				{
					_age = value;
					OnPropertyChanged("Age");
				}
			}
		}

		public event PropertyChangedEventHandler PropertyChanged;

		private void OnPropertyChanged(string propertyName)
		{
			if(PropertyChanged != null)
			{
				PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
			}
		}
	}
}
```


### DataTemplates

Ad-hoc bindings are helpful, but Silverlight offers a more structured approach to binding in the form of Data Templates. Just as controls have templates that define how they should look, Data Templates define how a data type should look.

* Defines how to present a type
* Used in conjunction with ItemsControls (ex. ListBox) and ContentControls (ex Buttons)
	* Any control that can accept an object in its Content property support Data Templates.
* A Data Template is just some Xaml markup with Binding expressions. 

Example without Data Templates:

```xml
<Grid>
  <Grid.ColumnDefinitions>
    <ColumDefinition Width="Auto" />
    <ColumnDefinition />
  </Grid.ColumnDefinitions>
  <Grid.RowDefinitions>
    <RowDefinition Height="Auto" />
    <RowDefinition Height="Auto" />
    <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>

  <TextBlock FontSize="18" Text="Given Name:" VerticalAlignment="Center "/>
  <TextBlock FontSize="18" Grid.Row="1" Text="Surname:" VerticalAlignment="Center "/>
  <TextBlock FontSize="18" Grid.Row="2" Text="Age:" VerticalAlignment="Center "/>

  <TextBox Grid.Column="1" Text="{Binding Path=GivenName, Mode=TwoWay}" />
  <TextBox Grid.Row="1" Grid.Column="1" Text="{Binding Path=Surname, Mode=TwoWay}" />
  <TextBox Grid.Row="2" Grid.Column="1" Text="{Binding Path=Age, Mode=TwoWay}" />
</Grid>
```

Example with Data Templates:

```xml
<UserControl.Resources>
  <DataTemplate x:Key="myPersonTemplate">
    <Grid>
      <Grid.ColumnDefinitions>
        <ColumDefinition Width="Auto" />
        <ColumnDefinition />
      </Grid.ColumnDefinitions>
      <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
      </Grid.RowDefinitions>

      <TextBlock FontSize="18" Text="Given Name:" VerticalAlignment="Center "/>
      <TextBlock FontSize="18" Grid.Row="1" Text="Surname:" VerticalAlignment="Center "/>
      <TextBlock FontSize="18" Grid.Row="2" Text="Age:" VerticalAlignment="Center "/>

      <TextBox Grid.Column="1" Text="{Binding Path=GivenName, Mode=TwoWay}" />
      <TextBox Grid.Row="1" Grid.Column="1" Text="{Binding Path=Surname, Mode=TwoWay}" />
      <TextBox Grid.Row="2" Grid.Column="1" Text="{Binding Path=Age, Mode=TwoWay}" />
    </Grid>
  </DataTemplate>
</UserControl.Resources>

<!-- This type of binding means you want the whole of the DataContext to be bound to this 
control. Recall that our DataContext is the single person object. Notice that silverlight
does not pick up the type automatically, you need to specify in the ContentTemplate -->
<ContentControl Content="{Binding}" ContentTemplate="{StaticResource myPersonTemplate}" />

```

### Binding to Collections

```csharp
<ListBox ItemsSource="{Binding}">
    <ListBox.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="Name: "></TextBlock>
                <TextBlock Text="{Binding Name}"></TextBlock>
                <Rectangle Fill="{Binding Color}"></Rectangle>
            </StackPanel>
        </DataTemplate>
    </ListBox.ItemTemplate>
</ListBox>
```

Alternatively, you can use a Resource dictionary template like in the previous example. This has the advantage of allowing you to bind to a control as a single item or a list using the same XAML. 

Each instance of a Data Template gets its own DataContext. 

#### Collection Updates

* The INotifyPropertyChanged interface deals with single objects.
* Use INotifyCollectionChanged to update list-bound controls. 
* ObservableCollection<T> 
	* We rarely have to implement this ourselves, use the generic collection

#### Grouping

To control grouping, you need to provide a CollectionViewSource:

```xml
<UserControl.Resources>
    <CollectionViewSource x:Key="mySource" Source="{Binding Path=RawItems}">
        <CollectionViewSource.GroupDescriptions>
            <PropertyGroupDescription PropertyName="EventTrack"></PropertyGroupDescription>
        </CollectionViewSource.GroupDescriptions>
    </CollectionViewSource>
</UserControl.Resources>
```

This also allows you to control Sorting. With a ListBox control, setting groups will only change the order in which the items are displayed. You can use the DataGrid control, which has an instrinsic understanding of groups and will display collapsible group headings when this is configured. 

* DataGrid understands grouping
* ItemsControl doesn't
	* but you do, and can make a visual distinction with a little effort. 
	* actually more flexible, you can come up with your own custom visualizations whereas DataGrid is opinionated. 
* When you wrap a collection in a CollectionViewSource, a Groups property is exposed. You can bind your ItemsControl directly to this Groups property!
	* the technique here is to nest ItemsControls with the top level being Groups and the nested control being Items

```xml
<ItemsControl ItemsSource="{Binding Path=Groups, Source={StaticResource groupedSource}}">
    <ItemsControl.ItemTemplate>
        <DataTemplate>
            <ItemsControl ItemsSource="{Binding Path=Items}"></ItemsControl>
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ItemsControl>
```
#### Hierarchical Binding

* TreeViewItem, MenuItem, custom controls
* Functionality provided by HeaderedItems control, which is the base class for all hierarchical controls. 
	* HeaderedItems derives from ItemsControl, so binding works in much the same way. 
	* You set the item source property to a collection, but the difference is in the template. 
* Must set the ItemTemplate property to a HierarchicalDataTemplate
	* HierarchicalDataTemplate adds one feature -- it too has an ItemsSource property where you can specify the children

```xml
<sdk:TreeView Grid.Row="1" Name="treeView">
    <sdk:TreeView.ItemTemplate>
        <sdk:HierarchicalDataTemplate ItemsSource="{Binding Path=Children}">
            <StackPanel>
                <TextBlock Text="{Binding Path=Type}"></TextBlock>
                <TextBlock Text=" : "></TextBlock>
                <TextBlock Text="{Binding Path=Name}"></TextBlock>
            </StackPanel>
        </sdk:HierarchicalDataTemplate>
    </sdk:TreeView.ItemTemplate>
</sdk:TreeView>
```

Hierarchical Data Binding happens on demand and can recurse infinitely. The example he used is outlining a Xaml application hierarchy in a TreeView where the TreeView itself is part of the tree. This would recurse infinitely until you ran out of memory or got bored. 

More precisely - a tree view always reads one level ahead. The children are pre-fetched because silverlight needs to know whether to show a collapsible arrow. 

## View Models

A View Model's job is to wrap some underlying data type in a way that makes it easy for the UI to present the data through Data Binding. 

ViewModels aim to solve a number of UI development challenges:

* Testing
	* problem: code behind lives in UI
	* instantiating the code behind calls initializecomponents() which will also instantiate the xaml and all of its baggage. 
* Flexibility and maintainability
	* Big, complex classes (code behind)
	* tends to happen gradually
	* VS doesn't help, as it tends to put your event handlers there and it is convenient. 
	* holding application state in UI
		* ex administering events, setting up new events, user might select a type from a list. In this situation, a lot of developers rely on a ComboBox to track the selection. Problem is, if the code that creates new talks goes to look at that combobox -- you have to instantiate and load the UI to populate the combobox in order to test. It also becomes harder to make changes to the UI, because your application logic implicitly expects that field to be represented by a combobox. If you change the combobox, you have to change the code depending on it.
	* Databinding insanity
		* tendency to depend on data binding behavior to implement features in application logic. 
		* ex relying on Data binding to validate input. Do not encode logic in your xaml!
* In the end, you want to avoid putting too much code in your code behind files. 
	* In an ideal world, all you would have is the constructor, but sometimes it is not possible to achieve that, but you should regard any code in the code behind as somewhat unsatisfactory. 

### Separated Presentation

This is a technique to avoid putting too much code in the code behind. 

* Bloated Code behind
	* UI manipulation
	* Interaction logic
	* Application logic
	* Kitchen sink
* Ideal code behind via Separated Presentation
	* UI manipulation
* Model should contain the application logic. 
	* however, you want to avoid directly connecting UI elements to model properties. Can lead to data binding insanity
* Because of this tendency, you typically want another class sitting in between code behind and Model -- the ViewModel!
	* contains Interaction Logic
		* ex handling autocomplete text boxes
	* adapts your application model for a typical user interface view

Note that we are calling it a ViewModel, but in reality there are a number of ways to accomplish separated presentation (MVC, MVP). We tend to avoid MVC because it is such a loaded term due to its history. It also is not an ideal fit for client side models which are interaction heavy whereas controllers are more concerned with actions.
 

### MVVM - ViewModels and data binding

* Model 
	* classic object model
	* comprised of POCO
	* no direct relationship to UI
	* model code should compile without any dependencies
	* often it is okay for server and client to share models - this is where WCF RIA services comes in.
* View
	* Xaml + Codebehind 
	* normally a UserControl
	* if you can split something up into a separate concept - do it. the more fine-grained your UI is the better off you are. It naturally discourages tendencies that lead to spaghetti code. 
	* for example, it is often useful to define a View that represents a single item in a list, rather than a Data Template
		* your data template would then become the View!
	* You will frequently regret creating Views that are too big and rarely regret creating those that are too simple. 
* ViewModel
	* For each view, you write a ViewModel - it's entire purpose is to serve a particular view
	* It's also possible for a view to have multiple ViewModels. 
	* Unlike the Model, it can use Silverlight types in its properties. 
		* ex. expose the visibility property of an element in the ViewModel. 
		* makes it possible to bind the visibility of an element directly to the ViewModel. 
	* Information flows between the View+ViewModel via DataBinding. This allows the ViewModel to change things in the view without needing to depend on the view. **Critical for testability**!

#### Simple Example - Forum 

**Models**

```csharp
public class MessageModel
{
    public UserModel MessageAuthor { get; set; }
    public string MessageTitle { get; set; }
    public string MessageBody { get; set; }
}

public class UserModel
{
    public string DisplayName { get; set; }
    public bool HasModerationPrivileges { get; set; }
}
```
**Services/Providers** -- to make testing easier -- used in application to retrieve models

```csharp
public interface IModelProvider
{
    UserModel LoggedInUser { get; }
    IEnumerable<MessageModel> GetAllMessages();
}

public class ModelProvider
{
    private static IModelProvider _provider;

    public static IModelProvider Get()
    {
        if (_provider == null)
        {
            _provider = new FakeModelProvider();
        }
        return _provider;
    }

    private class FakeModelProvider : IModelProvider
    {
        private IEnumerable<MessageModel> _messages;
 
        public FakeModelProvider()
        {
            LoggedInUser = new UserModel
            {
                DisplayName = "Robbie Hickey",
                HasModerationPrivileges = true
            };

            var user2 = new UserModel {DisplayName = "Arthur Dent"};
            var user3 = new UserModel {DisplayName = "Jane Doe"};

            _messages = new[]
            {
                new MessageModel
                {
                    MessageAuthor = LoggedInUser,
                    MessageTitle = "Welcome",
                    MessageBody = "Hello everyone."
                },
                new MessageModel
                {
                    MessageAuthor = user2,
                    MessageTitle = "Re: Welcome",
                    MessageBody = "Thanks!"
                },
                new MessageModel
                {
                    MessageAuthor = user3,
                    MessageTitle = "I'm bored",
                    MessageBody = "This forum is lame!"
                }
            };
        }

        public UserModel LoggedInUser { get; private set; }
        public IEnumerable<MessageModel> GetAllMessages()
        {
            return _messages;
        }
    }
}

```
**AllMessages View and ViewModel**

```xml
<UserControl x:Class="SilverlightApplication1.Views.AllMessagesView"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="300" d:DesignWidth="400">
    
    <Grid x:Name="LayoutRoot" Background="White">
        <ScrollViewer>
            <ItemsControl ItemsSource="{Binding Path=MessageItems}">
                <ItemsControl.ItemTemplate>
                    <vw:MessageItemView />
                </ItemsControl.ItemTemplate>
            </ItemsControl>
        </ScrollViewer>
    </Grid>
</UserControl>
```
```csharp
public partial class AllMessagesView : UserControl
{
    AllMessagesViewModel _vm;

    public AllMessagesView()
    {
        InitializeComponent();

        _vm = new AllMessagesViewModel(ModelProvider.Get());
        this.DataContext = _vm;
    }
}

public class AllMessagesViewModel
{
    public AllMessagesViewModel(IModelProvider provider)
    {
        MessageItems = provider.GetAllMessages()
                           .Select(message => new MessageItemViewModel(provider, message));
    }

    public IEnumerable<MessageItemViewModel> MessageItems { get; private set; }
}
```

**MessageItem View and ViewModel** - notice the d:DataContext where we are telling Visual Studio what type to expect as the ViewModel. d: comes from a namespace that declares design time metadata. Also notice the 'vm' namespace to reference the ViewModel. Note that this gives us no runtime assurances on the type of the viewmodel. We still need to ensure that. You will need to set this up if you want to use Visual Studio properties window to declare your bindings as I've described below

```xml
<UserControl x:Class="SilverlightApplication1.Views.MessageItemView"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:vm="clr-namespace:SilverlightApplication1.ViewModels"
    mc:Ignorable="d"
    d:DesignHeight="300" d:DesignWidth="400"
    d:DataContext="{d:DesignInstance vm:MessageItemViewModel}">
    
	<!-- Lots of xaml and data binding expressions-->
	...
	<StackPanel x:Name="moderatorActions" Visibility="{Binding Path=ModerationToolsVisibility}">
		<Button Content="Hide" />
		<Button Content="Move" />
	</StackPanel>
	...
</UserControl>
```

```csharp
// It's empty! Doesn't even need to load its view model
// All interesting code lives in ViewModel
public partial class MessageItemView : UserControl
{
    public AllMessagesView()
    {
        InitializeComponent();
    }
}

public class MessageItemViewModel
{
    private IModelProvider _provider;
    private MessageModel _message;

    public MessageItemViewModel(IModelProvider provider, MessageModel message)
    {
        _message = message;
        _provider = provider;
    }

    public string UserDisplayName {get { return _message.MessageAuthor.DisplayName; }}
    public string MessageTitle { get { return _message.MessageTitle; } }
    public string MessageBody {get { return _message.MessageBody; }}

	//use silverlight Visibility type!
    public Visibility ModerationToolsVisibility
    {
        get { return _message.MessageAuthor.HasModerationPrivileges ? Visibility.Visible : Visibility.Collapsed; }
    }
}
```

Note that you can allow Visual Studio to configure the bindings using the properties window for a control. Go to the Text field -> Right click -> Apply Data Binding. This will give you a popup with your DataContext instance where you can select a property. 

#### UI vs ViewModel

* Choosing the split is hard
* Feature-rich UI elements can be especially difficult
	* ex Listbox controls what element is selected, which in theory should live in ViewModel.
	* solution: let Listbox handle selection for you, and bind the SelectedItem property to the ViewModel using a two-way binding. 
	* Don't throw out perfectly good built-in behaviors!
* If a control already does what you want out of the box, favor pragmatism, let the control handle it
* If you find yourself modifying code heavily to work around the way a control works, that might be a sign you should put it in ViewModel. 

### Commands

In the earlier example, we were choosing whether to show the LoggedInUser moderation tools based on a property of the ViewModel. But what do you do when a user invokes one of the actions?

It might seem like this requires code behind, and up until Silverlight 4 that was the approach, but now we prefer Commands!

* Common pattern that combines two features
	* Inputs trigger invocation of an action
	* Whether the action is enabled. 

ICommand Interface
```csharp
public interface ICommand
{
	void Execute(object parameter);
	bool CanExecute(object parameter);
	event EventHandler CanExecuteChanged;
}
```
Buttons and Hyperlinks are able to use ICommand objects but we have to create them. Typical pattern is - if the UI needs to execute a command, it invokes a method on the ViewModel. Here is an example of a DelegateCommand which is responsible for relaying calls to specific methods on our ViewModel. This prevents us from having to write a new ICommand implementation for every one of our application's commands, which may be substantial:

```csharp
public class DelegateCommand : ICommand
{
	Action _handler;
	public DelegateCommand(Action handler)
	{
		_handler = handler;
	}
	
	public void Execute(object parameter)
	{
		_handler();
	}

	private bool _isEnabled = true;
	public bool IsEnabled
	{
		get { return _isEnabled; }
		set 
		{
			_isEnabled = value;
			if(CanExecuteChanged != null)
			{
				CanExecuteChanged(this, EventArgs.Empty);
			}
		}
	}
	
	public bool CanExecute(object parameter)
	{
		return IsEnabled;
	}

	public event EventHandler CanExecuteChanged;
}
```
Now, taking a previous example, we will re-implement using commmands:

```csharp
public class MessageItemViewModel
{
    private IModelProvider _provider;
    private MessageModel _message;
	private RelayCommand _replyCommand;

    public MessageItemViewModel(IModelProvider provider, MessageModel message)
    {
        _message = message;
        _provider = provider;

		_replyCommand = new DelegateCommand(OnReply);
    }

	private void OnReply()
	{
		// do something
	}

	public ICommand ReplyCommand { get { return _replyCommand; } }

    public string UserDisplayName {get { return _message.MessageAuthor.DisplayName; }}
    public string MessageTitle { get { return _message.MessageTitle; } }
    public string MessageBody {get { return _message.MessageBody; }}

	//use silverlight Visibility type!
    public Visibility ModerationToolsVisibility
    {
        get { return _message.MessageAuthor.HasModerationPrivileges ? Visibility.Visible : Visibility.Collapsed; }
    }
}
```

```xml
<UserControl x:Class="SilverlightApplication1.Views.MessageItemView"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:vm="clr-namespace:SilverlightApplication1.ViewModels"
    mc:Ignorable="d"
    d:DesignHeight="300" d:DesignWidth="400"
    d:DataContext="{d:DesignInstance vm:MessageItemViewModel}">
    
	<!-- Lots of xaml and data binding expressions-->
	...
	<Button Content="Reply" Command="{Binding Path=ReplyCommand}" />
	<StackPanel x:Name="moderatorActions" Visibility="{Binding Path=ModerationToolsVisibility}">
		<Button Content="Hide" />
		<Button Content="Move" />
	</StackPanel>
	...
</UserControl>
```
Now this is all very testable. If you want to simulate a user reply you can easily, and you can also enable/disable the command on demand to test behavior. 

## Printing

Not including for now as it is not especially relevant for my task..