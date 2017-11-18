<div style="design: block;margin: 0 auto"><img class="img-responsive" alt="IMS Global Learning Consortium, Inc. Logo" src="assets/ims-logo-h170w600.png"></div>

# IMS Global Learning Consortium, Inc.

# Caliper Analytics&reg; Certification Guide, version 1.1

## IPR and Distribution Notices

Recipients of this document are requested to submit, with their comments, notification of any relevant patent claims or other intellectual property rights of which they may be aware that might be infringed by any implementation of the specification set forth in this document, and to provide supporting documentation.

IMS takes no position regarding the validity or scope of any intellectual property or other rights that might be claimed to pertain to the implementation or use of the technology described in this document or the extent to which any license under such rights might or might not be available; neither does it represent that it has made any effort to identify any such rights. Information on IMS’s procedures with respect to rights in IMS specifications can be found at the IMS Intellectual Property Rights web page: [http://www.imsglobal.org/ipr/imsipr_policyFinal.pdf](http://www.imsglobal.org/ipr/imsipr_policyFinal.pdf).

Copyright &copy; 2017 IMS Global Learning Consortium. All Rights Reserved.

Use of this guide to develop products or services is governed by the license with IMS found on the IMS website: http://www.imsglobal.org/speclicense.html.

Permission is granted to all parties to use excerpts from this document as needed in producing requests for proposals.

The limited permissions granted above are perpetual and will not be revoked by IMS or its successors or assigns.

THIS GUIDE IS BEING OFFERED WITHOUT ANY WARRANTY WHATSOEVER, AND IN PARTICULAR, ANY WARRANTY OF NON INFRINGEMENT IS EXPRESSLY DISCLAIMED. ANY USE OF THIS GUIDE SHALL BE MADE ENTIRELY AT THE IMPLEMENTER'S OWN RISK, AND NEITHER THE CONSORTIUM, NOR ANY OF ITS MEMBERS OR SUBMITTERS, SHALL HAVE ANY LIABILITY WHATSOEVER TO ANY IMPLEMENTER OR THIRD PARTY FOR ANY DAMAGES OF ANY NATURE WHATSOEVER, DIRECTLY OR INDIRECTLY, ARISING FROM THE USE OF THIS GUIDE.

## Table of Contents

* 1.0 [Introduction](#introduction)
  * 1.1 [Status of this Document](#docStatus)
  * 1.2 [Conventions](#conventions)
  * 1.3 [Terminology](#terminology)
* 2.0 [Certification Prerequisites](#certPreReqs)
* 3.0 [Metric Profile Conformance](#profileConformance)
  * 3.1 [Basic Profile](#basicProfile)
  * 3.2 [Annotation Profile](#annotationProfile)
  * 3.3 [Assessment Profile](#assessmentProfile)
  * 3.4 [Assignable Profile](#assignableProfile)
  * 3.5 [Forum Profile](#forumProfile)
  * 3.6 [Grading Profile](#gradingProfile)
  * 3.7 [Media Profile](#mediaProfile)
  * 3.8 [Reading Profile](#readingProfile)
  * 3.9 [Session Profile](#sessionProfile)
  * 3.10 [Tool Use Profile](#toolUseProfile)
* 4.0 [Data Interchange Format](#dataFormat) OR \[TODO\] JSON-LD Binding
  * 4.1 [JSON-LD](#jsonld)  OR \[TODO\] Key Concepts
    * 4.1.1 [The Context](#jsonldContext)
    * 4.1.2 [Node Identifiers](#jsonldNodeIdentifiers)
    * 4.1.3 [Type Coercion](#jsonldNodeIdentifiers)
    * 4.1.4 [Expressing Events as JSON-LD](#jsonldEvents)
    * 4.1.5 [Expressing Entities as JSON-LD](#jsonldEntities)
* 5.0 [Transport Conformance](#transportConformance)
  * 5.1 [HTTP Transport Requirements](#http)
    * 5.1.1 [HTTP Message Requests](#httpRequest)
    * 5.1.2 [HTTP Message Responses](#httpResponse)
  * 5.2 [MQTT Transport Requirements](#mqtt)
    * 5.2.1 \[TODO\] . . .
* 6.0 [Using the Certification Service](#usingCertService)
* 7.0 [Certification Mark](#certMark)
* 8.0 [Certification Expiration and Renewal](#certRenewal)
* [Appendix A. Example messages](#appendixA)
* [List of Contributors](#contributors)
* [References](#references)
* [About this Document](#aboutThisDoc)

## <a name="introduction"></a>1.0 Introduction

The Caliper Analytics&reg; specification provides a structured approach to describing, collecting and exchanging learning activity data at scale.  Establishing a common vocabulary for describing learning interactions is a central objective.  Promoting data interoperability, data sharing and data-informed decision making are also important goals.

Caliper also defines an application programming interface (the Sensor API&trade;) for marshalling and transmitting event data from instrumented applications to target endpoints for storage, analysis and use.  Industry-wide adoption of Caliper offers the tantalizing prospect of a more unified learning data environment in which to build new and innovative services designed to measure, infer, predict, report and visualize.

### <a name="docStatus"></a>1.1 Status of this Document
This document is the Final Release, meaning the technical solution is now made available as a public document and as such several IMS Members have successfully completed conformance certification at the time of the release of this document.
    
IMS strongly encourages its members and the community to provide feedback to continue the evolution and improvement of the Caliper specification. To join the IMS developer and conformance certification community focused on Caliper please visit \[TODO\] . . . .
    
Public contributions, comments and questions can be posted here: \[TODO\] . . . .

### <a name="conventions"></a>1.2 Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](#rfc2119).  A Sensor implementation that fails to implement a MUST/REQUIRED/SHALL requirement or fails to abide by a MUST NOT/SHALL NOT prohibition is considered nonconformant.  SHOULD/SHOULD NOT/RECOMMENDED statements constitute a best practice.  Ignoring a best practice does not violate conformance but a decision to disregard such guidance should be carefully considered.  MAY/OPTIONAL statements indicate that implementers are entirely free to choose whether or not to implement the option.

### <a name="terminology"></a>1.3 Terminology

<a name="actorDef"></a>__Actor__: An actor is an [Agent](#agent) capable of initiating or performing an [action](#actionDef) on a thing or as part of a process.  A Caliper [Event](#event) includes an `actor` attribute for representing the [Agent](#agent). 

<a name="blankNodeDef"></a>__Blank Node Identifier__: a string that begins with "_:" that is used to identify an [Entity](#entity) for which an [IRI](#iriDef) is not provided.  An [Entity](#entity) provisioned with a blank node identifier is neither dereferenceable nor has meaning outside the scope of the [JSON-LD](#jsonldDef) document within which it resides.

<a name="actionDef"></a>__Action__: something performed or done to accomplish a purpose.  Caliper [Event](#event) subtypes define a controlled vocabulary of one or more [actions](#actions) relevant to the activity domain.  A Caliper [Event](#event) includes an `action` attribute for expressing the associated action.     

<a name="contextDef"></a>__Context__: a special [JSON-LD](http://json-ld.org/spec/latest/json-ld/) keyword that maps the terms employed in a JSON document to [IRIs](https://www.ietf.org/rfc/rfc3987.txt) that link to one or more published vocabularies.  Inclusion of a [JSON-LD](http://json-ld.org/spec/latest/json-ld/) context provides an economical way of communicating document semantics to services interested in consuming Caliper event data.

<a name="describeDef"></a>__Describe__: \[TODO\] . . . .

<a name="endpointDef"></a>__Endpoint__: a receiver or consumer of Caliper data that is bound to a specific network protocol.  

<a name="entityDef"></a>__Entity__: an object or a thing that participates in learning-related activity.  Caliper [Entity](#entity) types provide course-grained representations of applications, people, groups and resources that constitute the "stuff" of a Caliper [Event](#event).  Each [Entity](#entity) corresponds to a node in a directed graph.

<a name="eventDef"></a>__Event__: describes a relationship established between an [Agent](#agent) (the `actor`) and an [Entity](#entity) (the `object`) formed as a result of a purposeful `action` undertaken by the `actor` in connection to the `object` at a particular moment in time.

<a name="jsonldDef"></a>__JSON-LD__: a specification providing a JSON-based data serialization and messaging format, processing algorithms and API for working with [Linked Data](#linkedData).  The messages described in this specification are intended to be used in programming environments that support [JSON-LD](http://json-ld.org/spec/latest/json-ld/).

<a name="iriDef"></a>__IRI__: The Internationalized Resource Identifier (IRI) extends the Uniform Resource Identifier ([URI](#uriDef)) scheme by using characters drawn from the Universal character set rather than US-ASCII per [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt).  [Linked Data](#linkedData) rely on IRIs to refer to most nodes and properties.

<a name="iso8601Def"></a>__ISO 8601__: Caliper data and time values are formatted per ISO 8601 with the addition of millisecond precision.  The format is yyyy-MM-ddTHH:mm:ss.SSSZ where 'T' separates the date from the time while 'Z' indicates that the time is set to UTC.

<a name="linkedDataDef"></a>__Linked Data__: A set of design principles first articulated by Tim Berners-Lee for discovering, connecting, and sharing structured data over the Web.  The principles can be summarized as follows: use [IRIs](#iriDef)/[URIs](#uriDef) as names for things; use HTTP [IRIs](#iriDef)/[URIs](#uriDef) so that information about things (e.g., people, objects, concepts) can be retrieved using a standard format; link out to other relevant things by way of their [IRIs](#iriDef)/[URIs](#uriDef) in order to promote discovery of new relationships between things.

<a name="lisDef"></a>__LIS__: Learning Information Services&reg; (LIS&reg;) is an IMS standard that defines how systems manage the exchange of information that describes people, groups, memberships, courses and outcomes.
 
<a name="ltiDef"></a>__LTI__: Learning Tools Interoperability&reg; (LTI&reg;) is an IMS standard for integration of rich learning applications within educational environments.

<a name="metricProfileDef"></a>__Metric Profile__: models a learning activity or a supporting activity that helps facilitate learning.  Each profile provides a domain-specific set of terms and concepts that application designers and developers can draw upon to describe common user interactions in a consistent manner using a shared vocabulary.

<a name="objectDef"></a>__Object__: an [Entity](#entity) that an [Agent](#agent) interacts with that becomes the focus, target or object of an interaction.  A Caliper [Event](#event) includes an `object` attribute for representing the resource.

<a name="sensorDef"></a>__Sensor__: Software assets deployed within a learning application that implement the [Sensor API&trade;](#sensorAPIDef) for marshalling and transmitting Caliper data to a target endpoint.

<a name="sensorAPIDef"></a>__Sensor API&trade;__: The standard set of methods and supported parameters that a [Sensor](#sensorDef) implements according to this specification in order to transmit Caliper data in an interoperable way.

<a name="termDef"></a>__Term__: a word or short expression that expands to an [IRI](#iriDef) when mapped to a JSON-LD [context](#contextDef) document. Terms are employed by Caliper as `type` property string values in order to distinguish between various JSON representations of entities and events defined by the Caliper information model.

<a name="typeCoercionDef"></a>__Type Coercion__: the process of coercing values to a particular data type.

<a name="uriDef"></a>__URI__:  the Uniform Resource Identifier ([URI](#uriDef)) utilizes the US-ASCII character set to identify a resource.  Per [RFC 2396](#rfc2396) a [URI](#uriDef) "can be further classified as a locator, a name or both."  Both the Uniform Resource Locator ([URL](#urlDef)) and the Uniform Resource Name ([URN](#urnDef)) are considered subspaces of the more general [URI](#uriDef) space. 

<a name="urlDef"></a>__URL__: A Uniform Resource Locator ([URL](#urlDef)) is a type of [URI](#uriDef) that provides a reference to resource that specifies both its location and a means of retrieving a representation of it.  An HTTP [URI](#uriDef) is a [URL](#urlDef) and using HTTP IRIs/URIs to identify things is fundamental to Linked Data.

<a name="urnDef"></a>__URN__: A Uniform Resource Name ([URN](#urnDef)) is a type of [URI](#uriDef) that provides a persistent identifier for a resource that is bound to a defined namespace.  Unlike a [URL](#urlDef) a [URN](#urnDef) is location-independent and provides no means of accessing a representation of the named resource.  

<a name="uuidDef"></a>__UUID__: a 128-bit identifier that does not require a registration authority to assure uniqueness.  However, absolute uniqueness is not guaranteed although the collision probability is considered extremely low. Caliper recommends use of randomly or pseudo-randomly generated version 4 UUIDs.  Each Caliper [Event](#event) MUST be assigned a UUID that is expressed as a [URN](#urnDef) using the form `urn:uuid:<UUID>` as described in [RFC 4122](#rfc4122).

## <a name="certPreReqs"></a>2.0 Certification Prerequisites

Certain prerequisites must be met before you can certify your platform, application or service as Caliper compliant. 

* Your organization MUST be an IMS Contributing or Affiliate Member.
* You MUST pass the tests using this Certification service.
* The tests MUST be completed by a designated representative of the member organization and you must agree that there is no misrepresentation or manipulation of the results in the submitted report.
* You MUST submit your report via the Caliper Certification Service.

## <a name="profileConformance"></a>3.0 Metric Profile Certification

As described more fully in the [Caliper 1.1 specification](#caliperSpec) the Caliper information model defines a number of metric profiles, each of which models a learning activity or a supporting activity that helps facilitate learning.  Each profile provides a domain-specific set of terms for describing common user interactions. . . .

Each Caliper profile is also a unit of certification.  \[TODO\] . . . .

Certain [Event](#event) properties are required and MUST be specified.  Required properties include `id`, `type`, `actor`, `action`, `object` and `eventTime`.  All other [Event](#event) properties are considered optional and need not be referenced.  Adherence to the rules associated with each property referenced is mandatory.  Each [Entity](#entity) participating in the [Event](#event) MUST be expressed either as an object or as a string corresponding to the [Entity](#entity) [IRI](#iriDef).  The `action` vocabulary is limited to the supported actions described in the Caliper specification and no other.  

### <a name="basicProfile"></a>3.1 Basic Profile
 
#### Minimum Conformance
Create and send a *generic* Caliper [Event](#event) to a target endpoint.
 
#### Required action(s)
Any Caliper defined action can be used to describe the interaction.
 
#### Restrictions
* Use of the Basic Profile is limited to describing interactions not modeled in other profiles.  Any events described MUST be expressed using only the [Event](#event) supertype.
* The `type` value must be set to "Event". 
* An [Agent](#Agent) or one of its subtypes MUST be specified as the `actor` of the interaction.  The `actor` value MUST be expressed either as an object or as a string corresponding to the actor's IRI.
* The `action` vocabulary is limited to the supported actions described in the Caliper specification and no other.
* An [Entity](#entity) or one of its subtypes MUST be specified as the `object` of the interaction.  The `object` value MUST be expressed either as an object or as a string corresponding to the object's IRI.

### <a name="annotationProfile"></a>3.2 Annotation Profile
 
#### Minimum Conformance
Create and send a bookmarked [AnnotationEvent](#annotationEvent) to a target endpoint.  All other event types and associated actions included in the profile are considered optional for certification purposes.
    
#### Required action(s)  
[Bookmarked](#bookmarked)
 
#### Restrictions

##### AnnotationEvent Bookmarked
* The `type` value must be set to "AnnotationEvent". 
* A [Person](#person) MUST be specified as the `actor` of the interaction.
* The `action` value MUST be set to "Bookmarked".
* A [DigitalResource](#digitalResource) or one of its subtypes MUST be specified as the `object` of the interaction.
 
### <a name="assessmentProfile"></a>3.3 Assessment Profile
 
#### Minimum Conformance
Create and send both a started and submitted [AssessmentEvent](#assessmentEvent) to a target endpoint.  All other event types and associated actions included in the profile are considered optional for certification purposes.
 
#### Required action(s)  
[Started](#started), [Submitted](#submitted)
 
#### Restrictions

##### AssessmentEvent Started
* The `type` value must be set to "AssessmentEvent". 
* A [Person](#person) MUST be specified as the `actor` of the interaction.
* The `action` value MUST be set to 'Started'.
* An [Assessment](#assessment) MUST be specified as the `object` of the interaction.

##### AssessmentEvent Submitted
* The `type` value must be set to "AssessmentEvent". 
* A [Person](#person) MUST be specified as the `actor` of the interaction.
* The `action` value MUST be set to 'Submitted'.
* An [Assessment](#assessment) MUST be specified as the `object` of the interaction.
 
### <a name="assignableProfile"></a>3.4 Assignable Profile
 
#### Minimum Conformance
Create and send both a started and submitted [AssignableEvent](#assignableEvent) to a target endpoint. All other event types and associated actions included in the profile are considered optional for certification purposes.
 
#### Required action(s)  
[Started](#started), [Submitted](#submitted)
  
#### Assignable Started and Submitted Restrictions

##### AssignableEvent Started
* The `type` value must be set to "AssignableEvent". 
* A [Person](#person) or [Group](#group) MUST be specified as the `actor` of the interaction.
* The `action` value MUST be set to 'Started'.
* A [DigitalResource](#digitalResource) or one of its subtypes MUST be specified as the `object` of the interaction.

##### AssignableEvent Submitted
* The `type` value must be set to "AssignableEvent".
* A [Person](#person) or [Group](#group) MUST be specified as the `actor` of the interaction.
* The `action` value MUST be set to 'Submitted'.
* A [DigitalResource](#digitalResource) or one of its subtypes MUST be specified as the `object` of the interaction.
 
### <a name="forumProfile"></a>3.5 Forum Profile
 
#### Minimum Conformance
Create and send a posted [MessageEvent](#messageEvent) to a target endpoint.  All other event types and associated actions included in the profile are considered optional for certification purposes.
 
#### Required action(s)  
[Posted](#posted)
   
#### Restrictions

##### MessageEvent Posted
* The `type` value must be set to "MessageEvent".
* A [Person](#person) MUST be specified as the `actor` of the interaction.
* The `action` value MUST be set to 'Posted'.
* A [Message](#message) MUST be specified as the `object` of the interaction.
 
### <a name="gradingProfile"></a>3.6 Grading Profile
 
#### Minimum Conformance
Create and send a graded [GradeEvent](#gradeEvent) to a target endpoint.  The [Graded](#graded) action is required and MUST be implemented.
 
#### Required action(s)  
[Graded](#graded)
   
#### Restrictions

##### GradeEvent Graded
* The `type` value must be set to "GradeEvent".
* For auto-graded scenarios the [SoftwareApplication](#softwareApplication) MUST be specified as the `actor`.  Otherwise, a [Person](#person) MUST be specified as the `actor` of the interaction.
* The `action` value MUST be set to 'Graded'.
 
### <a name="mediaProfile"></a>3.7 Media Profile
 
#### Minimum Conformance
Create and send a [MediaEvent](#mediaEvent) to a target endpoint.  All other event types and associated actions included in the profile are considered optional for certification purposes.

#### Required action(s)  
[Started](#started), [Ended](#ended)

#### Restrictions
   
##### MediaEvent Started
* The `type` value must be set to "MediaEvent".
* A [Person](#person) MUST be specified as the `actor` of the interaction.
* The `action` value MUST be set to 'Started'.
* A [MediaObject](#mediaObject) or one of its subtypes MUST be specified as the `object` of the interaction.

##### MediaEvent Ended
* The `type` value must be set to "MediaEvent".
* A [Person](#person) MUST be specified as the `actor` of the interaction.
* The `action` value MUST be set to 'Ended'.
* A [MediaObject](#mediaObject) or one of its subtypes MUST be specified as the `object` of the interaction.
 
### <a name="readingProfile"></a>3.8 Reading Profile
 
#### Minimum Conformance
 Create and send both a navigatedTo [NavigationEvent](#navigationEvent) and a viewed [ViewEvent](#viewEvent) to a target endpoint.
 
 #### Required action(s)  
 [NavigatedTo](#navigatedTo) ([NavigationEvent](#navigationEvent)), [Viewed](#viewed) ([ViewEvent](#viewEvent))
 
#### Restrictions
 
##### NavigationEvent NavigatedTo
* The `type` value must be set to "NavigationEvent".
* A [Person](#person) MUST be specified as the `actor` of the interaction.
* The `action` value MUST be set to 'NavigatedTo'.
* A [DigitalResource](#digitalResource) or one of its subtypes MUST be specified as the `object` of the interaction.
 
##### ViewedEvent Viewed
* The `type` value must be set to "ViewEvent".
* A [Person](#person) MUST be specified as the `actor` of the interaction.
* The `action` value MUST be set to 'Viewed'.
* A [DigitalResource](#digitalResource) or one of its subtypes MUST be specified as the `object` of the interaction.
 
### <a name="sessionProfile"></a>3.9 Session Profile
 
#### Minimum Conformance
Create and send a logged in [SessionEvent](#sessionEvent) to a target endpoint. All associated actions included in the profile are considered optional for certification purposes.
 
#### Required action(s) 
[LoggedIn](#loggedIn)
 
#### Restrictions
 
##### SessionEvent LoggedIn
* The `type` value must be set to "SessionEvent".
* A [Person](#person) MUST be specified as the `actor` of the interaction.
* The `action` value MUST be set to 'LoggedIn'.
* A [SoftwareApplication](#softwareApplication) MUST be specified as the `object` of the interaction.
 
### <a name="toolUseProfile"></a>3.10 Tool Use Profile
 
#### Minimum Conformance
Create and send a used [ToolUseEvent](#toolUseEvent) to a target endpoint.
 
#### Required action(s) 
[Used](#used) action
 
#### Restrictions
 
##### ToolUseEvent Used
* The `type` value must be set to "ToolUseEvent".
* A [Person](#person) MUST be specified as the `actor` of the interaction.
* The `action` value MUST be set to 'Used'.
* A [SoftwareApplication](#softwareApplication) MUST be specified as the `object` of the interaction.

## <a name="dataFormat"></a>4.0 Data Interchange Format

\[TODO\] General intro OR MERGE 4.1 into 4.0

### <a name="jsonld"></a>4.1 JSON-LD
Caliper events and entities are serialized as [JSON-LD](#jsonldDef), a JSON-based data interchange format that encourages use of shared vocabularies and discoverable key:value identifiers when constructing JSON documents.

#### <a name="jsonldContext"></a>4.1.1 The Context
[JSON-LD](#jsonldDef) documents require inclusion of a *context*, denoted by the `@context` keyword, a property employed to map document [Terms](#termDef) to [IRIs](#iriDef).  [JSON-LD](#jsonldDef) contexts can be embedded inline or referenced externally in a document.  Inclusion of a [JSON-LD](#jsonldDef) context provides an economical way for Caliper to communicate document semantics to services interested in consuming Caliper event data.

IMS Global provides a remote Caliper 1.1 JSON-LD [context](http://purl.imsglobal.org/ctx/caliper/v1p1) document for mapping Caliper [Terms](#termDef) to [IRIs](#iriDef).  ~~Both node types and typed values are specified in the external IMS Caliper JSON-LD [context](http://purl.imsglobal.org/ctx/caliper/v1p1).~~  Implementers are encouraged to familiarize themselves with the term definitions described therein.

Caliper consumers require accurate [JSON-LD](#jsonldDef) context definitions to be capable of interpreting coerced values.  For Caliper defined [Terms](#terms) implementers need only reference the external IMS Caliper JSON-LD [context](http://purl.imsglobal.org/ctx/caliper/v1p1) in their [Event](#event) or [Entity](#entity) *[describe](#describeDef)* [JSON-LD](#jsonldDef) documents in order to link to the associated term definitions.  A Caliper [Event](#event) or [Entity](#entity) containing coerced values that do not map to an explicit context declaration will be considered nonconformant.

##### Requirements
* Each Caliper [Event](#event) and [Entity](#entity) *[describe](#describeDef)* document generated by a [Sensor](#sensor) MUST be provisioned with a [JSON-LD](#jsonldDef) `@context` defined as a property of the top-level object.  
* The `@context` property type MAY be defined as a JSON string, object or array.
  * If the top-level `@context` value is defined as a string it MUST be set to the Caliper remote context "http://purl.imsglobal.org/ctx/caliper/v1p1".
  * If the top-level `@context` value is defined inline as an object it MUST . . . . \[REVIEW\] FOR TOP-LEVEL CONTEXTS BAN "@context": {inline} ?
  * If the top-level `@context` value is defined as an array of multiple contexts, the remote Caliper [JSON-LD](#jsonldDef) context MUST be listed last in order to ensure that Caliper terms retain their primacy given that [JSON-LD](#jsonldDef) parsers rely on a "most-recently-defined-wins" approach when evaluating duplicate terms.
* Additional remote or inline _local_ contexts may be referenced any time a JSON object is defined in order to ascribe meaning to terms not described by the model.  Duplicate context references SHOULD be avoided.  Moreover nested _local_ contexts that are added to the _active_ context at processing time MUST NOT override Caliper terms defined by the top-level context.

#### <a name="jsonldNodeIdentifiers"></a>4.1.2 Node Identifiers
Caliper specifies the use of [IRIs](#iriDef) for identifying nodes (i.e., the things being described) and their attributes.  [IRIs](#iriDef) may be represented ~~either~~ as an absolute IRI containing a scheme, path and optional query and fragment segments ~~or as a relative IRI minus the scheme and/or domain that is resolved relative to a base IRI defined in a JSON-LD context.~~  If an [IRI](#iriDef) is deemed inappropriate for the resource a [blank node](#blankNodeDef) identifier may be assigned.

#### <a name="jsonldTypeCoercion"></a>4.1.3 Type Coercion
Caliper permits certain [Event](#event) and [Entity](#entity) property values to be expressed either as a JSON object or a string corresponding to the object's [IRI](#iriDef).  [JSON-LD](#jsonldDef) also supports the _coercion_ of data values to specified types based on value type mappings defined in a [JSON-LD](#jsonldDef) context.  In a [JSON-LD](#jsonldDef) context term definition the keywords `@id` or `@vocab` may be assigned as a value in order to signal to a [JSON-LD](#jsonldDef) parser that if the value is set to a string it is to be interpreted as an [IRI](#iriDef).  Type coercion of this sort provides representational flexibility that implementers are encouraged to leverage.

#### <a name="jsonldEvents"></a>4.1.4 Expressing Events as JSON-LD

* _Expressed as an object_:
  * A [JSON-LD](#jsonldDef) `@context` must be defined as outlined above in section \[TODO\] X.  
  * The `id` property MUST be assigned a 128-bit long universally unique identifier (UUID) formatted as a [URN](#urnDef) per [RFC 4122](#rfc4122), which describes a [URN](#urnDef) namespace for [UUIDs](#uuidDef). 
  * The `type` value MUST be set to the relevant Caliper term (e.g., "NavigationEvent").
  * The `actor` value MUST be set to [Agent](#agent) or one of its subtypes (e.g., [Person](#person)).  The `actor` value MUST be expressed either as an object or as a string corresponding to the actor's IRI. 
  * The `action` value MUST be set to the relevant action term (e.g., "NavigatedTo") specified by the governing Metric Profile. 
  * The `object` value MUST be set to . . . \[TODO\]  The `object` value MUST be expressed either as an object or as a string corresponding to the object's IRI.
  * An `eventTime` MUST be specified.  The value MUST be expressed as an ISO 8601 date and time value expressed with millisecond precision using the format YYYY-MM-DDTHH:mm:ss.SSSZ set to UTC with no offset specified.
  * All other [Event](#event) properties are considered optional.
  
#### <a name="jsonldEntities"></a>4.1.5 Expressing Entities as JSON-LD

* _Expressed as an object_:
  * If generated as a *[describe](#describeDef)* a [JSON-LD](#jsonldDef) `@context` must be defined as outlined above in section \[TODO\] X.  Otherwise, omit the otherwise duplicate `@context` property for Caliper entities participating in an [Event](#event) . . . [JSON-LD](#jsonldDef) inheritance rules.
  * The 'id' property MUST be assigned a valid [IRI](#iriDef) or a blank node identifier. The [IRI](#iriDef) MUST be unique and persistent.  The [IRI](#iriDef) SHOULD be dereferenceable; i.e., capable of returning a representation of the [Entity](#entity).  A [URI](#uriDef) employing the [URN](#urnDef) scheme MAY be provided although care should be taken when employing a location-independent identifier since it precludes the possibility of utilizing it to retrieve machine-readable data.
  * The `type` value MUST be set to the relevant Caliper term (e.g., "DigitalResource").
  * All other [Event](#event) properties are considered optional.
* _Expressed as a string_:
  * The value must be set to the entity's [IRI](#iriDef).
  
## <a name="transportConformance"></a>5.0 Transport Conformance
 
\[TODO\] Summarize transport options . . . .
 
### <a name="http"></a>5.1 HTTP Transport Requirements
 
A Caliper sensor utilizing the Hypertext Transport Protocol (HTTP) request-response messaging protocol MUST demonstrate that is capable of communicating with the Caliper certification service over HTTP with the connection encrypted by Transport Layer Security (TLS).  A Caliper sensor MUST also support message authentication using the HTTP `Authorization` request header as described in [RFC 6750](#rfc6750), [Section 2.1](https://tools.ietf.org/html/rfc6750#section-2).
 
A Caliper sensor certifying over HTTP MUST be capable of serializing and sending Caliper data using a Caliper [Envelope](#envelope), a JSON data structure that includes metadata about the emitting service as well as a `data` array property for holding the Caliper [Event](#event) and [Entity](#entity) payload.  Caliper [Event](#event) and [Entity](#entity) data MUST be transmitted as envelope `data` array values.  Each [Event](#event) and [Entity](#entity) included in the envelope MUST be expressed as JSON-LD.
 
#### <a name="httpRequest"></a>5.1.1  HTTP Message Requests
 
Each HTTP message sent to the Certification service MUST consist of a single serialized JSON representation of a Caliper [Envelope](#envelope).  Messages MUST be sent using the POST request method.
 
The following standard HTTP request headers MUST be set for each message sent to the certification service [Endpoint](#endpoint):
  
| Request Header | Requirements |
| :------------- | :----------- |
| Accept | \[TODO\] . . . |
| Authorization | The HTTP request header `Authorization` value MUST be set to the Bearer Token provided by the certification service and associated with the test endpoint. |
| Content-Type | The HTTP request header `Content-Type` value MUST be set to the IANA media type "application/json". |
| Host | \[TODO\] . . . |
 
#### <a name="httpResponse"></a>5.1.2  HTTP Message Responses
 
When communicating over HTTP the certification service endpoint will exhibit the following response behavior:
  
* To signal to a Caliper sensor that it has successfully received a message the certification service endpoint will reply with a `2xx` class status code.  The body of a successful response will be empty.
* If a Caliper sensor sends a message containing events and or entities without an enclosing [Envelope](#envelope), the certification service will reply with a `400 Bad Request` response.
* If a Caliper sensor sends a malformed Caliper endpoint (it does not contain `sensor`, `sendTime`, `dataVersion` and `data` properties of the required form), the certification service will reply with a `400 Bad Request` response.
* If a Caliper sensor sends a message without an `Authorization` request header of the RECOMMENDED form or sends a token credential that the certification service is unable to either validate or determine has sufficient privileges to submit Caliper data, the certification service will reply with a `401 Unauthorized` response.
* If a Caliper sensor sends a message with a `Content-Type` other than "application/json", the certification service will reply with a `415 Unsupported Media Type` response.
* If a Caliper sensor sends a message with an [Envelope](#envelope) that contains a `dataVersion` value that the endpoint cannot support the certification service will reply with a `422 Unprocessable Entity` response.
  
The certification service MAY respond to Caliper sensor messages with other standard HTTP status codes to indicate result dispositions of varying kinds.  The certification service MAY also communicate more detailed information about problem states, using the standard method for reporting problem details described in [RFC 7807](#rfc7807).
  
### <a name="mqtt"></a>5.2 MQTT Transport Requirements

A Caliper sensor utilizing the Message Queue Telemetry Transport (MQTT) publish-subscribe messaging protocol MUST demonstrate . . . .

\[TODO\] . . . .

## <a name="usingCertService"></a> 6.0 Using the Certification Service
 
Visit the Caliper Certification service at [https://www.imsglobal.org/sso/launch.php/caliper](https://www.imsglobal.org/sso/launch.php/caliper).  You MUST be logged in to the IMS Global website to access the Caliper certification service.  If you do not have an account, please register at [https://www.imsglobal.org/user/register](https://www.imsglobal.org/user/register).
 
The certification service provides a playground for testing your Caliper messages.  Click the "Start Testing" link under __Test Your Product__ to access the playground.

Once you are ready to commence certification testing, click the "Certify Your Product" link under __Certify Your Product__ to commence testing.  
 
The following steps will guide you through the process.  
 
1. Complete the online form by providing the following information:
 
   * Product name
   * Product version
   * Product URL
   * Product description
   * Caliper specification version
   * Member name
   * Member email address
   * Member organization
 
2. Click the green "Start Certification" button.  To terminate testing click the white "Cancel" button.
 
3. Follow the onscreen instructions to run the tests.  Configure the host software to send Caliper messages to the test endpoint URL provided onscreen.
 
4. Initiate test.  Send messages to the certification service endpoint.  
 
5. When messages are received by the Certification service the online directions will be replaced with a view displaying conformance progress. 
 
 \[TODO\] Continue describing steps . . .

## <a name="certMark"></a>7.0 Certification Mark

After submitting your successful conformance information and receiving confirmation and a registration number from IMS Global you may then apply the appropriate conformance mark. The IMS Global conformance chart will list your conformance details. If you have any questions, please feel free to contact us at any point.  Products without an IMS conformance registration number are not considered compliant by IMS Global.

## <a name="certRenewal"></a>8.0 Certification Expiration and Renewal

Caliper certification covers individual metric profiles only and is scoped to the specific version of the Caliper specification tested.  Major or minor releases of the Caliper specification and/or associated metric profiles will require recertification of your upgraded platform, application or service. All IMS Certifications require that you renew and retest your certification after one year.

## <a name="appendixA"></a>Appendix A. Example Messages

\[TODO\] Add examples

### Example: DUMP IN FAVOR OR REFERRING TO SPEC EXAMPLE
```
{
  "@context": {
    "id": "@id",
    "type": "@type",
    "caliper": "http://purl.imsglobal.org/caliper/",
    . . .
    "actor": {"@id": "caliper:actor", "@type": "@id"},
    . . .
    "action": {"@id": "caliper:action","@type": "@vocab"},
    . . .
  }
}
```

### Example: Using curl to post a Caliper Envelope Test Fixture to the Certification Service over HTTP
```text
curl --request POST \
--url https://caliper.imsglobal.org/caliper/2c925d43-7707-4fd7-ac87-c814afbe5621/message \
--header 'Accept: application/json' \ 
--header 'Authorization: Bearer 2c925d43-7707-4fd7-ac87-c814afbe5621' \
--header 'Content-Type: application/json' \
--data @caliperEnvelopeToolUseEvent.json
``` 

## <a name="contributors"></a>Contributors

The following Caliper Working Group participants contributed to the writing of this guide:

#### Authors

| Name | Organization |
| :--- | :----------- |
| Anthony Whyte | University of Michigan |
| Markus Gylling | IMS Global |
| Lisa Mattson | IMS Global |

## <a name="references"></a>References

<a name="caliperSpec"></a>Anthony Whyte, Viktor Haag, Linda Feng, Matt Ashbourne, Wes LaMarche and Etienne Pelaprat.  Caliper Analytics® Specification, version 1.1.  30 November 2017.  URL: \[TODO\] . . . .

<a name="jsonldSyntax"></a>__JSON-LD Syntax__.  W3C.  M. Sporny, D. Longley, G. Kellog, M. Lanthaler and N. Lindström.  JSON-LD 1.1.  A JSON-based Serialization for Linked Data. 15 February 2017.  URL: http://json-ld.org/spec/latest/json-ld/

<a name="jsonldProcessing"></a>__JSON-LD Processing__.  D. Longley, G. Kellog, M. Lanthaler, M. Sporny.  JSON-LD 1.1 Processing Algorithms and API.  15 February 2017.  URL: http://json-ld.org/spec/latest/json-ld-api/

<a name="linkedData"></a>__Linked Data__.  Tim Berners-Lee.  "Linked Data."  W3C internal document.  July 2006, rev. June 2009.  URL: https://www.w3.org/DesignIssues/LinkedData.html

<a name="rfc2119"></a>__RFC 2119__.  IETF.  S. Bradner.  "Key words for use in RFCs to Indicate Requirement Levels."  March 1997.  URL: https://tools.ietf.org/html/rfc2119

<a name="rfc2396"></a>__RFC 2396__.  IETF.  T. Berners-Lee, R. Fielding, L. Masinter.  "Uniform Resource Identifiers (URI): Generic Syntax."  August 1998.  URL: https://www.ietf.org/rfc/rfc2396.txt

<a name="rfc3987"></a>__RFC 3987__.  IETF. M. Duerst and M. Suignard.  "Internationalized Resource Identifiers (IRIs).  January 2005.  URL: https://www.ietf.org/rfc/rfc3987.txt

<a name="rfc4122"></a>__RFC 4122__.  IETF. P. Leach, M. Mealling and R. Salz.  "A Universally Unique Identifier (UUID) URN Namespace."  July 2005.  URL: https://tools.ietf.org/html/rfc4122

<a name="rfc6750"></a>__RFC 6750__.  IETF.  M. Jones and D. Hardt.  "The OAuth 2.0 Authorization Framework: Bearer Token Usage."  October 2012.  URL: https://tools.ietf.org/html/rfc6750

<a name="rfc7807"></a>__RFC 7807__.  IETF.  M. Nottingham, E. Wilde.  "Problem Details for HTTP APIs."  March 2017.  URL: https://tools.ietf.org/html/rfc7807

## <a name="aboutThisDoc"></a>About this Document

IMS Global Learning Consortium, Inc. ("IMS Global") is publishing the information contained in this document ("Guide") for purposes of scientific, experimental, and scholarly collaboration only.

IMS Global makes no warranty or representation regarding the accuracy or completeness of the Guide.

This material is provided on an "As Is" and "As Available" basis.

The Guide is at all times subject to change and revision without notice.

It is your sole responsibility to evaluate the usefulness, accuracy, and completeness of the Guide as it relates to you.

IMS Global would appreciate receiving your comments and suggestions.

Please contact IMS Global through our website at [http://www.imsglobal.org](http://www.imsglobal.org).

Please refer to Document Name: IMS Caliper Analytics&reg; Certification Guide, version 1.1

Date: 30 November 2017

This document contains trademarks of the IMS Global Learning Consortium including the IMS Logos, Learning Tools Interoperability&reg; (LTI&reg;), Accessible Portable Item Protocol&reg; (APIP&reg;), Question and Test Interoperability&reg; (QTI&reg;), Common Cartridge&reg; (CC&reg;), AccessForAll&trade;, OneRoster&reg;, Caliper Analytics&reg; and SensorAPI&trade;. For more information on the IMS trademark usage policy see trademark policy page - https://www.imsglobal.org/trademarks
