.. _about-collector-configuration-tutorial-k8s:

*****************************************************************************************
Tutorial: Configure the Splunk Distribution of the OpenTelemetry Collector on Kubernetes
*****************************************************************************************

.. meta::
    :description: Follow this tutorial for a walkthrough of configuring the Splunk Distribution of OpenTelemetry Collector to collect telemetry in a Kubernetes environment.

.. toctree::
   :hidden:
   :maxdepth: 3

   collector-config-tutorial-start
   collector-config-tutorial-edit

The Splunk Distribution of the OpenTelemetry Collector is a :new-page:`distribution <https://docs.splunk.com/Splexicon:Distribution>` of the OpenTelemetry Collector that includes components, installers, and default settings so that it's ready to work with Splunk Observability Cloud.

Follow this tutorial for a walkthrough of configuring the Splunk Distribution of OpenTelemetry Collector to collect telemetry in common situations.

.. raw:: html

    <h2>What's in this tutorial</h2>

After completing this tutorial, you can accomplish the following tasks:

1. Create a demo Kubernetes cluster on your machine.

2. Configure the OpenTelemetry Collector using the Helm chart.

3. Configure the Collector to filter and send logs to Splunk Cloud Platform.

.. raw:: html

    <h2>How to use this tutorial</h2>

Follow the tutorial parts in order:

1. Set up and monitor a Kubernetes cluster on your machine. See :ref:`collector-config-tutorial-start-k8s`.

2. Edit and apply the Collector configuration to filter and send logs to Splunk Cloud Platform. See :ref:`collector-config-tutorial-edit-k8s`.

.. raw:: html

    <h2>Prerequisites</h2>

To get the most out of this tutorial, you need an operating system capable of running Minikube and Helm, such as Linux, Windows, or macOS. This tutorial uses macOS as the operating system.

Additionally, you must have your Splunk Observability Cloud realm and access token to deploy the Splunk Distribution of OpenTelemetry Collector. For more information, see :ref:`admin-org-tokens`.

To send logs to Splunk Cloud Platform, find your Splunk HTTP Event Collector (HEC) endpoint and token. See :ref:`hec-endpoints` for more information.

This tutorial uses a Splunk Cloud Platform free trial account.

.. raw:: html

    <h2> Get started </h3>

To get started with the tutorial, see :ref:`collector-config-tutorial-start-k8s`.
