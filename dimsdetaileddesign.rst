.. dimsdetaileddesign:

====================
DIMS detailed design
====================

This section shall be divided into the following paragraphs to describe each
software unit of the CSCI. If part of all of the design depends upon system
states or modes, this dependency shall be indicated. If design information
falls into more than one paragraph, it may be presented once and referenced
from the other paragraphs. Design conventions needed to understand the design
shall be presented or referenced. Interface characteristics of software units
may be described here, in Section 4, or in Interface Design Descriptions
(IDDs). Software units that are databases, or that are used to access or
manipulate databases, may be described here or in Database Design Descriptions
(DBDDs).

(Project-unique identifier of a software unit, or designator of a group of software units)
------------------------------------------------------------------------------------------

This paragraph shall identify a software unit by project-unique identifier and
shall describe the unit. The description shall include the following
information, as applicable. Alternatively, this paragraph may designate a group
of software units and identify and describe the software units in
subparagraphs. Software units that contain other software units may reference
the descriptions of those units rather than repeating information.

    * Unit design decisions, if any, such as algorithms to be used, if not previously selected

    * Any constraints, limitations, or unusual features in the design of the software unit

    * The programming language to be used and rationale for its use if other than the specified CSCI language

    * If the software unit consists of or contains procedural commands (such as
      menu selections in a database management system (DBMS) for defining forms
      and reports, on-line DBMS queries for database access and manipulation,
      input to a graphical user interface (GUI) builder for automated code
      generation, commands to the operating system, or shell scripts), a list
      of the procedural commands and reference to user manuals or other
      documents that explain them.

    * If the software unit contains, receives, or outputs data, a description
      of its inputs, outputs, and other data elements and data element
      assemblies, as applicable. Paragraph 4.3.x of this DID provides a list of
      topics to be covered, as applicable. Data local to the software unit
      shall be described separately from data input to or output from the
      software unit. If the software unit is a database, a corresponding
      Database Design Description (DBDD) shall be referenced; interface
      characteristics may be provided here or by referencing section 4 or the
      corresponding Interface Design Description(s).

    * If the software unit contains logic, the logic to be used by the software unit, including, as applicable:


        * Conditions in effect within the software unit when its execution is initiated

        * Conditions under which control is passed to other software units

        * Response and response time to each input, including data conversion, renaming, and data transfer operations

        * Sequence of operations and dynamically controlled sequencing during the software unit's operation, including:

            * The method for sequence control

            * The logic and input conditions of that method, such as timing variations, priority assignments

            * Data transfer in and out of memory

            * The sensing of discrete input signals, and timing relationships between interrupt operations within the software unit


    * Exception and error handling
