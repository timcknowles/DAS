# DAS APP REQUIREMENTS

![DAS logo](https://pbs.twimg.com/profile_images/533750516077977600/3k85HmOJ_400x400.png)


## Background:

Around 3 million general anaesthetics are given in the UK each year. When you have a general anaesthetic your anaesthetist inserts a device into your throat to ensure oxygen enters the body whilst you are asleep. This process is termed airway management.  In the vast majority of cases, this process is uncomplicated. However, if this process becomes difficult there can be serious life-threatening consequences. This is described as an airway emergency. It is vital that for future anaesthetics any previous problems with airway management are communicated. 

The Difficult Airway Society (DAS) maintains a nationwide standalone difficult airway database (DAD) of patients who have had difficult airway management and provides patients with an airway alert card to carry. 

# Responsive WebApp to handle the following core functions

## User registration 

* Clinical user reigstration (create a clinical user) [authentication ideally with NHS mail OAuth](https://comms-mat.s3.eu-west-1.amazonaws.com/Comms-Archive/NHSmail+Single+Sign-on+Technical+Guidance.pdf) or with email link validation in the meantime. 
    - first name
    - last name
    - professional role
    - GMC number
    - NHS.net email address (this should be the default expectation other email domains will require manual whitelisting)

* Patient demographics (create a patient)
    - suggest using subset of [NHS digital PDS API](https://digital.nhs.uk/developer/api-catalogue/personal-demographics-service-fhir) schema
    - note in the current sytem no demographics are saved to the cloud. A HASH of the NHS number is stored alongside the NHS number and this is linked to the aiway event. The other demographics are emailed and saved on a local offline database. This is used to create and post the airway alert card.
    - manually add mobile number and current personal email.
    - add a confirmation of patient consent field (current workflow involves a signed and scanned paper consent form - can we digitalise this ? e.g. with a text message validation link style workflow) 

* Create an airway alert
    - use existing DAS difficult airway database schema
    - example fields
        - date of incident
        - time of incident
        - free text summary of incident 
        - difficult mask ventilation
        - difficult supraglottic airway ventilation
        - difficult tracheal intubation 
        - grade of direct laryngoscopy 
        - free text: plan for future anaesthetics 

    - primary key: hashed from NHS number and date of birth.

* Inform the patient's GP
    - [GP connect Messaging](https://digital.nhs.uk/services/gp-connect/develop-gp-connect-services/specifications-for-developers#gp-connect-messaging-specification-listed-by-capability)
    - [GP connect specification](https://simplifier.net/guide/gp-connect-send-document/Home/Introduction/Introduction?version=1.3.2-public-beta)
    - send a secure message to the patient's GP informing of the difficult airway alert. 

*  Inform the patient (see airway alert card below)

## Admin dashbboard

* user management
* patient management 

## Bi-directional FHIR REST API

* communicate airway alerts from and to hospital EPR systems
* Use EPIC as test EPR for airway alert FHIR endpoint
* [EPIC FHIR Sandbox Condition.Create](https://fhir.epic.com/Specifications?api=949#1ParamType115750)
* [EPIC FHIR Sandbox Condition.Read](https://fhir.epic.com/Specifications?api=951)
* [SNOMED Code and Concept ID 763326004 |Difficult mask ventilation (finding)](https://termbrowser.nhs.uk/?perspective=full&conceptId1=763326004&edition=uk-edition&release=v20220803&server=https://termbrowser.nhs.uk/sct-browser-api/snomed&langRefset=999001261000000100,999000691000001104)
* [SNOMED Code and Concept ID 766781004 |Difficult supraglottic airway ventilation (finding)](https://termbrowser.nhs.uk/?perspective=full&conceptId1=766781004&edition=uk-edition&release=v20220803&server=https://termbrowser.nhs.uk/sct-browser-api/snomed&langRefset=999001261000000100,999000691000001104)
* [SNOMED Code and Concept ID 718447001 |Difficult tracheal intubation (finding)](https://termbrowser.nhs.uk/?perspective=full&conceptId1=718447001&edition=uk-edition&release=v20220803&server=https://termbrowser.nhs.uk/sct-browser-api/snomed&langRefset=999001261000000100,999000691000001104)
> **_NOTE:_**  in order to aim for the lowest common denominator consider a authenicated client app which can be embedded as an html link (each hospital organisation gets a client key and can query the API for the presence of an alert.  )


## Airway alert 'cards'

* Akin to the NHS covid QR pass
* Digital 'aiway alert card' containing QR code dowloadable to patient's smart phone. 
* contains unique code for authorised clinician to retireve details from the database
* should be printable if a conventional paper card is preferred by the patient 



