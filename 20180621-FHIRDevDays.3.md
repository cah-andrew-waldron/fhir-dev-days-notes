FHIR Dev Days
====

Dev days SECOND day:

 * FHIR messaging, the unloved paradigm - Cityside 2
 * FHIR and Protobuf - Harborside 4
 * Scalable Data Science with FHIR - Harborside 4
 * Bulk Data - Harborside 4

Total side note: https://www.projectbaseline.com/

Day Introduction
====

Skipped it. Assumed it was for the student. Actually wanted to get to the HealthKit talk.

FHIR messaging, the unloved paradigm
====

Abstract: This session covers the highlights of the FHIR Messaging Paradigm - what is it, why would you use it? Within the FHIR community messaging is usually seen as an old fashioned way of data interchange, yet at the same time: messaging is probably the most widely used paradigm for data exchange in healthcare, and a large number of FHIR projects are based on FHIR messaging. So: what's the power of messaging?

This is essentially a loosly coupled paradigm. Not synchronous. Also making the assumption that the recipient has NOT seen the patient before. In other words, we need to provide much more context around the patient. This indicates that we need to send a whole bunch of resources about the patient as opposed to allowing the recipient to process the parts they actually care about. This is also Trigger based as opposed to being called.

So what does this look like in FHIR? The message just like a document, is a bundle. Inside of the bundle they have a MessageHeader FHIR resource. The MessageHeader sounds like an ISA header. Its the metadata and focus of the message. I can't tell if you can send multiple events at once or if they are a single message per bundle.

Messaging protocol contains both the messages themselves as well as acknowledgement of these messages. Messages should have a unique identifier in the header of sorts that assists the recipient in determining duplicates and whether or not to process those messages. Yet another part of the protocol is that you should hold the "last response" in a cache for a minimum of 15 minutes. Any unacknowledged messages will be re-sent within about 1/2 hr.

Trigger events? They are specified here: https://www.hl7.org/fhir/messaging.html. Essentially the trigger can be anything though, it just specifies the focus resource that has been modified. In other words, if a Patient resource changes a message would be sent with a patient as the focus of the message.

Guess what? They also have the ability to subscribe to a stream. Surprised? You shouldn't be. The subscription itself is a message that has criteria, metadata, and more. 


FHIR and Protobuf
====

Abstract: N/A
URL: http://github.com/google.fhir/
Bazel for building: https://bazel.build/
Talk by: Patrik Sundberg (sundberg@google.com)

#### Protocol Buffers

What are they? Documentation here: https://developers.google.com/protocol-buffers/. Think POJO, it has its own language for defining the data structure. Each field is provided a unique number for serialization (tag number). Definitions are "compiled" to auto-generated code that you can reference from your actual code. Whenever you change the defn, just "recompile" to regen the code templates.

Their pipeline looks like:

    "Raw Data"
      --[mapping]-->
    "Raw ProtoBuf"
      --[mapping + joins]-->
    "FHIR"
      --[generated code - Open Source NOW!]-->
    "Protocol Buffers"
      --[GCP - WIP NOW]-->
    "TensorFlow"

The challenge is the first step in this process is slow. They need an efficient mechanism for storing the RAW data. At google the choice for storage is always protocol buffers. As soon as they encode something internally in Protocol Buffers these files are immediately queryable when they are saved in GCP. This makes this the obvious choice for this reason as well. JSON works, but it isn't as optimized. Similar offerings out there are Avro, Thrift, Presto. (https://avro.apache.org/, https://thrift.apache.org/, https://prestodb.io/)

Writing protobuf code is done by parsing StructureDefinitions then converting this to the Protobuf Definitions. They are doing their best to enforce constraints in the ProtoBuf format as opposed to doing it on the other end of things. Their code inlines value sets when possible (limited lists). They also tried to add support for extensions and other parts of the specification. The goal? Support the entire FHIR specification as completely as possible. Because resources can be self referencing or contain any other resource, they all must be contained in the same Protocol Buffer definition file. This results in a sizable definition.

Nuances: For extensions they combine the extension details into the main field itself. This results in "field.value" and "field.extension" as opposed to just being able to use "field" and get the value directly and "\_field" for the extension.

For some types of primitives they store the string value instead of the parsed value. Example of this is double. For date times they store these as longs (timestamp). This makes it easier and faster to work with.

FHIR JSON parser must be written in order to interact with the ProtoBuff. One interesting thing you can do is you can take a single extension or resource and generate the proto definition for this to see how it flattens and modifies these.

The TensorFlow training is still a work in progress but it will be out and open source soon. We should be able to make predictions in healthcare much the same way we do with other things. They will be training models, we can do the same. The way they look at features is resources are features rather than extracting normal features.

Different versions of the spec will result in a full migration of data from RAW. In other words, you'll need to re-process from RAW.

Question from Afterwords: Note that the first part of the process where they get to FHIR is not really something they could open source because it is mostly manual effort. This would be difficult to open source. One concept they had was maybe they will change the model and give the tools to the customer to allow them to cleanse the data. Especially since the customer knows the data the best.

Scalable Data Science with FHIR
====

Abstract: As  healthcare datasets grow, so do the challenges of exploring and analyzing them. This tutorial looks at how FHIR can help even with petabyte-size datasets by natively encoding FHIR resources in popular tools for data science at scale. Attendees will use interactive Jupyter Notebooks and the Apache Spark processing engine to explore such data in an efficient, columnar format. Participants will work through data engineering and analysis examples and touch on machine learning. These examples are based on Bunsen, an open source project to simplify analyzing FHIR with tools like Apache Spark. For the best experience in this tutorial, attendees are encouraged to have Docker installed on their machines and run the following command to install a Docker image with the data science environment we will use:

* Docker Image: docker pull cerner/bunsen-notebook
* Docker Details: http://engineering.cerner.com/bunsen/0.4.0/docker.html
* Bunsen Project: https://engineering.cerner.com/bunsen/0.4.0/introduction.html

Talking about tools and data. But the problem is that they have a large amount of intrinsic complexity that makes it difficult to use in popular machine learning tools. So they project the data, which is to say that they simplify the data to meet their needs. They are building a lake of data based on the FHIR structures. This results in portable data models and training. This also results in reproducible research. What are the models? JSON, XML, RDF, ProtoBuf, Parquet. The last of these is for this talk.

Parquet is column clustered rather than row clustered. This makes field scans faster. You can run the full demonstration from their github. Just following along on the screen. Data generated from Synthea

Bulk Data
====

Abstract: Healthcare organizations have many reasons for bulk-data export, such as populating a data warehouse for operational or clinical analytics, leveraging population health and decision support tools from external vendors, and submitting data to regulatory agencies. Today, bulk export is often accomplished with proprietary pipelines, and data transfer operations frequently involve an engineering and field mapping project. Learn about an exciting new effort by SMART and HL7 to bring the FHIR standard to bear on these challenges of bulk-data export.

Talk By: Dan Gottlieb and Josh Mandel

All code that they are going to show is open source and open standards. They wanted to extend the FHIR API to retrieve population level data sets. The specifications are still in Flux. Please connect with them for feedback on these specifications. They are trying to help solve scenarios like:

 * EHR -> CSV -> FHIR -> work with it
 * EHR ---[millions of requests]--> FHIR
 * Submission of patient information to national registries. Also very slow.

Same applies to updates. Fact is, REST is great, but not for anything bulk. These are problems that need to be solved. He is using https://www.i2b2.org/ base to store and process data.

We established the problems. Now, lets apply FHIR more like what we would do with the custom CSV exports. They are hoping to have a pipeline concept as part of this process. Example pipelines:

 * EHR --[bulk data request]--> DeIdentification --> ETL --> ResearchDb
 * EHR --[bulk data request]--> Encryption --> SFTP --> External Partner

These are just examples. This should be able to be a request that is nightly or whatever. This is file focused. This is attempting to use existing technologies. Authorization and authentication built in. Out of scope for this:

 * BAAs, SLAs and DUAs. NOTE: Government has projects underway to do this but it isn't part of bulk data
 * Real time data
 * Data transformations themselves
 * Patient matching

Basic process:

 1. Kick off request from the "customer" server or client. Follows:
     * Operations
         * FHIR operation for all patients: http://baseurl/Patient/$export
         * FHIR operation for a patient group: http://baseurl/Group/[group id]/$export
         * FHIR operation for all server data: http://baseurl/$export
     * Always include the header: `Prefer: respond-async`. This can be done in any FHIR request.
     * Query string parameters:
         * \_outputFormat : response format. Only `ndjson` right now
         * \_since : request only FHIR resources modified since a particular date/time
         * \_type : The types of resources you are interested in requesting
         * [group id] : another way to narrow down the results
 1. Server responds with a location URL.
     * Status is queried with a get request to this URL.
         * `X-progress` will be returned with status string.
         * `Retry-after` will be returned with seconds till retry
         * 200 with a response body will indicate completion. This body will be JSON with output files.
         * Response could be an error or it could also contain some of the errors along with some successes.
     * DELETE operation on this URL will cancel the data request
 1. List of output files which is an array of resource types + files
    * Finally the server can download these files with get requests

All requests must be authenticated. Typically done with OAuth, same as for SMART.

Tools that are available:

 * SMART bulk data server reference implementation: https://bulk-data.smarthealthit.org/
 * Also have a sample client at the same URL
 * Bulk data -> BigQuery. See this in the presentation as a link. Synthea data, what?? :)
 * Link to the repository: https://github.com/smart-on-fhir/fhir-bulk-data-docs

QUESTIONS:
 * What about "to" on the request to filter end date?
 * Any way to filter field list or transform with GraphQL?
 * Expiration time on files prepared for download on the server?
 * ...maybe... more filters on downloads - site filters or ICD filters?
 * 












