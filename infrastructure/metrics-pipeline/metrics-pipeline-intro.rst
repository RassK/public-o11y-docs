
.. _metrics-pipeline-intro:

******************************************************
Introduction to metrics pipeline management
******************************************************

.. meta::
    :description: Introduction to metrics pipeline management in Splunk Infrastructure Monitoring

|hr|

:strong:`Available in Enterprise Edition`

|hr|

.. meta::
    :description: Introduction to metrics pipeline management in Splunk Observability Cloud.

Metrics pipeline management is an evolution of Splunk Observability Cloud metrics platform that offers you solutions to
centrally manage metric cardinality.

With metrics pipeline management, you have more control over how you ingest and store your metrics, so you can lower
costs and improve monitoring performance without updating the configuration of your
:ref:`Splunk Distribution of the OpenTelemetry Collector <otel-intro>`. To remove data pre-ingest using the Collector,
see :ref:`configure-remove`.

What is metric cardinality?
=============================

Metric cardinality is the number of unique metric time series (MTS) produced by a combination of metric name and its
associated dimensions. Therefore, a metric has high cardinality when it has a high number of dimension keys, and a high
number of possible unique values for those dimension keys.

For example, you send in data for a metric ``http.server.duration``.

* If ``http.server.duration`` has only 1 dimension ``endpoint`` with 3 unique values: ``A``, ``B``, and ``C``, then
  ``http.server.duration`` generates 3 metric time series (MTS).
* If you add another dimension ``region`` with 3 unique values: ``us-east``, ``us-west``, and ``eu``, then
  ``http.server.duration`` generates 3 (endpoints) * 3 (regions) = 9 MTS.

Even though ``http.server.duration`` only has 2 dimensions, metric cardinality is already 9 since each dimension has
multiple possible values.

To learn more about MTS, see :ref:`metric-time-series`. To learn more about the Observability Cloud data model, see
:ref:`data-model`.

How does metrics pipeline management work?
========================================================

The driving mechanisms behind metrics pipeline management are aggregation and data dropping. For each metric you send to
Observability Cloud, you can control the metric volume with a set of aggregation and data dropping rules.

.. note:: A new aggregated MTS has a resolution of 10 seconds. Metrics pipeline management rolls up the raw data
   points received into one aggregated data point, for each MTS associated with the metric. If your systems emit data
   points over a period that's much longer than 10 seconds, you might have difficulty reconciling your raw data with
   the aggregated data. To learn more, see the section :ref:`mts-aggregation-rollup-period`.

* Aggregation rules let you roll up your selected metric data into new metrics that take up less storage and increase
  computational performance. To learn more, see :ref:`aggregation`.
* Data dropping rules let you discard any metrics you don't want to retain for monitoring. To learn more, see
  :ref:`data-dropping`.

By aggregating combinations of dimensions that provide useful insights while dropping a large amount of the unaggregated
raw data, you can significantly reduce your organization's data footprint.

.. _aggregation:

Aggregation rules
--------------------------------------------------------------------------------

Data you send from your services to Observability Cloud can have high cardinality. Instead of adjusting how you are
sending in your data before you send it, aggregation lets you summarize your data in Observability Cloud based on
dimensions you consider important.

.. mermaid::

   flowchart LR

   accTitle: Data aggregation diagram
   accDescr: Metrics pipeline management (MPM) receives raw incoming metric time series (MTS). You choose an MTS to aggregate, and perform the aggregation, then you choose whether to keep or drop the raw MTS. MPM keeps the aggregated MTS and any raw MTS that you chose to keep.
   
   Raw[(Incoming raw MTS)] ---|MPM|ChooseDimensions{"`Choose MTS to aggregate`"} ---|Perform aggregation|CreateNew("`New aggregated MTS with rolled-up
   metrics`") ---|Keep or drop raw MTS|OriginalMTS[(Kept MTS and new MTS)]

By selecting specific dimensions to keep, you can aggregate your data points into a new metric with fewer dimensions,
creating a specific view of dimensions that are important. You can then obtain a more simplified and concentrated view
of your data when you don't need to view metrics across all dimensions.

When you select specific dimensions, metrics pipeline management generates a new metric. The system creates new MTS
based on the dimensions you select and rolls up data points for each MTS. By default, aggregation rules roll up the
data points into the new MTS using ``sum``, ``min``, ``max``, ``count``, ``delta``, ``avg``, and ``latest`` functions.
You can use the new aggregated MTS in the same way as any other MTS in Observability Cloud.

How is this different from post-ingestion aggregation at query time?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

When you configure charts or detectors, you can aggregate your data using analytic functions, such as ``sum``, and then
group your data by specific dimensions, such as ``sum by region``. This aggregation occurs after Observability Cloud
has stored your raw MTS, so you still pay for storing the data.

With metrics pipeline management, you can aggregate your MTS as you store it and retain only aggregated metrics. Since
you're storing fewer dimensions for each data point, and metrics pipeline management roles up the metric values, you
save storage costs.

Example
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You send a metric called ``http.server.duration`` for a containerized workload using Splunk Infrastructure Monitoring.

Your workload has 10 endpoints, 20 regions, 5 services, and 10,000 containers. Each of the 5 services has 10,000
containers and 10 endpoints.

Your data is coming in at the container ID level, generating 10 (endpoints) * 5 (services) * 20 (regions) * 10,000 (containers) = 1,000,000 MTS.

You can reduce your metric cardinality by aggregating one or multiple dimensions.

Aggregate using one dimension
--------------------------------------------------------------------------------

You are only interested in the source region of your data, so you create an aggregation rule that groups your data by
the ``region`` dimension.

The aggregated metric removes all other dimensions and retains only the ``region`` dimension based on your rule. There
are only 20 different values for ``region``, so only Observability Cloud only ingests 20 MTS.

Aggregate using multiple dimensions
--------------------------------------------------------------------------------

You want to continue monitoring endpoints, regions, and services for your data, but don't need to monitor container IDs.
You create an aggregation rule that groups your data by the dimensions you want to keep.

The aggregated metric removes the ``container_id`` dimension and retains ``endpoint``, ``region``, and ``service``
based on your rule. Your new metric volume is: 10 (endpoints) * 20 (regions) * 5 (services) = 1,000 MTS.

.. _mts-aggregation-rollup-period:

MTS aggregation rollup period
===============================================================================

If your systems send periodic data points, but the period is longer than 10 seconds, then the result of MTS aggregation
might not be what you expect.

For example, suppose your systems generate data points every 5 seconds. Two successive data points have timestamps
that differ by 5 seconds. If your systems immediately transmit the points to Observability Cloud, the system ingests
two data points every 10 seconds. Metrics pipeline management can roll up the two data points into one aggregated
data point with a resolution of 10 seconds, which is the result you expect.

If you are sending data points, but they don't always arrive with the same frequency,
Observability Cloud might receive two data points in the first 10 seconds, then twelve data points in the next 10
seconds. In both cases, metrics pipeline management rolls up the raw points into a single aggregated data point.

Also, if you want to send data points every second and you want to keep the resolution of the incoming data points, don't
use MTS aggregation.

Potential issues
--------------------------------------------------------------------------------

The difference between the timestamp that your systems add to a raw data point when it's created and the time
the system uses when it aggregates data points can cause one of the following issues:

* The starting and ending time of aggregated MTS might shift. A data point generated by your server
  might come in some time after its creation time as recorded in its timestamp. In this case, the entire aggregated
  MTS shifts to a more recent time on the chart, indicating that the start time was more recent than the actual timestamp. This shift occurs
  because metrics pipeline management ignores the data point timestamp and instead uses the time it ingested the
  data point.

  For example, if your data points have a 10:00 timestamp, but Observability Cloud doesn't start receiving them
  until 10:10, the aggregated MTS seems to start at 10:10 instead of 10:00.
* The aggregated MTS might appear to have an incorrect duration.

Solutions
--------------------------------------------------------------------------------

Avoid these aggregation issues by using the following options:

* Do your own MTS aggregation before sending data to the system, by reconfiguring the OTel collector to drop unwanted dimensions.
* Aggregate data using SignalFlow when you generate charts or create detectors.


.. _data-dropping:

Data dropping rules
===============================================================================

When you have a new aggregated metric, you might no longer need the original unaggregated data. You
can also drop a metric without adding an aggregation rule. Data dropping rules let you discard any data you don't want
to monitor, so you can save storage space and reduce cardinality.

.. note::
    - You must be an admin to drop data.
    - You can drop new incoming data, but you can't drop data that Observability Cloud has already ingested.
    - You can't recover dropped data. Before you drop data, see :ref:`data-dropping-impact`.

Example
--------------------------------------------------------------------------------

Once you have new aggregated metrics created by aggregation rules, you can drop the raw unaggregated data for
``http.server.duration``.

Scenario for metrics pipeline management
===============================================================================

See :ref:`aggregate-drop-use-case`.

Create your first metric rules
===============================================================================

To start using metrics pipeline management, see :ref:`use-metrics-pipeline`.

.. note:: Metrics pipeline management is not available for metrics ingested through the ``https://ingest.signalfx.com/v1/collectd`` endpoint.
