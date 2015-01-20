## GenericWebService Overview

The GenericWebService is a SOAP service that leverages WSE (Web Service Enhancements). It serves as the broker for calls that are made to an Operation Library, the assemblies of which are loaded at runtime. These Operation Libraries are what do the actual work, and are responsible for translating the request into a resultset. The GenericWebService currently has 3 operation library assemblies:

1. OITLibrary
2. OITMapProcessor
3. ESGenericWebServiceLibrary

DLLs must be registered in the web.config of the GenericWebService or the host process will not know where to find them:

```xml
<DLLS>
	<add name="Library" type="OITLibrary.Library,OITLibrary.dll" />
	<add name="Mapper" type="OITMapProcessor.MapProcessor,OITMapProcessor.dll" />
	<add name="EstimatedPayments" type="ESGenericWebServiceLibrary.ESLibrary,ESGenericWebServiceLibrary.dll" />
</DLLS>
```

Each assembly should contain an implementation of the IContract interface:

```csharp
public interface IContract
{
	// returns a list of supported operations
    string Operations();
	// takes an operation xml request and returns an xml result for the operation
    string Operate(string requestXml, PingTrustCaller user);
	// takes an operation name and returns a boolean of whether the operation is supported by the assembly
    bool SupportOperation(string requestedOperation);
}
```

Using IContract.SupportOperation, the GenericWebService will determine which DLL should handle the incoming request. If no operation is found then an exception is thrown. 

While it now has additional methods, they exist as a convenience to the consumer. Every call that gets made to the GenericWebService gets funneled into the Operate method. Operate takes an XML request string that should conform to the following format:

```xml
<Operations>
	<Operation>
		<Name>{{Operation Name}}</Name>
		<Parameters>
			<Parameter1>{{Parameter1 value}}</Parameter1>
			<Parameter2>{{Parameter2 value}}</Parameter2>
		</Parameters>
	</Operation>
</Operations>

```
Note that the Parameters node is optional, and only needs to be passed when parameters are required by the stored procedure. 

The Operation Name can be determined by looking in the Operation Library assemblies at the Extension class. The class contains a Dictionary of library operations, which map the operation name to the name of a stored procedure:

```csharp
public static class Extension
{
    public static Dictionary<string, string> LibraryOperations = new Dictionary<string, string>
                          {   {"Entity", "xml_GetEntitiesForMapper"}, 
                              {"Entities", "xml_GWS_GetEntities"},
							  // ommitted for brevity
                          };
}
```

As you have probably guessed, the Parameters node should contain the stored procedure parameters in the order in which they are required. Because of the fact that the Operation Library is returning the result of the stored procedure, there is no standardized result format. 

If you want to know what operations the endpoint supports, you can call the Operations method, which will return every operation that the assembly supports as well as the parameters it requires

## Calling the GenericWebService

> TFS Project Location: $/OnesourceIncomeTax/RS/trunk/Build/dotNet/Services/GenericWebServiceCaller
> TFS DLL Location: $/OnesourceIncomeTax/RS/trunk/Build/dotNet/References/GenericWebServiceCaller.dll

The contstructor of the WebServiceProxy class for the GenericWebService takes three parameters:

1. An instance of ILog
2. A string representing the OIT account
3. A string representing the source system

```csharp
using GenericWebServiceCaller;

public class Program
{
	public static ILog Log = LogManager.GetLogger(typeof(WebServiceProxy));

	public static void Main(string[] args)
	{
		var proxy = new WebServiceProxy(Log, "PROV", "IncomeTax");
		var unitXml = "<Operations><Operation><Name>Units</Name></Operation></Operations>";

		if(proxy.Initialize())
		{
			var xmlResults = proxy.GetSourceData(unitXml);
		}	
		else
		{
			// proxy.ErrorStatus will contain the error message
		}
	} 
}

```

## Testing the GenericWebService

You can use the following project: $/OnesourceIncomeTax/RS/trunk/Build/dotNet/Services/WebServiceTestHarness

This is a Windows Forms application that allows you to specify an environment to execute in, a method to call, and the operations xml to pass. You can also override the source account and target application. This is great for seeing what the response format will be and to test expected behavior. 

 ![](http://i.imgur.com/alNE2Jf.png)

## Operation XML Format for Mapper

These examples are taken straight from the output of the Operations endpoint.

### Entity => xml_GetEntitiesForMapper

Request Format
```xml
<Operations>
   <Operation>
    <Name>Entity</Name>
  </Operation>
</Operations>
```
Example Response

```xml
<MappableItemData>
    <MappableItemDatum>
        <EntityID>N1910101390007WO</EntityID>
        <CompanyNo></CompanyNo>
        <Name>1065 Entity - K-1 Transfer From</Name>
        <Company_Type>Partnership</Company_Type>
    </MappableItemDatum>
    <MappableItemDatum>
        <EntityID>N1910101390007WQ</EntityID>
        <CompanyNo></CompanyNo>
        <Name>1065 Entity - K-1 Transfer To</Name>
        <Company_Type>Partnership</Company_Type>
    </MappableItemDatum>
    <MappableItemDatum>
        <EntityID>N191010133000C0Z</EntityID>
        <CompanyNo></CompanyNo>
        <Name>1120 REIT</Name>
        <Company_Type>Subsidiary</Company_Type>
    </MappableItemDatum>
</MappableItemData>
 
```

### GetEstimatedPayments => xml_ES_GetEstimatedPayments

Request Format
```xml
<Operations>
    <Operation>
        <Name>GetEstimatedPayments</Name>
        <Parameters>
            <Year>string</Year>
            <TimePeriod ID=TimePeriodID>
                <Jurisdiction>string</Jurisdiction>
            </TimePeriod>
        </Parameters>
    </Operation>
</Operations>
```
Example Response
```xml
<EstimatedPayments>
    <TimePeriod ID="1">
        <Company Number="A001">
            <Jurisdiction ID="FED">
                <Amount>15000</Amount>
            </Jurisdiction>
        </Company>
        <Company Number="A002">
            <Jurisdiction ID="FED">
                <Amount>25000</Amount>
            </Jurisdiction>
        </Company>
    </TimePeriod>
    <TimePeriod ID="2">
        <Company Number="A001">
            <Jurisdiction Id="AL">
                <Amount>15000</Amount>
            </Jurisdiction>
            <Jurisdiction Id="CA">
                <Amount>16000</Amount>
            </Jurisdiction>
        </Company>
        <Company Number="A002">
            <Jurisdiction Id="AL">
                <Amount>25000</Amount>
            </Jurisdiction>
            <Jurisdiction Id="CA">
                <Amount>26000</Amount>
            </Jurisdiction>
        </Company>
    </TimePeriod>
</EstimatedPayments>
```

### SaveMapSet => xml_SaveMapSet

Request Format

```xml
<Operations>
    <Operation>
        <Name>SaveMapSet</Name>
        <Parameters>
            <SourceSystemID>string</SourceSystemID>
            <TargetSystemID>string</TargetSystemID>
            <MapSetID>string</MapSetID>
            <MapSetName>string</MapSetName>
            <MapSetDescription>string</MapSetDescription>
            <MapSetXml>string</MapSetXml>
            <Year>string</Year>
            <UserID>string</UserID>
            <Output>string</Output>
        </Parameters>
    </Operation>
</Operations>
```

### LoadAllMapSetWithMapSummary => xml_LoadAllMapSetWithMapSummary

Request Format
```xml
<Operations>
    <Operation>
        <Name>LoadAllMapSetWithMapSummary</Name>
        <Parameters>
            <SourceSystemID>string</SourceSystemID>
            <TargetSystemID>string</TargetSystemID>
            <Year>string</Year>
            <Output>string</Output>
        </Parameters>
    </Operation>
</Operations>

```