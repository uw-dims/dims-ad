.. tobeintegrated:

================
To be integrated
================

.. todo::

    dittrich Sat Feb 21 16:46:17 PST 2015

    This section was taken from ``http://foswiki.prisem.washington.edu/bin/edit/Development/ELK``
    It is being integrated into this and/or other Sphinx documents.
    To that end, the page above is being lifted wholesale into this
    section and after conversion to reST, will be migrated to the
    appropriate final placement.

..

ELK Overview
------------


Ansible role
------------

.. todo::

   The following describes the former monolithic installation of all ELK
   components in a single Ansible playbook. We have subsequently split them
   out into separate roles for each of the components to more easily distributed
   them across multiple servers, not all reside in the development workstation
   VM image.

..

The Ansible roles to install, start, and run the ELK stack can be found in the
``ansible-playbooks`` Git repo in ``ansible-playbooks/roles/ELK``. In order for
this role to be run on a desktop VM (only the desktop side is running at the
moment; work is currently being done on the server side), the role ELK must be
listed in the ``vagrant_desktop_provision.yml`` file. Logstash requires Java, but
it maybe not be the Java that is installed via the ``jdk`` role. More testing
must be done with regards to this issue.

Elasticsearch is installed and started via the Ansible role. If all works
appropriately, nothing has to change with Elasticsearch itself. During
development, we do need to be able to clear the Elasticsearch database,
otherwise the data never clears. This will be discussed in more depth
later.

Logstash parsing
----------------

Logstash collects, parses, and outputs data via configuration files. Data can
be input in a number of ways, and currently we are using standard input. Data
can also be parsed a number of ways, and we are using the grok filter to match
events within the data sets to a given pattern. Logstash ships with many
built-in patterns, and we can also build are own. We have already done this to
parse three different sets of data. Once the data is parsed, and the timestamp
is fixed to match the event date and time rather than the time the event was
input to Logstash, then the data is output to Elasticsearch.

Data sets
"""""""""

Currently, we have three different sets of canned data. We are working on a
fourth, and soon we hope to move to live data. The fourth data set, output from
RPC scripts, is crucial to being able to parse live, streaming data. The data
sets can be found in two places. There is a sample repo called
``dims-sample-data``. The data sets can also be found on a VM provisioned with
the ELK Ansible role at ``/opt/dims/data/sample-data``.

.. warning::

   Use of "Team Cymru data" is deprecated. This is not time-series data
   that Kibana is suited for analyzing. Rather, the Team Cymru ``whois``
   lookup service enriches data that contains just IP addresses. It was
   not meant for this data to be treated the same as other time-series
   event data.

..

[[http://www.team-cymru.org/About/][Team Cymru]] data file: ``ip-as.txt``

UFW data file: ``floyd-kern.log``

Websense data file: ``websense-samples.txt``


Patterns
""""""""

Patterns for the data can be found in two places: in the ``ansible-playbooks``
repo at ``ansible-playbooks/roles/ELK/files``, and on an ELK-provisioned
desktop VM at ``/opt/dims/patterns/``.

Team CYMRU pattern: ``cymru``

UFW pattern: ``ufw``

Websense pattern: ``websense``

Configuration files
"""""""""""""""""""

There is one configuration file per data set to be input, parsed, and sent to
Elasticsearch. We are using a script to input each data set via standard input.
Theoretically, every data set could be processed via one configuration file and
by using a file input, but that isn't how we're running things right now.
Configuration files can be found in two places: in the ``ansible-playbooks``
repo at ``ansible-playbooks/roles/ELK/files``, and on an ELK-provisioned
desktop VM at ``/opt/dims/etc``.

Team Cymru config file: ``logstash-cymru.conf``

UFW config file: ``logstash-ufw.conf``

Websense config file: ``logstash-websense.conf``

Run scripts
"""""""""""

We have helper scripts to load data into Logstash and send to Elasticsearch.
Essentially, the script pulls together the data set and runs the Logstash
command with the data as standard input. A log file for the script is made, and
if the debugging flag is turned on, output will be sent to it for later
analysis. When new patterns and configurations are being tested, the script can
limit the number of events to send to Logstash as well. The output files will
be found on an ELK-provisioned desktop VM at ``/vagrant/``. The helper scripts
can be found in two places: in the ``ansible-playbooks`` repo at
``ansible-playbooks/roles/ELK/files``, and on an ELK-provisioned desktop VM at
``/opt/dims/bin``.

Team Cymru run script: ``demo.logstash.addcymru``

UFW run script: ``demo.logstash.addufw``

Websense run script: ``demo.logstash.addwebsense``

To run a script with debugging output enabled from the command line type:

::

        $ bash -x demo.logstash.addDATASET

..

...where ``DATASET`` is one of the following: ``cymru``, ``ufw``, or ``websense``.

Please note, every time you run one of these scripts to load data, that data
will be added to Elasticsearch again. So it will seem like the data for that
data set has doubled when looking at it in Kibana. To avoid this, use the
helper script to wipe and restore the Elasticsearch database discussed near the
end of this page.

Kibana viewing
--------------

The Ansible role installs Kibana, changes the name of a dashboard file, and
installs and starts nginx to serve the Kibana web pages. It also sets the
homepage of the Firefox browser to Kibana so as soon as the browser is open, it
immediately goes to a page showing our data. There is a default dashboard which
can be reached at ``http://localhost/#/dashboard/file/default.json``. This
default has a time frame set of a day previous to the current time. Most of our
data, right now, does not fall within that time frame because it is canned data
from months ago. This caused some problems in viewing the data that did get
sent all the way through to Kibana because it was outside of the time frame. We
have created a few custom dashboards so the data can be immediately viewed and
looked at. The Kibana page provides a time series graph as well as a breakdown
of the fields Logstash parsed from the event data.

Dashboards are fairly easily customizable. There is a lot that can be changed,
and most of it can be changed via the web app itself or by creating json
documents. There is a lot more that could be done with the dashboards than what
our custom dashboards currently do, but at least now they show the data
immediately, in the correct time frame, without our having to manually
manipulate the time frame. Point the ELK-provisioned desktop VM's Firefox
browser to ``http://localhost/#/dashboard/file/[datasetdashboardfilename]``.
See below for the rest of the URLs. Also, the dashboard documents themselves
can be found in two places: in the ``ansible-playbooks`` repo at
``ansible-playbooks/roles/ELK/files``, and on an ELK-provisioned desktop VM at
``/opt/kibana3/app/dashboards``, with the same file names as for the URLs.

.. warning::

   Use of "All_DIMS_Data" is deprecated. We need to move away from demos
   using static "canned" datasets to processing a constant stream of live
   data.

..

Dashboard file to show all DIMS data (currently 3 data sets); ``All_DIMS_Data.json``

Dashboard file to show only Team Cymru data: =Team_CYMRU_Data.json=

Dashboard file to show only UFW data: =UFW_Data.json=

Dashboard file to show only Websense data: =Websense_Data.json=

.. warning::

   Use of "All_DIMS_Data" is deprecated. We need to move away from demos
   using static "canned" datasets to processing a constant stream of live
   data.

..

When an ELK-provisioned desktop VM is immediately provisioned, it will be
pointed to the ``All_DIMS_Data.json`` page. This will show, in one frame, all
three data sets.

Clearing the Elasticsearch database to view different data sets in Kibana
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. warning::

   We need to move away from demos using static "canned" datasets to processing
   a constant stream of live data.

..

One issue we had with testing viewing data was that once data went to
Elasticsearch, it was persistent, even upon halting and bringing back up the
desktop VM. This is good news, except it made testing really difficult because
the VM would have to be built from scratch. We developed a script to clear the
database. This involved removing files associated with the nodes and indices
Elasticsearch uses to hold the data. Once these were cleared, but their
structure left in tact, then the data was gone from Elasticsearch and also
Kibana.

Running the wipe/restore Elasticsearch database script
""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. warning::

   We need to move away from demos using static "canned" datasets to processing
   a constant stream of live data.

..

The script is called demo.logstash.deleteESDB. It can be used in two different
ways: to only clear the database and to clear the database as well as run the
Logstash helper script(s) to add any of the data back.

For more information, on the command line type: ::

    $ bash demo.logstash.deleteESDB help

..

To only clear the database, on the command line type: ::

    $ bash demo.logstash.deleteESDB

..

To clear and add data back, on the command line type: ::

    $ bash demo.logstash.deleteESDB [dataset1 .. dataset3]

..

... where ``datasetN`` can be any of the following: ``cymru``, ``ufw``, and/or
``websense``. You can mix and match as much as you want. After the script
finishes, refresh the Kibana webpage. If you only add one data set back, point
the browser to the matching URL for that data set, and the page will load with
the data viewable. If you don't point it to the matching URL or you send it to
the default dashboard, you might have to adjust the time frame to see the data.

Adjusting the time frame in Kibana
""""""""""""""""""""""""""""""""""

At the top of the Kibana page, the time frame is shown. If the page is pointed
to the default dashboard, it will say something like "a day ago to a few
seconds ago". With live data, this might be helpful, but not so much with our
canned data at the moment. To change the time frame, click the arrow to the
right of the indicated time. This should drop down a menu. You can choose any
of the preset changes, or you can choose 'Custom'. This will take you to a
pop-up kind of window, and you can reset the time frame to view the data in.

For current reference, the following are the general date/time of where the
data we can load to Kibana will be located.

Team Cymru time location: whenever the data set was sent through Logstash. This
means these events can be seen, if loaded, on the default dashboard, but it is
the only set. This data set has no times, so since we don't alter the timestamp
while parsing the data, this set automatically gets the time it was given to
Logstash. Actually, all of the events parsed by Logstash get this time attached
to them, but if the events have dates/times, we will use that parsed data to
alter the Logstash timestamp so the events are mapped according to when they
occurred, rather than when they were input to Logstash. The Team Cymru Data
dashboard will also show the data appropriately.

UFW data time location: this set is spread out quite nicely from around
6/15/2014 to around 7/20/2014. This is a rough estimate. The UFW Data dashboard
will also show the data appropriately.

Websense time location: this set does have date/time data and we do alter the
Logstash timestamp with that data, but all of the events happened at basically
the same time. Change the time frame to between 2/3/2014 and 2/5/2014, and
you'll see a pretty tall line appear. The Websense Data dashboard will also
show the data appropriately.

RPC Data use case/demo
----------------------

When Megan left this off (~9/4/14), she was having trouble getting the amount
of data in the RPC file (``dims-sample-data/rwfind_201302210110_18463``) to
actually load. The Logstash pattern and configuration file worked when the
``demo.logstash.addrpcflow`` script was in debug mode, only processing 10 lines
of data.

A repo branch was set up for this task. See ``ansible-playbooks`` repo, branch
``RPCdataELK``.

The process for this use case is the same as the other types of data described
above, you should just be able to run the =demo.logstash.addrpcflow= script.
Issues related to this still exist however: * You should run the
=demo.logstash.deleteESDB= first before testing because when the desktop VM is
provisioned right now, it loads all the data.  * Megan manually cut the
=rwfind_201302210110_18463.txt= file down to just the relevant data, so that's
one piece of the pipeline she didn't get done. She's not sure of a way for
Logstash parsing to truly ignore lines. She tried making a pattern that would
just accept anything that was "left over" when it didn't match actual RPC logs,
but...those were being graphed/counted in Kibana. She's pretty sure it's just a
simple python script to knock off lines until they start having more than, but
she didn't get that far.

Elasticsearch on a Server VM
----------------------------

Ansible Role
""""""""""""

See Git repo ``ansible-playbooks``, at ``ansible-playbooks/roles/essinstall``.
This installs Elasticsearch in a manner that where nodes can be
started/stopped/added/deleted more flexibly than the way it runs on the desktop
VMs.

Basic usage
"""""""""""

Once Elasticsearch is installed on a server VM, change directories to where it
was installed (should be ``/opt/elasticsearch-1.1.1``).Make sure you have
``sudo`` powers. Run ``bin/elasticsearch`` This will run through starting the
first node of a cluster. Unless the cluster name and node names are changed in
the elasticsearch configuration file (see
``/opt/elasticsearch-1.1.1/config/elasticsearch.yml``). This node is the master
node. Further configurations of shards and replica shards (for failover
purposes), among many other things, can be changed in the configuration file.
Once multiple VMs can be deployed at the same time, the discovery of nodes on a
cluster needs to be looked at closer and better understood. It does seem fairly
"automagical."

Helpful Elasticsearch Server links
""""""""""""""""""""""""""""""""""

.. _Starting at the beginning, an empty cluster: http://www.elasticsearch.org/guide/en/elasticsearch/guide/current/_an_empty_cluster.html
.. _Checking the health of a cluster: http://www.elasticsearch.org/guide/en/elasticsearch/guide/current/cluster-health.html
.. _The Definitive Guide to Elasticsearch: http://www.elasticsearch.org/guide/en/elasticsearch/guide/current/index.html
.. _Elastic Security\: Deploying Logstash, ElasticSearch, Kibana "securely" on the Internet: http://blog.eslimasec.com/2014/05/elastic-security-deploying-logstash.html

+ `Starting at the beginning, an empty cluster`_
+ `Checking the health of a cluster`_
+ `The Definitive Guide to Elasticsearch`_
+ `Elastic Security: Deploying Logstash, ElasticSearch, Kibana "securely" on the Internet`_


Related Links
-------------

.. _How to Apply Messaging to Cloud Apps with RabbitMQ, Node.js, Redis, and More: http://blog.pivotal.io/pivotal/products/how-to-apply-messaging-to-cloud-apps-with-rabbitmq-node-js-redis-and-more

+ `How to Apply Messaging to Cloud Apps with RabbitMQ, Node.js, Redis, and More`_

