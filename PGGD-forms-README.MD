# PGGD Forms

## Background

In 2009-10, eight Java forms were created under the MinesOnline project. This project was a precursor to the MyMinesOnline project.

The forms were created as Java forms reading and writing to the MERLIN and ONLINEAPP schemas in the Mines Oracle database.

A number of the forms were decommissioned, leaving four active forms:

* PGGD-01 - Notice of intention to drill a petroleum well or bore
  * Creates a new borehole record in MERLIN.  
  * Records a borehole activity in MERLIN.
  * Generates a PDF for the submitter to provide to the land owner  
* PGGD-02 - Notice of completion, alteration or abandonment of petroleum well or bore
  * Updates the status of the borehole record in MERLIN.  
  * Records a borehole activity in MERLIN.  
* PGGD-03 - Notice of intention to carry out hydraulic fracturing activities  
  * Records a borehole activity in MERLIN.  
* PGGD-04 - Notice of completion of hydraulic fracturing activities
  * Records a borehole activity in MERLIN.
  * Captures engineering and operations data about the hydraulic fracturing in MERLIN.  
  * A copy of the generated form is sent to DES.  
  * This data is referred to if someone queries fracturing operations, e.g. chemicals used.  

## Current business process

## Future state business process

### PGGD-01 Sequence Diagram

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/Sequence-Diagram-PGGD-01.png" width="85%"><br>
Figure x: PGGD-01 sequence diagram</p>

### PGGD-02 Sequence Diagram

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/Sequence-Diagram-PGGD-02.png" width="70%"><br>
Figure x: PGGD-02 sequence diagram</p>

### PGGD-03 Sequence Diagram

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/Sequence-Diagram-PGGD-03.png" width="85%"><br>
Figure x: PGGD-03 sequence diagram</p>

### PGGD-04 Sequence Diagram

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/Sequence-Diagram-PGGD-04.png" width="70%"><br>
Figure x: PGGD-04 sequence diagram</p>

## Technology

### Technology as-is

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD-tech-as-is.png" width="40%"><br>
Figure x: PGGD technology as-is</p>

### Technology to-be

## Forms as-is

### PGGD-01 as-is

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD01_form.png" width="90%"><br>
Figure x: PGGD-01 Form</p>

### PGGD-02 as-is

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD02_form.png" width="95%"><br>
Figure x: PGGD-02 Form</p>

### PGGD-03 as-is

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD03_form.png" width="100%"><br>
Figure x: PGGD-03 Form</p>

### PGGD-04 as-is

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD04_form.png" width="100%"><br>
Figure x: PGGD-04 Form</p>

### PGGD-01 Notice as-is

This is the document that is generated as PDF and emailed to the submitter:  
[PGGD-01 Notice of intention to drill a well or bore](https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD01-notice.pdf)

### PGGD-02 Notice as-is

This is the document that is generated as PDF and emailed to the submitter:  
[PGGD-01 Notice of intention to drill a well or bore](https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD01-notice.pdf)

### PGGD-03 Notice as-is

This is the document that is generated as PDF and emailed to the submitter:  
[PGGD-03 Notice of intention to carry out hydraulic fracturing activities](https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD03-notice.pdf)

### PGGD-04 Notice as-is

This is the document that is generated as PDF and emailed to the submitter:  
[PGGD-01 Notice of intention to drill a well or bore](https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD01-notice.pdf)

## PGGD Forms to-be

### PGGD-01 to be
<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/new-PGGD01-form.PNG" width="70%"><br>
Figure x: New PGGD-01 Form</p>

### PGGD-02 to be
<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/new-PGGD02-form.png" width="80%"><br>
Figure x: New PGGD-02 Form</p>

### PGGD-03 to be
<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/new-PGGD03-form.png" width="80%"><br>
Figure x: New PGGD-03 Form</p>

### PGGD-04 to be
<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/new-PGGD04-form.png" width="80%"><br>
Figure x: PGGD-04 Form</p>

### Database schema as-is

The PGGD forms _write_ data to the following tables:

* QDEX ONLINEAPP.DOCUMENT:
* MERLIN BHF_BOREHOLES:
  * PGGD-01 creates a new borehole record in this table  
  * PGGD-02 updates the borehole record in this table
* EPF_COMPANY_REPORTS
* MERLIN BHF_BOREHOLE_ACTIVITY: Records the FRAC activity type only  
  * PGGD-03 writes INTENTION against the BORE_ID with a START_DATE ad END_DATE
  * PGGD-04 writes the STATUS of the activity with a START_DATE ad END_DATE:
    * C - Completion
    * AL - Alteration
    * O - Other
    * PC - Partial completion
    * AB - Abandoned
* MERLIN BHF_MATERIALS_USED - 

The PGGD-03 and PGGD-04 forms _read_ from the following tables:

* MERLIN BHF_MATERIALS_USED - this populates the fluids and chemicals drop-down field values  

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD-db-schema-as-is.png" width="80%"><br>
Figure x: PGGD database schema</p>

### Database schema to-be
