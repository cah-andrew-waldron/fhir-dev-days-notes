FHIR Dev Days
====

Dev days SECOND day:

 * Accessing Health Records with HealthKit - Harborside 4
 * Overview of Argonaut Initiatives - Cityside 1
 * Structure Definition 101 - Harborside 4
 * Distributing decision support with FHIR - Cityside 2
 * Data query, US core - Harborside 4
 * Clinical studies, how can FHIR help? - Cityside 2
 * HSPC - Cityside 2 OR XDS on FHIR - Harborside 4

Day Introduction
====

Skipped it. Assumed it was for the student. Actually wanted to get to the HealthKit talk.

Accessing Health Records with HealthKit
====

Abstract: Health Records is new feature released earlier this year as part of iOS 11.3 which allows consumers to see their medical records right on their iPhone. Consumers can now connect to over 500 hospitals and clinics that provide a FHIR API compatible with the Argonaut Data Query Implementation Guide. Earlier this month, Apple introduced a Health Records API for developers and medical researchers to integrate Health Records data into their apps with the user’s permission. In this session you will learn more about this API from members of the Apple Health Software team.

This is specifically in iOS 12. Data is stored in FHIR format. Data can be presented as a timeline. Goal: Help users understand their data better. They currently cover 500+ hospitals which is more than 50 institutions. The data is stored on the user's device. They also aggregate that data across the spectrum and store it as well.

Agenda for the talk:

* New sample types (model objects)
* Access request and authorization model
* How does FHIR apply to HealthKit
* How to protect privacy in this domain

#### New Sample Types

New sample types. HKSampleType is the core. HKClinicalType is new! This groups conditions, medications, lab results. There are new identifiers as well: HKClinicalTypeIdentifier. Examples: ".allergyRecord", ".conditionRecord", ".immunizationRecord", ".labResultRecord", etc. New HK sample type for clinical data: HKClinicalRecord. This has a few important fields, the clinical type (see previous examples), the display name (user facing presentation value - LOINC lookup for the canonical name), and fhirResource (...getting back to this later).

Source of the data is important. In HealthKit they model this as HKSource. The name in this object is the name of the institution. They also have a bundle identifier which identifies the bundle that the data came from. (SIDE AAAAA - why not NPI or an actual value?)

#### Authorization and Access

This data is obviously sensitive. They have introduced a new authorization flow specifically around this data. When the application requests authorization for health data:

1. They present a screen that informs the user what they are granting access to
1. Accepting this allows the user to select which categories of data they are willing to share. You can provide a message on this screen and a privacy policy notifying the user how you are going to protect them.
1. The user has a third screen where they can choose "Ask before sharing" or "Automatically Share". Default is the first of these.

What does this mean? This means it is possible you would need to request authorization every time the user opens your application. Basic process:

* Configure your project
    * Add the "health records" capability in XCode. This is a one time change.
* Request Access
    * He has a long example. You can find examples online.
    * You won't know the user's answer. You'll just either get data or not.
* Query the data
    * HKSampleQuery - one shot query
    * HKAnchoredObjectQuery - ?? no idea
    * HKObserverQuery - active observation of some particular data 

You can control the timing of the request screen using the getRequestStatusForAuthorization method. If the result is ".shouldRequest" then you can present an additional workflow screen explaining why the request is going to be made.

There is a possiblility of a "Required Types". If the user doesn't grant access to all of these types then you will get an error. You can handle this "errorRequiredAuthorizationDenied" error and inform the user why they need to grant access to all of this data.

#### FHIR Applications To HealthKit

We should probably all know this. They have focused on 8 only (see terrible screen shot on phone for this list). The raw data is available from the HKFHIRResource. The data field has the JSON serialized data. This can be pulled out with various methods which can probably be looked up online (JSONSerialization, SWIFT Codable?). You need to use source, resource type and identifier to uniquely identify health records. All three fields can be used for de-duplication. Another live demonstration.

#### Privacy

This is important. I'm sure they'll end up banning anything that doesn't meet their standards.

Overview of Argonaut Initiatives
====

Abstract: This session will provide an overview of Argonaut Initiatives: Provider Directory, Scheduling, Data Query, Document Query

Going through a background of Argonaut and the timeline for the project. Started with the JASON task for recommendations that can be found here (https://www.healthit.gov/sites/default/files/facas/Joint_HIT_JTF_JTF%20HITPC%20Final%20Report%20Presentation%20v3_2014-10-15.pdf). Driven the MU3.

Argonaut consists of StructureDefinitions for most of the common data types that would be transferred. Argonaut is on DSTU2 vs US FHIR Core is on STU3. Essentially the US Core is balloted vs Argonaut is not.

Another thing they are working on with Argonaut was creating a provider directory: http://build.fhir.org/ig/argonautproject/provider-directory/. This should be easy but it really wasn't. They are going to be still working further on this.

Project around patient and provider scheduling: https://github.com/argonautproject/scheduling. This was focused on finding available appointments. Provider based or patient based scheduling. They also have a pub/sub model for getting slots where the application can handle getting provider apppointments.

Adding CDS Hooks support. See this website for details on CDS hooks (https://cds-hooks.org/). This will be published officially later this summer. There will be other talks delving into this but please use the sandbox and also definitely go through the quick start guide (https://cds-hooks.org/quickstart/). 

Whats coming up next?

 * Clinical notes
 * *BULK DATA ACCESS TO CLINICAL DATA* - this was started by SMART last year. So look into that:
    * Recommended in this talk: https://github.com/smart-on-fhir/fhir-bulk-data-docs
    * http://docs.smarthealthit.org/flat-fhir/
    * http://wiki.hl7.org/index.php?title=201801_Bulk_Data
    * Using ndjson as the format? http://ndjson.org/
 * Simple assessment questionairres


Structure Definition 101
====

Abstract: The StructureDefinition resource is the main workhorse of profiling in FHIR. This tutorial will show you how this resource is used to represent the basic validation tools in a StructureDefinition for those that want to have a sense of the internals of FHIR validation

This is a foundational conformance resource because it defines the FHIR specification itself. This is defined just like any other resource. This resource provides two things:

* Provides a logical definition for the structure and business rules on the core resources
* Describes constraints on the existing StructureDefinition

Where do you find the core structure definitions? A: Online. registry.fhir.org, simplifier.net. Those are two good sites to find this.

One interesting thing you can do with this is only populate a differential based on a snapshot (full specification). This allows a project such as argonaut to populate only the specifics of the project as opposed to populating all of the details. Also noteworth - some of the structure definitions refer to other structure definitions. In other words, they need to be looked up in order to fully flush out the details.

Note that if you want to create your own profiles two easy ways to do this are:

* Trifolia: https://trifolia.lantanagroup.com/
* Forge: https://fire.ly/forge/

So, yeah, lots and lots of depth in what you can do with the StructureDefinition but the goal is always meta data essentially and snapshot generation.

Distributing decision support with FHIR
====

Abstract: The ability of healthcare IT systems to react quickly and effectively to emerging public health concerns is a significant challenge. See how the FHIR Clinical Reasoning Module can be used to help address this challenge by enabling the exchange of decision support knowledge

Decision support looks like the following

 * Dev team creates recommended paths. "Med A Presribe" -> "canned options"
 * EHR Hook signals these paths
 * Provider replies by selecting a path or something else such as "This path is invalid"

Note, this is over-simplified. The RxNorm database is used to normalize Rx information. The dosage can be entered different ways. Every system is different. So, to make this work we need key resources for standardization of concepts. These resources we will be using from the FHIR specification are:

 * ValueSet
 * Library
 * ActivityDefinition
 * PlanDefinition

There tons of details but what I'm seeing here is that this is relatively prescriptive. You create a number of different fixed resources and an exacutable. These combine together to provide decision support. The executable is written in CQL which stands for Clinical Quality Language. You can find more information on CQL itself here: http://www.hl7.org/implement/standards/product_brief.cfm?product_id=400

Caveats: different versions of the DSTU2 vs STU3 can cause problems. Required data fields and types must be provided (RxNorm data and dosage information as examples). Honestly not supper impressed because this is so specific to just giving a list of cards back from a single request. Thats basically it.

...and now lunch break.

Keynote
====

Besides the introduction this was an amazing talk. The following were the highlights:

 * FHIR adapters
     * Converting FHIR to protocol buffers: https://github.com/google/fhir
     * Converting CSV to FHIR
 * Data mining and research
     * FHIR adapters in big query
     * Machine learning models - need a new model that isn't factor based. These discard too much data and accuracy. This site explains more about that (https://ehrintelligence.com/news/google-study-uses-entire-patient-ehr-for-predictive-analytics)
 * Search capabilities:
     * Using relational terms to find out what the doctor is actually looking for
     * Coming up with the one scentence synopsis for the patient (this is an ultimate goal)
     * Revealing test results

Google plans on open sourcing these tools as they continue to develop them.

More links found that seem to be mildly relevant:
 * Health care direction: https://ai.googleblog.com/2018/03/making-healthcare-data-work-better-with.html
 * Protocol Buffer release: https://apievangelist.com/2018/03/13/google-releases-a-protocol-buffer-implementation-of-the-fast-healthcare-interoperability-resources-fhir-standard/

Data query, US core
====

Abstract: The US Core defines the minimum conformance requirements for accessing patient data as defined by the Argonaut pilot implementations and the ONC 2015 Edition Common Clinical Data Set (CCDS). This tutorial will give a high-level overview of the profiles, their importance, and future plans

The only search they agreed to is querying current details for the allergy profile for example is getting current allergy information. 

Goodness, this is boring. He is just going through models and extensions in Argonaut and which details can be searched. It is basically most of the fields can be searched in some way. Also talking about extensions. They have extensions. Yup, thats a thing.

Oh now he is introducing the fact that there are 23 value sets for the data.

CapabilityStatement (formerly known as conformance). This is something where the server can indicate to the client the features that it supports. This is in the specification and is a resource that can be requested. Goooooo FHIR.



Clinical studies, how can FHIR help?
====

Abstract: Clinical studies form an essential component of getting new drugs to market, and thereby improving healthcare outcomes for people the world over. In the Clinical Research realm there has been considerable interest in moving forward with integrating the healthcare record with the research systems; this is grouped under the topic of electronic Source (or eSource) and is expected to provide huge benefits for researchers, site staff and most importantly patients. This session will cover the concepts involved in the setup and conduct of a Clinical Study and illustrate these using the ResearchStudy and ResearchSubject resources. Code samples will be using Python



