# Lodgement Form Survey PID minting

The Persistent Identifier (PID) is the unique identifier for a survey that is submitted to the department. GSQ needs industry to use this PID when submitting data to the department.

## Minting a new PID

Customers lodge _Notice of Intention to Survey/ Notification of geophysical survey_ forms to the department. This means that the survey is proposed but not completed.

We need to tell the submitter of the form what the Survey PID is and what it is used for. We can do this by emailing the PID to the submitter after successful submission.

For Survey Plans, we want an automated process of creating the Survey Plan survey record in Geoprops. From there, SGS can manually enter the remainder of the Survey information in Geoprops. For Survey Plans, there is only minting, not matching.

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/survey-minting-sequence-diagram.png" width="100%"><br>
Figure 2: Lodgement Portal Survey PID minting</p>

### Matching an existing PID

When the submitter submits a _Notice of survey completion (or abandonment, etc.)_ form, we need to tie this lodgement to the _proposed_ survey record in the Geoprops database.

For instance:

* A customer earlier submitted a [PA-21A - Notice of intention to carry out seismic survey or scientific or technical survey](https://www.dnrme.qld.gov.au/__data/assets/pdf_file/0004/259897/pa-21a-notice-intention-survey.pdf)
* The system gave them the PID of the Survey, e.g. SS36789
* Now the customer lodges a [PA-22A - Notice of completion of seismic survey or scientific or technical survey](https://www.dnrme.qld.gov.au/__data/assets/pdf_file/0005/259898/pa-22a-notice-completion-survey.pdf)
* The customer enters the PID of the Survey SS36789 so we can tie the _completion notice_ to the _intention notice_, i.e. tie to the completed survey to the proposed survey.
* The _status_ of the Survey can now be changed manually in Geoprops to _completed_.
* When the customer later lodges a _Seismic Survey Report - Final_, again they enter the PID so that the report is tied to Survey SS36789.

<p align="center">
<img src="https://github.com/geological-survey-of-queensland/gsq-lodgement-portal/blob/master/images/survey-matching-sequence-diagram.png" width="100%"><br>
Figure 3: Lodgement Portal Survey PID matching</p>

## To-be Survey PID Minting business process for Geophysical Surveys

NOTE: This process requires the existing Notice of Survey forms (PA-21A Notice of Intention to carry out seismic survey or scientific or technical survey and PM 1/2013 Notification of geophysical survey) to be enhanced or be replaced by a new form.

1. The submitter submits a Notification of geophysical survey form in the Lodgement Portal.
1. On submission, a new survey is created in the Geological Properties database using the data submitted in the Survey form.  
    a. Type = Type from lodgement form. This will be either **Geophysical** - (http://linked.data.gov.au/def/survey-type/geophysical) or **Seismic**
(http://linked.data.gov.au/def/survey-type/seismic)  
    b. Title = Title from lodgement form  
    c. Description = Description from lodgement form  
    d. Method = Method from lodgement form  
    e. Access Rights = rights from lodgement portal conditional logic configuration  
    e. Start Date = Survey Start Date from lodgement form  
    f. End Date = Survey End Date from lodgement form  
1. An email is sent to the submitter acknowledging the submission. This email contains the **Survey PID**. There are instructions for how to use this PID in future submissions.

## To-be Survey PID Minting business process for Survey Plans

1. The submitter submits a Survey Plan form in the Lodgement Portal.
1. On submission, a new survey of type **Survey Plan** is created in the Geological Properties database using the data submitted in the Survey Plan form.  
    a. Type = "Spatial" - [http://linked.data.gov.au/def/survey-type/spatial]  
    b. Title = Title from lodgement form  
    c. Description = Description from lodgement form  
    d. Method = "Ground" - [http://linked.data.gov.au/def/survey-method/ground]  
    e. Access Rights = rights from lodgement portal conditional logic configuration  
    e. Start Date = Survey Completion Date from lodgement form  
    f. End Date = Signature Date from lodgement form  
1. An email is sent to the submitter acknowledging the submission. This email contains the **Survey Plan PID**. There are no instructions required for how to use this PID.

## To-be Survey PID Matching business process

1. The submitter starts a Survey form in the Lodgement Portal.  
1. The submitter selects a PID using a PID selector component (same user experience as borehole selector component).  
    a. The user can type in a PID or the survey title.  
    b. The system returns the PID and name of the survey to the submitter.  
1. The form is filled with the following survey metadata from the Geological Properties database.  
    a. Title  
    b. Description = Description from lodgement form  
    c. Method = Method from lodgement form  
    d. Start Date = Survey Start Date from lodgement form  
    e. End Date = Survey End Date from lodgement form  
1. On submission, the survey in the Geological Properties database is updated using the data submitted in the Survey form.  
    a. Type does not change  
    b. Title = Title from lodgement form  
    c. Description = Description from lodgement form  
    d. Method = Method from lodgement form  
    e. Access Rights = rights from lodgement portal conditional logic configuration  
    e. Start Date = Survey Start Date from lodgement form  
    f. End Date = Survey End Date from lodgement form  

## Lodgement Portal survey forms covered by the Minting process

* PA-21A Notice of Intention to carry out seismic survey or scientific or technical survey
* PM 1/2013 Notification of geophysical survey  

NOTE: This process may see these existing Notice of Survey forms replaced by a new form.

## Lodgement Portal survey forms covered by the Matching process

* PA-22A Notice of Completion of seismic survey or scientific or technical survey  
* Geophysical Survey Report - Final  
* Seismic Survey Report - Final  
* Seismic Survey Report - Reprocessing  
* Seismic Survey Report - Other
