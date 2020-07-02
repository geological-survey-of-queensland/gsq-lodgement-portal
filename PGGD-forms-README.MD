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
  * Updates the borehole created in PGGD-01 in MERLIN.  
* PGGD-03 - Notice of intention to carry out hydraulic fracturing activities  
  * Updates the borehole created in PGGD-01.  
  * Records a borehole activity in MERLIN.  
  * Captures engineering and operations data about the hydraulic fracturing in MERLIN.  
  * This data is referred to if someone queries fracturing operations, e.g. chemicals used.  
  * A copy of the generated form is sent to DES.  
* PGGD-04 - Notice of completion of hydraulic fracturing activities
  * Records a borehole activity in MERLIN.
  * Captures engineering and operations data about the hydraulic fracturing in MERLIN.  
  * A copy of the generated form is sent to DES.  

## Current business process

## Future state business process

The future state business processes use the GDMP Lodgement Portal and Geological Properties database for form submission, creation of boreholes, update of boreholes, and recording borehole events.

### PGGD-01 Sequence Diagram

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/Sequence-Diagram-PGGD-01.png" width="85%"><br>
Figure 1: PGGD-01 sequence diagram</p>

#### To-be PGGD-01 business process

1. The operator will use the **Notice of intention to drill a well or bore** Word template (or their own template) to enter the details currently entered into the existing PGGD-01 form.  
    a. This notice is given to the landholder by the operator.  
    b. The operator will provide a copy of the completed template to the department.  
1. The operator completes the new Lodgement Portal PGGD-01 online form and attaches the **Notice of intention**.
1. On submission, a new borehole is created in the Geological Properties database using the data submitted in the PGGD-01 form. The status is set to **Proposed**.
1. An email is sent to the submitter acknowledging the submission. This email contains the **Borehole PID** and instructions for how to use this PID as the unique identifier for the later form submissions.
1. The following business rules are applied:  
    a. The Well Name must be unique, i.e. not in Geoproperties DB.  
    b. The Estimated Commencement Date must be 10 business days after today.  

### PGGD-02 Sequence Diagram

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/Sequence-Diagram-PGGD-02.png" width="85%"><br>
Figure 2: PGGD-02 sequence diagram</p>

#### To-be PGGD-02 business process

1. The operator will use the **Notice of completion, alteration or abandonment of a well or bore** Word template (or their own template) to enter the details currently entered into the existing PGGD-02 form.  
    a. This notice is given to the landholder by the operator.  
    b. The operator will provide a copy of the completed template to the department.  
1. The operator completes the new Lodgement Portal PGGD-02 online form and attaches the **Notice of completion**.
1. . A new ‘Status’ and ‘Status Event’ with a ‘Start Date’ and ‘End Date’ will be created in the ‘Status History’ table.
    a. If Notice type ( selected on the form) = Completion or Cased and Suspended then the form displays Drill start date and Rig release date; if Notice type = Alteration Abandonment, Water Observation Borehole, Water Supply Borehole and Producing Hydrocarbons then the form displays Activity start date and Activity end date.
    b. If Notice type = Never Drilled then do not display any dates
    c. If Notice type = Not Drilled then create new Status = Never Used and Status Event = Never Drilled and Status Start Date = Date of lodgement of form.
    d. If Notice type = Cased and Suspended then create new Status = Cased and Suspended and Status Event = Cased & Suspended and Status Start Date = Rig Release Date.
    e. If Notice type = Water Observation Borehole then create new Status = Monitoring and Status Event = Conversion to Water Observation Borehole and Status Start Date = Activity End Date
    f. If Notice type = Water Supply Borehole then create new Status = Water Supply and Status Event = Converted to Water Supply Borehole and Status Start Date = Activity End Date
    g. If Notice Type = Producing Hydrocarbons then create new Status = On Production and Status Event = Producing Hydrocarbons.  Status Start Date = Activity Start Date and Status End Date = Activity End Date.
    h. Drill start date links to Drill Start Date in Boreholes
    i. Rig release date links to Rig Release Date in Boreholes
    j. Activity start date links to Status Start Date in Boreholes as below
    k. Activity end date links to Status End Date in Boreholes as below
    l. If Notice type = Completion, then new Status must be created with Status = Completed, and Status Event = Completion.  Status Start Date is Rig Release date
    m. If Notice type = Alteration, date fields are Activity start date and Activity end date
    n. If Notice type = Alteration, then need to create a new Status = Previous Status, Status Event = Alteration and Activity start date links to Status Start Date and Activity end date links to Status End Date.
    o. If Notice type = Abandonment, date fields are Activity start date and Activity end data
    p. If Notice type = Abandonment, then create a new Status = Capped and Abandoned with Status Event = Abandonment and Activity start date links to Status Start Date and Activity end date links to Status End Date.
1. An email is sent to the submitter acknowledging the submission. This email contains the **Borehole PID** and instructions for how to use this PID as the unique identifier.

### PGGD-03 Sequence Diagram

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/Sequence-Diagram-PGGD-03.png" width="85%"><br>
Figure 3: PGGD-03 sequence diagram</p>

#### To-be PGGD-03 business process

1. The operator will use the **Notice of intention to carry out hydraulic fracturing activities** Word template (or their own template) to enter the details currently entered into the existing PGGD-01 form.  
    a. This notice is given to the landholder by the operator.  
    b. The operator will provide a copy of the completed template to the department.  
1. The operator completes the new Lodgement Portal PGGD-03 online form and attaches the **Notice of intention**.
1. On form submission, a new borehole status event **Hydraulic fracturing on notice** is created in the Geological Properties database table **Borehole_status**. The status_start_date is set to the **Estimated commencement date** captured in the form and status_end_date is set to the **Estimated completion date** captured in the form.
1. An email is sent to the submitter acknowledging the form submission.

### PGGD-04 Sequence Diagram

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/Sequence-Diagram-PGGD-04.png" width="85%"><br>
Figure 4: PGGD-04 sequence diagram</p>

#### To-be PGGD-04 business process

1. The operator will use the **Notice of completion of hydraulic fracturing activities** Word template (or their own template) to enter the details currently entered into the existing PGGD-04 form.  
    a. This form includes the summary of the actual composition of the hydraulic fracturing fluid used.  
    b. This notice is given to the landholder by the operator.  
    c. The operator will provide a copy of the completed template to the department.
1. The operator completes the new Lodgement Portal PGGD-04 online form and attaches the **Notice of completion**.
1. If the end status is **Completion** or **Partial Completion**, the operator attaches a **detailed actual composition** of the hydraulic fracturing fluids used.
1. If the operator answers "Yes" to the question of "_Did any incidents occur that may have resulted in environmental harm or caused an adverse impact on any underlying or overlying acuifiers whilst undertaking activities?_": The submitter must attach a detailed statement of the incident using the Word template **Hydraulic Fracturing Incident Report**.
1. If Completion Status = Abandoned then the Status with the Status Event of Hydraulic fracturing on notice changes the Status Event to Hydraulic fracturing abandoned and Activity end date is added to or replaces the Status End Date.
If Completion Status = Partial Completion, Alteration, Completion or Other then  Status with the Status Event of Hydraulic fracturing on notice must have the Status Event changed from Hydraulic fracturing on notice to Completion of hydraulic fracturing and Fracturing start date overwrites Status Start Date and Fracturing end date overwrites Status End Date.
1. The uploaded documents are recorded as a Dataset_resource against the borehole in the Geological Properties database.
1. An email is sent to the submitter acknowledging the form submission.

## Technology

### Technology as-is

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD-tech-as-is.png" width="40%"><br>
Figure 5: PGGD technology as-is</p>

### Technology to-be

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD-forms-tech-architecture.png" width="80%"><br>
Figure 6: PGGD technology to-be</p>

## Forms as-is

### PGGD-01 as-is

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD01_form.png" width="90%"><br>
Figure 7: PGGD-01 Form</p>

### PGGD-02 as-is

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD02_form.png" width="95%"><br>
Figure 8: PGGD-02 Form</p>

### PGGD-03 as-is

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD03_form.png" width="100%"><br>
Figure 9: PGGD-03 Form</p>

### PGGD-04 as-is

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD04_form.png" width="100%"><br>
Figure 10: PGGD-04 Form</p>

### PGGD-01 Notice as-is

This is the document that is generated as PDF and emailed to the submitter:  
[PGGD-01 Notice of intention to drill a well or bore](https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD01-notice.pdf)

### PGGD-02 Notice as-is

This is the document that is generated as PDF and emailed to the submitter:  
[PGGD-02 Notice of completion, alteration or abandonment of a well or bore](https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD02-notice.pdf)

### PGGD-03 Notice as-is

This is the document that is generated as PDF and emailed to the submitter:  
[PGGD-03 Notice of intention to carry out hydraulic fracturing activities](https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD03-notice.pdf)

### PGGD-04 Notice as-is

This is the document that is generated as PDF and emailed to the submitter:  
[PGGD-04 Notice of completion of hydraulic fracturing activities](https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD04-notice.pdf)

## PGGD Forms to-be

### PGGD-01 form to be
Click here to view the form:  
[PGGD-01 Notice of intention to drill a well or bore](https://rawcdn.githack.com/geological-survey-of-queensland/gsq-lodgement-portal/7137f8bd7e0720a27892fc637d24101f7147f830/pggd01.html)

### PGGD-02 form to be
Click here to view the form:  
[PGGD-02 Notice of completion, alteration or abandonment of a well or bore](https://rawcdn.githack.com/geological-survey-of-queensland/gsq-lodgement-portal/e9b7bc7ee62e0a9368773faf507d7753d10c15d3/pggd02.html)

### PGGD-03 form to be
Click here to view the form:  
[PGGD-03 Notice of intention to carry out hydraulic fracturing activities](https://rawcdn.githack.com/geological-survey-of-queensland/gsq-lodgement-portal/7137f8bd7e0720a27892fc637d24101f7147f830/pggd03.html)

### PGGD-04 form to be
Click here to view the form:  
[PGGD-04 Notice of completion of hydraulic fracturing activities](https://rawcdn.githack.com/geological-survey-of-queensland/gsq-lodgement-portal/7137f8bd7e0720a27892fc637d24101f7147f830/pggd04.html)

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
* MERLIN BHF_MATERIALS_USED

The PGGD-03 and PGGD-04 forms _read_ from the following tables:

* MERLIN BHF_MATERIALS_USED - this populates the fluids and chemicals drop-down field values  

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/PGGD-db-schema-as-is.png" width="80%"><br>
Figure 15: PGGD database schema</p>

### Database schema to-be

The to-be PGGD forms use the Lodgement Portal database schema and the Geological Properties database schema.

## Data migration

To-do
