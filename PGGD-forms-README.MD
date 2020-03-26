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

## Busines process

### Current business process

### Future state business process

### PGGD Forms as-is

* PGGD-01  
* PGGD-02  
* PGGD-03  
* PGGD-04  

### Database schema as-is
