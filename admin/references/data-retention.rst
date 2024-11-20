.. _data-o11y:

******************************************************
Data retention in Splunk Observability Cloud
******************************************************

.. meta::
   :description: Data retention for Splunk Observability Cloud.

The following sections list the data retention for each product in Splunk Observability Cloud.

.. note:: 

  The data retention listed in each section applies to real-time metrics. Archived metrics have a retention period of 31 days, and this period doesn't depend on the Splunk Observability Cloud product.

  To learn more about archived metrics, see :ref:`archived-metrics-intro`.

.. _im-data-retention:

Data retention in Infrastructure Monitoring
====================================================

The following table shows the retention time period for each data type in Infrastructure Monitoring. 

.. list-table:: 
   :widths: 25 25
   :header-rows: 1
   :width: 100%

   * - :strong:`Data resolution`
     - :strong:`Retention`
   * - 1 second resolution
     - 
       * Standard license edition: 8 days
       * Enterprise license edition: 3 months 
   * - 10 seconds or more
     - 
       * 13 months

.. _rum-data-retention:

Data retention in Real User Monitoring (RUM)
==============================================

The following table shows the retention time period for each data type in RUM. 

.. list-table:: 
   :widths: 25 25
   :header-rows: 1
   :width: 100%

   * - :strong:`Data type`
     - :strong:`Retention`
   * - Spans 
     - 
       * 8 days
   * - :ref:`Troubleshooting MetricSets <troubleshooting-metricsets>` 
     - 
       * 8 days
   * - :ref:`Monitoring MetricSets <monitoring-metricsets>`
     - 
       * 13 months 
   * - :ref:`Session Replay <rum-session-replay>` 
     - 
       * 8 days

.. _apm-data-retention:

Data retention in Application Performance Monitoring (APM)
=====================================================================
The following table shows the retention time period for each data type in APM. 

.. list-table:: 
   :widths: 20 25
   :header-rows: 1
   :width: 100%

   * - :strong:`Data type`
     - :strong:`Retention`
   * - Traces
     - 
        * All raw traces: 8 days
        * Traces of interest viewed in the Splunk APM user interface: up to 13 months. See :ref:`apm-extended-trace-retention` to learn how to extend the retention of specific traces of interest. 
   * - :ref:`Troubleshooting MetricSets <troubleshooting-metricsets>`
     - 
       * 8 days   
   * - :ref:`Monitoring MetricSets <monitoring-metricsets>`
     - 
       * 13 months 
   * - :ref:`Profiling data <profiling-intro>`
     - 
       * 8 days

.. _oncall-data-retention:

Data retention in Splunk On-Call
============================================

Data collected by Splunk On-Call is retained unless you request that your data be deleted. For more information, see :new-page:`Splunk On-Call Security FAQ <https://www.splunk.com/en_us/support-and-services/on-call-security-faq.html>`.

Data retention in Splunk Synthetic Monitoring 
===============================================

The following table shows the retention time period for each data type in Splunk Synthetic Monitoring. 

.. list-table:: 
   :widths: 25 25
   :header-rows: 1
   :width: 100%

   * - :strong:`Data type`
     - :strong:`Retention`
   * - Run results  
     - 
       * Standard license edition: 8 days
       * Enterprise license edition: 3 months 
   * - Metric data 
     - 
       * 13 months for both Standard and Enterprise.
