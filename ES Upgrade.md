## ES 4.5 Upgrade

Due to OITCore being updated to 4.5 (why?) the ESCalculationEngine and related assemblies have been updated to 4.5 as well. This breaks the Estimated Payments solution. 

### ES Projects Depending on ESCalculationEngine

* ESDataAnalysis.Web
* ESDataEntry.Web
* ESDataOverrides.Web
* ESDataReview.Web
* ESFederalDataSources.Web
* ESFederalTaxAdjustments.Web
* ESFilingGroups.Web
* ESImport.Web
* ESJursidictions.Web
* ESMiscDataItems.Web
* ESStateModsAdjustments.Web
* ESTaxLaw.Web
* ESImporter

### Problems after upgrading dependent projects to 4.5

* ESDataOverrides build fails in DomainService
	* Resolved by reverting to v2 edmx schema
* ESDataReview build fails in DomainService
	* Resolved by reverting to v2 edmx schema
* ESFilingGroups build fails in DomainService
	* Resolved by reverting to v2 edmx schema
* ESJurisdictions build fails in DomainService
	* Resolved by reverting to v2 edmx schema
* ESStateModsAdjustments build fails in DomainService
	* Resolved by reverting to v2 edmx schema
* ESTaxLaw build fails in DomainService
	* Resolved by reverting to v2 edmx schema

They all for the most part seem to fail with some variant of:

> Parameter 'xxx' of domain method 'xxx' must be an entity type exposed by the DomainService. The entity type can be exposed either directly in a query operation or indirectly through an association.

It appears that VS tries to update all EDMX files to version 3 of the schema. This may be the source of the issue?





