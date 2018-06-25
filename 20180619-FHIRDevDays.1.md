FHIR Dev Days
====

Dev days first day:

 * FHIR 4, whats new: cityside 2
 * Java HAPI - Harborside 3
 * FHIR + LOINC: Harborside 3
 * FHIR Path - Cityside 1
 * Lighthouse Lab - Cityside 2
 * Validation in .NET + Java - Cityside 2
 * BlueButton - Harborside 4

Opening Ceremony
====

FHIR is importand and 8 years old. Go FHIR. General feel goodedness. A bit of an introduction to the conference. After the intro an excellent talk on the overview of problems that FHIR hopes to solve. A bit about bulk data but generally noteworthy that CMS adoption and gov adoption will be slow which will limit the rate of bulk data and other similar changes. Also noteworth that images and large data blobs will not typicalky be in the core FHIR data exchange but will instead be externally hosted and referenced via their cloud URI. This would presumably be secured in some mechanism but nothing specific about that. The imaging company LifeImage came up during this overview.

FHIR 4, Whats New
====

Abstract: Focuses on the FHIR roadmap: what new features are planned for Release 4, and also discusses on what it means that release 4 will include "normative" content (and how to deal with version change)

After a bit of microphone issues... 

Right now FHIR is actually not really used that frequently. This is more growing from a standards and new dev perspective. So essentially we are talking about new Dev. They have a survey they have released that is querying how FHIR is working and how it isn't working. They have a certification for FHIR proficiency right now. The basic test is the R3 test, the advanced test is the professional certification. Getting this will allow you to work with companies as a certifier or implementer or a consultant. This should be coming this year. The market for this is lower than expceted and he wonders as to why.

SMART app launch specification. That is going to be coming out very soon. This will work with R2 - R4. There are a few resources in R4 that are actually normative. These resources include CodeSystem, ValueSet, ConceptMap, StructureDefinition, Patient, and Observation. Normative resources are more or less fixed in their format and structure. Critically it means they aren't going to make breaking changes to these resources. Anything that doesn't make normative ends up as STU (Standard for Trial Use). 

What is a breaking change? A: this is complicated. There is a page online for the answer to this question that the speaker is actively working on. 

HL7 members get to decide about the maturity model of the specification via the Ballotting process. For everyone else we can create reviews and submissions into the specification. These will be reviewed and balloted. Substantial changes close approximately 3 months before specification ratification. 

Right now the only projects out there are typically on R2 (release 2) or newer. Therefore it is likely that projects need to support N-2 versions. They are working on an endpoint for something that is a bit version agnostic. Also they are going to have an endpoint for querying supported versions as well as a query string (`fhirVersion`) or methodology for requesting a specific version in response to requests.

When they are working with releases they have a R2 -> R3 diff. This takes a great deal of time to assemble but it specifies how this will work. They create transform scripts that specify how these resources relate to each other. They will continue this process for future releases. R3 -> R4 is coming up here.

*New: GraphQL support (https://graphql.org/)*. This allows you to select which items you want back from the server and can also short circuit the round trips to the server and return all of the data that you need as a single packet. Goal: collapse unnecessary depth and simpler REST calls. This is only supported by HAPI and test FHIR. 

*Adding Bulk Data Support* This means that they have defined alternative formats for getting bulk data and alternative calls for retrieving this data. The implementation guide for this is published in draft mode. The following are details:
* Certification and authentication mechanism
* Async request pattern
* Newline delimited JSON

Discussion about: Json vs Avro/Parquet/Protobuf. The latter of these is more compact but they are more difficult to work with and more averse to version changes. Here are some interesting links: https://avro.apache.org/, https://github.com/google/protobuf, https://parquet.apache.org/

Breaking changes: lots of these! They have a page with known breaking changes. Some of them may require deconstructing and rebuilding resources from scratch. The speaker tries to condense the list of breaking changes down to something palletable.

Note that there is a diff link at the bottom of every page and you can see the W3C diff between the current version and the current version. This is on every page of the specification, that is.

They have one open issue which they are still settling. This is around JSON format questions. Specifically talking about how we refer to string values. In JSON the value of the property is the real value. If you care about extensions you can use '\_elementName':'Extension'. This makes it easier for client developers because the property "elementName" will have the value expected. This makes it slow for bulk data operations because it is slow to parse and handle extensions. 

New content in the specification: case reporting, occupational health data, lab test catalog, etc. See the slides for more examples of these contents.

FHIR Foundation - governance, process, policies, ballots, etc. They provide this for the community as well when necessary. They can run integration services and registry services for other customers. 

Next for the specification?

* Pushing this from a regulatory perspective in Europe, Africa and the US
* Improve aggregate data operations
* Formalize the review process
* Endpoint discovery

Java HAPI
====

Abstract: This tutorial will cover the basics of HAPI FHIR, including how to work with the data model, the parsers and the client. It also includes information on how to set up your development environment, and has all of the information a first-time Java developer will need to get started.

You can find the slides from this talk here: https://goo.gl/iCkNf2. Well, that isn't actually his talk, but it is essentially the same thing. This is going to be a tutorial for how to use the HAPI API in Java. He works for "https://ehealthinnovation.org/". He is also putting up the code here: https://github.com/FirelyTeam/fhirstarters Note that you can definitely think about using FHIR even when you aren't actually using the REST API directly. It still has advantages.

FHIR + LOINC
====

Abstract: FHIR and LOINC go together like chips and salsa: FHIR’s standardized resources and API are the perfect delivery vehicle for clinical data coded with LOINC, the freely available international vocabulary standard for identifying health measurements, observations, and documents. LOINC is now ubiquitous in health data systems worldwide, and is an essential ingredient of system interoperability. In this session, we’ll get acquainted with the basics of LOINC, tour the common FHIR resources where you can make use of LOINC coded health data, and explore the LOINC-specific features in FHIR’s terminology services.

For information, visit his website at: https://danielvreeman.com/loinc-on-fhir/

Most of LOINC is funded by the federal gov. This is all about terminology standardization. You can download the reference terminology dictionary from the LOINC website here: https://loinc.org/downloads/loinc/.

Breakdown of LOINC:
	* Code
	* Component
	* Property
	* Timing
	* System
	* Scale
	* Method (optional)

This allows the description of concepts in parts. This applies to individual results. They use the same concept for collections, for example vitals, which represents multiple related observations.

They also have a newer concept of structured answer lists which is useful for something like patient reported outcomes measures. These are framed more in the structure of question and answer in a bundled list.

They publish approximately two updates per year. June + December. They just finished a publication last week.

LOINC is basically built into the HAPI FHIR code base now. See https://fhir.loinc.org 


FHIR Path
====

Abstract: FHIRPath is a path traversal and extraction language much like XPath. It is used -amongst other things- for validation of instances. Learn about its syntax and structure so you can author FHIRPath expressions with confidence

This is currently in STU1 (standard for trial use 1). They are working on the normative version for this right now. Essentially they need a way to have searched parameters, but they also need a way to specify which field to search on. Example of this is XPath - listing out which field they are talking about in the xml document. This is easy to express verbally but they didn't have a langage agnostic way of describing this.

Two primary uses:

* Search parameters
* Specific data extraction
* Field definition and description - ex: VALIDATION!

This is a way to *extract* data but it is NOT a way to update data. This works on other types of data in addition to FHIR, but it was written initially for FHIR. This is actually used in CQL as well. Nice!

#### Data navigation

Data can be shown as a tree. Just another way to think about data that we normally represent in flattended structures. So long as you can represent your data in a tree format - you can run FHIR path on top of it. Cheers!

What does it look like?

  > patient.name.given

This navigates down a tree from the root to ALL matching nodes. You can specify the type of the data (patient) or this is optional if we know that we are talking about a patient.

We can also navigate by axis:

  > patient.children()
  > patient.descendents()

You can combine these together as well. Think `patient.descendents().name`.

Other options:

* Select by index (ZERO based): `patient.name[0]`
* Select first/last: patient.name.last().given.first()
* Also have `skip` and `take`
* Can also do `count()` which returns the number of matching elements

In FHIRPath, everything is a list. In other words, we don't know how many nodes/elements will match so everything will return a list. Boom, that happened. 

What else can you do?

#### Expressions

We need a way to select a path based on a condition. For example if we want to limit the number of patient names to "2" as opposed to the very general FHIR specification. Any of these are boolean expressions. Example:

  > patient.name.count() <= 2

If there are less than 2 names, return true but otherwise false. 

The boolean operations are "and" , "or", "xor", and "implies". Also have compare, string manip, arithmatic, supersetOf, subsetOf, and the turnary operator. For turnary operator you can use "iif(patient.name.first = 'stuff', 'yes', 'no')"

Comparisons can only be used with primitive types. So, if you do something that is non-determinate then you can not reliably do something like:

  > patient.name[0].given = "stuff"
  > patient.name.given = "stuff"   // only works with just one name. Otherwise false/failure

Types in FHIR path: string (single quoted), bool, int, decimal, datetime, time, quantity. You can look these up online. You can find mapping in the FHIR specification appendix for FHIR path.

There is an `is` operator which allows you to filter based on type or allows you to perform an operation only when something is of the correct type. This is also useful with `ofType` which finds things of a specific type.

#### Filtering and Projections

Filtering is used as follows:

  > patient.name.where(use='official')

This will find the NAME where the official flag is set to true. As opposed to `patient.where(name.use='official')`. That second query would find patients where they had an official name.

The `select` function is used to determine what we want to return. Think LINQ select statement. So an example:

  > patient.name.select(given & ' ' & family)

This is what he calls projecting or mapping. The key here is they have to have one item on the left and one on the right. Anything that has multiple would return an error.

#### Missing data

What do we do when we have empty lists or no data given the selectors?

  > patient.name.given = 'duck'

The short answer is empty list. This is essentially treating an empty list like a NIL/NULL value. What does this mean? They have a three value logic going on here. (true/false/unknown)

This is where implies comes into play. The above statement returns empty list when there are no names, we might instead do:

  > patient.name.given.exists() implies patient.name.given = 'duck'

This will always return true or false whether or not the name actually exists. Try it out? Go look at the FHIR path specification.


Lighthouse Lab
====

The VA has a developer sandbox (right now its called "Lighthouse"). You can find documentation here: https://www.oit.va.gov/developer/ They are trying to create an eco system for innovation. Platform. Plug-in. Value. Keyword. 

Their problem statements:

* Patient direct portal + care
* Insitutional cost drivers

If you can solve either of these problems, they want your solution.

This is a bunch of fluff. Signed up for the github repo here: https://github.com/department-of-veterans-affairs/VA-Micropurchase-Repo
The actual portal itself is here: http://portal.lighthouse.vaftl.us/
Getting started guide: http://portal.lighthouse.vaftl.us/getting-started

They are using OCTA for authorization + authentication

Bottom line: they are creating a FHIR test server. 

API Key: GrxXgh76tu6GogbLLFvPJr35E4kWdNPaaupVWNNj