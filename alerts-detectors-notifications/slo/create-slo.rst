.. _create-slo:

*****************************************************************************************
Measure and track your service health metrics with service level objectives (SLOs)
*****************************************************************************************


.. meta::
    :description: Learn how to create a service level objective (SLO) in Splunk Observability Cloud.

For each service that you use to indicate system health in Splunk Observability Cloud, you can define an SLO and how to measure it.


Create an SLO
=================

Follow these steps to create an SLO.

#. From the landing page of Splunk Observability Cloud, go to :strong:`Detectors & SLOs`.
#. Select the :strong:`SLOs` tab.
#. Select :guilabel:`Create SLO`.
#. Configure the service level indicator (SLI) for your SLO.

    To use a service as the system health indicator for your SLI configuration, follow these steps:

        .. list-table::
          :header-rows: 1
          :widths: 40 60
          :width: 100%

          * - :strong:`Field name`
            - :strong:`Actions`
          * - :guilabel:`Metric type`
            - Select :guilabel:`Service & endpoint` from the dropdown menu
          * - :guilabel:`Environment`
            - Open the dropdown menu and check the boxes for the environments where you want to apply this SLO
          * - :guilabel:`Service:endpoint`
            - * Search for the service you want to create an SLO for
              * (Optional) Add an endpoint for the selected service
          * - :guilabel:`Indicator type`
            - Select either success rate or latency to use as the measurement for your SLO target:
              
              * Request success: Measure the proportion of requests that result in a successful response over the duration of the compliance window
              
              * Request latency: Measure the proportion of requests that load within the specified latency over the duration of the compliance window
          * - :guilabel:`Filters`
            - Enter any additional dimension names and values you want to apply this SLO to. Alternatively, use the ``NOT`` filter, represented by an exclamation point ( ! ), to exclude any dimension values from this SLO configuration.

    To use a metric of your choice as the system health indicator for your SLI configuration, follow these steps:

        #. For the :guilabel:`Metric type` field, select :guilabel:`Custom metric` from the dropdown menu. The SignalFlow editor appears.
        #. In the SignalFlow editor, you can see the following code sample:

              .. code-block:: python

                  G = data('good.metric', filter=filter('sf_error', 'false'))
                  T = data('total.metric')
              
              * Line 1 defines ``G`` as a data stream of ``good.metric`` metric time series (MTS). The SignalFlow ``filter()`` function queries for a collection of MTS with value ``false`` for the ``sf_error`` dimension. The filter distinguishes successful requests from total requests, making ``G`` good events variable.
              * Line 2 defines ``T`` as a data stream ``total.metric`` MTS. ``T`` is the total events variable.

           Replace the code sample with your own SignalFlow program. You can define good events and total events variables using any metric and supported SignalFlow function. For more information, see :new-page:`Analyze data using SignalFlow <https://dev.splunk.com/observability/docs/signalflow>` in the Splunk Observability Cloud Developer Guide.
        
        #. Select appropriate variable names for the :guilabel:`Good events (numerator)` and :guilabel:`Total events (denominator)` dropdown menus.

        .. note:: Custom metric SLO works by calculating the percentage of successful requests over a given compliance period. This calculation works better for counter and histogram metrics than for gauge metrics. Gauge metrics are not suitable for custom metric SLO, so you might get confusing data when selecting gauge metrics in your configuration.

#. Define your SLO and how to measure it.

    .. list-table::
      :header-rows: 1
      :widths: 20 80
      :width: 100%
      
      * - :strong:`Field name`
        - :strong:`Actions`
      * - :guilabel:`Target (%)`
        - Enter the target you want to set for this SLO. 
      * - :guilabel:`Latency (ms)`
        - Only available and required for request latency SLI type. Enter the target loading time for your service requests.
      * - :guilabel:`Compliance window`
        - Select a compliance window for this SLO from the dropdown menu.

#. Set up alerting for your SLO. You can subscribe to get notifications for the following alerts.

    .. list-table::
      :header-rows: 1
      :widths: 20 80
      :width: 100%

      * - :strong:`Alert`
        - :strong:`Description`
      * - :guilabel:`Breach event`
        - | Alerts when the service level indicator (SLI) doesn't meet the target over the specified compliance window. 
          | :strong:`Note:` Breach event alerting is selected by default and always runs in the background.
      * - :guilabel:`Error budget`
        - Alerts when the remaining error budget is less than 10% of the estimated error budget for the compliance window.
      * - :guilabel:`Burn rate`
        - Alerts when the rate of consumption of your SLO error budget exceeds a healthy threshold for the specified compliance window. To learn more, see :ref:`burn-rate-alerts`.

#. Splunk Observability Cloud automatically generates a name for your SLO. You can change this auto-generated name, as long as the SLO name is unique.

#. Select :guilabel:`Create` to create the SLO.







