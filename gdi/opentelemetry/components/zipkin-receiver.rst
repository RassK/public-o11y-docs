.. _zipkin-receiver:

*************************
Zipkin receiver
*************************

.. meta::
      :description: The Zipkin receiver receives spans from Zipkin versions 1 and 2.

The Zipkin receiver gathers spans from Zipkin versions 1 and 2. The supported pipeline type is ``traces``. See :ref:`otel-data-processing` for more information.

Get started
======================

Follow these steps to configure and activate the component:

1. Deploy the Splunk Distribution of OpenTelemetry Collector to your host or container platform:

  - :ref:`otel-install-linux`
  - :ref:`otel-install-windows`
  - :ref:`otel-install-k8s`

2. Configure the Zipkin receiver as described in the next section.
3. Restart the Collector.

Sample configurations
--------------------------------

To activate the Zipkin receiver, add ``zipkin`` to the ``receivers`` section of your configuration file, as in the following sample configurations. 

See :ref:`zipkin-receiver-settings` for more details.

.. code-block:: yaml


  receivers:
    zipkin:
      endpoint: 0.0.0.0:9412

The following settings are configurable:

* ``endpoint``: ``host:port`` on which the receiver is going to receive data. ``0.0.0.0:9411`` by default.
* ``parse_string_tags``: If enabled, the receiver attempts to parse string tags or binary annotations into int/bool/float. ``false`` by default.

Additional settings
^^^^^^^^^^^^^^^^^^^^^^^^^^

The Zipkin receiver uses helper files for additional capabilities:

* HTTP settings, including CORS. See more in GitHub at :new-page:`HTTP Configuration Settings <https://github.com/open-telemetry/opentelemetry-collector/blob/main/config/confighttp/README.md#server-configuration>`
* TLS and mTLS settings. Learn more in GitHub at :new-page:`TLS Configuration Settings <https://github.com/open-telemetry/opentelemetry-collector/blob/main/config/configtls/README.md>`.

.. _zipkin-receiver-settings:

Settings
======================

The following table shows the configuration options for the Zipkin receiver:

.. raw:: html

  <div class="metrics-standard" category="included" url="https://raw.githubusercontent.com/splunk/collector-config-tools/main/cfg-metadata/receiver/zipkin.yaml"></div>

Troubleshooting
======================

.. include:: /_includes/troubleshooting-components.rst
