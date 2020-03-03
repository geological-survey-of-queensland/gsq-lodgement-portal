# GSQ Lodgement Portal

## Purpose of this document

Provide a specification for the new GSQ Lodgement Portal that:  
* Accurately defines the business requirements
* Is a key input to software design for a software developer  

The primary input into this document is the data model defined in the [Resources Industry Report Profile](https://github.com/geological-survey-of-queensland/industry-report-profile).

## Background

Resource authority holders need to submit a range of statutory reports on their activities. Reports include all mineral exploration, geothermal exploration, greenhouse gas exploration, petroleum exploration, petroleum well, seismic survey or airborne geophysical survey, mineral development licence, petroleum lease, and petroleum pipeline licence reports and some other exploration related reporting.

When government issues a permit to a holder, the legislation requires a permit holder to submit reports to the Department when certain activities are performed under the conditions of the permit.  
* The reports contain knowledge of the geological resource. 
* Historically, industry reports have been lodged through [QDEX Reports](https://qdexguest.dnrm.qld.gov.au/portal/site/qdex/search).  
* GDMP will deliver a new lodgement portal to replace QDEX Reports.  
* Most industry reports have a confidentiality period before becoming open file.  

## The Lodgement Portal

### Objectives of the Lodgement Portal

1. Provide a single portal for industry and GSQ to lodge georesources reports.
2. Provide data validation at point of data entry.
3. Provide a database store to store in-progress and submitted reports.
4. Harvest the submitted metadata, data and datasets for insertion into CKAN, S3 and the [GeoProperties Database](https://github.com/geological-survey-of-queensland/ssor-database).
5. Provide search functionality for GSQ staff to search for reports.
6. Replace QDEX Reports.

### Vendor deliverables

1. An online lodgement portal to allow industry users to submit reports and upload associated geoscience data files in a structured, itemised format.  
    a.Data must be able to be submitted through a human-computer interface  
    b.Data must be able to be submitted through a computer-computer interface  
2. Automated workflows to process the geoscience data submitted to the geological properties database, and data catalogue and data object store.
3. A method of enabling the upload of large geoscience data files to the data object store.

## Four versions of the Lodgement Portal

There are 4 versions of lodgement portal to be built:

1. MVP Lodgement Portal to replace the current lodgement functionality of QDEX reports - described on this page.
2. Version 2 Lodgement Portal that parses the data lodged in the petroleum, coal, and mineral reporting guideline templates - described on [Version2-portal-README.MD](/Version2-portal-README.MD)
3. Version 3 Lodgement Portal that does some cool stuff - described on [Version3-portal-README.MD](/Version3-portal-README.MD)
4. Version 4 Lodgement Portal that does some additional cool stuff - described on [Version4-portal-README.MD](/Version4-portal-README.MD)

## Iterative delivery of the versions

It is vital that GSQ exit QDEX Reports by June 30, 2020. Hence, we will iterate through the 4 versions:

1. Analyse, design, build, and deploy Version 1 - the MVP Lodgement Portal. This portal replaces QDEX Reports.  
  a. The MVP software (Version 1) provides the platform and "plumbing" for all future versions - i.e. web portal, authentication, authorisation, lodgement form, lodgement database, metadata, CKAN integration, S3 integration, spatial processing.  
  b. The MVP software enables data migration out of QDEX Reports into the Lodgement Portal.  
  c. The MVP software enables data migration out of QDEX Reports into the Data Catalogue (Open Data Portal, Private Data Portal, and Data Object Store (S3)).  
2. Analyse, design, build, and deploy Version 2 - inheriting the functionality of the MVP and building the parsing functionality to populate the Geoproperties database.
3. Analyse, design, build, and deploy Versions 3 and 4.

## MVP Lodgement Portal activity diagram

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/MVP-report-lodgement-activity-diagram.png" width="70%"><br>
Figure 1: MVP Lodgement Portal activity diagram</p>

## MVP Lodgement Portal conceptual data model

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/lodgement-portal-conceptual-design.png" width="100%"><br>
Figure 4: Lodgement Portal conceptual data model</p>

## MVP Lodgement Portal data elements

|Element|Field name|Remarks|Source|
|---|---|---|---|
|report_id|Report ID|A unique, persistent identifer|System|
|report_alias|Report alias|An alternative identifier for the report. Digital Object Identifier will be recorded here.|System|
|report_title|Report title|A textual name|Text|
|report_description|Report description|A textual description of the report|Text|
|report_type|Report type|Lookup to controlled list of report types|Vocab|
|report_status|Report status|Lookup to controlled list of status|Vocab|
|report_permit|Permit|The permit number(s) covered by the report|Lookup|
|report_is_of_feature|Geoadmin feature|The features targeted in the report|Lookup|
|report_is_of_site|Site|The sites targeted in the report|Lookup|
|report_is_of_suvey|Generated by|The PID of the survey or other activity that generated the report|Lookup|
|report_data_category|Earth science data category|The data categories featured in the report|Vocab|
|report_commodity|Commodity|The target or actual commodities featured in the report|Vocab|
|report_owner|Report owner|Party that owns the resource<br>A lookup to controlled list of organisations|Lookup|
|report_author|Author|Party who authored the resource|Text|
|report_submitter|Submitter|The logged-in user who lodged the report|System|
|report_start_time|Report period start date|The temporal coverage of the report|Date|
|report_end_time|Report period end date|The temporal coverage of the report|Date|
|report_lodge_time|Created|Date and time the report was lodged (date of receipt)|System Date|
|report_open_file_date|Issued|Date of formal issuance (open file publication). Calculated for new reports.|Date|
|report_access_rights|Data access rights|Controls user and system access to the resource|Vocab|
|report_geometry|Geometry|Spatial coverage of the report. If no spatial data is submitted with the report and the report is for a permit(s), the spatial coverage is created based on the permit extents boundary.|Geometry|
|report_details|Report details|Report-specific additional information|Key:Values|
|report_dataset|Dataset link|Links to related datasets including raw data|Hyperlink|

### Report_alias data elements (sub-table)

|Element|Field name|Remarks|Source|
|---|---|---|---|
|report_alias|Report alias|An alternative identifier for the report|User|
|report_alias_source|Alias source|The source of the alternative identifier, e.g. Digital Object Identifier|User|
|report_alias_detail|Alias detail|Details of the alias in textual form|User|

### Report_status data elements (sub-table)

|Element|Field name|Remarks|Source|
|---|---|---|---|
|report_status|Report status|Lookup to controlled list of status. In the MVP, this will default to "Submitted".|Lookup|
|status_start_date|(hidden)|The date the status was set|System|
|status_end_date|(hidden)|The date the previous status ended|System|

### Geometry data elements (sub-table)

The spatial coverage of the report. If no spatial data is submitted with the report and the report is for a permit(s), the spatial coverage is created based on the permit extents boundary. The geometry is stored in the PostGIS component of the database.

|Element|Field name|Remarks|Source|
|---|---|---|---|
|geometry_id|Geometry ID|The identifier of the geometry to allow retrieval of the geometrty.|WKT|
|geometry_format|Geometry format|The format of the geometry: a point, a line, a polygon, etc.|WKT|

### Report_details data elements (sub-table)

Enables the collection of various data, held as key:value pairs, i.e. report_detail_type:report_detail_value. e.g. tectonic:Kalkadoon

|Element|Field name|Remarks|Source|
|---|---|---|---|
|report_detail_type|Report detail type|Report-specific additional information type|Vocab|
|report_detail_value|Report detail value|Report-specific additional information in textual or numeric form|User|

### Dataset_resource data elements (sub-table)

The datasets submitted with the report, e.g. PDF files, wireline logs, CSV files, are stored as objects in S3. Each resource is described with DCAT2-compliant metadata. This table lists all of the dataset resources linked to a report.

|Element|Field name|Remarks|Source|
|---|---|---|---|
|dct:identifier|PID|Persistent identifier for the resource|System|
|dct:title|Title|A name given to the item|User|
|dct:description|Description|A free-text account of the item.|User|
|dct:type|Resource type|The nature of the resource, e.g. LAS file|Vocab|
|dct:format|Format|The file format, physical medium, or dimensions of the resource.|User|
|dct:byteSize|Byte size|The size of the resource in bytes|System|
|dct:dateSubmitted|Date submitted|Date of submission of the resource|System|

## Lodgement Form PID minting

The Persistent Identifier (PID) is the unique identifier for a site or survey that is submitted to the department. GSQ needs industry to use this PID when submitting data to the department.

### Minting a new PID

Customers lodge _Notice of Intention_ forms to the department. This means that the borehole or the survey is proposed but not completed.

We need to tell the submitter of the form what the PID is and what it is used for. We can do this by displaying the PID in the "Lodgement Successful" screen. We want to do after the form submission so we don't get orphan PIDs, i.e. PIDs created without the accompanying lodgement metadata.

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/pid-request-sequence-diagram.png" width="100%"><br>
Figure 2: Lodgement Portal PID minting</p>

### Matching an existing PID

When the submitter submits a _Notice of completion (or abandonment, etc.)_ form, we need to tie this lodgement to the _proposed_ borehole or survey record in the GSQ database.

For instance:

* A customer earlier submitted a [PA-21A - Notice of intention to carry out seismic survey or scientific or technical survey](https://www.dnrme.qld.gov.au/__data/assets/pdf_file/0004/259897/pa-21a-notice-intention-survey.pdf)
* The system gave them the PID of the Survey, e.g. SS36789
* Now the customer lodges a [PA-22A - Notice of completion of seismic survey or scientific or technical survey](https://www.dnrme.qld.gov.au/__data/assets/pdf_file/0005/259898/pa-22a-notice-completion-survey.pdf)
* The customer enters the PID of the Survey SS36789 so we can tie the _completion notice_ to the _intention notice_, i.e. tie to the completed survey to the proposed survey.
* The _status_ of the Survey can now be changed to _completed_.
* When the customer later lodges a _Seismic Survey Report - Final_, again they enter the PID so that the report is tied to Survey SS36789.

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/pid-match-sequence-diagram.png" width="100%"><br>
Figure 3: Lodgement Portal PID matching</p>

## Report Types Covered by Petroleum & Gas Reporting Template

The following reports will be submitted through the [Petroleum and Gas Reporting Template](https://www.dnrme.qld.gov.au/mining-resources/initiatives/pandg-reporting-guideline-2018) (.xls).

* They will be lodged through the Lodgement Portal with the submitter completing the standard report metdata in the lodgement form.
* The submitter will upload the **.xls reporting template** through the lodgement form.
* The submitter will upload any additional data files through the lodgement form.
* When submitted, the metadata is written to the Lodgement Portal database. The **.xls reporting template** and any additional files are stored in S3.
* The .xls file is harvested with the data inserted into the Geoproperties database.
* The report metadata is pushed through to CKAN as a **Report dataset** with links to the data objects in S3.

|Report Type|Concept|Notation|QDEX Count|PID|
|---|---|---|---|---|
|Hydraulic Fracturing Activity Report|hydraulic-fracturing-activity-report |HFACR|1291|Match|
|Petroleum Report - Production Information|petroleum-report-production-information |PROINF|0|N/A|
|Petroleum Report - Resource and Reserves Information|petroleum-report-resource-and-reserves-information |RESINF|0|N/A|
|Scientific or Technical Survey Report|scientific-or-technical-survey-report |STSURV|86|N/A|
|Seismic Survey Report - Final|seismic-survey-report-final |SSFINL|820|Match|
|Seismic Survey Report - Other|seismic-survey-report-other |SSOTHR|415|Match|
|Seismic Survey Report - Reprocessing|seismic-survey-report-reprocessing |SSREPR|98|Match|
|Well Completion Report|well-completion-report |WELCOM|14794|Match|
|Well Production Testing Report|well-production-testing-report |WELTST|1927|Match|
|Well or Bore Abandonment Report|well-or-bore-abandonment-report |WELAB|1141|Match|

## Report Types Covered by Mineral & Coal Reporting Template

The following reports will be submitted through the [Mineral Reporting Template](https://www.dnrme.qld.gov.au/mining-resources/initiatives/mineral-coal-reporting-guideline) _or_ [Coal Reporting Template](https://www.dnrme.qld.gov.au/mining-resources/initiatives/mineral-coal-reporting-guideline) (.xls files).

* They will be lodged through the Lodgement Portal with the submitter completing the standard report metdata in the lodgement form.
* The submitter will upload the **.xls reporting template** through the lodgement form.
* The submitter will upload any additional data files through the lodgement form.
* When submitted, the metadata is written to the Lodgement Portal database. The **.xls reporting template** and any additional files are stored in S3.
* The .xls file is harvested with the data inserted into the Geoproperties database.
* The report metadata is pushed through to CKAN as a **Report dataset** with links to the data objects in S3.
* No PID minting or maching is required.

|Report Type|Concept|Notation|QDEX Count|
|---|---|---|---|
|Permit Report - Annual|permit-report-annual |ANNUAL|37235|
|Permit Report - Final|permit-report-final |FINAL|10240|
|Permit Report - Partial Relinquishment|permit-report-partial-relinquishment |RELINQ|8860|
|Permit Report - Surrender|permit-report-surrender |SURR|3|

## Report Types Not Covered by Reporting Guidelines

* They will be lodged through the Lodgement Portal with the submitter completing the standard report metdata in the lodgement form.
* The submitter will upload any additional data files through the lodgement form.
* When submitted, the metadata is written to the Lodgement Portal database. Any additional files are stored in S3.
* There is no data automatically harvested into the Geoproperties database. This will be a manual process by GSQ staff.
* The report metadata is pushed through to CKAN as a **Report dataset** with links to the data objects in S3.
* No PID minting or maching is required.

|Report Type|Concept|Notation|QDEX Count|
|---|---|---|---|
|Geophysical Survey Report - Acquisition|geophysical-survey-report-acquisition|-|0|
|Geophysical Survey Report - Final|geophysical-survey-report-final|GEOPSR|10|
|Geophysical Survey Report - Logistics|geophysical-survey-report-logistics|-|0|
|Geothermal Report - Annual Reserves|geothermal-report-annual-reserves |GTHARR|1|
|Geothermal Report - Injection|geothermal-report-injection |GTHIR|0|
|Geothermal Report - Injection Testing|geothermal-report-injection-testing |GTHITR|0|
|Geothermal Report - Production|geothermal-report-production |GTHPR|0|
|Geothermal Report - Production Testing|geothermal-report-production-testing |GTHPT|0|
|Greenhouse Gas Report - Injection|greenhouse-gas-report-injection |GHGIR|0|
|Greenhouse Gas Report - Storage Capacity|greenhouse-gas-report-storage-injection |GHGSIR|0|
|Greenhouse Gas Report - Storage Injection|greenhouse-gas-report-storage-capacity|GHGSSC|16|
|Mine Plan Lodgement|mine-plan-lodgement|MPLODG|66|
|Permit Report - Final Relinquishment|permit-report-final-relinquishment|FINREQ|634|
|Permit Report - Mineral Associated Water|permit-report-mineral-associated-water|MINAW|1047|
|Petroleum Report - Cumulative Water Production|petroleum-report-cumulative-water-production|CUMPRD|98|
|Petroleum Report - Field Information|petroleum-report-field-information|PETFLD|116|
|Petroleum Report - Infrastructure|petroleum-report-infrastructure|PETIR|505|
|Petroleum Report - Non-Associated Water|petroleum-report-non-associated-water|PETNAW|17|
|Water Report - Performance Review|water-report-performance-review|WATPRR|49|

## Report Types lodged as PDF forms

The following report types are PDF forms.

* These will be lodged as PDF forms through the lodgement portal.
* The submitter will complete the lodgement form metadata.
* PID minting or PID matching will be performed as defined.

|Report Type|Mint or Match|
|---|---|
|[PA-21A](https://www.dnrme.qld.gov.au/__data/assets/pdf_file/0004/259897/pa-21a-notice-intention-survey.pdf) Notice of Intention to carry out seismic survey or scientific or technical survey|Mint survey|
|[PA-22A](https://www.dnrme.qld.gov.au/__data/assets/pdf_file/0005/259898/pa-22a-notice-completion-survey.pdf) Notice of Completion of seismic survey or scientific or technical survey|Match survey|
|[PM 1/2013](https://www.dnrme.qld.gov.au/__data/assets/pdf_file/0003/289605/notification-geophysical-survey.pdf) Notification of geophysical survey|Mint survey|
|[PA-42](https://www.dnrme.qld.gov.au/__data/assets/pdf_file/0008/259901/pa-42-notice-of-intention.pdf) Notice of intention to convert a petroleum well to a water observation bore or water supply bore|N/A|
|[WRA-05A](https://www.dnrme.qld.gov.au/__data/assets/pdf_file/0006/259935/wra-05a-notification-completion-conversion.pdf) Notice of completion of conversion of petroleum well to water supply bore or water observation bore|N/A|
|[MMOL-44](https://www.dnrme.qld.gov.au/__data/assets/pdf_file/0003/289605/notification-geophysical-survey.pdf) Notice of decommissioning a well, water observation bore, water monitoring bore or water supply bore|N/A|

## Report Types that are deprecated - no longer lodged to DNRME

The following report types are no longer current.

* However, the report metadata for these reports is to be migrated from QDEX Reports into the Lodgement Portal.
* Any reports that have not already been published in the Open Data Portal need to be published into the relevant Open Data Portal or Private Data Portal based on the QDEX Reports confidentiality flag. Metadata into CKAN, data objects into S3.

|Report Type|Concept|Notation|QDEX Count|
|---|---|---|---|
|Permit Report - Other|permit-report-other|-|3649|
|Permit Report - Six Month|permit-report-six-month|6MTH|7183|
|Petroleum Report - Other|petroleum-report-other |EPPOTH|784|
|Petroleum Report - Transmission|petroleum-report-transmission |TRANS|233|
|Water Report - Other|water-report-other |WATOTH|131|
|Well Report Other|well-report-other |WELOTH |555|
|Well proposal|well-proposal |WELPRO|4358|

## Report Types that will not be lodged through lodgement portal

The following report types will be submitted directly to the department, not lodged through the lodgement portal.

* However, the report metadata for these reports is to be migrated from QDEX Reports into the Lodgement Portal.
* Any reports that have not already been published in the Open Data Portal need to be published into the relevant Open Data Portal or Private Data Portal based on the QDEX Reports confidentiality flag. Metadata into CKAN, data objects into S3.

|Report Type|Concept|Notation|QDEX Count|
|---|---|---|---|
|Collaborative Drilling Initiative - Final|collaborative-drilling-initiative-final|CEIFIN|14|
|Collaborative Drilling Initiative - Proposals|collaborative-drilling-initiative-proposals|CEIPRO|47|
|Collaborative Exploration Initiative - Final|collaborative-exploration-initiative-final|CDIFIN|37|
|Industry Consultative Report|industry-consultative-report|OTHER|31|
|Industry Network Initiative - Final|industry-network-initiative-final|INIFIN|2|
|Production (Petroleum)|production-petroleum|PROD|610|
|Reserves (Petroleum)|reserves-petroleum|RESERV|741|

## Lodgement Portal vocabularies

The vocabularies used in this profile are:

1. [Georesources Report Type](https://vocabs.gsq.digital/vocabulary/georesource-report)
2. [Earth Science Data Category](https://vocabs.gsq.digital/vocabulary/earth-science-data-category) - the category(s) of data contained in the report
3. [Data Access Rights](https://vocabs.gsq.digital/vocabulary/data-access-rights)
4. [Commodity](https://vocabs.gsq.digital/vocabulary/gsq-commodity)
5. Report detail type.
6. Report status.

## Lodgement Portal authentication and authorisation

The Lodgement Portal requires authentication using the Identity Broker:

* External users authenticate using QGCIDM (QGOV)
* Internal users authenticate using ADFS

The Lodgement Portal requires user authorisation:

* External users have rights to submit lodgement forms
* Internal users have rights to submit lodgement forms or view lodged forms

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/lodgement-portal-authentication-authorisation.png" width="60%"><br>
Figure 5: Lodgement Portal external user authentication and authorisation</p>

## Alignment to QDEX Reports Metadata
|QDEX Reports Field|Lodgement Portal Metadata Field|
|----|----|
| Report Number|Report PID|
| Report Title|Report Title|
| Report Status|Report Access Rights - vocab lookup|
| Report Type|Report Type - Georesource Report Type vocab lookup|
| Author Name|Report Author|
| Lodger|Report Submitter (the logged in user)|
| Submitter|Report Owner - party database lookup|
| Locality|Goes into JSON|
| Map References|Goes into JSON|
| Commodity|Report commodity - Commodity vocab lookup|
| Keywords|Report Data Category - Earth science data category vocab lookup (where they match)|
| Tenure|Report Permit - permit service lookup|
| Tenure Holder|Goes in JSON - can infer from permit lookup|
| Tectonic|Report is of feature - vocab lookup|
| Stratigraphy| Report is of feature - vocab lookup|
| Age|Goes into JSON|
| Date of Report|Report Start Time + Report End Time|
| Date of Review|Goes into JSON - not used anymore|
| Date of Receipt|Report Lodged Date|
| Date Due for Open|Report Open File Date|
| Date of Open|Move into JSON|
| Project Names|Report Details|
| Mines/Prospect Names|Report is of Site|
| Well Names|Report is of Site|
| Seismic Survey Names|Report is of Survey|

## Data Migration out of QDEX Reports

The following data is to be migrated from QDEX Reports. Historically, QDEX Reports has been used to publish other types of publications.

* The new lodgement portal will focus on reports lodged by industry (Exploration Reports and Industry Consultative Reports).
* Other types of publications will be published through the Open Data Portal.

|Name|Description|QDEX Count|Migrate metadata to|Migrate data objects to|
|---|---|---|---|---|
|QDEX - Exploration Reports|The result of mandatory reporting requirements to the government by mineral, coal and petroleum explorers in Queensland. The collection commenced with the introduction of the exploration permitting system in Queensland in the 1950's and continues to the present day with several hundred reports added annually.|97065|Lodgement Portal + CKAN|Confidential to Private S3/<br>Open to Open S3|
|Industry Consultative Reports|Reports created by external parties and of geological significance that are submitted to DNRM and associated with the exploration industry, but not tied to tenure or legislation|31|Lodgement Portal + CKAN|Confidential to Private S3/<br>Open to Open S3|
|Queensland Geological Maps|A collection of current Geological Maps published by the Geological Survey of Queensland. The collection also includes Geology Compilation Plots compiled from recent project work.|419|Open Data Portal|Open Data S3|
|GSQ Record Series|Publications produced as part of the record series by the Geological Survey of Queensland.|1299|Open Data Portal CKAN|Open Data S3|
|Soils and Land Resources Reports|Information on Queensland soils, acid sulfate soils, land systems, agricultural land suitability, agricultural land capability, available in land resources reports and maps and land management manuals.|390|No migration - take archive snapshot|No migration - take archive snapshot|
|Exploration Reports|Exploration Reports specifically related to drilling and non-drilling carried out by recipients of Queensland Government exploration grants, including the Collaborative Exploration Initiative grants under the Strategic Resources Exploration Program.|119|Open Data Portal CKAN|Open Data S3|
|Departmental Publications|Departmental Publications including Queensland Government Mining Journal (QGMJ).|2075|Open Data Portal CKAN|Open Data S3|
|GSQ-Commissioned Industry Studies/Reports|Reports on Studies undertaken by Industry, but commissioned by the Geological Survey of Queensland.|15|Open Data Portal CKAN|Open Data S3|

## See also

* [Industry report profile](https://github.com/geological-survey-of-queensland/industry-report-profile)

## License

This code repository's content are licensed under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/), the deed of which is stored in this repository here: [LICENSE](LICENSE).

## Contacts

*System owner*:  
**Mark Gordon**  
Geological Survey of Quensland  
Department of Natural Resources, Mines and Energy  
Brisbane, QLD, Australia  
<mark.gordon@dnrme.qld.gov.au>  

*Author*:  
**David Crosswell**  
Enterprise Architect  
Cross-Lateral Enterprises
<https://crosslateral.com.au>
