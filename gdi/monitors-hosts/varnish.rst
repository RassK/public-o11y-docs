.. _telegraf-varnish:

Varnish
=======

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Telegraf Varnish monitor. See benefits, install, configuration, and metrics

The Splunk Distribution of OpenTelemetry Collector uses the Smart Agent receiver with the
``telegraf/varnish`` monitor type to collect Varnish metrics.

This integration is available on Kubernetes and Linux.

Benefits
--------

.. include:: /_includes/benefits.rst

Requirements
------------

This integration uses the ``varnishstat`` command. The Collector or
Smart Agent must run on the same host as the Varnish server.

Run the following command to activate the Smart Agent to run the
``varnishstat`` command:

.. code:: bash

   usermod -a -G varnish signalfx-agent

Installation
------------

.. include:: /_includes/collector-installation-linux.rst

Configuration
-------------

.. include:: /_includes/configuration.rst

Example
~~~~~~~

To activate this integration, add the following to your Collector
configuration:

.. code:: yaml

   receivers:
     smartagent/varnish:
       type: telegraf/varnish
       ...  # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/varnish]

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

The following table shows the configuration options for this monitor:

.. list-table::
   :widths: 18 18 18 18
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``useSudo``
      - No
      - ``bool``
      - If running as a restricted user, activate this flag to use
         ``sudo``. The default value is ``false``.
   - 

      - ``binary``
      - No
      - ``string``
      - The location of the ``varnishstat`` binary. The default value is
         ``/usr/bin/varnishstat``.
   - 

      - ``stats``
      - No
      - ``list of strings``
      - Which stats to gather. You can use glob matching. For example,
         ``stats = ["MAIN.*"]``. The default value is ``[MAIN.*]``.
   - 

      - ``instanceName``
      - No
      - ``string``
      - Optional name for the Varnish instance to query. The setting has
         the same effect of passing the ``-n`` parameter value.

Metrics
-------

The following metrics are available for this integration:

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/telegraf/monitors/varnish/metadata.yaml"></div>


Notes
~~~~~

.. include:: /_includes/metric-defs.rst

Troubleshooting
---------------

.. include:: /_includes/troubleshooting-components.rst
