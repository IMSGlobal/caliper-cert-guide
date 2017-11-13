<div style="design: block;margin: 0 auto"><img class="img-responsive" alt="IMS Global Learning Consortium, Inc. Logo" src="assets/ims-logo-h170w600.png"></div>

# IMS Global Learning Consortium, Inc.

# Caliper Analytics&reg; Certification Guide, version 1.1

## IPR and Distribution Notices

Recipients of this document are requested to submit, with their comments, notification of any relevant patent claims or other intellectual property rights of which they may be aware that might be infringed by any implementation of the specification set forth in this document, and to provide supporting documentation.

IMS takes no position regarding the validity or scope of any intellectual property or other rights that might be claimed to pertain to the implementation or use of the technology described in this document or the extent to which any license under such rights might or might not be available; neither does it represent that it has made any effort to identify any such rights. Information on IMS’s procedures with respect to rights in IMS specifications can be found at the IMS Intellectual Property Rights web page: [http://www.imsglobal.org/ipr/imsipr_policyFinal.pdf](http://www.imsglobal.org/ipr/imsipr_policyFinal.pdf).

Copyright &copy; 2017 IMS Global Learning Consortium. All Rights Reserved.

Use of this specification to develop products or services is governed by the license with IMS found on the IMS website: http://www.imsglobal.org/speclicense.html.

Permission is granted to all parties to use excerpts from this document as needed in producing requests for proposals.

The limited permissions granted above are perpetual and will not be revoked by IMS or its successors or assigns.

THIS SPECIFICATION IS BEING OFFERED WITHOUT ANY WARRANTY WHATSOEVER, AND IN PARTICULAR, ANY WARRANTY OF NON INFRINGEMENT IS EXPRESSLY DISCLAIMED. ANY USE OF THIS SPECIFICATION SHALL BE MADE ENTIRELY AT THE IMPLEMENTER'S OWN RISK, AND NEITHER THE CONSORTIUM, NOR ANY OF ITS MEMBERS OR SUBMITTERS, SHALL HAVE ANY LIABILITY WHATSOEVER TO ANY IMPLEMENTER OR THIRD PARTY FOR ANY DAMAGES OF ANY NATURE WHATSOEVER, DIRECTLY OR INDIRECTLY, ARISING FROM THE USE OF THIS SPECIFICATION.

## Table of Contents

* 1.0 [Introduction](#introduction)
  * 1.1 [Status of this Document](#docStatus)
  * 1.2 [Conventions](#conventions)
  * 1.3 [Terminology](#terminology)
* 2.0 [The Certification Process](#certProcess)
  * 2.1 [Certification Testing Process](#certTestingProcess)
  * 2.1.1 [Requirements for Caliper 1.1 Certification](#certRequirements)
  * 2.1.2 [Caliper Certification Mark](#certMark)
* 3.0 [Metric Profile Certification](#profileCert)
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
* 4.0 [Service Provider Sensor Conformance](#sensor)
* 5.0 [Transport Conformance](#transport)
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

<a name="actionDef"></a>__Action__: something performed or done to accomplish a purpose.  Caliper [Event](#event) subtypes define a controlled vocabulary of one or more [actions](#actions) relevant to the activity domain.  A Caliper [Event](#event) includes an `action` attribute for expressing the associated action.     

<a name="contextDef"></a>__Context__: a special [JSON-LD](http://json-ld.org/spec/latest/json-ld/) keyword that maps the terms employed in a JSON document to [IRIs](https://www.ietf.org/rfc/rfc3987.txt) that link to one or more published vocabularies.  Inclusion of a [JSON-LD](http://json-ld.org/spec/latest/json-ld/) context provides an economical way of communicating document semantics to services interested in consuming Caliper event data.

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

<a name="pii"></a>__Personally Identifiable Information (PII)__: information that can be used on its own, or with other information, to identify, locate, or contact a single person, or to identify a single person within a context.

<a name="sensorDef"></a>__Sensor__: Software assets deployed within a learning application that implement the [Sensor API&trade;](#sensorAPIDef) for marshalling and transmitting Caliper data to a target endpoint.

<a name="sensorAPIDef"></a>__Sensor API&trade;__: The standard set of methods and supported parameters that a [Sensor](#sensorDef) implements according to this specification in order to transmit Caliper data in an interoperable way.

<a name="termDef"></a>__Term__: a word or short expression that expands to an [IRI](#iriDef) when mapped to a JSON-LD [context](#contextDef) document. Terms are employed by Caliper as `type` property string values in order to distinguish between various JSON representations of entities and events defined by the Caliper information model.

<a name="typeCoercionDef"></a>__Type Coercion__: the process of coercing values to a particular data type.

<a name="uriDef"></a>__URI__:  the Uniform Resource Identifier ([URI](#uriDef)) utilizes the US-ASCII character set to identify a resource.  Per [RFC 2396](#rfc2396) a [URI](#uriDef) "can be further classified as a locator, a name or both."  Both the Uniform Resource Locator ([URL](#urlDef)) and the Uniform Resource Name ([URN](#urnDef)) are considered subspaces of the more general [URI](#uriDef) space. 

<a name="urlDef"></a>__URL__: A Uniform Resource Locator ([URL](#urlDef)) is a type of [URI](#uriDef) that provides a reference to resource that specifies both its location and a means of retrieving a representation of it.  An HTTP [URI](#uriDef) is a [URL](#urlDef) and using HTTP IRIs/URIs to identify things is fundamental to Linked Data.

<a name="urnDef"></a>__URN__: A Uniform Resource Name ([URN](#urnDef)) is a type of [URI](#uriDef) that provides a persistent identifier for a resource that is bound to a defined namespace.  Unlike a [URL](#urlDef) a [URN](#urnDef) is location-independent and provides no means of accessing a representation of the named resource.  

<a name="uuidDef"></a>__UUID__: a 128-bit identifier that does not require a registration authority to assure uniqueness.  However, absolute uniqueness is not guaranteed although the collision probability is considered extremely low. Caliper recommends use of randomly or pseudo-randomly generated version 4 UUIDs.  Each Caliper [Event](#event) MUST be assigned a UUID that is expressed as a [URN](#urnDef) using the form `urn:uuid:<UUID>` as described in [RFC 4122](#rfc4122).

## 2.0 <a name="certProcess"></a>The Certification Process

### 2.1 <a name="certMark"></a>Conformance Testing Process

The process for conformance testing implementations of OneRoster includes the following:

1. Visit the Caliper Certification Suite at [https://www.imsglobal.org/sso/launch.php/caliper](https://www.imsglobal.org/sso/launch.php/caliper)
  
2. Follow the onscreen instructions to run the tests.  

3. Once the test has been successfully run, submit a print out of the test results along with the following information to [conformance@imsglobal.org](#mailto:conformance@imsglobal.org):

  * Your name
  * Your organization
  * Your product name and version
  * Date the product was tested
  * Whether you are a Service/Data Provider or a Service/Data Consumer
  * ~~Whether you support the REST version~~
  * The optional features supported by your system (these must also have been subjected to conformance testing).

All Tests must be passed successfully to be considered IMS compliant.

#### <a name="certMark"></a>2.1.2 Caliper Certification Mark

After you have submitted your successful conformance information to [conformance@imsglobal.org](#mailto:conformance@imsglobal.org) and received confirmation and a registration number from IMS Global you may then apply the appropriate conformance mark. The IMS Global conformance chart will list your conformance details. If you have any questions, please feel free to contact us at any point.  Products without an IMS conformance registration number are not considered compliant by IMS Global.

## <a name="profileCert"></a>3.0 Metric Profile Certification

As described more fully in the [Caliper 1.1 specification](#caliperSpec) the Caliper information model defines a number of metric profiles, each of which models a learning activity or a supporting activity that helps facilitate learning.  Each profile provides a domain-specific set of terms for describing common user interactions.

Each Caliper profile is also a unit of certification.  \[TODO\] . . . .

### General considerations

\[TODO\] . . .

* Certain [Event](#event) properties are required and MUST be specified.  Required properties include `id`, `type`, `actor`, `action`, `object` and `eventTime`.  All other [Event](#event) properties are considered optional and need not be referenced.  Adherence to the rules associated with each property referenced is mandatory.  
* Each [Entity](#entity) participating in the [Event](#event) MUST be expressed either as an object or as a string corresponding to the [Entity](#entity) [IRI](#iriDef).
* The `action` vocabulary is limited to the supported actions described in the Caliper specification and no other.

 ### <a name="basicProfile"></a>3.1 Basic Profile
 
 #### Minimum Conformance
 Create and send a *generic* Caliper [Event](#event) to a target [Endpoint](#endpoint).  At least one Caliper [action](#actions) MUST be implemented.
 
 #### Requirements
 * Use of the Basic Profile is limited to describing interactions not modeled in other profiles.  Only generic events are allowed to be described using the [Event](#event) supertype.
 
 ### <a name="annotationProfile"></a>3.2 Annotation Profile
 
 #### Minimum Conformance
  Create and send an [AnnotationEvent](#annotationEvent) to a target [Endpoint](#endpoint).  The [Bookmarked](#bookmarked) action is required and MUST be implemented.  All other supported actions are considered optional.
 
 #### Additional Requirements  
 * A [Person](#person) MUST be specified as the `actor` of the interaction.
 * The `generated` [Annotation](#annotation) SHOULD be specified.  If expressed as an object both the `annotator` and `annotated` [DigitalResource](#digitalResource) SHOULD be referenced.
 
 ### <a name="assessmentProfile"></a>3.3 Assessment Profile
 
 #### Minimum Conformance
 Create and send an [AssessmentEvent](#assessmentEvent) to a target [Endpoint](#endpoint).  The [Started](#started) and [Submitted](#submitted) actions are required and MUST be implemented.  All other supported events and actions are considered optional.
 
 #### Additional Requirements
 * A [Person](#person) MUST be specified as the `actor` of the interaction.
 * The [Attempt](#attempt) SHOULD reference both the `assignee` and the assigned [Assessment](#assessment) or [AssessmentItem](#assessmentItem).
 * For a [Started](#started) action, the learner's `generated` [Attempt](#attempt) SHOULD be specified.  If the [Attempt](#attempt) is included, it  MUST be specified with the `count` value set to 1 for a first attempt and incremented by 1 for each subsequent attempt.
 * For a [Paused](#paused), [Resumed](#resumed) and [Reset](#reset) actions, if the [Attempt](#attempt) is specified, the current `count` value MUST NOT be changed.
 * For a [Restarted](#restarted) action, if the [Attempt](#attempt) is specified, the `count` value MUST be incremented by 1.
 * Parent-child relationships that exist between [AssessmentItem](#assessmentItem) and [Assessment](#assessment) attempts MAY be represented via the [Attempt](#attempt) `isPartOf` property.
 * For a [Completed](#completed) action, the learner's `generated` [Response](#response) MAY be specified.  The [Response](#response) SHOULD reference the associated `attempt`.
 * When navigating to an [Assessment](#assessment) the [DigitalResource](#digitalResource) or [SoftwareApplication](#softwareApplication) that constitutes the referring context MAY be specified as the `referrer`.  For an [AssessmentItemEvent](#assessmentItemEvent) the prior [AssessmentItem](#assessmentItem), if known, MAY be specified as the `referrer`.
 
 ### <a name="assignableProfile"></a>3.4 Assignable Profile
 
 #### Minimum Conformance
 Create and send an [AssignableEvent](#assignableEvent) to a target [Endpoint](#endpoint). The [Started](#started) and [Submitted](#submitted) actions are required and MUST be implemented.  The [Completed](#completed) action SHOULD be implemented.  All other supported events and actions are considered optional.
 
 #### Additional Requirements  
 * For the [AssignableEvent](#assignableEvent), a [Person](#person) or [Group](#group) MUST be specified as the `actor` of the interaction; For the [NavigationEvent](#navigationEvent) and [ViewEvent](#navigationEvent), a [Person](#person) MUST be specified as the `actor` of the interaction.
 * The [Attempt](#attempt) SHOULD reference both the `assignee` and the assigned  [AssignableDigitalResource](#assignableDigitalResource).
 * For a [Started](#started) action, the learner's `generated` [Attempt](#attempt) SHOULD be specified.  If the [Attempt](#attempt) is included, it  MUST be specified with the `count` value set to 1 for a first attempt and incremented by 1 for each subsequent attempt.
 * For a [Paused](#paused), [Resumed](#resumed) and [Reset](#reset) actions, if the [Attempt](#attempt) is specified, the current `count` value MUST NOT be changed.
 * For a [Restarted](#restarted) action, if the [Attempt](#attempt) is specified, the `count` value MUST be incremented by 1.
 * Parent-child relationships that exist between [AssignableDigitalResource](#assignableDigitalResource) attempts MAY be represented via the [Attempt](#attempt) `isPartOf` property.
 * For [Completed](#completed) actions, the learner's `generated` [Response](#response) MAY be specified.  The [Response](#response) SHOULD reference the associated `attempt`.
 * Parent-child relationships that exist between [DigitalResource](#digitalResource) attempts MAY be represented via the [Attempt](#attempt) `isPartOf` property.
 * For [Completed](#completed) actions, the learner's `generated` [Response](#response) MAY be specified.  The [Response](#response) SHOULD reference the associated `attempt`.
 * When navigating to a [AssignableDigitalResource](#assignableDigitalResource) the [DigitalResource](#digitalResource) or [SoftwareApplication](#softwareApplication) that constitutes the referring context MAY be specified as the `referrer`.
 
 ### <a name="forumProfile"></a>3.5 Forum Profile
 
 #### Minimum Conformance
 Create and send a [MessageEvent](#messageEvent) to a target [Endpoint](#endpoint). The [Posted](#posted) action is required and MUST be implemented.  All other supported events and actions are considered optional. 
 
 #### Additional Requirements  
 * A [Person](#person) MUST be specified as the `actor` of the interaction.
 * When the [Message](#message) is in the form of a reply, the prior [Message](#message) that prompted the reply SHOULD be referenced via the [Message](#message) `replyTo` property.
 * Parent-child relationships that exist between a [Message](#message), [Thread](#thread) and a [Forum](#forum) MAY be represented by use of the `isPartOf` property.
 * When navigating to a [Forum](#forum), [Thread](#thread) or [Message](#message) the [DigitalResource](#digitalResource) or [SoftwareApplication](#softwareApplication) that constitutes the referring context MAY be specified as the `referrer`.
 
 ### <a name="gradingProfile"></a>3.6 Grading Profile
 
 #### Minimum Conformance
 Create and send a Caliper [GradeEvent](#gradeEvent) to a target [Endpoint](#endpoint).  The [Graded](#graded) action is required and MUST be implemented.
 
 #### Additional Requirements
 * For auto-graded scenarios the [SoftwareApplication](#softwareApplication) MUST be specified as the `actor`.
 * For a [Graded](#graded) action, the `generated` [Score](#score) SHOULD be specified.
 
 ### <a name="mediaProfile"></a>3.7 Media Profile
 
 #### Minimum Conformance
 Create and send a [MediaEvent](#mediaEvent) to a target endpoint. The [Started](#started) and [Ended](#ended) actions are required and MUST be implemented.  The [Paused](#paused), [Resumed](#resumed) and [Restarted](#restarted) actions SHOULD be implemented.  All other supported events and actions are considered optional.
 
 #### Additional Requirements  
 * A [Person](#person) MUST be specified as the `actor` of the interaction.
 * A [MediaObject](#mediaObject) or one of its subtypes MUST be specified as the `object` of the interaction.
 * A [MediaLocation](#mediaLocation) MAY be specified as the `target` in order to indicate the current location in an audio or video stream.
 * For a [Started](#started) or [Restarted](#restarted) action, the [MediaLocation](#mediaLocation) `currentTime` value MUST be set to the beginning or initial starting location in the audio or video stream.
 * For a [Paused](#paused) action the [MediaLocation](#mediaLocation) `currentTime` value MUST be set to the location in the audio or video stream where the pause occurred.
 * For a [Resumed](#resumed) action the [MediaLocation](#mediaLocation) `currentTime` value MUST be set to the location in the audio or video stream where the previous pause occurred.
 * For a [Ended](#ended) action the [MediaLocation](#mediaLocation) `currentTime` value MUST be set to the ending or closing location in the audio or video stream.
 * For other [MediaEvent](#mediaEvent) supported actions the [MediaLocation](#mediaLocation) `currentTime` value MUST be set to the location in the audio or video stream where the action occurred.
 * When navigating to a [MediaObject](#mediaObject) the [DigitalResource](#digitalResource) or [SoftwareApplication](#softwareApplication) that constitutes the referring context MAY be specified as the `referrer`.
 
 ### <a name="readingProfile"></a>3.8 Reading Profile
 
 #### Minimum Conformance
 Create and send a [NavigationEvent](#navigationEvent) and a [ViewEvent](#viewEvent) to a target [Endpoint](#endpoint).  The [NavigatedTo](#navigatedTo) and [Viewed](#viewed) actions are required and MUST be implemented.
 
 #### Additional Requirements  
 * A [Person](#person) MUST be specified as the `actor` of the interaction.
 * A [DigitalResource](#digitalResource) or one of its subtypes MUST be specified as the `object` of the interaction.
 * A [Frame](#frame) MAY be specified as the `target` in order to indicate an indexed segment or location.
 * When navigating to digital content the [DigitalResource](#digitalResource) or [SoftwareApplication](#softwareApplication) that constitutes the referring context MAY be specified as the `referrer`.
 
 ### <a name="sessionProfile"></a>3.9 Session Profile
 
 #### Minimum Conformance
 Create and send a [SessionEvent](#sessionEvent) to a target [Endpoint](#endpoint). The [LoggedIn](#loggedIn) action is required and MUST be implemented.
 
 #### Requirements  
 * Although optional, the relevant user `session` SHOULD be specified.
 * For a [LoggedIn](#loggedIn) action, if the `actor` is attempting to access a particular [DigitalResource](#digitalResource) the resource MAY be designated as the `target` of the interaction.
 
 ### <a name="toolUseProfile"></a>3.10 Tool Use Profile
 
 #### Minimum Conformance
 Create and send a Caliper [ToolUseEvent](#toolUseEvent) to a target [Endpoint](#endpoint).  The [Used](#used) action is required and MUST be implemented.
 
 #### Additional Requirements
 * A [Person](#person) MUST be specified as the `actor` of the interaction.
 * A [SoftwareApplication](#softwareApplication) MUST be specified as the `object` of the interaction.

## <a name="sensor"></a>4.0 Service Provider Sensor Conformance

A Caliper service provider MUST be capable of serializing and sending a Caliper [Envelope](#envelope) containing the following payloads to the designated certification suite [Endpoint](#endpoint).

* A JSON array consisting of one or more Caliper [Event](#event) documents, each expressed as [JSON-LD](#jsonldDef).
* A JSON array consisting of one or more Caliper [Entity](#entity) "describe" documents, each expressed as [JSON-LD](#jsonldDef).
* A JSON array consisting of a mix of one or more Caliper [Event](#event) and [Entity](#entity) describe documents, each expressed as [JSON-LD](#jsonldDef). 

## <a name="transport"></a>5.0 Transport Conformance ~~/ REST-base Exchange Conformance~~

Business requirements informed by industry best practices will determine the choice of transport protocol for Caliper [Sensor](#sensor) and [Endpoint](#endpoint) implementers.  _Note that the IMS Caliper certification suite currently requires implementers seeking certification to send data to the certification test [Endpoint](#endpoint) using HTTPS with a bearer token credential consistent with [RFC 6750](#rfc6750)._  Where an alternate transport protocol is preferred for performance or other considerations, it is recommended to add that support in addition to HTTP transport for maximum interoperability.

Irrespective of the chosen transport protocol, each message sent by a [Sensor](#sensor) to a target [Endpoint](#endpoint) MUST consist of a single JSON representation of a Caliper [Envelope](#envelope). 

#### 5.1 HTTP Transport Requirements

A Caliper service provider utilizing the Hypertext Transport Protocol (HTTP) MUST demonstrate that is capable of communicating with the Caliper certification suite over HTTP with the connection encrypted by Transport Layer Security (TLS).  A Caliper service provider MUST also support message authentication using the HTTP `Authorization` request header as described in [RFC 6750](#rfc6750), [Section 2.1](https://tools.ietf.org/html/rfc6750#section-2).

##### 5.1.1 General Message Requirements
* Each message MUST consist of a single serialized JSON representation of a Caliper [Envelope](#envelope).
* Messages MUST be sent using the POST request method.

##### 5.1.2 HTTP Request Headers

The following standard HTTP request headers MUST be set for each message sent to the certification suite [Endpoint](#endpoint):
 
 | Request Header | Requirements |
 | :------------- | :----------- |
 | Accept | \[TODO\] . . . |
 | Authorization | The HTTP request header `Authorization` value MUST be set to the Bearer Token provided by the certification suite. |
 | Content-Type | The HTTP request header `Content-Type` value MUST be set to the IANA media type "application/json". |
 | Host | \[TODO\] . . . |
 
##### 5.1.3 Certification Suite HTTP Response Behavior
 When communicating over HTTP the certification suite [Endpoint](#httpEndpoint) will exhibit the following response behavior:
 
 * To signal to a Caliper service provider that it has successfully received a message the certification suite [Endpoint](#httpEndpoint) will reply with a `2xx` class status code.  The body of a successful response will be empty.
 * If a Caliper service provider sends a message containing events and or entities without an enclosing [Envelope](#envelope), the certification suite will reply with a `400 Bad Request` response.
 * If a Caliper service provider sends a malformed Caliper [Envelope](#envelope) (it does not contain `sensor`, `sendTime`, `dataVersion` and `data` properties of the required form), the certification suite will reply with a `400 Bad Request` response.
 * If a Caliper service provider sends a message without an `Authorization` request header of the RECOMMENDED form or sends a token credential that the certification suite is unable to either validate or determine has sufficient privileges to submit Caliper data, the certification suite will reply with a `401 Unauthorized` response.
 * If a Caliper service provider sends a message with a `Content-Type` other than "application/json", the certification suite will reply with a `415 Unsupported Media Type` response.
 * If a Caliper service provider sends a message with an [Envelope](#envelope) that contains a `dataVersion` value that the [Endpoint](#httpEndpoint) cannot support the certification suite will reply with a `422 Unprocessable Entity` response.
 
 The certification suite MAY respond to Caliper service provider messages with other standard HTTP status codes to indicate result dispositions of varying kinds.  The certification suite MAY also communicate more detailed information about problem states, using the standard method for reporting problem details described in [RFC 7807](#rfc7807).

## <a name="contributors"></a>Contributors

The following Caliper Working Group participants contributed to the writing of this guide:

#### Authors

| Name | Organization |
| :--- | :----------- |
| Anthony Whyte | University of Michigan |
| Markus Gylling | IMS Global |

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

<a name="wordnet"></a>__WordNet__.  Princeton University.  WordNet&reg;.  A lexical database for English. 2010.  URL: https://wordnet.princeton.edu/

## <a name="aboutThisDoc"></a>About this Document

IMS Global Learning Consortium, Inc. ("IMS Global") is publishing the information contained in this document ("Specification") for purposes of scientific, experimental, and scholarly collaboration only.

IMS Global makes no warranty or representation regarding the accuracy or completeness of the Specification.

This material is provided on an "As Is" and "As Available" basis.

The Specification is at all times subject to change and revision without notice.

It is your sole responsibility to evaluate the usefulness, accuracy, and completeness of the Specification as it relates to you.

IMS Global would appreciate receiving your comments and suggestions.

Please contact IMS Global through our website at [http://www.imsglobal.org](http://www.imsglobal.org).

Please refer to Document Name: IMS Caliper Analytics&reg; Certification Guide, version 1.1
Candidate Final Specification v1.1

Date: 30 November 2017

This document contains trademarks of the IMS Global Learning Consortium including the IMS Logos, Learning Tools Interoperability&reg; (LTI&reg;), Accessible Portable Item Protocol&reg; (APIP&reg;), Question and Test Interoperability&reg; (QTI&reg;), Common Cartridge&reg; (CC&reg;), AccessForAll&trade;, OneRoster&reg;, Caliper Analytics&reg; and SensorAPI&trade;. For more information on the IMS trademark usage policy see trademark policy page - https://www.imsglobal.org/trademarks