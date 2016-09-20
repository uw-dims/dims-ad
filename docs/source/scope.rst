.. _scope:

Scope
=====

.. _identification:

Identification
--------------

This Architecture Design document (version |release|) describes
the proposed high-level design of the
Distributed Incident Management System (DIMS) architecture. Its
purpose is to provide the reader with an overview of the major system
components of the former Public Regional Information Security Event
Management (PRISEM) system, the Operational Security Trust (Ops-Trust)
portal system (now *Trident*), and the components of the DIMS *dashboard* front-end
and data processing back end that integrate these two existing
systems.

DIMS is funded by the Department of Homeland Security under contract HSHQDC-
13-C-B0013. For more information, see the documents
:ref:`dimsocd:dimsoperationalconceptdescription`,
:ref:`dimssr:dimssystemrequirements`, and other documents
referenced in Section :ref:`referenceddocs`.

The scope of this document is limited to description of the
architectural elements, data types (and their sources, volumes, and
retention periods), data flows, user interfaces, etc. It is assumed
the reader is familiar with the underlying motivations for the system
as described in the :ref:`dimsocd:dimsoperationalconceptdescription`
and :ref:`dimssr:dimssystemrequirements` documents.

.. attention::

    This document was originally written in the months after the
    initial start of period of performance of the DIMS project
    contract, which was September 13, 2013. Things have changed
    over the Base Period, Option Period, and two extensions.
    This, and all related DIMS documents, were originally written
    as *forward-looking* documents using future tense. This, and
    all related project documents, are undergoing updates
    to reflect changes that have occured (including switching
    to past tense and renaming as necessary) on a best-effort
    basis.  Keep this in mind while reading this document, and
    feel free to report any errors you encounter by filing
    a bug report or issuing a pull request.

..

.. _documentoverview:

Document overview
-----------------

The structure of this document has been adapted principally from MIL-STD-498
(see Section :ref:`referenceddocs`). Following this section are:

+ Section :ref:`referenceddocs` lists related documents.

+ Section :ref:`systemwidedecisions` describes the system-wide
  decisions that guide the design of the Distributed Incident
  Management System.

+ Section :ref:`dimsarchitecturaldesign` describes the DIMS architectural
  design.

+ Section :ref:`dimsdetaileddesign` provides details on the hardware
  and software subsystem design.

+ Section :ref:`requirementstraceability` describes traceability back
  to requirements expressed in the :ref:`dimssr:dimssystemrequirements`
  document.

+ Section :ref:`notes` provides an alphabetical listing of acronyms and
  abbreviations used in this document.

+ Section :ref:`license` includes the copyright and software license under
  which DIMS is being released.

