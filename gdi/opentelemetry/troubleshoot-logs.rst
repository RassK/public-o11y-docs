.. _tshoot-logs:

****************************************************************
Troubleshoot log collection
****************************************************************

.. meta::
      :description: Describes known issues when collecting logs with the Splunk Distribution of the OpenTelemetry Collector.

This document describes common issues related to log collection with the Collector.

.. note:: 
  
  To collect logs see:

  * :ref:`kubernetes-config-logs`
  * :ref:`linux-config-logs`
  * :ref:`windows-config-logs`
  
To troubleshoot the health and performance of the Collector see the :new-page:`OpenTelemetry Project troublehooting docs <https://opentelemetry.io/docs/collector/troubleshooting>`. It includes information about troubleshooting tools and debugging.

My source isn't generating logs
=========================================

If using Linux, run the following commands to check if the source is generating logs:

.. code-block:: bash

  tail -f /var/log/myTestLog.log
  journalctl -u my-service.service -f


If using Windows, run the following command to check if the source is generating logs:

.. code-block:: shell

  Get-Content myTestLog.log 

.. _fluentd-collector-troubleshooting:

Fluentd isn't configured correctly
=========================================

Do the following to check the Fluentd configuration:

#. Check that td-agent is running. On Linux, run ``systemctl status td-agent``. On Windows, run ``Get-Service td-agent``.
#. If you changed the configuration, restart Fluentd. On Linux, run ``systemctl restart td-agent``. On Windows, run ``Restart-Service -Name td-agent``.
#. Check fluentd.conf and conf.d/\*. ``@label @SPLUNK`` must be added to every source to activate log collection.
#. Manual configuration might be required to collect logs off the source. Add configuration files to in the conf.d directory as needed.
#. Activate debug logging in fluentd.conf (``log_level debug``), restart td-agent, and check that the source is generating logs.

While every attempt is made to properly configure permissions, it is possible that td-agent does not have the permission required to collect logs. Debug logging should indicate this issue.

It's possible that the ``<parser>`` section configuration does not match the log events.

If you see a message such as "2021-03-17 02:14:44 +0000 [debug]: #0 connect new socket", Fluentd is working as expected. You need to activate debug logging to see this message.

The Collector isn't configured properly
=========================================

.. note:: Fluentd is part of the Splunk Distribution of OpenTelemetry Collector, but deactivated by default for Linux and Windows. To activate it, use the ``--with-fluentd`` option when installing the Collector for Linux, or the ``with_fluentd = 1`` option when installing the Collector for Windows.

Do the following to check the Collector configuration:

#. Go to ``http://localhost:55679/debug/tracez`` to check zPages for samples. You might need to configure the endpoint.
#. Activate logging exporter. See :ref:`logging-exporter` for more information.
#. Run ``journalctl -u splunk-otel-collector.service -f`` to collect the logs for you to review.
#. Review :ref:`otel-splunk-collector-tshoot` if you can't find what you need in the logs.

Test the Collector by sending synthetic data
==================================================================================

You can manually generate logs. By default, Fluentd monitors journald and /var/log/syslog.log for events.

.. code-block:: bash

  echo "2021-03-17 02:14:44 +0000 [debug]: test" >>/var/log/syslog.log
  echo "2021-03-17 02:14:44 +0000 [debug]: test" | systemd-cat

.. caution:: Fluentd requires properly structured syslog to pick up the log line.

.. _unwanted_profiling_logs:

Unwanted profiling logs appearing in Splunk Observability Cloud
==================================================================================

By default, the Splunk Distribution of the OpenTelemetry Collector sends AlwaysOn Profiling data using the Splunk HEC exporter. See :ref:`no_profiling_data` for more information.

.. _disable_log_collection:

Exclude log data in the Collector
==================================================================================

Depending on its configuration, the Splunk Distribution of the OpenTelemetry Collector might collect and send logs to Splunk Observability Cloud through a ``logs`` pipeline that uses the Splunk HEC exporter. 

To turn off logs colletion, see :ref:`exclude-log-data` for more information.

Send logs to Splunk Cloud Platform or Enterprise using the Collector
==================================================================================

To send logs from the Collector to Splunk Cloud Platform or Splunk Enterprise, see :ref:`send_logs_to_splunk`.

