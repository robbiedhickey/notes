## Steps to Fix Dagger

1. Drop and re-add references to otxDataAccess, otxLogic and otxUtilities
	1. currently they are pointing at WorkflowTools, which is why most OIT folks can't build this
2. otxLogic is out of date, our references copy doesn't have the client sub id changes. We need to address this.
3. Unit test projects have a number of problems. I've excluded them from the solution build and all is working. 

In order to upgrade this project to VS2013 we will need to update Dagger to the Silverlight 5 runtime. http://msdn.microsoft.com/en-us/vstudio/dn250998.aspx

Silverlight 4 has reached the end of its product lifecycle.

## Deploying Dagger

Deployments are a manual process for Dagger. Changes must be communicated to the stakeholders on the OSP team.

### Contacts

Database Changes:

* CWT DBA Team
	* CWT DBA Team <CWT.DBATeam@thomsonreuters.com>

Code Changes:

* Abhinav Mukesh - Lead Software Engineer 
	* Mukesh, Abhinav (Tax&Accounting) <abhinav.mukesh@thomsonreuters.com>
* Raj Gopal Naramsetty - Senior Software Engineer
	* Naramsetty, Raj Gopal (Tax&Accounting) <rajgopal.naramsetty@thomsonreuters.com>
* Srinivasulu Setty - Contractor
	* Setty, Srinivasulu <srinivasulu.thirumalasetty@thomsonreuters.com>


## Exporting Dagger to Another DB

### Tables

##### Maps

```sql
/****** Object:  Table [dbo].[Maps]    Script Date: 10/14/2014 9:29:52 AM ******/
SET ANSI_NULLS OFF
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[Maps](
	[Map_ID] [int] NOT NULL,
	[Source_System_ID] [int] NOT NULL,
	[Target_System_ID] [int] NOT NULL,
	[Txn_Start] [datetime] NOT NULL,
	[Txn_End] [datetime] NOT NULL,
	[Year] [smallint] NOT NULL,
	[Map_Type_ID] [int] NOT NULL,
	[Map_Name] [nvarchar](64) NOT NULL,
	[Map_Description] [nvarchar](64) NULL,
	[Map_Xml] [xml] NULL,
	[User_ID] [nvarchar](64) NULL,
 CONSTRAINT [PK_Maps] PRIMARY KEY CLUSTERED 
(
	[Map_ID] ASC,
	[Source_System_ID] ASC,
	[Target_System_ID] ASC,
	[Txn_Start] ASC,
	[Txn_End] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]

GO
```

##### MapSet

```sql
/****** Object:  Table [dbo].[MapSet]    Script Date: 10/14/2014 9:30:48 AM ******/
SET ANSI_NULLS OFF
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[MapSet](
	[MapSet_ID] [int] NOT NULL,
	[Source_System_ID] [int] NOT NULL,
	[Target_System_ID] [int] NOT NULL,
	[Txn_Start] [datetime] NOT NULL,
	[Txn_End] [datetime] NOT NULL,
	[Year] [smallint] NOT NULL,
	[MapSet_Name] [nvarchar](64) NOT NULL,
	[MapSet_Description] [nvarchar](64) NULL,
	[IsDefault] [tinyint] NOT NULL,
	[User_ID] [nvarchar](64) NULL,
 CONSTRAINT [PK_MapSet] PRIMARY KEY CLUSTERED 
(
	[MapSet_ID] ASC,
	[Source_System_ID] ASC,
	[Target_System_ID] ASC,
	[Txn_Start] ASC,
	[Txn_End] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
```

##### MapSetMapXref

```sql
/****** Object:  Table [dbo].[MapSetMapXref]    Script Date: 10/14/2014 9:31:08 AM ******/
SET ANSI_NULLS OFF
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[MapSetMapXref](
	[MapSet_ID] [int] NOT NULL,
	[Source_System_ID] [int] NOT NULL,
	[Target_System_ID] [int] NOT NULL,
	[Txn_Start] [datetime] NOT NULL,
	[Txn_End] [datetime] NOT NULL,
	[Map_ID] [int] NOT NULL,
	[User_ID] [nvarchar](64) NULL,
 CONSTRAINT [PK_MapSetMapXref] PRIMARY KEY CLUSTERED 
(
	[MapSet_ID] ASC,
	[Source_System_ID] ASC,
	[Target_System_ID] ASC,
	[Map_ID] ASC,
	[Txn_Start] ASC,
	[Txn_End] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
```

### StoredProcedures

##### LoadAllMapSetWithMapSummary

Corresponds to DaggerSystemService.GetMapsXml

SQL

```sql
/****** Object:  StoredProcedure [dbo].[xml_LoadAllMapSetWithMapSummary]    Script Date: 10/14/2014 10:16:47 AM ******/
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO
create proc [dbo].[xml_LoadAllMapSetWithMapSummary] @Source_System_ID int, 
                                   @Target_System_ID int,
                                   @Year             smallint,                                  
                                   @DebugFlag tinyint = 0
as
begin --{

  SET NOCOUNT ON
  SET ANSI_NULLS ON
  SET QUOTED_IDENTIFIER ON

  if @DebugFlag >= 1
  begin
    select 'xml_LoadAllMapSetWithMapSummary --> Entry'
  end

  declare  @xmlout xml
  declare @DB_Max_Date datetime
  
  set @DB_Max_Date = dbo.fn_Get_DB_Max_Date()

  set @xmlout = (select 
                   ( select MapSet_Name as Name, MapSet_Description as Description, MapSet_ID as MapSet_ID, IsDefault, 
                            ( select Map_ID as 'Map_ID'
                              from MapSetMapXref B
                              where Source_System_ID = @Source_System_ID
                              and   Target_System_ID = @Target_System_ID
                              and   A.MapSet_ID = B.MapSet_ID
                              and   B.Txn_End = @DB_Max_Date
                              for xml path ('AssociatedMaps'), type )
                     from MapSet A	
                     where Source_System_ID = @Source_System_ID
                     and   Target_System_ID = @Target_System_ID			  
                     and   Year = @Year
                     and   Txn_End = @DB_Max_Date
                     for xml path ('MapSet') , type, root('MapSets')),
                   ( select Map_Name AS Name, Map_Description AS Description, Map_Type_ID as Type_ID, Map_ID AS Map_ID
                     from Maps
                     where Source_System_ID = @Source_System_ID
                     and   Target_System_ID = @Target_System_ID			  
                     and   Year = @Year			
                     and   Txn_End = @DB_Max_Date					  		  
                     for xml path ('Map') , type, root('Maps') )
                for xml path (''), type, root('MapsInfo') ) 

  select @xmlout

  if @DebugFlag >= 1
  begin
    select 'xml_LoadAllMapSetWithMapSummary --> Exit'
  end

  return
  
end --}

GO
```

Operation XML

```xml
<Operation>
  <Name>LoadAllMapSetWithMapSummary</Name>
  <Parameters>
    <SourceSystemID>1</SourceSystemID>
    <TargetSystemID>2</TargetSystemID>
    <Year>2012</Year>
  </Parameters>
</Operation>
```

##### LoadMap

Corresponds to DaggerSystemService.GetMappingXMLForMap

SQL

```sql
/****** Object:  StoredProcedure [dbo].[xml_LoadMap]    Script Date: 10/14/2014 10:23:30 AM ******/
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO



CREATE PROCEDURE [dbo].[xml_LoadMap] @Source_System_ID int, @Target_System_ID int, @Map_ID int
AS
BEGIN

  SET NOCOUNT ON
  SET ANSI_NULLS ON
  SET QUOTED_IDENTIFIER ON

  declare @DB_Max_Date datetime

  set @DB_Max_Date = dbo.fn_Get_DB_Max_Date() 

  select Map_Xml
  from   Maps
  where  Source_System_ID = @Source_System_ID
  and    Target_System_ID = @Target_System_ID
  and    Map_ID           = @Map_ID
  and    Txn_End          = @DB_Max_Date

END

GO
```

Operation XML

```xml
<Operation>
  <Name>LoadMap</Name>
  <Parameters>
    <SourceSystemID>1</SourceSystemID>
    <TargetSystemID>2</TargetSystemID>
    <MapID>1</MapID>
  </Parameters>
</Operation>
```

##### SaveMap

Corresponds to DaggerSystemService.SaveMapInfo

SQL
```sql
/****** Object:  StoredProcedure [dbo].[xml_SaveMap]    Script Date: 10/14/2014 10:24:09 AM ******/
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO



CREATE PROCEDURE [dbo].[xml_SaveMap] @Source_System_ID int, @Target_System_ID int, @Map_ID int, @Map_Name nvarchar(64),
                              @Map_Type_ID int, @Map_Xml xml, @Map_Description nvarchar(64), 
                              @Year smallint, @User_ID nvarchar(64), @SaveMethod smallint
AS
BEGIN -- {

  SET NOCOUNT ON
  SET ANSI_NULLS ON
  SET QUOTED_IDENTIFIER ON

  declare @New_Map_ID int
  declare @Error_Num  int
  declare @Error_Msg  varchar(255)
  declare @xmlout xml
  declare @Success tinyint
  declare @DB_Max_Date datetime
  declare @CurDate     datetime

  set @Success = 1 -- Success
  set @DB_Max_Date = dbo.fn_Get_DB_Max_Date()
  set @CurDate     = getdate()

  -- Fetch Map_Type_ID for the given Map_ID
  if (@Map_ID IS NOT NULL AND ( @Map_Type_ID IS NULL  or @Year IS NULL) )
  begin
     select @Map_Type_ID = Map_Type_ID, @Year = Year 
     from  Maps
     where Source_System_ID = @Source_System_ID
     and   Target_System_ID = @Target_System_ID
     and   Map_ID           = @Map_ID
     and   Txn_End          = @DB_Max_Date
  end

  if (@SaveMethod = 1) -- Create new map
  begin -- {
     select @New_Map_ID = max(Map_ID) from Maps where Source_System_ID = @Source_System_ID and Target_System_ID = @Target_System_ID

     set @New_Map_ID = isnull(@New_Map_ID,0) + 1

     insert into Maps (Map_ID, Source_System_ID, Target_System_ID, Map_Name, Map_Type_ID, Map_Xml, Map_Description, Year, User_ID, Txn_Start, Txn_End)
     Values (@New_Map_ID, @Source_System_ID, @Target_System_ID, @Map_Name, @Map_Type_ID, @Map_Xml, @Map_Description, @Year, @User_ID, @CurDate, @DB_Max_Date)

     select @Error_Num  = @@ERROR

     if @Error_Num<>0
     begin -- {
        select @Error_Msg = 'Error with Saving Map - Insert Failed in Maps'
        set @Success = 0
     end -- }
  end -- }
  else if (@SaveMethod = 2) -- Update Map info (Name/Description)
  begin -- {
    -- For Temporal DB Update -- Set the Txn End for the existing record as current date
    Update Maps
    set   Txn_End = @CurDate
    where Source_System_ID = @Source_System_ID
    and   Target_System_ID = @Target_System_ID
    and   Map_ID           = @Map_ID
    and   Txn_End          = @DB_Max_Date

    select @Error_Num  = @@ERROR

    if @Error_Num = 0
    begin --{
       insert into Maps (Map_ID, Source_System_ID, Target_System_ID, Map_Name, Map_Type_ID, Map_Xml, Map_Description, Year, User_ID, Txn_Start, Txn_End)
       select Map_ID, Source_System_ID, Target_System_ID, @Map_Name, Map_Type_ID, Map_Xml, @Map_Description, Year, @User_ID, @CurDate, @DB_Max_Date
       from   Maps
       where  Source_System_ID = @Source_System_ID
       and    Target_System_ID = @Target_System_ID
       and    Map_ID           = @Map_ID
       and    Txn_End          = @CurDate

       select @Error_Num  = @@ERROR

    end --}

    if @Error_Num <> 0
    begin --{
      select @Error_Msg = 'Error with Saving Map - while updating map info'
      set @Success = 0     
    end --}
  end -- }
  else if (@SaveMethod = 3) -- Update Map Xml
  begin -- {
    -- For Temporal DB Update -- Set the Txn End for the existing record as current date
    Update Maps
    set   Txn_End = @CurDate
    where Source_System_ID = @Source_System_ID
    and   Target_System_ID = @Target_System_ID
    and   Map_ID           = @Map_ID
    and   Txn_End          = @DB_Max_Date

    select @Error_Num  = @@ERROR

    if @Error_Num = 0
    begin --{
       insert into Maps (Map_ID, Source_System_ID, Target_System_ID, Map_Name, Map_Type_ID, Map_Xml, 
                         Map_Description, Year, User_ID, Txn_Start, Txn_End)
       select Map_ID, Source_System_ID,  Target_System_ID, Map_Name, Map_Type_ID, @Map_Xml, 
               Map_Description, Year, @User_ID, @CurDate, @DB_Max_Date
       from   Maps
       where  Source_System_ID = @Source_System_ID
       and    Target_System_ID = @Target_System_ID
       and    Map_ID           = @Map_ID
       and    Txn_End          = @CurDate

       select @Error_Num  = @@ERROR
    end -- }

    if @Error_Num<>0
    begin --{
      select @Error_Msg = 'Error with Saving Map - while updating map association'        
      set @Success = 0     
    end --}
  end -- }

  set @xmlout = ( select @Success as Success, @Error_Msg as Error,
                         ( select Map_Name AS Name, Map_Description AS Description, Map_Type_ID as Type_ID, Map_ID AS Map_ID
                           from Maps
                           where Source_System_ID = @Source_System_ID
                           and   Target_System_ID = @Target_System_ID			  
                           and   Year = @Year
                           and   Txn_End = @DB_Max_Date
                           and   Map_ID = case when @Map_ID IS NULL then @New_Map_ID else @Map_ID end		  
                           for xml path ('Map') , type 		     
                          ) 
                  for xml path ('Result'), type )
   select    @xmlout 

END -- }

GO
```

Operation XML

```xml
<Operation>
  <Name>SaveMap</Name>
  <Parameters>
    <SourceSystemID>1</SourceSystemID>
    <TargetSystemID>2</TargetSystemID>
    <MapID>1</MapID>
    <MapName>mapName</MapName>
    <MapTypeID>1</MapTypeID>
    <MapXml><![CDATA[<xml></xml>]]></MapXml>
    <MapDescription>description</MapDescription>
    <Year>2012</Year>
    <UserID>123</UserID>
    <SaveFlag>true</SaveFlag>
  </Parameters>
</Operation>
```

##### SaveMapSet

Corresponds to DaggerSystemService.SaveMapSetInfo

```sql
/****** Object:  StoredProcedure [dbo].[xml_SaveMapSet]    Script Date: 10/14/2014 10:24:38 AM ******/
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO


CREATE PROCEDURE [dbo].[xml_SaveMapSet] @Source_System_ID int, @Target_System_ID int, @MapSet_ID int, 
                                        @MapSet_Name nvarchar(64), @MapSet_Description  nvarchar(64), @MapSetMapXml xml,
                                        @Year smallint, @User_ID nvarchar(64), @IsDefault tinyint = 0
AS
BEGIN

-- 1. Save all info with Default --> @IsDefault should be 1 and goes through the code and calls proc_SetDefaultMapSet
-- 2. Save all info but Default  --> @IsDefault is expected to be zero.  Goes through the code but does not call proc_SetDefaultMapSet
-- 3. Just Modify Default --> @MapSet_Name, @MapSet_Description and @MapSetMapXml all needs to be null. Goes to proc_SetDefaultMapSet call directly.

    SET NOCOUNT ON
    SET ANSI_NULLS ON
    SET QUOTED_IDENTIFIER ON

	declare @Maps table (Map_ID int null)

    declare @Max_Date        datetime,
            @Cur_Date        datetime,
            @Work_MapSet_ID  int,
            @Error_Desc      nvarchar(128),
            @Error_Num       int

    set @Max_Date = dbo.fn_Get_DB_Max_Date()
    set @Cur_Date = getdate()
    set @Work_MapSet_ID = @MapSet_ID
    if (@IsDefault is null) set @IsDefault = 0

    if ( @MapSet_Name is not null or @MapSet_Description is not null or @MapSetMapXml is not null )
    begin
		if (@MapSetMapXml IS NOT NULL )
		begin 
		  insert into @Maps (Map_ID)
		  select a.b.value('.','int') as Map_ID
		  from   @MapSetMapXml.nodes ('/AssociatedMaps/Map_ID') a(b)
		end

		if ( @MapSet_ID is null ) --> New Mapset
		begin
		  select @Work_MapSet_ID = max(MapSet_ID) from MapSet where Source_System_ID = @Source_System_ID and Target_System_ID = @Target_System_ID

		  set @Work_MapSet_ID = isnull(@Work_MapSet_ID, 0) + 1
		  set @IsDefault = case when @Work_MapSet_ID = 1 then 1 else 0 end

		  insert into MapSet (MapSet_ID, Source_System_ID, Target_System_ID, MapSet_Name, MapSet_Description, Year, User_ID, Txn_Start, Txn_End, IsDefault)
		  values (@Work_MapSet_ID, @Source_System_ID, @Target_System_ID, @MapSet_Name, @MapSet_Description, @Year, @User_ID, @Cur_Date, @Max_Date, @IsDefault)

		  insert into MapSetMapXref (MapSet_ID, Source_System_ID, Target_System_ID, Map_ID, User_ID, Txn_Start, Txn_End)
		  select @Work_MapSet_ID, @Source_System_ID, @Target_System_ID, Map_ID, @User_ID, @Cur_Date, @Max_Date
		  from   @Maps
		end
		else  --> Existing MapSet
		begin
		  if exists (select 'x' from MapSet where Source_System_ID = @Source_System_ID and Target_System_ID = @Target_System_ID and MapSet_ID = @MapSet_ID )
		  begin
			update MapSet
			set    Txn_End = @Cur_Date
			where  Source_System_ID = @Source_System_ID 
			and    Target_System_ID = @Target_System_ID 
			and    MapSet_ID        = @Work_MapSet_ID
			and    Txn_End          = @Max_Date

			update MapSetMapXref
			set    Txn_End = @Cur_Date
			where  Source_System_ID = @Source_System_ID 
			and    Target_System_ID = @Target_System_ID 
			and    MapSet_ID        = @Work_MapSet_ID
			and    Txn_End          = @Max_Date

			insert into MapSet (MapSet_ID, Source_System_ID, Target_System_ID, MapSet_Name, MapSet_Description, Year, User_ID, IsDefault, Txn_Start, Txn_End)
			values (@Work_MapSet_ID, @Source_System_ID, @Target_System_ID, @MapSet_Name, @MapSet_Description, @Year, @User_ID, @IsDefault, @Cur_Date, @Max_Date)

			insert into MapSetMapXref (MapSet_ID, Source_System_ID, Target_System_ID, Map_ID, User_ID, Txn_Start, Txn_End)
			select @Work_MapSet_ID, @Source_System_ID, @Target_System_ID, Map_ID, @User_ID, @Cur_Date, @Max_Date
			from   @Maps
		  end
		  else
		  begin
			set @Error_Desc = 'Error while saving mapset - MapSet identifier passed does not exist'
			goto endofproc
		  end
		end
    end

    if (@IsDefault = 1) 
    begin 
       if not exists ( select 'x' from MapSet where Source_System_ID = @Source_System_ID and Target_System_ID = @Target_System_ID 
                       and MapSet_ID = @Work_MapSet_ID and Year = @Year and IsDefault = 1 and Txn_End = @Max_Date )
       begin 
          exec proc_SetDefaultMapSet @Source_System_ID, @Target_System_ID, @Work_MapSet_ID, @Year, @User_ID, @Cur_Date, @Max_Date
       end
    end

endofproc:
    if (@Error_Desc is null)
    begin
       select MapSet_Name as Name, MapSet_Description as Description, MapSet_ID as MapSet_ID, IsDefault,
		         (select Map_ID as 'Map_ID'
                  from MapSetMapXref B
                  where Source_System_ID = B.Source_System_ID
                  and   Target_System_ID = B.Target_System_ID
                  and   A.MapSet_ID      = B.MapSet_ID
                  and   B.Txn_End        = @Max_Date
                  for xml path (''), type, root('AssociatedMaps') )
	   from MapSet A	
       where Source_System_ID = @Source_System_ID
       and   Target_System_ID = @Target_System_ID			  
       and   Year             = @Year
       and   MapSet_ID        = @Work_MapSet_ID
       and   A.Txn_End        = @Max_Date
       for xml path ('MapSet') , type, root ('Result')
    end
    else
    begin
       select @Error_Desc as Error
       for xml path('Result')
    end

END

GO
```

Operation XML

```xml
<Operation>
  <Name>SaveMapSet</Name>
  <Parameters>
    <SourceSystemID>2</SourceSystemID>
    <TargetSystemID>1</TargetSystemID>
    <MapSetID>2</MapSetID>
    <MapSetName>name</MapSetName>
    <MapSetDescription>description</MapSetDescription>
    <MapSetXml><![CDATA[<xml></xml]]></MapSetXml>
    <Year>2012</Year>
    <UserID>123</UserID>
    <IsDefault>1</IsDefault>
  </Parameters>
</Operation>
```

##### CopyMap

Corresponds to DaggerSystemService.CopyMap

Operation XML

```xml
<Operation>
  <Name>CopyMap</Name>
  <Parameters>
    <SourceSystemID>1</SourceSystemID>
    <TargetSystemID>2</TargetSystemID>
    <MapID>1</MapID>
    <UserID>123</UserID>
  </Parameters>
</Operation>
```

##### DeleteMap

Corresponds to DaggerSystemService.DeleteMap

SQL
```sql
/****** Object:  StoredProcedure [dbo].[xml_CopyMap]    Script Date: 10/14/2014 10:25:12 AM ******/
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER OFF
GO




CREATE PROCEDURE [dbo].[xml_CopyMap]  @Source_System_ID int, @Target_System_ID int, 
                              @Map_ID int,@User_ID nvarchar(64)
AS
BEGIN -- {

  declare @New_Map_ID int
  declare @Error_Num  int
  declare @Error_Msg  varchar(255)
  declare @xmlout xml
  declare @Success tinyint
  declare @DB_Max_Date datetime
  declare @Map_Name nvarchar(64)
  declare @StartPos int
  declare @MapMaxName nvarchar(64)

  set @Success = 1 -- Success
  set @DB_Max_Date = dbo.fn_Get_DB_Max_Date()

  if (@Map_ID IS NOT NULL )
  begin -- {
    select @New_Map_ID = max(Map_ID) from Maps where Source_System_ID = @Source_System_ID and Target_System_ID = @Target_System_ID
    set @New_Map_ID = isnull(@New_Map_ID,0) + 1

    --Selecting the values of Map_Name for the Old Map_ID
    Select @Map_Name = Map_Name
    from Maps
    where Map_ID = @Map_ID

    select @StartPos = 0  
    select @StartPos = charindex('of ',@Map_Name,1)+ 3

    -- Select Maximum Value of OrgChart Name
    select @MapMaxName = Map_Name
    from Maps
    where  Map_ID = (select max(Map_ID)
                     from Maps
                     where Map_Name like '%' + 
                     substring( @Map_Name, @StartPos, len(@Map_Name) - @StartPos +1))

    -- Call the Proc to Create Copy of Given name  
    exec proc_CopyOfName @Map_Name output, @MapMaxName

    insert into Maps (Map_ID, Source_System_ID, Target_System_ID, Map_Name, Map_Type_ID, Map_Xml, 
                      Map_Description, Year, User_ID, Txn_Start, Txn_End)
    select @New_Map_ID, @Source_System_ID, @Target_System_ID,@Map_Name,Map_Type_ID, Map_Xml, 
              Map_Description, Year,@User_ID,getdate(),@DB_Max_Date
    from Maps
    where Source_System_ID = @Source_System_ID
    and   Target_System_ID = @Target_System_ID
    and   Map_ID           = @Map_ID
    and   Txn_End          = @DB_Max_Date

    select @Error_Num  = @@ERROR

    if @Error_Num<>0
    begin -- {
      select @Error_Msg = 'Error with xml_CopyMap - Insert Failed in Map Copy'
      set @Success = 0
    end -- }
  end -- }

  select @Success as Success, @Error_Msg as Error, 
           ( select Map_Name AS Name, Map_Description AS Description, 
                    Map_Type_ID as Type_ID, Map_ID AS Map_ID
			 from Maps
             where Source_System_ID = @Source_System_ID
             and   Target_System_ID = @Target_System_ID			  
             and   Map_ID           = @New_Map_ID
             and   Txn_End          = @DB_Max_Date
			 for xml path ('Map') , type 		     
            )
  for xml path (''), type, root('Result')

END -- }

GO
```

Operation XML

```xml
<Operation>
  <Name>DeleteMap</Name>
  <Parameters>
    <SourceSystemID>1</SourceSystemID>
    <TargetSystemID>2</TargetSystemID>
    <MapID>1</MapID>
    <UserID>123</UserID>
  </Parameters>
</Operation>
```

##### DeleteMapSet

Corresponds to DaggerSystemService.DeleteMapSet

SQL
```sql
/****** Object:  StoredProcedure [dbo].[xml_DeleteMapSet]    Script Date: 10/14/2014 10:25:53 AM ******/
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER OFF
GO



CREATE PROCEDURE [dbo].[xml_DeleteMapSet] @Source_System_ID int, @Target_System_ID int, @MapSet_ID int, @User_ID nvarchar(64)
AS
BEGIN -- {

  SET NOCOUNT ON
  SET ANSI_NULLS ON
  SET QUOTED_IDENTIFIER ON

  declare @Error_Num  int
  declare @Error_Msg  varchar(255)
  declare @xmlout xml
  declare @Success tinyint
  declare @DB_Max_Date datetime
  declare @CurDate     datetime
  declare @IsDefault   tinyint

  set @Success = 1 -- Success
  set @DB_Max_Date = dbo.fn_Get_DB_Max_Date()
  set @CurDate     = getdate()

  if (@MapSet_ID IS NULL )
  begin -- {
      select @Error_Msg = 'Error with Deleting a mapset - MapSet does not exist'
      set @Success = 0

      goto procend
  end -- }

  -- Default MapSet could not be deleted.
  select @IsDefault = IsDefault from MapSet where Source_System_ID = @Source_System_ID and Target_System_ID = @Target_System_ID  and MapSet_ID = @MapSet_ID

  if @IsDefault = 1 
  begin -- {
      select @Error_Msg = 'Error with Deleting a mapset - Default MapSet could not be deleted '
      set @Success = 0

      goto procend
  end -- }

  if (@MapSet_ID IS NOT NULL )
  begin -- {
    if exists (select 'x' from MapSet 
               where Source_System_ID = @Source_System_ID
               and   Target_System_ID = @Target_System_ID
               and   MapSet_ID        = @MapSet_ID
               and   Txn_End          = @DB_Max_Date )
    begin -- {
      Update MapSet
      set   Txn_End = @CurDate
      where Source_System_ID = @Source_System_ID
      and   Target_System_ID = @Target_System_ID
      and   MapSet_ID        = @MapSet_ID
      and   Txn_End          = @DB_Max_Date

      select @Error_Num  = @@ERROR

      if @Error_Num = 0
      begin -- {
         insert into MapSet (MapSet_ID, Source_System_ID, Target_System_ID, MapSet_Name, MapSet_Description, Year, Txn_Start, Txn_End, User_ID, IsDefault)
         select MapSet_ID, Source_System_ID, Target_System_ID, MapSet_Name, MapSet_Description, Year, @CurDate, @CurDate, @User_ID, 0
         from   MapSet
         where  Source_System_ID = @Source_System_ID
         and    Target_System_ID = @Target_System_ID
         and    MapSet_ID        = @MapSet_ID
         and    Txn_End          = @CurDate

         select @Error_Num  = @@ERROR
      end -- }

      if @Error_Num<>0
      begin -- {
        select @Error_Msg = 'Error with deleting mapset - Delete Failed in MapSet'
        set @Success = 0
        goto procend
      end -- }
    end -- }
    else 
    begin -- {
        select @Error_Msg = 'Error with Deleting a mapset - MapSet was not found'
        set @Success = 0

        goto procend
    end -- }
    
    if exists (select 'x' from MapSetMapXref
               where Source_System_ID = @Source_System_ID
               and   Target_System_ID = @Target_System_ID
               and   MapSet_ID        = @MapSet_ID
               and   Txn_End          = @DB_Max_Date )
    begin -- {
      Update MapSetMapXref
      set   Txn_End = @CurDate
      where Source_System_ID = @Source_System_ID
      and   Target_System_ID = @Target_System_ID
      and   MapSet_ID        = @MapSet_ID
      and   Txn_End          = @DB_Max_Date
     
      select @Error_Num  = @@ERROR

      if @Error_Num = 0
      begin -- {
         insert into MapSetMapXref(MapSet_ID, Source_System_ID, Target_System_ID, Map_ID, Txn_Start, Txn_End, User_ID)
         select MapSet_ID, Source_System_ID, Target_System_ID, Map_ID, @CurDate, @CurDate, @User_ID
         from   MapSetMapXref
         where Source_System_ID = @Source_System_ID
         and   Target_System_ID = @Target_System_ID
         and   MapSet_ID        = @MapSet_ID
         and   Txn_End          = @CurDate
         
         select @Error_Num  = @@ERROR
      end -- }
  
      if @Error_Num<>0
      begin -- {
        select @Error_Msg = 'Error with deleting mapset - map association was not deleted' 

        set @Success = 0
--??? Rollback transaction.
        goto procend
      end -- }
    end -- }

  end -- }

procend:
  set @xmlout = ( select @Success as Success, @Error_Msg as Error for xml path ('Result'), type )

  select    @xmlout 

END -- }

GO
```

Operation XML

```xml
<Operation>
  <Name>DeleteMapSet</Name>
  <Parameters>
    <SourceSystemID>1</SourceSystemID>
    <TargetSystemID>2</TargetSystemID>
    <MapSetID>1</MapSetID>
    <UserID>123</UserID>
  </Parameters>
</Operation>
```	

## OSW Components

The DaggerBroker gets the SystemsXml and the MapTypesXml. I don't see any references to this in our code base, so this must be owned by another team. 

How do we update this to include EntityManager?

### Stored Procedures

##### xml_GetMapSystemInfo

##### xml_GetMapTypes



