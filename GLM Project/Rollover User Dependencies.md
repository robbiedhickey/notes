### Rollover User Dependencies

The problem we are trying to solve is that the current Rollover procedures have a dependency on an OIT UserId. The GLM integration folks will be passing us the GLM user name for audit purposes, which doesn't map to an OIT userId. Jitin and I discussed defaulting to an integration user, but it doesn't really give us any valuable information. We want to be able to log what user requested a rollover from GLM's perspective. 

The way the rollover proc works, if null is passed then it will fail validation. If a value is passed, the proc will use that UserId to look up a UserName, which subsequently will be used in entries to the log_message table. If the UserId is bogus, or for some other reason does not map to an OIT user, then the proc will log an empty string.

### Proposal

I propose that we modify the logic that is defaulting the user name to an empty string. If instead we default to the passed in 'userId', we can retain who initiated the request from an audit perspective. Basically, for all lines where we are doing the following:

```sql
select @LVC_USER_NAME = isnull(@LVC_USER_NAME, '')
```

Replace with this:

```sql
select @LVC_USER_NAME = isnull(@LVC_USER_NAME, @IPVC_USER_ID)
```

This ensures that we are logging the GLM Username regardless of whether the user exists in our OIT tables. Just a note -- as it stands today, the Log_Message table has a number of entries where the UserName is null or empty. 

We could also prefix the GLM users with something like GLM_* to differentiate them from typical OIT user names. 

### spRollover_Charts Analysis

The rollover proc calls a number of children to delegate rollover work. Each reference to a user id in the subsequent procs follows one of the following conventions:

##### Inserting to Batch_List table

```sql
  select @LVC_USER_NAME = User_Name
  from   Registered_Users
  where  User_ID = @IPVC_USER_ID

  insert into Batch_List (Year,Batch_Type,Batch_Name,User_Name,Time_Stamp)
  select @IPSI_FROM_YEAR, 'Chart_Rollover','Tmp Rollover Batch ',  @LVC_USER_NAME, getdate()
```
##### Executing a validation procedure
```sql
  exec spRollover_Validation_TCC @IPVC_USER_ID,
                                         @IPSI_FROM_YEAR,
                                         @LVC_TCC_OBJ_TYPE,
                                         @IPBI_BATCH_ID,
                                         @IPVC_RSCS_FLAG,
                                         @LTI_VALIDATION_OPTION,
                                         @IPTI_DEBUG_FLAG
```

##### Select username from user_id and insert to log_message (usually within a validation procedure)

```sql
    if substring(@IPVC_USER_ID,1,4) = 'TTAX'
       select @LVC_USER_NAME = @IPVC_USER_ID
    else
      select @LVC_USER_NAME = User_Name
      from   Registered_Users a
      where  User_ID = @IPVC_USER_ID
  end

  select @LVC_USER_NAME = isnull(@LVC_USER_NAME, '')
```

Also note that in this example, we are defaulting to the UserId if it is a TTAX user, so this change is not without precedent. 

##### List of procs that are affected (bold indicates a change is required)

* **spBuildBatchList**
	* selects username and inserts to batch_list
* spRollover_TCC
	* calls spRollover_Validation_TCC
* **spRollover_Validation_TCC**
	* Selects username and inserts to log_message
* spRollover_FCOA
	* Calls validation spRollover_Validation_FCOA
* **spRollover_Validation_FCOA**
	* Selects username and inserts to log_message
* spRollover_FADJ
	* calls validation spRollover_Validation_Adj
* **spRollover_Validation_Adj**
	* Selects username and inserts to log_message
* spRollover_ICOA
	* debug messages
* spRollover_FedIntlXref
	* calls validation spRollover_Validation_FedIntlXref
* **spRollover_Validation_FedIntlXref**
	* Selects username and inserts to log_message
* spRollover_DASTMCOA
	* calls validation spRollover_Validation_DASTM
* **spRollover_Validation_DASTM**
	* Selects username and inserts to log_message
* spRollover_SchMXref
	* calls validation spRollover_Validation_SchMXref
* **spRollover_Validation_SchMXref**
	* Selects username and inserts to log_message
* spRollover_XRates
	* calls validation spRollover_Validation_XRates
* **spRollover_Validation_XRates**
	* Selects username and inserts to log_message
* spRollover_BatchPTRC
	* debug message
* spRollover_CTDG
	* debug message
* spRollover_CTDGroupMapping
	* calls validation spRollover_Validation_CTDGroupMapping
* **spRollover_Validation_CTDGroupMapping**
	* Selects username and inserts to log_message
* spRollover_SchM3Codes
	* debug message
* spRollover_SchM3Mapping
	* calls validation spRollover_Validation_SchM3Mapping
* **spRollover_Validation_SchM3Mapping**
	* Selects username and inserts to log_message
* spRollover_BatchStAALogicCharts
	* debug message
* spRollover_StateAAFormXRef
	* debug message
* spRollover_StateTIFormMap
	* debug message
* spRollover_PartAATransChart
	* debug message
* spRollover_PartK1TransChart	
	* calls validation spRollover_Validation_PartK1TransChart
* **spRollover_Validation_PartK1TransChart**
	* Selects username and inserts to log_message
* spRollover_ForeignPartK1TransChart
	* calls validation spRollover_Validation_ForeignPartK1TransChart
* **spRollover_Validation_ForeignPartK1TransChart**
	* Selects username and inserts to log_message
* **spRollover_SubFInclusionXrefChart**
	* Selects username and inserts to log_message
* **spRollover_Charts_AAR**
	* directly inserts user id to log_message
	* This is interesting and answers some of my questions, it is the first example of something other than a UserName being stored in this table. This supports what we want to try.
* spRollover_NAICMapping
	* calls validation spRollover_Validation_NAICMapping
* **spRollover_Validation_NAICMapping**
	* Selects username and inserts to log_message
* **spRollover_Validation_ICOA**
	* Selects username and inserts to log_message
* **spRollover_Validation_SchM3Codes**
	* Selects username and inserts to log_message

