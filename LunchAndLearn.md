Outline for FHIR talk:

 * Primary targets of FHIR
     * Patient
     * Provider
     * Devices (all participants, both continuous and discrete)

 * Argonaut project + whats coming
     * Scheduling
     * Provider directory
     * REST Implementation Guidelines
     * Built on US Core StructureDefinitions

 * Who is using FHIR
     * Apple HealthKit
     * Google ProtoBuffers
     * VA Lighthouse
     * CMS Bluebutton
     * Firly - Simplifier
     * Cerner - Bunsen

 * Whats coming next: Bleeding Edge
     * FHIR Mapping language
     * GraphQL access for FHIR

 * Limitations of FHIR
     * STU2 is what most people have implemented. R3 and R4 are not mostly supported
     * Bulk data API and other features are still experimental and subject to change
     * Not all EHR systems support FHIR right now
     * Scientific research community and Payers are second class citizens

 * What can we do with FHIR at Fuse
     * Push customers to use FHIR
     * Map incoming data to FHIR in the case that the customer is not sufficiently advanced
     * Use FHIR for internal messaging
     * Steal terminology and reference data sets (Think: LOINC)
     * Think about FHIR as a way to link the pharmacy and provider more closely
     * Use FHIR as a data model and data store (HAPI)
