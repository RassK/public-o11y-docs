.. _otel-kubernetes-config:

*********************************************************************************
Configure the Collector for Kubernetes with Helm
*********************************************************************************

.. meta::
      :description: Optional configurations for the Splunk Distribution of OpenTelemetry Collector for Kubernetes.

After you've :ref:`installed the Collector for Kubernetes <otel-install-k8s>`, these are the available settings you can configure. Additionally, see also :ref:`the advanced configuration options <otel-kubernetes-config-advanced>` and :ref:`otel-kubernetes-config-logs`.

.. caution:: 

  The :new-page:`values.yaml <https://github.com/signalfx/splunk-otel-collector-chart/blob/main/helm-charts/splunk-otel-collector/values.yaml>` file lists and explains all supported configurable parameters for the Helm chart components. See :ref:`helm-chart-components` for more information. :strong:`Review it to understand how to configure this chart`.

  The Helm chart can also be configured to support different use cases, such as trace sampling and sending data through a proxy server. See :new-page:`Examples of chart configuration <https://github.com/signalfx/splunk-otel-collector-chart/blob/main/examples/README.md>` for more information.

.. _otel-kubernetes-config-distro:

Configure the Kubernetes distribution
============================================

If applicable, use the ``distribution`` parameter to provide information about the underlying Kubernetes deployment. This parameter allows the connector to automatically scrape additional metadata. The following options are supported:

* ``aks`` for Azure AKS
* ``eks`` for Amazon EKS
* ``eks/fargate`` for Amazon EKS with Fargate profiles
* ``gke`` for Google GKE or Standard mode
* ``gke/autopilot`` for Google GKE or Autopilot mode
* ``openshift`` for Red Hat OpenShift

Apply the following to configure your distribution:

.. code-block:: bash

  # aks deployment
  --set distribution=aks,cloudProvider=azure 

  # eks deployment
  --set distribution=eks,cloudProvider=aws 

  # eks/fargate deployment (with recommended gateway)
  --set distribution=eks/fargate,gateway.enabled=true,cloudProvider=aws 

  # gke deployment
  --set distribution=gke,cloudProvider=gcp 

  # gke/autopilot deployment
  --set distribution=gke/autopilot,cloudProvider=gcp 

  # openshift deployment (openshift can run on multiple cloud providers, so cloudProvider is excluded here)
  --set distribution=openshift   

For example:

.. code-block:: yaml

  splunkObservability:
    accessToken: xxxxxx
    realm: us0
  clusterName: my-k8s-cluster
  distribution: gke

Configure Google Kubernetes Engine 
-----------------------------------------------------------------------------

Configure GKE Autopilot
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To run the Collector in GKE Autopilot mode, set the ``distribution`` option to ``gke/autopilot``:

.. code-block:: yaml

  distribution: gke/autopilot

Search for "Autopilot overview" on the :new-page:`Google Cloud documentation site <https://cloud.google.com/docs>` for more information.

.. note:: GKE Autopilot doesn't support native OpenTelemetry logs collection.

The Collector agent daemonset can have problems scheduling in Autopilot mode. If this happens, do the following to assign the daemonset a higher priority class to ensure that the daemonset pods are always present on each node:

1. Create a new priority class for the Collector agent:

  .. code-block:: yaml

    cat <<EOF | kubectl apply -f -
    apiVersion: scheduling.k8s.io/v1
    kind: PriorityClass
    metadata:
      name: splunk-otel-agent-priority
    value: 1000000
    globalDefault: false
    description: "Higher priority class for the Splunk Distribution of OpenTelemetry Collector pods."
    EOF

2. Use the created priority class in the helm install/upgrade command using the ``--set="priorityClassName=splunk-otel-agent-priority"`` argument, or add the following line to your custom values.yaml:

  .. code-block:: yaml


    priorityClassName: splunk-otel-agent-priority

GKE ARM support
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The default configuration of the Helm chart supports ARM workloads on GKE. Make sure to set the distribution value to ``gke``:

.. code-block:: yaml


  distribution: gke

.. _config-eks-fargate:

Configure Amazon Elastic Kubernetes Service Fargate
-----------------------------------------------------------------------------

To run the Collector in the Amazon EKS with Fargate profiles, set the required ``distribution`` value to ``eks/fargate``, as shown in the following example:

.. code-block:: yaml

  distribution: eks/fargate

.. note:: Fluentd and native OpenTelemetry logs collection are not automatically configured in EKS with Fargate profiles.

This distribution operates similarly to the ``eks`` distribution, but with the following distinctions:

* The Collector agent daemonset is not applied since Fargate does not support daemonsets. Any desired Collector instances running as agents must be configured manually as sidecar containers in your custom deployments. This includes any application logging services like Fluentd. Set ``gateway.enabled`` to ``true`` and configure your instrumented applications to report metrics, traces, and logs to the gateway ``<installed-chart-name>-splunk-otel-collector`` service address. Any desired agent instances that would run as a daemonset should instead run as sidecar containers in your pods.
* Since Fargate nodes use a VM boundary to prevent access to host-based resources used by other pods, pods are not able to reach their own kubelet. The cluster receiver for the Fargate distribution has two primary differences between regular ``eks`` to work around this limitation:
   * The configured cluster receiver is deployed as a two-replica StatefulSet instead of a Deployment, and uses a Kubernetes Observer extension that discovers the cluster's nodes and, on the second replica, its pods for user-configurable receiver creator additions.Using this observer dynamically creates the Kubelet Stats receiver instances that report kubelet metrics for all observed Fargate nodes. The first replica monitors the cluster with a ``k8s_cluster`` receiver, and the second cluster monitors all kubelets except its own (due to an EKS/Fargate networking restriction).
   * The first replica's Collector monitors the second's kubelet. This is made possible by a Fargate-specific ``splunk-otel-eks-fargate-kubeletstats-receiver-node`` node label. The Collector ClusterRole for ``eks/fargate`` allows the ``patch`` verb on ``nodes`` resources for the default API groups to allow the cluster receiver's init container to add this node label for designated self monitoring.

.. _otel-kubernetes-config-clustername:

Configure the cluster name
============================================

Use the ``clusterName`` parameter to specify the name of the Kubernetes cluster. This parameter is optional for the ``eks``, ``eks/fargate``, ``gke``, and ``gke/autopilot`` distributions, but required for all of others.

Apply the following to configure your cluster name:

.. code-block:: bash

  --set clusterName=my-k8s-cluster

For example:

.. code-block:: yaml

  clusterName: my-k8s-cluster

.. _otel-kubernetes-config-environment:

Configure the deployment environment
===========================================

If applicable, use the ``environment`` parameter to specify an additional ``deployment.environment`` attribute to be added to all telemetry data. This attribute helps Splunk Observability Cloud users investigate data coming from different sources separately. Example values include ``development``, ``staging``, and ``production``.

.. code-block:: yaml

  splunkObservability:
    accessToken: xxxxxx
    realm: us0
  environment: production

.. _otel-kubernetes-config-cloud:

Configure a cloud provider
=================================

If applicable, use the ``cloudProvider`` parameter to provide information about your cloud provider. The following options are supported:

* ``aws`` for Amazon Web Services
* ``gcp`` for Google Cloud Platform
* ``azure`` for Microsoft Azure

To set your cloud provider and configure ``cloud.platform`` for the resource detection processor, use: 

.. code-block:: bash

  --set cloudProvider={azure|gcp|eks|openshift} 

For example:

.. code-block:: yaml

  splunkObservability:
    accessToken: xxxxxx
    realm: us0
  clusterName: my-k8s-cluster
  cloudProvider: aws

.. _otel-kubernetes-config-add-components:

Add additional components to the configuration
======================================================

To use any additional OTel component, integration or legacy monitor, add it the relevant sections of the configuration file. Depending on your requirements, you might want to include it in the ``agent`` or the ``clusterReceiver`` component section of the configuration. See more at :ref:`helm-chart-components`.

For a full list of available components and how to configure them, see :ref:`otel-components`. For a list of available application integrations, see :ref:`monitor-data-sources`.

How to collect data: agent or cluster receiver?
-----------------------------------------------------------------------------

Read the following table to decide which option to chose to collect your data:

.. list-table:: 
  :header-rows: 1
  :width: 100%
  :widths: 20 40 40 

  * - 
    - Collect via the Collector agent 
    - Collect via the Collector cluster receiver 

  * - Where is data collected?
    - At the node level.
    - At the Kubernetes service level, through a single point.

  * - Advantages
    - * Granularity: This option ensures that you capture the complete picture of your cluster's performance and health. 
      * Fault tolerance: If a node becomes isolated or experiences issues, its metrics are still being collected independently. This gives you visibility into problems affecting individual nodes.
    - Simplicity: This option simplifies the setup and management. 

  * - Considerations
    - Complexity: Managing and configuring agents on each node can increase operational complexity, specifically agent config file management.
    - Uncomplete data: This option might result in a partial view of your cluster's health and performance. If the service collects metrics only from a subset of nodes, you might miss critical metrics from parts of your cluster.

  * - Use cases
    - - Use this in environments where you need detailed insights into each node's operations. This allows better issue diagnosing and optimizing performance. 
      - Use this to collect metrics from application pods that have multiple replicas that can be running on multiple nodes.
    - Use this in environments where operational simplicity is a priority, or if your cluster is already simple and has only 1 node.

Example: Add the MySQL receiver
-----------------------------------------------------------------------------

This example shows how to add the :ref:`mysql-receiver` to your configuration file.

Add the MySQL receiver in the ``agent`` section
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To use the Collector agent daemonset to collect ``mysql`` metrics from every node the agent is deployed to, add this to your configuration:

.. code:: yaml

  agent:
    config:
      receivers:
        mysql:
          endpoint: localhost:3306
          ...

Add the MySQL receiver in the ``clusterReceiver`` section
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To use the Collector cluster receiver deployment to collect ``mysql`` metrics from a single endpoint, add this to your configuration:

.. code:: yaml

  clusterReceiver:
    config:
      receivers:
        mysql:
          endpoint: mysql-k8s-service:3306
          ...

Example: Add the Rabbit MQ monitor
-----------------------------------------------------------------------------

This example shows how to add the :ref:`rabbitmq` integration to your configuration file.

Add RabbitMQ in the ``agent`` section
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you want to activate the RabbitMQ monitor in the Collector agent daemonset, add ``mysql`` to the ``receivers`` section of your agent section in the configuration file:

.. code:: yaml

  agent:
    config:
      receivers:
        smartagent/rabbitmq:
          type: collectd/rabbitmq
          host: localhost
          port: 5672
          username: otel
          password: ${env:RABBITMQ_PASSWORD}

Next, include the receiver in the ``metrics`` pipeline of the ``service`` section of your configuration file:

.. code:: yaml

  service:
    pipelines:
      metrics:
        receivers:
          - smartagent/rabbitmq

Add RabbitMQ in the ``clusterReceiver`` section
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Similarly, if you want to activate the RabbitMQ monitor in the cluster receiver, add ``mysql`` to the ``receivers`` section of your cluster receiver section in the configuration file:

.. code:: yaml

  clusterReceiver:
    config:
      receivers:
        smartagent/rabbitmq:
          type: collectd/rabbitmq
          host: rabbitmq-service
          port: 5672
          username: otel
          password: ${env:RABBITMQ_PASSWORD}

Next, include the receiver in the ``metrics`` pipeline of the ``service`` section of your configuration file:

.. code:: yaml

  service:
    pipelines:
      metrics:
        receivers:
          - smartagent/rabbitmq

.. _otel-kubernetes-config-hostnetwork:

Configure the agent's use of the host network
======================================================

By default, ``agent.hostNetwork`` is set to ``true``. This grants DaemonSet pods of the agent access to the node's host network, allowing them to monitor specific elements. Enable this setting to monitor certain control plane components and integrations that require host network access.

Set ``agent.hostNetwork`` to ``false`` to turn off host network access. This might be necessary to comply with certain organization security policies. If host network access is disabled, the agent's monitoring capabilities might be limited.

This value is disregarded for Windows.

Activate AlwaysOn Profiling
=================================

AlwaysOn Profiling in Splunk APM continuously captures stack traces, helping you identify performance bottlenecks or issues in your code. Activating profiling lets your Kubernetes applications produce and forward this data to Splunk Observability Cloud for visualization. 

The Collector ingests profiling data using the ``logs`` pipeline.

Learn more at :ref:`zero-config` and :ref:`profiling-intro`.

Set up profiling 
-------------------------------------------

You can activate profiling while installing the Collector for Kubernetes using the UI wizard, or by modifying your configuration files.

Profiling uses two main components: the Collector, responsible for receiving and exporting the profiling data to Splunk Observability Cloud, and the Operator, which auto-instruments applications so they can generate and emit traces along with profiling data. 

There are two main scenarios:

* Profiling using both Collector and Operator: The Operator auto-instruments your applications, which then send the profiling data to the Collector.
* Profiling using only the Collector: You manually instrument your applications to generate profiling data, which is then sent directly to the Collector.

Activate profiling with the Collector and the Operator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To activate profiling with the Collector and the Operator, activate the :guilabel:`Profiling` option in the UI, or deploy the Helm chart with the following configuration:

For the Collector:

.. code-block:: yaml

  splunkObservability:
    accessToken: CHANGEME
    realm: us0
    logsEnabled: true
    profilingEnabled: true

For the Operator:

.. code-block:: yaml

  operator:
    enabled: true

Additionally, deploy the cert-manager for the Operator if it hasn't been already.

.. code-block:: yaml
  
  certmanager:
    enabled: true

With the above configuration:

* The Collector is set up to receive profiling data.
* The Operator is deployed and auto-instruments applications based on target pod annotations, allowing these applications to generate profiling data.

Activate profiling only with the Collector
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you want to only use the Collector and have manually instrumented applications, ensure that ``splunkObservability.logsEnabled=true`` and ``splunkObservability.profilingEnabled=true`` is set in your configuration.

.. caution:: With this option, you need to manually set up instrumented applications to send profiling data directly to the Collector.

Provide tokens as a secret
=================================

Instead of having the tokens as clear text in the config file, you can provide them as a secret created before deploying the chart. See :new-page:`secret-splunk.yaml <https://github.com/signalfx/splunk-otel-collector-chart/blob/main/helm-charts/splunk-otel-collector/templates/secret-splunk.yaml>` for the required fields.

.. code-block:: yaml

  secret:
    create: false
    name: your-secret

.. _otel-kubernetes-config-resources:

Add additional telemetry sources
===========================================

Use the ``autodetect`` configuration option to activate additional telemetry sources.

Set ``autodetect.prometheus=true`` if you want the Collector to scrape Prometheus metrics from pods that have generic Prometheus-style annotations. Add the following annotations on pods to allow a fine control of the scraping process:

* ``prometheus.io/scrape: true``: The default configuration scrapes all pods. If set to ``false``, this annotation excludes the pod from the scraping process.
* ``prometheus.io/path``: The path to scrape the metrics from. The default value is ``/metrics``.
* ``prometheus.io/port``: The port to scrape the metrics from. The default value is ``9090``.

If the Collector is running in an Istio environment, set ``autodetect.istio=true`` to make sure that all traces, metrics, and logs reported by Istio are collected in a unified manner.

For example, use the following configuration to activate automatic detection of both Prometheus and Istio telemetry sources:

.. code-block:: yaml

  splunkObservability:
    accessToken: xxxxxx
    realm: us0
  clusterName: my-k8s-cluster
  autodetect:
    istio: true
    prometheus: true

.. _otel-kubernetes-discovery-mode:

Activate discovery mode on the Collector
============================================

Use the discovery mode of the Splunk Distribution of OpenTelemetry Collector to detect metric sources and create
a configuration based on the results.

See :ref:`discovery-mode-k8s` for instructions on how to activate discovery mode in the Helm chart.

.. _otel-kubernetes-deactivate-telemetry:

Deactivate particular types of telemetry
============================================

By default, OpenTelemetry sends only metrics and traces to Splunk Observability Cloud and sends only logs to Splunk Platform. You can activate or deactivate any kind of telemetry data collection for a specific destination. 

For example, the following configuration allows the Collector to send all collected telemetry data to Splunk Observability Cloud and the Splunk Platform if you've properly configured them:

.. code-block:: yaml

  splunkObservability:
    metricsEnabled: true
    tracesEnabled: true
    logsEnabled: true
  splunkPlatform:
    metricsEnabled: true
    logsEnabled: true

Configure Windows worker nodes
===============================================

The Splunk Distribution of OpenTelemetry Collector for Kubernetes supports collecting metrics, traces, and logs (using OpenTelemetry native logs collection only) from Windows nodes. All Windows images are available in the ``quay.io/signalfx/splunk-otel-collector-windows`` repository.

Use the following configuration to install the Helm chart on Windows worker nodes:

.. code-block:: yaml

  isWindows: true
  image:
    otelcol:
      repository: quay.io/signalfx/splunk-otel-collector-windows
  logsEngine: otel
  readinessProbe:
    initialDelaySeconds: 60
  livenessProbe:
    initialDelaySeconds: 60

If you have both Windows and Linux worker nodes in your Kubernetes cluster, you need to install the Helm chart twice. One of the installations with the default configuration set to ``isWindows: false`` is applied on Linux nodes. The second installation with the values.yaml configuration (shown in the previous example) is applied on Windows nodes.

Deactivate the ``clusterReceiver`` on one of the installations to avoid cluster-wide metrics duplication. To do this, add the following lines to the configuration of one of the installations:

.. code-block:: yaml

  clusterReceiver:
    enabled: false
