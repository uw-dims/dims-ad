.. _systemwidedecisions:

System-wide design decisions
============================

.. todo::

    .. note::

        This section shall be divided into paragraphs as needed to present
        DIMS-wide design decisions, that is, decisions about the DIMS's
        behavioral design (how it will behave, from a user's point of view, in
        meeting its requirements, ignoring internal implementation) and other
        decisions affecting the selection and design of the software units that
        make up the DIMS. If all such decisions are explicit in the DIMS
        requirements or are deferred to the design of the DIMS's software
        units, this section shall so state. Design decisions that respond to
        requirements designated critical, such as those for safety, security,
        or privacy, shall be placed in separate subparagraphs. If a design
        decision depends upon system states or modes, this dependency shall be
        indicated. Design conventions needed to understand the design shall be
        presented or referenced. Examples of DIMS-wide design decisions are the
        following:


        * Design decisions regarding inputs the DIMS will accept and outputs it
          will produce, including interfaces with other systems, HWCIs, DIMSs,
          and users (4.3.x of this DID identifies topics to be considered in this
          description). If part or all of this information is given in Interface
          Design Descriptions (IDDs), they may be referenced.
  
        * Design decisions on DIMS behavior in response to each input or
          condition, including actions the DIMS will perform, response times and
          other performance characteristics, description of physical systems
          modeled, selected equations/algorithms/rules, and handling of unallowed
          inputs or conditions.
  
        * Design decisions on how databases/data files will appear to the user
          (4.3.x of this DID identifies topics to be considered in this
          description). If part or all of this information is given in Database
          Design Descriptions (DBDDs), they may be referenced.
  
        * Selected approach to meeting safety, security, and privacy
          requirements.
  
        * Other DIMS-wide design decisions made in response to requirements, such
          as selected approach to providing required flexibility, availability,
          and maintainability.

    ..

..

.. _background:

Background on Existing Core Components
--------------------------------------

To understand what the DIMS system is intended to provide, it is important to
understand its role in the context of distributed and collaborative incident
response. DIMS leverages the capabilities of several existing systems each
provide key functions necessary for incident response, but are not presently
designed to work together. Integrating these capabilities will result in an
increase in the capacity to respond.

.. Building DIMS from open source components will keep the cost of
.. acquisition and operation to a reasonable level that is
.. sustainable by SLTT government entities who are cash-strapped,
.. resource-limited, yet critical to providing services to the
.. general public who are their consituents.

Figure :ref:`DimsSystemOverview` depicts a high-level diagram of the dataflows
between DIMS and related system.

.. _DimsSystemOverview:

.. figure:: images/Overview-DIMS-system.png
   :width: 70%
   :align: center

   Overview of DIMS System

..

DIMS provides a user interface layer on the front end, as well as a data
processing layer on the back end, that integrates with several existing
systems.

+ The first is the Security Information Event Management (SIEM) system at the
  core of the PRISEM project, and the technologies associated with it to
  perform behavioral detection of malicious activity from network flow data and
  support forensic analysis of historic data to respond and recover from
  attacks that evade detective mechanisms. This system collects and processes
  security related events and network flow data and supports a collective
  approach to responding and recovering from security events.

+ The second system is the Ops-Trust portal system, used by a community of
  several hundred computer security professionals with operational and research
  roles in industry, government, and academia. This system is primarily
  designed to facilitate trust group maintenance and communication to deal with
  emerging threats and events of international scope.

+ The third are the suite of "big data" style open source unstructured data
  storage, log processing, log visualization, and other tools that are part of
  the ELK stack, MozDef, and CIF.
  
+ Additional tools that can be used for visualization can be similarly
  integrated (such as Mal4s), by building them into the system deployment
  infrastructure like any other components used in DIMS. This type of
  `framework` model, if generalized, allows any of a number of open source
  security tools to be made available to the incident responder.

The DIMS software system will bring these systems together into a collaborative
environment for shared analysis and shared response of shared threats, both
within a regional trust community, as well as across multiple such trust
communities in other regions.  Through vertical sharing of indicators of
compromise from US-CERT to the regional level, and lateral sharing across
regional entities, the objective is to scale actionable information sharing to
state, local, territorial, and tribal (SLTT) government entities across the
United States, and extend the sharing to international trust groups who make up
the global fabric of the internet.

.. _DataFlowsBetweenStakeholders:

.. figure:: images/stix-dataflows-v1.png
   :width: 70%
   :align: center

   Data Flows Between Stakeholders

..

Figure :ref:`DataFlowsBetweenStakeholders` depicts the data flows
between a subset of the stakeholders who will be using the DIMS
software system. The solid lines depict data that has the highest
degree of sensitivity and trust, often being transmitted in
un-redacted form (possibly tagged with TLP indicators for most
restricted sharing). The dashed lines depict data flows that are at
lower levels of trust, and may be transmitted only in redacted form
(possibly tagged with TLP indicators for the least restricted
sharing). The type of data shared may be structured IOC and
Observables in STIX format, Course of Action information in either PDF
or structured format, `Situational Awareness Report` (:term:`SITREP`)
documents that describe observed campaign level activity at a high
level, possibly with structure data containing IOCs or Observables to
assist recipients in searching for related activity, and incident
reports that may similarly be a combination of human-readable PDF and
machine-readable IOCs/Observables. There are two types of data that
will be shared in most use cases: high-frequency, high-volume,
automated data feeds of `reputation` data and IOCs/Observables coming
from analytic and research groups; low-frequency, low-volume, manually
triggered bundles of IOCs/Observables, Course of Action information,
and/or high-level SITREPs for specific
incident-level up to campaign-level activity. The DIMS software,
layered on top of the Ops-Trust portal system, will facilitate
production of these reports and transmission/reception of structure
data files and facilitate automated processing of the structure data
files to pre-process data for an analyst to consume when ready, rather
than forcing the analyst to do a lot of work manipulating files,
processing their contents, and manually entering data into report
generation front ends in web based portals.

Figure :ref:`PRISEMInitialDeploymentAndFlows` depicts the high-level
data flow relationships for the Security Information Event Management
(SIEM) system and Botnets detector subsystem used in the PRISEM
project as it was initially deployed in 2009. The City of Seattle (the
first and to this date largest participant organization) has multiple
security devices sending event logs into the system.  It also
generates NetFlow V5 records that are processed by real-time
detectors, and archived for historical query capability. The logs are
collected one site, then forwarded to the central SIEM for processing
at the University of Washington.

.. _PRISEMInitialDeploymentAndFlows:

.. figure:: images/cos-hw-deployment-v3.png
   :width: 50%
   :align: center

   PRISEM Initial Deployment and Flows

..

Figure :ref:`Netflow-Architecture` depicts a prototypical NetFlow
collection and archiving model. The PRISEM system uses a slightly
modified version of this model. Unlike the diagram in Figure 4, the
PRISEM system processes NetFlow records as they enter the `NetFlow
Collector` in the center of the diagram, sending copies to the Botnets
system detectors. One of the processes receiving these records
performs the storage task, however it converts the NetFlow V5 records
to SiLK format before storing them. The SiLK tool suite is then used
to process these historic logs (e.g., performing historic queries).


.. _Netflow-Architecture:

.. figure:: images/Netflow-Architecture.png
   :width: 50%
   :align: center

   Netflow Architecture

..

Figure :ref:`Botnets-Architecture` shows the high-level architecture
of the Botnets network flow-based behavioral detector system. One or
more NetFlow V5 feeds are combined into a single feed, which
duplicates each NetFlow record and fans them out in to N different
detectors. Each detector maintains its own state and sends out alerts
when appropriate via SNMP, standard output to users in realtime, or to
the Unix syslog service. (In Figure 5, syslog events are sent to a
remote syslog server and processed by ZenOSS, an open source IT
monitoring system. In the PRISEM system, all detectors alert via
syslog, which are processed by the Log Matrix Threat Center
application.)

.. _Botnets-Architecture:

.. figure:: images/Botnets-Architecture.png
   :width: 70%
   :align: center

   Botnets System High-Level Architecture

..

Figure :ref:`PRISEM-Architecture` shows the central system
architecture of the PRISEM system. Shown in green are the Security
Information Event Management (SIEM) system and event log archive in
the bottom right. The box in the upper right depicts an instance of
the network flow monitoring (“Botnets” detector system) and SiLK data
archiving, which is typically housed on-site at participant networks
due to sensitivity of network flow data. A central instance of the
Collective Intelligence Framework (CIF) v0.1 database provides
historic information about known malicious activity, which is used to
pull watchlists that the Botnets detectors use for behavioral
detection. A virtual machine server provides processing and AMQP
broker functions to integrate data from multiple sources and correlate
it across participating organizations, and optionally anonymize or
filter any data prior to sharing. At present, a vendor-proprietary
portal provides the graphical user interface front-end for
participants, with the primary PRISEM systems residing behind a
vendor-supported firewall, with command line utilities and AMQP access
provided via an OpenVPN server for secure access. The DIMS dashboard
will front-end this portal and support additional capabilities that
are available on the PRISEM back-end via the AMQP broker (See Figure
TODO-26 and Figure TODO-27).

.. _PRISEM-Architecture:

.. figure:: images/prisem-system-architecture-v1.png
   :width: 60%
   :align: center

   PRISEM Architecture

..

Figure :ref:`OpsTrustArchitectureDiagram` shows the basic architecture
of the Ops-Trust portal system. This system is a combination of a
web-based portal, a wiki for information archiving, an email server,
and DNS and LDAP services tied to OpenID authentication services to
provide single-signon capability. All of these services are provided
via four separate virtual machines, co-resident in a single 1U server
that is backed up off-site. The instance depicted in
:ref:`OpsTrustArchitectureDiagram` is hosted on Ops-Trust hardware. A
development instance will be set up at the UW for DIMS development,
and potentially multiple local test instances will be set up for
PRISEM users (and possibly also Agora members, currently running in
the multiple hundreds of individuals) to beta-test DIMS features.

.. _OpsTrustArchitectureDiagram:

.. figure:: images/ops-trust-system-architecture.png
   :width: 60%
   :align: center

   Ops-Trust Architecture Diagram

..

The Ops-Trust portal stores attributes about each member. Figure :ref:`OpsTrustMemberPage` shows the
account for the PI, which includes: user UUID; home time zone; nearest
airport (to facilitate contact and meet-ups when one is on travel);
how to contact via email, postal mail, SMS, IM, and phone; and current
PGP encryption key. The portal lets you sign up for email lists, and
switch between "trust groups". After signing up for (and
optionally being approved for membership) email lists, the user is
included on list email routed through the mail server, and granted
access to the appropriate section of the wiki.

.. _OpsTrustMemberPage:

.. figure:: images/ops-trust-memberpage.png
   :width: 70%
   :align: center

   Ops-Trust Member Information Page

..

The DIMS system will take advantage of the foundation of services
provide by this portal in several ways. It will use it as a means of
storing more information about users, the network assets they protect,
the policies and mechanisms for anonymizing and filtering data based
on TLP tagging, etc. It will also use it as a mechanism to distribute
data to users as needed (e.g., alerts about email threads that pertain
to the network assets they protect, providing a means to download
OpenVPN certificates and SSH keys, as a mechanism for storing and
organizing data associated with incidents and campaigns they are
dealing with, etc.) The ability to manage encrypted communications and
multiple email lists facilitates trusted communication and offers a
basis for sending structured threat information in encrypted form,
directly from one user to another, or from a user to all members of a
list.

.. _swdevmethodology:

Software Development Methodology
--------------------------------

As the DIMS system relies upon and integrates multiple existing and
future open source software components, it is being developed using an
Agile programming development methodology (as opposed to the classic
'waterfall' development methodology with its sequential processes.)
This document, therefore, is a 'living document' that will be updated
as the project proceeds and as cyclic input/feedback from users and
testers is received. Sections to be addressed in future releases of
this document are listed as TBA.

.. _opensource:

Use of Open Source components
-----------------------------

.. todo::

   Explain reasoning about use of Open Source components.

..


