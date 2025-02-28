---
stand_alone: true
ipr: trust200902
title: Power and Energy Efficiency
abbrev: POWEFF
category: std
consensus: true
submissiontype: IETF
area: operations
workgroup: GREEN Working Group

docname: draft-splm-poweff-latest
obsoletes: I-D.draft-opsawg-poweff
#updates: 4710 # Remove if not needed/Replace

lang: en
kw:
  - Internet-Draft

pi:
  - toc
  - sortrefs
  - symrefs

normative:
  I-D.draft-lindblad-tlm-philatelist-03:

informative:
  I-D.draft-palmero-ivy-ps-almo-01:
  I-D.draft-almprs-sustainability-insights-02:

author:
-   ins: J. Lindblad
    name: Jan Lindblad
    organization: All For Eco
    email: jan.lindblad+ietf@for.eco
-   ins: S. Mitrovic
    name: Snezana Mitrovic
    organization: Cisco Systems
    email: snmitrov@cisco.com
-   ins: M. Palmero
    name: Marisol Palmero
    organization: Cisco Systems
    email: mpalmero@cisco.com
-   ins: G. Salgueiro
    name: Gonzalo Salgueiro
    organization: Cisco Systems
    email: gsalguei@cisco.com

--- abstract

This document specifies a device YANG "dashboard" data model that allows devices to report which power measurement and control functions they offer.  This basic YANG model is applicable to any kind of device, regardless of whether the device itself has any support for YANG-based management interfaces or not.  The YANG model simply allows a device to describe what it can report, and which interfaces are available to request this data.  Devices that lack any on-board YANG-based management interfaces provide this information in form of a YANG instance data file.  This file may be readable from an on-board web server on the device, or hosted anywhere else.

--- middle

# Introduction

As highlighted during the [IAB workshop on environmental impacts](https://datatracker.ietf.org/doc/html/draft-iab-ws-environmental-impacts-report-00), visibility is a very important first step. Paraphrasing Peter Drucker's mantra of "You cannot improve what you don't measure".  During the workshop the need for standardized metrics was established, to avoid proprietary, redundant and even contradictory metrics across vendors.

POWEFF is considered a first step, part of the Sustainability Telemetry Specification referred as part of the Sustainability Insights {{?I-D.draft-almprs-sustainability-insights-02}} IETF draft (a newer version may exist).  That is where the overall problem statement, solution principles and other components of the proposed solution can be found.  Specifically, this work is meant to fit in with the {{I-D.draft-lindblad-tlm-philatelist-03}} framework.

This Power Consumption and Energy Efficiency Telemetry Specification (POWEFF) provides a way for a controller to understand what a device offers in terms of power related sensors and controls.  It also provides machine readable metadata for the sensors, such as which units of measurement are used, what is included in the reported data, the precision of the data, etc.  This is referred to as the device dashboard.

This document also contains embryonic definitions of recommended datasets and attributes defining a common data model to report Power Consumption and Energy Efficiency on assets, with multuple implementation levels, that new devices may choose to implement.  Standardized calculations utilizing the specified datasets and attributes which will yield a power consumption value for any asset or network element, and standardized calculations utilizing the specified datasets and attributes which will yield the energy efficiency value for any asset or network element.

## Requirements language

{::boilerplate bcp14}

# Terminology

Terminology and abbreviations used in this document:

Asset
: Refers to hardware, software, applications, or services. An asset can be physical or virtual, as defined in the Asset Lifecycle Management and Operations {{?I-D.draft-palmero-ivy-ps-almo-01}} IETF draft.

Scope 1
: Emissions directly caused by actions of the organization, such as when fossil fuels are burned when the organization is operating a fossil vehicle. See [Greenhouse Gas protocol](https://ghgprotocol.org/).

Scope 2
: Emissions indirectly caused by actions of the organization, but under control of the organization. For example, when electric energy is purchased, causing a provider utility to make emissions on behalf of the organization. See [Greenhouse Gas protocol](https://ghgprotocol.org/).

Scope 3
: Emissions the organization indirectly causes others to make, but outside the organizations direct control. Examples include the energy customers consume when operating the organization's products, or when employees commute to work at the organization. See [Greenhouse Gas protocol](https://ghgprotocol.org/).

Scope 4
: Refers to the term used in Greenhouse Gas (GHG) accounting and reporting to describe emissions that occur as a result of an organization's value chain activities, but are not directly controlled or owned by the organization. Scope 4 emissions are considered indirect emissions and are typically associated with activities that are upstream or downstream from a organization's operations. Such as when equipment provided by the organization enables a video conference, without which greater emissions from business travel would have happened.

CO2eq
: Carbon dioxide equivalents, a measure of the disruptive force of greenhouse gas emissions.

Power
: Refers to the (e.g. electrical or optical) energy per unit of time, supplied to operate an asset, such as a smartphone. It is usually measured in units of Watts.

Energy Efficiency
: refers to the ability of an asset to perform its intended functions while minimizing energy consumption. It refers to the ratio between the useful output energy and input energy given by an asset. In a router or a switch, it is a measure of how efficiently the network element utilizes energy resources to transmit and process data or perform other network-related tasks. See [Energy efficiency wikipedia](https://en.wikipedia.org/wiki/Energy_efficiency).

# Motivation

The main objective of POWEFF is to enable Network Controllers to measure, report and control power and energy related metrics from networks with many and diverse devices, providing the necessary insights to improve the overall CO2eq emission for use cases of which the asset is part.  Basically emissions that address direct use-phase emissions of Scope 3, Category 11 "use of sold products".

It includes emissions from the use of goods and services sold by the reporting company or vendor in the reporting year. A vendor’s Scope 3 emissions from use of sold products include the scope 1 and scope 2 emissions of end users. End users include both consumers and business customers that use final assets. It is important to note that Scope 3 category 11, reports around 75% of the total Scopes 1, 2 and 3 reported by a given asset. See [Cisco ESG Reporting Hub](https://www.cisco.com/c/m/en_us/about/csr/esg-hub/environment/goals.html#scope-1-3-emissions).

Power and energy consumption Telemetry data available for different infrastructure vendors today is characterized by inconsistency and best effort:

- Availability of primary data.  Data is often only available on a case by case basis
- Varying APIs.  Where Telemetry might be available, access methods, data contents and formats are specific to platforms or elements
- Limitations.  Some useful or essential data items are never collected by the relevant hardware or software
- Precision.  Data often contains significant margins of error, both from random noise and systematic errors
- Varying definitions.  Calculated values use differing inputs and algorithms, limiting the value of any possible comparison and aggregation
- Opacity.  Lack of transparency of how and what is being measured makes it very hard to ascertain fair comparisions.

## Proposed Solution Outline

Formulate a Power and Energy Efficiency Telemetry Specification to promote consistency:

Data
: Definition of datasets and attributes that will define a common data model to report power and energy consumption on hardware and software assets

Calculation
: Define a standardized calculation utilizing the specified datasets and attributes which will yield an energy consumption value for any asset.

Implementing any Sustainability Solution at scale for customers with a broad range of equipment requires at minimum consistently available Power Consumption/Energy Efficiency Telemetry. Telemetry standardization will benefit numerous stakeholders, including Corporate Social Responsibility (CSR), who have a need for Power Consumption Telemetry data for a variety of purposes.

## Use Cases

- Monitoring power and energy efficiency based on common metrics.
- Enhance reporting and provide a comprehensive overview for potentially improving power usage during the operational phase.
- Consumption per device, e.g. wireless environment.
- Capabilities to optimize energy consumption when assets are not in use, e.g. idle and allocated power.
- Hardware Lifecycle. Circular economy enables to restore product value at the end of life, there are several options, reuse, remanufacturing, recycling, repurpose, etc.

More elaborate use cases, e.g. define carbon footprint for network's usage, might also be derived from POWEFF model, even discussion and common understanding will be required.

# Information Model

Implementors of this specification can choose the implementation level that is appropriate for their device at the current time.  As the implementation matures, higher implementation levels may be chosen over time.  Each implementation level is a superset of the previous level.

## Level 0, Proprietary Dashboard Only

At level 0, the device implements only proprietary dashboards, without implmementing any dashboards with predefined content. This allows controllers to find the power sensors already present in the implementation, and read the associated metadata, but may not be well prepared to really understand the meaning of the data being read.  The dashboard may be provided by an on-board YANG-based management protocol, or delivered as a YANG instance data file from an on-board webserver, or delievered as a file by some other mechanism (e.g. web server elsewhere).

For level 0, the Network Element implements the Philatelist YANG module ietf-tlm-philatelist-provider. This gives the controller one or more proprietary dashboard with whatever contents the implementor sees fit.

## Level 1, Current Total Power Draw

At level 1, the device implements a very small, but well defined dashboard, and lists it using the Philatelist
ietf-tlm-philatelist-provider module. The level 1 dashboard consists of a single dashboard item. This dashboard item provides a way for the Network Controller to read the current total power draw of the Network Element.

~~~ yang-tree
module: ietf-poweff-level-1
  +--rw poweff
     +--ro stats
        +--ro device-current-total-power-draw?   uint32
~~~
{: title="YANG tree diagram of the Level 1 Dashboard."}

The following requirements MUST be fulfilled by the Network Element implementing the level 1 and higher dashboards.
+ The reported telemetry data MUST be correct with regards to what is included and not included for all reported telemetry data values
+ The metadata MUST be correct with regards to measurement units for all reported telemetry data values
+ The metadata MUST be correct with regards to apparent/real RMS power, for all reported power and energy data values
+ The power consumption values reported MUST NOT be underestimated over time in actual field use

If Network Elements declaring conformance to the level 1, or higher, dashboard of this specification, do not actually fulfill the conditions required in this document, that may be construed as violating the [EU Green Claims Directive (GCD), EU 2023/0085(COD)](https://oeil.secure.europarl.europa.eu/oeil/popups/ficheprocedure.do?lang=en&reference=2023/0085(COD))

## Level 2, Add Energy and Susbsystem Breakdown

At level 2, on top of all level 1 reporting, the Network Element also reports the gross energy usage over time (the integral over time of the power draw), and the power draw can be further inspected for each major subsystem within the device.

~~~ yang-tree
module: ietf-poweff-level-2

  augment /ietf-poweff-level-1:poweff/ietf-poweff-level-1:stats:
    +--ro device-total-energy-spent?         uint32
    +--ro device-total-energy-spent-since?   yang:date-and-time
    +--ro subsystem* [name]
       +--ro name                  subsys-name-t
       +--ro current-power-draw?   uint32
       +--ro children*             -> ../../subsystem/name
~~~
{: title="YANG tree diagram of the Level 2 Dashboard."}

## Level 3, Add Fundamental Power Control

From this level onward, a YANG-based management protocol is required, since standards based configuration control of the device is required.

At level 3, all the reporting functions of level 2 are required, and also basic control over device global power-save modes. The controller may choose one of several power saving modes for the Network Element. Network Element implementors or Standards Defining Organizations (SDOs) may also augment the mode selection with additional power saving modes.

The basic principle for the power saving controls is for the Network Controller to specify how much degradation of the maximum possible delivered performace it could tolerate, and for the Network Element to decide on what power saving measures that can be taken, while still fulfilling expectations. The Network Element SHOULD also provide an estimate of how much power can be saved under the given conditions.

This document specifies four power save modes and two power-save conditions that apply generally to the power save modes.

fully-powered
: The subsystem is fully powered, i.e. does not take any power-saving measures that would risk lowering the performance below normal levels.

powered-off
: The subsystem is completely powered off, i.e. it is drawing no or little power while also delivering zero performance.

napping
: The subsystem is napping, i.e. is taking frequent but brief pauses in the service it provides. The Network Controller may specify a max-additional-latency. This determines the maximum tolerated length of the pauses with reduced performance. This means the maximum additional delay that this subsystem would incur on e.g. detecting incoming traffic or performing its function.

throttling
: The subsystem is throttling, i.e. is running with reduced capacity in the service it provides. The Network Controller may specify a max-capacity-reduction. This determines the maximum tolerated reduction of performance.

For example, if a Network Controller applied throttling with a max-capacity-reduction value at 50% onto a transport subsystem or service that consists of 3 underlaying links of equal capacity, the Network Element might decide to shut down one of the three links.

For all the power-save modes (except the fully-powered mode, in which these have no effect) the two following general conditions also apply:

max-time-to-cancel-power-save
: The maximum time the Controller allows the subsystem to recover full performance. The subsystem should not
engage in power-saving measures that take longer than this time to revert to full performance.

estimated-power-reduction
: The subsystem's own estimate on how much of its own power draw that is reduced by the power-saving measures in effect.

For example, if a Network Controller applied throttling with a max-capacity-reduction value at 50% onto a transport subsystem or service that consists of 3 underlaying links of equal capacity, the Network Element might decide to shut down one of the three links. The Network Element might then report an estimated-power-reduction of 33%.

~~~ yang-tree
module: ietf-poweff-level-3

  augment /ietf-poweff-level-1:poweff:
    +--rw power-save
       +--rw subsystem* [name]
          +--rw name                   -> /poweff/stats/subsystem/name
          +--rw (selected-power-save-mode)?
          |  +--:(fully-powered)
          |  |  +--rw fully-powered?               empty
          |  +--:(powered-off)
          |  |  +--rw powered-off?                 empty
          |  +--:(napping)
          |  |  +--rw napping
          |  |     +--rw max-additional-latency?   microseconds
          |  +--:(throttling)
          |     +--rw throttling
          |        +--rw max-capacity-reduction?   percent
          +--rw max-time-to-cancel-power-save?     microseconds
          +--ro estimated-power-reduction?         uint32
~~~
{: title="YANG tree diagram of the Level 3 Dashboard."}

## Level 4, Add Service Attribution

At level 4, the Network Element also provides a list of services/tenants/clients/domains/functions that it delivers value towards, and attributes the Network Element's power draw to each of the services.  The list of services may include one "overhead/idle/other/unknown" entry that absorbs any overhead not attributable to a particular service. The power draw MAY be further subdivided for each service by using a dot notation.

One service instance called '-idle-' may be present in the list and absorb any overhead/idle/other/unknown kind of power draw that the system would not allocate to any service. It is up to the implementor to decide what a 'service' means for this type of system. It may be any kind of service that it delivers user value towards.

For example, if a system serves three customers, X, Y and Z, their power draw could be declared as follows:

| name          | current-power-draw | children                    |
|---------------|--------------------|-----------------------------|
| X             |                 45 | vpn                         |
| X.vpn         |                 39 | eth1/16 eth2/33 eth3/11     |
| X.vpn.eth1/16 |                 14 |                             |
| X.vpn.eth2/33 |                 12 |                             |
| X.vpn.eth3/11 |                  9 |                             |
| Y             |                 26 |                             |
| Z             |                 19 |                             |
| -idle-        |                 48 |                             |

The sum of the current-power-draw top level entries  (in this example: X, Y, Z and -idle-, with values 45 + 26 + 19 + 48 = 138) must match the value provided in ietf-poweff-level-1:device-current-total-power-draw

The sub-service values (e.g. X.vpn, 39W) need to be lower than or equal to (but do not necessarily need to match) their parent level (e.g. X, 45W).

Note: the name of the children have been abbreviated in the diagram above. In the actual payload, the full names would always be used, e.g. 'eth1/16' above would actually be communicated as 'X.vpn.eth1/16'.

~~~ yang-tree
module: ietf-poweff-level-4

  augment /ietf-poweff-level-1:poweff/ietf-poweff-level-1:stats:
    +--ro service* [name]
       +--ro name                  string
       +--ro current-power-draw?   uint32
       +--ro children*             -> ../../service/name
~~~
{: title="YANG tree diagram of the Level 4 Dashboard."}

## Level 5, Add Service Level Power Control

At level 5, the device additionally implements power-save modes per delivered service. The structure is exactly the same as the level 3 structure, except that it is for services rather than subsystems. A service would be something that is relevant and meaningful from a customer's or user's perspective. It is up to the Network Element implementor to decide exactly what constitutes a service.

~~~ yang-tree
module: ietf-poweff-level-5

  augment /ietf-poweff-level-1:poweff/ietf-poweff-level-3:power-save:
    +--rw service* [name]
       +--rw name                        -> /poweff/stats/service/name
       +--rw (selected-power-save-mode)?
       |  +--:(fully-powered)
       |  |  +--rw fully-powered?               empty
       |  +--:(powered-off)
       |  |  +--rw powered-off?                 empty
       |  +--:(napping)
       |  |  +--rw napping
       |  |     +--rw max-additional-latency?   microseconds
       |  +--:(throttling)
       |     +--rw throttling
       |        +--rw max-capacity-reduction?   percent
       +--rw max-time-to-cancel-power-save?     microseconds
       +--ro estimated-power-reduction?         uint32
~~~
{: title="YANG tree diagram of the Level 5 Dashboard."}

# YANG Modules

## Module ietf-poweff-types.yang
~~~~ yang
{::include yang/ietf-poweff-types.yang}
~~~~
{: sourcecode-markers="true"
sourcecode-name="ietf-poweff-types@2024-04-16.yang”}

## Module ietf-poweff-level-1.yang
~~~~ yang
{::include yang/ietf-poweff-level-1.yang}
~~~~
{: sourcecode-markers="true"
sourcecode-name="ietf-poweff-level-1@2024-04-16.yang”}

## Module ietf-poweff-level-2.yang
~~~~ yang
{::include yang/ietf-poweff-level-2.yang}
~~~~
{: sourcecode-markers="true"
sourcecode-name="ietf-poweff-level-2@2024-04-16.yang”}

## Module ietf-poweff-level-3.yang
~~~~ yang
{::include yang/ietf-poweff-level-3.yang}
~~~~
{: sourcecode-markers="true"
sourcecode-name="ietf-poweff-level-3@2024-04-16.yang”}

## Module ietf-poweff-level-4.yang
~~~~ yang
{::include yang/ietf-poweff-level-4.yang}
~~~~
{: sourcecode-markers="true"
sourcecode-name="ietf-poweff-level-4@2024-04-16.yang”}

## Module ietf-poweff-level-5.yang
~~~~ yang
{::include yang/ietf-poweff-level-5.yang}
~~~~
{: sourcecode-markers="true"
sourcecode-name="ietf-poweff-level-5@2024-04-16.yang”}

# Deployment Considerations

POWEFF data models define the data schemas for power consumption and energy efficiency data. POWEFF data models are based on YANG. YANG data models can be used independently of the transport and can be converted into any encoding format supported by the network configuration protocol. YANG is therefore largely management protocol independent.

To enable the exchange of POWEFF data among all interested parties, deployment considerations that are out of the scope of this document, will need to include:

   * The data structure to describe all metrics and quantify relevant data consistently, i.e. specific formats like XML or JSON encoded message would be deemed valid or invalid based on POWEFF models.
   * The process to share and collect POWEFF data across the consumers consistently, including the transport mechanism. The POWEFF YANG models can be used with network management protocols such as NETCONF {{?RFC6241}}, RESTCONF {{?RFC8040}}, streaming telemetry, etc. OpenAPI specification could be considered to consume POWEFF metrics.
   * How the configuration of assets should be done.

# Security Considerations

The security considerations mentioned in section 17 of {{?RFC7950}} apply.

POWEFF brings several security and privacy implications because of the various components and attributes of the information model. For example, each functional component can be tampered with to give manipulated data. POWEFF when used alone or with other relevant data, can identify an individual, revealing Personal Identifiable Information (PII). How the configuration of assets should be accomplished could lead to data being accessed by unauthorized entities.

Methods exist to secure the communication of management information. The transport entity of the functional model MUST implement methods for secure transport. This document also contains an Information model and Data-Model in which none of the objects defined are writable. If the objects are deemed sensitive in a particular environment, access to them MUST be restricted using appropriately configured security and access control rights. The information model contains several optional elements which can be enabled or disabled for the purpose of privacy and security. Proper authentication and audit trail MUST be included for all the users/processes that access POWEFF data.

# IANA Considerations

##  The IETF XML Registry

FIXME

## The YANG Module Names Registry

FIXME

--- back

# Change log
{:numbered="false"}

RFC Editor Note: This section is to be removed during the final publication of the document.

* From version draft-opsawg-poweff-02 to draft-splm-poweff-00

  - Renamed draft
  - Updated one author affiliation
  - Moved WG to GREEN
  - Moved YANG modules to yang/ directory
  - Switched to building using MartinThompsons RFC build env.

* From version -01 to -02

  - Adapted to leverage the updated Philatelist framework
  - Added the dashboard concept

* From version -00 to -01

  - Major rewrite as a device level YANG framework
  - Added the implementation levels concept

* Version -00

  - Initial version.


# Acknowledgments
{:numbered="false"}

This document was created by meaningful contributions from Per Andersson, Jeff Apcar, Derek Engi, Esther Roure Vila, Pascal Thubert, Klaus Verschure, Joel Goergen, Colin Seward, Michael King, Angelo Fienga and Suresh Krishnan.

The authors wish to thank them and many others for their helpful comments and suggestions.
