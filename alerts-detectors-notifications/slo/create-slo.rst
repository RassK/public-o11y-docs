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
#. Select the service or metric you want to use as the system health indicator for your SLO.

    To use a service as the health indicator for your SLO, follow these steps:

        .. list-table::
          :header-rows: 1
          :widths: 40 60
          :width: 100%

          * - :strong:`Field name`
            - :strong:`Actions`
          * - Metric type
            - Select :guilabel:`Service & endpoint` from the dropdown menu
          * - Environment
            - Open the dropdown menu and check the boxes for the environments where you want to apply this SLO
          * - Service\:\endpoint
            - * Search for the service you want to create an SLO for
              * (Optional) Add an endpoint for the selected service
          * - Filters
            - Enter any additional dimension names and values you want to apply this SLO to

    To use a custom metric as the health indicator for your SLO, follow these steps:

        .. list-table::
          :header-rows: 1
          :widths: 40 60
          :width: 100%

          * - :strong:`Field name`
            - :strong:`Actions`
          * - Metric type
            - Select :guilabel:`Custom metric` from the dropdown menu
          * - Numerator metric
            - Search for the metric you want to use for the success request count
          * - Denominator metric
            - Search for the metric you want to use for the total request count

#. Define your SLO and how to measure it.

    .. list-table::
      :header-rows: 1
      :widths: 20 80
      :width: 100%
      
      * - :strong:`Field name`
        - :strong:`Actions`
      * - Target (%)
        - Enter the target you want to set for this SLO. 
      * - Compliance window
        - Select a compliance window for this SLO from the dropdown menu.

#. Set up alerting for your SLO. You can subscribe to get notifications for the following alerts.

    .. list-table::
      :header-rows: 1
      :widths: 20 80
      :width: 100%

      * - :strong:`Alert`
        - :strong:`Description`
      * - Breach event
        - | Alerts when the service level indicator (SLI) doesn't meet the target over the specified compliance window. 
          | :strong:`Note:` Breach event alerting is selected by default and always runs in the background.
      * - Error budget
        - Alerts when the remaining error budget is less than 10% of the estimated error budget for the compliance window.
      * - Burn rate
        - Alerts when the rate of consumption of your SLO error budget exceeds a healthy threshold for the specified compliance window. To learn more, see :ref:`burn-rate-alerts`.

#. Splunk Observability Cloud automatically generates a name for your SLO. You can change this auto-generated name, as long as the SLO name is unique.

#. Select :guilabel:`Create` to create the SLO.







