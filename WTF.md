 **Integrating the Healthcare Enterprise**

![IHE\_LOGO\_for\_tf-docs](images/IHE-logo.jpeg)

**[IHE ITI](https://profiles.ihe.net/ITI)**

**Technical Framework Supplement**

**Whitepaper on Topological Federation of document sharing**

**Revision 0.1 - Draft**

Date: TODO:  Update

Author: ITI Technical Committee

Email: iti@ihe.net

**Please verify you have the most recent version of this white paper. See [here](http://profiles.ihe.net/ITI) for the Published version.

**Foreword**

This is a whitepaper to the IHE IT Infrastructure Technical Framework. Each whitepaper undergoes a process of public comment and publication. IHE whitepapers are informative, and are used to describe or investigate the needs for normative publications.

Comments are invited and can be submitted using the [ITI Public Comment form](http://www.ihe.net/ITI_Public_Comments/) or by creating a [GitHub Issue](https://github.com/IHE/ITI.WTF/issues/new?assignees=&labels=&template=public-comment-issue-template.md&title=).

General information about IHE can be found at [http://www.ihe.net](http://www.ihe.net).

Information about the IHE IT Infrastructure domain can be found at [https://www.ihe.net/IHE_Domains](https://www.ihe.net/IHE_Domains/).

Information about the organization of IHE Technical Frameworks and Supplements and the process used to create them can be found at [https://www.ihe.net/about_ihe/ihe_process](https://www.ihe.net/about_ihe/ihe_process/) and [https://www.ihe.net/resources/profiles](https://www.ihe.net/resources/profiles/).

The current version of the IHE Technical Framework can be found at [https://profiles.ihe.net/](https://profiles.ihe.net/).

**CONTENTS**

<!-- TOC depthFrom:1 depthTo:2 -->
<!--
- [Introduction to this Supplement](#introduction-to-this-supplement)
- [Open Issues and Question](#open-issues-and-question)
- [Closed Issues](#closed-issues)
- [IHE Technical Frameworks General Introduction](#ihe-technical-frameworks-general-introduction)
	- [9 Copyright Licenses](#9-copyright-licenses)
- [IHE Technical Frameworks General Introduction Appendices](#ihe-technical-frameworks-general-introduction-appendices)
    - [Appendix A - Actor Summary Definitions](#appendix-a---actor-summary-definitions)
	- [Appendix B - Transaction Summary Definitions](#appendix-b---transaction-summary-definitions)
	- [Appendix D - Glossary](#appendix-d---glossary)
- [Volume 1 - Profiles](#volume-1---profiles)
- [34 IUA Profile](#34-iua-profile)
    - [34.1 IUA Actors, Transactions, and Content Modules](#341-iua-actors-transactions-and-content-modules)
    - [34.2 IUA Actor Options](#342-iua-actor-options)
    - [34.3 IUA Required Actor Groupings](#343-iua-required-actor-groupings)
    - [34.4 IUA Overview](#344-iua-overview)
    - [34.5 IUA Security Considerations](#345-iua-security-considerations)
    - [34.6 IUA Cross Profile Considerations](#346-iua-cross-profile-considerations)
- [Volume 2 - Transactions](#volume-2---transactions)
    - [3.71 Get Access Token [ITI-71]](#371-get-access-token-iti-71)
    - [3.72 Incorporate Access Token [ITI-72]](#372-incorporate-access-token-iti-72)
    - [3.102 Introspect Token [ITI-102]](#3102-introspect-token-iti-102)
    - [3.103 Get Authorization Server Metadata [ITI-103]](#3103-get-authorization-server-metadata-iti-103)
- [33 MHD Profile](#33-mhd-profile)
- [9 ATNA Profile](#9-atna-profile)
-->
<!-- /TOC -->

TODO:  Fix

# 1 Introduction

As civilization becomes increasingly connected, it is becoming increasingly important and expected that all stakeholders in a health information ecosystem can access the information they need quickly, seamlessly, and without concern for jurisdictional, geographic, or organizational boundaries. 
To do so requires vast information exchange networks that span entire countries, or ideally, the world. 

The [Integrating the Healthcare Enterprise (IHE)](http://www.ihe.net) standards profiling
organization has previously published a Health Information Exchange (HIE) Whitepaper (TODO: link) that gives an overview of the IHE integration profiles and policy decisions that should be considered when planning the architecture of a document sharing community. This whitepaper expands on the HIE whitepaper by exploring strategies for integrating multiple document sharing communities into one federated exchange ecosystem with wide and comprehensive reach at the national or global level. Readers are expected to be familiar with the concepts and ideas presented in the HIE whitepaper, as those concepts will be used and expanded upon here. 

## 1.1 Scope

The scope of this whitepaper is to expand upon the IHE HIE Whitepaper by providing additional guidance on how existing document sharing communities can be interconnected to form a unified federated exchange ecosystem with wide and comprehensive reach. This whitepaper will assume that since existing networks are being connected, document sharing must be federated across the networks, but each network might use either a federated or centralized architecture. This whitepaper will not address any topics regarding the creation of a document sharing community or affinity domain. Readers interested in those topics are advised to look to the following existing IHE resources:

- HIE Whitepaper
- Template for XDS Affinity Domain Deployment Mapping
- Metadata Handbook
- TODO:  Link

While document sharing will be federated across networks, this whitepaper recommends the use of a centralized directory, likely operated by an entity not part of any of the networks, that aggregates technical and organizational information about all participants in all connected networks. Examples will be based on the IHE mCSD integration profile (TODO:  link), which offers the functionality needed by such a directory, and also offers a transaction that can be used to synchronize the central directory with directories operated by each network.  

In general, it is recommended that inter-network communication be architected such that members of one network need not understand the topology of the other interconnected networks. Networks should expose to each other standard interfaces that abstract away their underlying topologies to the greatest extent possible, and this can be accomplished with thoughtful and consistent use of IHE's integration profiles. That said, there a a few instances where members of one network might need to introspect into another network. The two cases this whitepaper will address are

1. Situations where information received from another network needs to be attributed to a particular organization inside of that network
2. Situations where a message needs to be delivered to a particular recipient within another network

In both of these cases, the centralized directory will be used to correlate identifiers in document sharing messages with Organization Resources in the directory, such that a discrete association can be made programmatically.

In addressing these use cases, this whitepaper's scope is limited to the exchange of healthcare documents across networks. Document exchange provides a good baseline for healthcare data interoperability, as it simplifies exchange by grouping data into documents. Document sharing allows for the exchange of both structured data (for example, CDA or FHIR documents) as well as unstructured data (for example, scanned documents, PDF files, etc.). This makes document exchange a good choice for establishing baseline data exchange, whereas other forms of exchange, such as search and read of discrete FHIR resources via a RESTful API, are more advanced and can be added to an existing exchange ecosystem once baseline goals are met. As such, this whitepaper focuses only on document sharing, and does not address problems that might arise from other forms of data exchange. 

## 1.2 Intended Audience

The intended audience of this whitepaper includes those involved in promoting, coordinating, architecting or participating in federated exchange across existing health information sharing networks. This paper covers high level data exchange philosophies and architectures, but does not cover implementation details. Such details can be found in the underlying IHE profiles leveraged here. Furthermore, the intended audience is assumed to have a strong understanding of document sharing communities and other topics covered in the IHE HIE whitepaper. 

Some intended audiences include:

- Those charged with enabling widespread healthcare data exchange
- Health Information Exchange Executives and Architects
- Health-IT Vendor Architects and Developers
- Standards Development Architects
- Academic Health Data Exchange Stakeholders

## 1.3 High Level Concepts
The following concepts and definitions will be used throughout this whitepaper. 

### Document Sharing Community
A **Document Sharing Community** is defined by XCPD (TODO:  Link) and XCA (TODO:  Link) as a group of facilities/enterprises that have agreed to work together using a common set of policies for the purpose of sharing health information within the community via an established mechanism. 

Within each community, there exists a pair of gateways that are used to exchange data across community boundaries. 
The Initiating Gateway is used by members within a community to send messages to actors outside of the community. 
The Responding Gateway is the access point for actors outside of the community to access services within the community. 

### Home Community ID
A **Home Community ID** (HCID) is a globally unique identifier for a community. 
Note that in the case where a community contains a single organization, a home community ID will also uniquely identify that organization, but in the general case where a community contains several organizations, the HCID will be distinctly different from individual organization identifiers. 

### Document Sharing Network
A **Document Sharing Network** is defined as a collection of one or more Document Sharing Communities and/or Document Sharing Networks that agree to a common, higher level governance structure, participate in document sharing with one another, and share network central resources among one another. 

Document Sharing Networks might provide their participant communities with a variety of services. 
The most common are centralized directories, certificate authorities, and centralized network gateways. 

### Network Gateway
A **Network Gateway** is a system, central to a given network, that facilitates data exchange across network boundaries. 
The Initiating Gateway is used by participants within a network to send messages to actors belonging to other networks. 
The Responding Gateway is the access point for actors belonging to other networks to access services within the network.

Network Gateways expose the same interfaces and are interacted with in the same way as community gateways. The difference is that a Network Gateway might provide access to several communities. 
Therefore, where network gateways are used, participants can interact with the network gateway in the same way as a community gateway, with the caveat that the network gateway might process information for several Home Community IDs. 

### Multi-Layered Document Sharing Network
A **Multi-Layered Document Sharing Network** is a document sharing network that contains other document sharing networks as members. That is to say, such a network is a "network of networks". 

For the sake of clarity, this whitepaper will refer to the most atomic actors in a multi-layered network as the "bottom" layers, and the least atomic actors as the "top" layers. 

![Multi-Layered Diagram](images/multi_layered_network.png)

**Figure # - Example Multi-Layered Network**

### Document Sharing Federation
**Document Sharing Federation** refers to the act of architecting a document sharing network such that actors within the network exchange healthcare documents directly with one another rather than through a central actor. 
Since there is no central actor that facilitates exchange, the network actors must join together to form a larger entity - the document sharing network. 
In this model, the removal of any actor from the network is subtractive - the network has lost the information made available by that actor. 

In contrast, a centralized network has a set of central actors that aggregate information, and a set of ancillary actors that communicate with the central actors. 
In this contrasting model, the removal of ancillary actors does not subtract from the information available in the network, since that information remains available in the central actors. 

Federation can occur at any layer of a multi-layered document sharing network, though it becomes more likely as higher and higher layers are added, because centralization becomes cumbersome as the network topological depth increases. 

### Care Services Directory
A **Care Services Directory** is a common, authoritative registry of the healthcare organizations, locations, practitioners, etc. and their contact information (both electronic and otherwise). 
A document sharing network, at minimum, needs a directory that contains the set of organizations that are members of the network and the communication endpoints for document sharing. 
More advanced networks will also want to have information about the network topology in the directory, as well as information about the healthcare locations, practitioners, jurisdictions, services, and organizational business relationships. 

IHE offers the Mobile Care Services Directory (mCSD) (TODO:  link) integration profile to specify how such a directory should operate, and the mCSD White Paper (TODO:  link) offers additional explanation on how the mCSD profile can be implemented to solve the business needs of a healthcare information exchange. 

### Patient Identity Management/Linking
In order to successfully and safely exchange patient health information within and across document sharing communities and networks, it is imperative that the parties doing the exchange are able to establish agreement about the patient that is the subject of their communication. 

The concepts and profiles discussed in section 5 of the HIE Whitepaper (TODO:  link) can be successfully applied across communities and networks.
An important note is that in cases where interaction with network gateways that represent multiple communities is needed, only the XCPD profile currently offers the functionality needed to give systems outside of the network access to patient identities for each community within the network. 
Other IHE profiles, such as the PDQ family of profiles, do not allow distinguishing between different identities across different communities, and so would only be sufficient when interacting with gateways that represent a single community or offer the facade of a single community.

### Query and Retrieve
**Document Query and Retrieve** refers to a document sharing model where documents are exchanged in a two step process. 
In the first step, the document consumer actor sends a request for a list of available documents. 
In the second step, the document consumer actor reviews the obtained list of documents and retrieves certain documents from the list. 

This model is often referred to as the "pull" model. 
It is generally used in situations where the actors consuming the document need to be able to search and retrieve information about a patient. 
This is most often the case for end user stories that involve reviewing the medical history, or current medical chart, for the patient. 

Document Query and Retrieve is enabled by the IHE XCA profile (TODO:  link) in a network setting, but can also be enabled by the MHD profile (TODO:  Link). 

### Message Delivery
**Message Delivery** refers to a document sharing model where an information source wants to communicate healthcare information to a particular, intended recipient. 
In this model, documents are prepared by the source and then need to be communicated and routed to a recipient that consumes them directly. 

This model can be thought of as a direct replacement for email, fax, postal mail, etc. and is often referred to as a "push" model.

Message Delivery is enabled by the IHE XDR (TODO:  link), MHD, and XCDR (TODO:  link) integration profiles. 

### Translation Capabilities
When a community or network is being constructed in a completely greenfield space, ie, one that does not have any existing technology to reconcile with, a single information exchange standard can be chosen such that all systems seamlessly and natively interoperate with each other.
However, when existing networks become interconnected, and as old systems are replaced with new systems, there will eventually be a need to be able to translate between different communication standards. 
This might mean translating between different IHE integration profiles, such as XCA and the FHIR based MHD profiles, between proprietary data exchange formats and standard formats, or between formats offered by different standards bodies, such as HL7 FHIR IPA and IHE MHD+CDA documents. 

One of IHE's general principles is to describe interactions between systems and to avoid specifying implementations within systems. 
This design principle lends itself well to translation, since it means that as long as the interface exposed by the translating system fully conforms to the relevant IHE integration profile, other systems implementing the other ends of those profiles will be able to interoperate without even realizing that translation is taking place. 

One of the primary use cases of the IHE MHD integration profile is to act as a translation layer between FHIR based systems and systems that implement IHE's XDS family of integration profiles. 
Therefore, IHE has already done the work of mapping the MHD elements to their XDS equivalents, making translation between MHD and XD* profiles exceptionally natural. 

Translation will generally be added to networks in the gateways. 
The gateways will expose one set of interfaces to the systems inside of the network, and a separate set of interfaces to systems outside of the network. 
They might even expose multiple functionally sets of interfaces inside or outside of the network, in order to offer a choice to the systems they must communicate with around which communication protocols can be used. 

Example: Community A exposes two endpoints on its Responding Gateway: XCA and MHD. 
The community is implemented as an XDS Affinity Domain internally, so the MHD interface must translate between XDS and MHD messages. 
Systems outside of the community can use either endpoint, depending on their communication preferences, and in both cases the same set of documents are returned from the internal XDS Affinity Domain. 

Example: Community B exposes an XCA endpoint to the rest of the network it is a part of, according to network governance. Internally, Community B contains a mix of systems that use XDS.b and MHD. 
The XCA Responding Gateway of Community B is grouped with XDS.b and MHD Document Consumers, so that it can accept requests from outside of the community and federate them among the internal systems that contain data being searched for. 
Once it receives responses from all systems, it must translate those responses into an XCA response to meet the network expectations. 
It will do so in a way that is transparent, such that actors outside of the community are not aware of the mixed architecture of the community. 

### Facade Community
Rather than expose internal networks and communities directly, a network could be designed to expose a single facade community to other networks with which it is interconnected.
Such a community would have a single Home Community ID, different from those of the internal communities. 
The network gateways would therefore be responsible for aggregating information from all of the systems within, in order to appear as a single cohesive community to the outside. 

A network offering a facade community must be able to act as a single community for all interactions. 
It must maintain a patient identity cross reference manager, so that patient identities across the internal communities can be aggregated into a single patient identity in the facade community.
When it receives a document list query, it must be able to forward that query to all communities that are part of the facade, and aggregate the responses into a single unified response. 
When it receives a message for delivery, it must be able to route the message to the appropriate community without using a home community id. 

The advantages of such an architecture is that it offers a simplified mechanism for interacting with the network for systems outside of the network, at the cost of complicating the network architecture. 

# 2 Federation Use Cases

## Document Access

TODO:  Write User Story

## Message Delivery

TODO:  Write User Story

# 3 Example Network Topologies

## Single Community With Single Affinity Domain
This will be the case where the network has a single community, and that community itself has a single source for clinical data. 

TODO:  Flesh out

## Single Facade Community
This will be the case where a network has a single community. That community has a responding gateway that aggregates data from within the community and generates aggregate documents for the community as a whole. 

TODO:  Flesh out

## Single Community With Multiple Affinity Domains
This is the case where the network has a single community. That community might have one or more XDS affinity domains which the Responding Gateway gathers documents.

TODO:  Flesh out

## Multiple Community Network
This is the case where the network has multiple communities within. Some message types need to be addressed to a specific community when interacting with the network gateway. 

TODO:  Flesh out

# 4 Directory Structure

## General Guidance
This section will lay out guidance on how to represent different topologies in an mCSD directory

TODO:  Flesh out and adapt the below

### Representing Federations in Directories

(Content moved and tweaked from mCSD Vol 1 1:46.8 - can remove or reduce that section)

This section provides guidance for representing and making use of federations in a directory (mCSD or otherwise) to enable electronic communication, for example defining local points of connectivity within a community, or defining a Health Information Exchange (HIE) that allows multiple communities to interoperate. It focuses on the following resources: Endpoint, Organization and OrganizationAffiliation.

Many current Endpoint directories based on FHIR are purpose-built, which is to say they are deployed to a server that only hosts Organization and Endpoint resources, and only for the use case of Endpoint lookup. For this reason, directories often reflect network details directly in the Organization resource, such as:
- The organization's role in the network, like participant or sub-participant, expressed as the type of organization.
- The organization's relationship to its connectivity vendor, expressed as the organization hierarchy (i.e., partOf).
- The organization's connectivity state as an extension.
- Supported profiles, purposes of use, etc. as extensions.
- The organization's identity as a home community ID, for use in IHE Document Sharing profiles.

When the organization's structure and its network capabilities need to vary independently (e.g., an organization uses two connectivity vendors), directories typically handle this by creating parallel instances of the Organization resource that then have to be merged by custom code to display.

We anticipate these conflicts increasing over time due to many forces:
- Implementers taking advantage of profiles like mCSD to represent more comprehensive organizational and personnel structures.
- Implementers scaling by delegating maintenance of organization sub-trees to the organizations themselves.
- Directories consolidating/federating over time into more comprehensive "phone books", where a given organization participates in multiple HIEs. One example would be the USA ONC TEFCA Recognized Coordinating Entity, which will be maintaining a directory that consists of entries supplied by each Qualified Health Information Network (QHIN).

In this guidance, we allow organization structure and network details to vary independently by moving network details out of the Organization and into the Endpoint and OrganizationAffiliation resources.

##### Endpoint to an Organization

The simplest usage model for a client is when the organization it needs to contact has a dedicated Endpoint resource in Organization.endpoint. Because this Endpoint is Organization-specific, it does not matter to the client who hosts it. Some examples follow.

Note: The managingOrganization of an Endpoint is who users need to contact for support. It may or may not be the same as the organization that hosts it. Since hosting is not reflected in the directory, we are indicating it in the diagrams below by the URLs.

Organization A hosts its own Endpoint:
<div>
{%include dir-org-specific-endpoint-self.svg%}
</div>
<div style="clear: left;"/>
**Figure 1:46.8.1-1: Organization-specific Endpoint Hosted by the Organization**

Organization A is directly reachable by an endpoint hosted by its parent Organization B:
<div>
{%include dir-org-specific-endpoint-parent.svg%}
</div>
<div style="clear: left;"/>
**Figure 1:46.8.1-2: Organization-specific Endpoint Hosted by Parent**

Organization C is directly reachable by an endpoint hosted by its affiliated Organization D:
<div>
{%include dir-org-specific-endpoint-affil.svg%}
</div>
<div style="clear: left;"/>
**Figure 1:46.8.1-3: Organization-specific Endpoint Hosted by Affiliation**

Organization E is directly reachable by an endpoint hosted by a hidden (i.e., not in the directory) Intermediary F:
<div>
{%include dir-org-specific-endpoint-inter.svg%}
</div>
<div style="clear: left;"/>
**Figure 1:46.8.1-4: Organization-specific Endpoint Hosted by Hidden Intermediary**

##### Endpoint to a Structure

When an Organization with an Endpoint has a complex structure, for example, sub-organizations, clients can often make use of this structure:

<div>
{%include dir-endpoint-to-org-hierarchy.svg%}
</div>
<div style="clear: left;"/>

**Figure 1:46.8.2-1: Endpoint to Organizational Hierarchy**

Typical directories will take an organizational hierarchy to imply accessibility to parts of the structure, for example:
- For FHIR REST endpoints, the URL is simply the Service Base URL as specified in [FHIR R4 3.1.0.1.2](http://hl7.org/fhir/R4/http.html#general). Clients can expect to find resources related to Organizations A, B and C.
- For XCA endpoints, a client querying Organization A for documents (e.g., using \[ITI-38\]) may receive documents from Organizations A, B and C. If these organizations have identifiers of type Home Community ID in the directory, clients can expect to see these identifiers in the returned document metadata.
- For XDR endpoints, a client sending a Provide and Register Document Set-b (\[ITI-41\]) request to Organization A can optionally specify Organizations B and/or C in intendedRecipient.
- For MHD endpoints, a client sending a Provide Document Bundle (\[ITI-65\]) request to Organization A can optionally specify Organizations B and/or C in intendedRecipient.

Specific details of addressing to federated recipients are out of the scope of this IG.

Examples of this kind of federated structure are shown in [ITI TF-1: Appendix E.9](https://profiles.ihe.net/ITI/TF/Volume1/ch-E.html#E.9.3), for XCA Responding Gateways.

By contrast, OrganizationAffiliations by themselves do not necessarily imply this kind of electronic accessibility. For this reason, this IG defines the code "DocShare-federate", which explicitly declares that the participatingOrganization is accessible as a federated organization via the OrganizationAffiliation.endpoint.

The following diagram shows the same accessibility, but using OrganizationAffiliation.

<div>
{%include dir-endpoint-to-org-affiliates.svg%}
</div>
<div style="clear: left;"/>

**Figure 1:46.8.2-2: Endpoint to Organizational Affiliates**

In addition, these mechanisms may be combined. This may be useful, for example, when adding an existing organizational structure to an HIE.

<div>
{%include dir-endpoint-to-hybrid-org-structure.svg%}
</div>
<div style="clear: left;"/>

**Figure 1:46.8.2-3: Endpoint to Hybrid Organizational Structure**

##### Grouping Actors

Grouped actors may be represented as well, although not explicitly. In the following example, Participant A is reachable by either an MHD endpoint or XDR endpoints. The directory
does not reflect which endpoint is the adapter or the adaptee.

<div>
{%include dir-endpoint-xdr-mhd.svg%}
</div>
<div style="clear: left;"/>

**Figure 1:46.8.3-1: Endpoints to Grouped Actors**

##### Endpoint Discovery Usage

The following example shows the steps used by a Care Services Selective Consumer to navigate a directory to find suitable electronic service Endpoints to some desired Organizations. In this example, a "suitable" Endpoint means it supports an IHE Document Sharing profile, and is based on .connectionType, .extension:specificType, .payloadType, .payloadMimeType, and status (both Endpoint.status as well as the actual status of the electronic service). The example uses the [mCSD-profiled OrganizationAffiliation] StructureDefinition-IHE.mCSD.OrganizationAffiliation.DocShare.html) that indicates federated connectivity for Document Sharing (e.g., affiliated organizations may be addressed as intendedRecipient). The pseudocode below uses a depth-first, first-match search, and does not protect against loops.

Until a suitable Endpoint is found or the search is complete, check the following in this order:
- Locate the desired Organization resource.
- Check if it has a suitable Organization.endpoint.
- Find OrganizationAffiliation resources where the Organization is the .participatingOrganization, and OrganizationAffiliation.code = DocShare-federate.
- For each OrganizationAffiliation found:
  - Check if it has a suitable OrganizationAffiliation.endpoint.
  - Check if it has a suitable OrganizationAffiliation.organization.endpoint.
  - Continue searching for a suitable Endpoint by traversing the OrganizationAffiliation resources recursively (i.e., where the OrganizationAffiliation.organization of the current resource is the .participatingOrganization of the next resource).
- If there is an Organization.partOf (i.e., a parent), check if it has a suitable Organization.endpoint.
  - Continue searching for a suitable Endpoint by traversing Organization.partOf recursively.

Rather than a first-match search, the Care Services Selective Consumer might search for and decide among multiple electronic paths to the same Organization. For example:
- It finds a suitable Endpoint resource for the target Organization, but instead uses an Endpoint for an Organization two levels higher to make a broader search for records.
- It finds suitable Endpoint resources for equivalent mechanisms, XDR \[ITI-41\] and MHD \[ITI-65\], and chooses MHD as the preferred transaction.
- It finds suitable Endpoint resources to the same Organization via two different HIEs, and prefers one HIE based on lower fees and authorization differences.

### Relationships between mCSD Resources
This section will cover how different mCSD Resources relate to one another from the perspective of representing network topology

TODO:  Flesh out

### Inclusion of Message Delivery Addresses
This section will discuss how to represent message delivery addresses in an mCSD directory

TODO:  Flesh out

## Examples
In this section we will take the example topologies from above and show how they would be represented in an mCSD directory

### Single Community With Data Aggregator

TODO

### Single Community With Multiple Affinity Domains 

TODO

### Multiple Community Network 

TODO

# 5 Example Use Case Solutions
This section will describe how the use cases in section 2 are enabled by the above suggestions

TODO:  Flesh out

## Document Access 

TODO

## Message Delivery

TODO

# 6 Miscellaneous

## Further Reading

## Scratchpad
TODO:  This section SHALL be removed prior to public comment

![General Diagram](images/diagram.png)

**Figure 1: Diagram**
