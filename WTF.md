 **Integrating the Healthcare Enterprise**

![IHE\_LOGO\_for\_tf-docs](images/IHE-logo.jpeg)

**[IHE ITI](https://profiles.ihe.net/ITI)**

**Technical Framework Supplement**

**Whitepaper on Topological Federation of document sharing**

**Revision 0.1 - Draft**

Date: June 10, 2022

Author: ITI Technical Committee

Email: iti@ihe.net

**Please verify you have the most recent version of this document.** See [here](http://profiles.ihe.net/ITI) for Trial Implementation and Final Text versions and [here](https://www.ihe.net/resources/public_comment/) for Public Comment versions.

**Foreword**

This is a whitepaper to the IHE IT Infrastructure Technical Framework. Each whitepaper undergoes a process of public comment and publication. IHE whitepapers are informative, and are used to describe or investigate the needs for normative publications.

Comments are invited and can be submitted using the [ITI Public Comment form](http://www.ihe.net/ITI_Public_Comments/) or by creating a [GitHub Issue](https://github.com/IHE/ITI.WTF/issues/new?assignees=&labels=&template=public-comment-issue-template.md&title=).

General information about IHE can be found at [http://www.ihe.net](http://www.ihe.net).

Information about the IHE IT Infrastructure domain can be found at [https://www.ihe.net/IHE_Domains](https://www.ihe.net/IHE_Domains/).

Information about the organization of IHE Technical Frameworks and Supplements and the process used to create them can be found at [https://www.ihe.net/about_ihe/ihe_process](https://www.ihe.net/about_ihe/ihe_process/) and [https://www.ihe.net/resources/profiles](https://www.ihe.net/resources/profiles/).

The current version of the IHE Technical Framework can be found at [https://profiles.ihe.net/](https://profiles.ihe.net/).

**CONTENTS**

<!-- TOC depthFrom:1 depthTo:2 -->

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

<!-- /TOC -->

# Introduction to this Supplement

## Problem Statement

blah blah... blah blah...


![General Diagram](images/diagram.png)

**Figure 1: Diagram**

blah blah

## High Level Concepts: Federation and Directories
Federation
Bridge/adapter

## Federation Use Cases

## Federation/Bridging Mechanisms
(TBD whether we want to put directory representation before or after the specific transaction guidance)

### Identities in a Federation/Bridge
(May be better as part of the "High Level Concepts" section)

Example: Org 1.2.3 exposes two endpoints: XCA and MHD. One interface is an adapter over the other (not sure if we need to show both ways). Whether the requester calls one or the other interface, they get clinical data from the same organization / set of identities. We would call this a TBD (bridge? adapter?)

Example: Org 1.2.3 exposes an XCA endpoint, and behind that endpoint is another org 4.5.6 and MHD. One interface is an adapter over the other (not sure if we need to show both ways). Whether the requester calls one or the other interface, they get clinical data from the same organization / set of identities.

### Federated Push

### Federated Pull

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
- Directories consolidating/federating over time into more comprehensive "phonebooks", where a given organization participates in multiple HIEs. One example would be the USA ONC TEFCA Recognized Coordinating Entity, which will be maintaining a directory that consists of entries supplied by each Qualified Health Information Network (QHIN).

In this guidance, we allow organization structure and network details to vary independently by moving network details out of the Organization and into the Endpoint and OrganizationAffiliation resources.

##### 1:46.8.1 Endpoint to an Organization

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

##### 1:46.8.2 Endpoint to a Structure

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

##### 1:46.8.3 Grouping Actors

Grouped actors may be represented as well, although not explicitly. In the following example, Participant A is reachable by either an MHD endpoint or XDR endpoints. The directory
does not reflect which endpoint is the adapter or the adaptee.

<div>
{%include dir-endpoint-xdr-mhd.svg%}
</div>
<div style="clear: left;"/>

**Figure 1:46.8.3-1: Endpoints to Grouped Actors**

##### 1:46.8.4 Endpoint Discovery Usage

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
