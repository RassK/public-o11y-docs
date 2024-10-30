.. _get-started-java:

************************************************************
Instrument Java applications for Splunk Observability Cloud
************************************************************

.. meta::
   :description: Instrument Java applications automatically to export spans and metrics to Splunk Observability Cloud.

.. toctree::
   :hidden:

   Requirements <java-otel-requirements>
   Instrument your Java application <instrumentation/instrument-java-application>
   Instructions for app servers <instrumentation/java-servers-instructions>
   Metrics and attributes <configuration/java-otel-metrics-attributes>
   Connect trace data with logs <instrumentation/connect-traces-logs>
   Configure the Java agent <configuration/advanced-java-otel-configuration>
   Manual instrumentation <instrumentation/java-manual-instrumentation>
   Performance overhead <performance>
   Troubleshoot the Java agent <troubleshooting/common-java-troubleshooting>
   Migrate to metrics 2.x <migrate-metrics>
   Migrate from SignalFx Java agent <troubleshooting/migrate-signalfx-java-agent-to-otel>
   Version 1.x (Deprecated) <version1x/get-started-1x>
   About Splunk OTel Java <splunk-java-otel-distribution>

The Splunk Distribution of OpenTelemetry Java provides a Java Virtual Machine (JVM) agent that automatically adds APM instrumentation to your Java application or service. The instrumentation captures distributed traces and sends them to Splunk Observability Cloud. See :ref:`splunk-java-otel-dist` for more information.

To instrument your Java application, follow these steps:

#. Check compatibility and requirements. See :ref:`java-otel-requirements`.
#. Instrument your Java application. See :ref:`instrument-java-applications`.
#. Configure your instrumentation. See :ref:`configure-java-instrumentation`.

You can also automatically instrument your Java applications along with the Splunk Distribution of OpenTelemetry Collector installation. Automatic discovery and configuration removes the need to install and configure the Java agent separately. See :ref:`discovery_mode` for more information.

