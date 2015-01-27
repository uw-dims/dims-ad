.. systemwidedecisions:

============================
System-wide design decisions
============================

.. note:: TBD

   This section shall be divided into paragraphs as needed to present DIMS-wide
   design decisions, that is, decisions about the DIMS's behavioral design (how
   it will behave, from a user's point of view, in meeting its requirements,
   ignoring internal implementation) and other decisions affecting the selection
   and design of the software units that make up the DIMS. If all such decisions
   are explicit in the DIMS requirements or are deferred to the design of the
   DIMS's software units, this section shall so state. Design decisions that
   respond to requirements designated critical, such as those for safety,
   security, or privacy, shall be placed in separate subparagraphs. If a design
   decision depends upon system states or modes, this dependency shall be
   indicated. Design conventions needed to understand the design shall be
   presented or referenced. Examples of DIMS-wide design decisions are the
   following:


      * Design decisions regarding inputs the DIMS will accept and outputs it
	will produce, including interfaces with other systems, HWCIs, DIMSs, and
	users (4.3.x of this DID identifies topics to be considered in this
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
