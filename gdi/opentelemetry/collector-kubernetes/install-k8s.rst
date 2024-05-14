.. _otel-install-k8s:

******************************************
Install the Collector for Kubernetes
******************************************

.. meta::
      :description: Describes how to install the Splunk Distribution of OpenTelemetry Collector for Kubernetes.

The Splunk Distribution of OpenTelemetry Collector for Kubernetes is a Helm chart for the Splunk Distribution of OpenTelemetry Collector. Use Helm charts to define, install, and upgrade Kubernetes applications.

Install the chart using one of these methods:

* :ref:`Helm chart <helm-chart>`
* :ref:`Resource YAML manifests <resource-yaml-manifests>`
* :ref:`Kubernetes Operator (Alpha) <k8s-operator>`

.. _helm-chart:

Install the Collector with the Helm chart
==============================================

Use the Helm chart to do the following:

* Create a Kubernetes DaemonSet along with other Kubernetes objects in a Kubernetes cluster.
* Receive, process, and export metric, trace, and log data for Splunk Enterprise, Splunk Cloud Platform, and Splunk Observability Cloud.

.. _helm-chart-supported-distros:

Supported Kubernetes distributions
---------------------------------------

The Helm chart works with default configurations of the main Kubernetes distributions. Use actively supported versions:

* :new-page:`Vanilla (unmodified version) Kubernetes <https://endoflife.date/kubernetes>`
* :new-page:`Amazon Elastic Kubernetes Service <https://endoflife.date/amazon-eks>`
* :new-page:`Azure Kubernetes Service <https://endoflife.date/azure-kubernetes-service>`
* :new-page:`Google Kubernetes Engine <https://endoflife.date/google-kubernetes-engine>`
* :new-page:`Red Hat OpenShift <https://access.redhat.com/support/policy/updates/openshift#dates>`
* Minikube. This distribution was made for local developers and is not meant to be used in production. 
   - Minikube was created to spin up various past versions of Kubernetes.
   - Minikube versions don't necessarily align with Kubernetes versions. For example, the :new-page:`Minikube v1.27.1 releases notes <https://github.com/kubernetes/minikube/releases/tag/v1.27.1>` state the default Kubernetes version was bumped to v1.25.2.

While the chart should work for other Kubernetes distributions, the default :new-page:`values.yaml <https://github.com/signalfx/splunk-otel-collector-chart/blob/main/helm-charts/splunk-otel-collector/values.yaml>` configuration file could require additional updates.

.. _helm-chart-components:

Helm chart components
------------------------------------------------

The Helm chart for the Collector has three components: agent, cluster receiver, and gateway (optional).

For use cases about the different components, see the GitHub documentation :new-page:`Splunk OpenTelemetry Collector Helm Chart Components: Use Cases <https://github.com/jvoravong/splunk-otel-collector-chart/blob/Feature-components-doc/docs/components.md#use-cases>`.

Agent component
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The agent component is deployed to each node in the Kubernetes cluster as a DaemonSet, and monitors all the data sources within each node.

The agent component consists of the following config files:

* daemonset.yaml

  * Defines a DaemonSet to ensure that some (or all) nodes in the cluster run a copy of the agent pod.
  * Collects data from each node in the Kubernetes cluster.

* configmap-agent.yaml

  * Provides configuration data to the agent component.
  * Contains details about how the agent collects and forwards data.

* service-agent.yaml (optional)

  * Defines a Kubernetes Service for the agent.
  * Used for internal communication within the cluster or for exposing specific metrics or health endpoints.

Cluster receiver component
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The cluster receiver component runs as a single pod in the cluster created by a deployment, and collects data from a single location. Use this component in scenarios where telemetry data is available from a cluster-wide service or endpoint.

The cluster receiver component consists of the following config files:

* deployment-cluster-receiver.yaml

  * Defines a deployment to manage the replicated application for the cluster receiver.
  * Receives and processes data at the cluster level.

* configmap-cluster-receiver.yaml

  * Provides configuration data to the cluster receiver.
  * Contains details about how the receiver processes and forwards the data it collects.

* pdb-cluster-receiver.yaml

  * Defines a Pod Disruption Budget (PDB) for the cluster receiver.
  * Ensures that a certain number or percentage of replicas remain available during operations like node maintenance.

* service-cluster-receiver-stateful-set.yaml (optional)

  * Defines a Kubernetes service for the cluster receiver.
  * Associated with a StatefulSet and used for load balancing, internal communication, or exposing specific endpoints.

Gateway component (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The gateway component serves as an intermediary. It receives, processes, enriches, and forwards data, enhancing data exportation. Use it primarily in larger clusters to scale monitoring capabilities.

The gateway component consists of the following config files:

* deployment-gateway.yaml

  * Defines a Deployment for the gateway.
  * Processes and forwards data between the agents/receivers and external destinations.

* configmap-gateway.yaml

  * Provides configuration data to the gateway.
  * Contains details about how the gateway processes, transforms, and forwards the data it receives.

* service.yaml

  * Defines a Kubernetes Service for the gateway.
  * Used for internal communication within the cluster for accepting data from the agent or cluster receiver and forwarding it to the Splunk backend endpoint.

* pdb-gateway.yaml

  * Defines a Pod Disruption Budget (PDB) for the gateway.
  * Ensures that a certain number or percentage of replicas of the gateway remain available during voluntary disruptions.

Prerequisites
------------------------------------------------

You need the following resources to use the chart:

* :new-page:`Helm 3 <https://helm.sh/docs/intro/install/>`. Helm 2 is not supported.
* Administrator access to your Kubernetes cluster.

.. _collector-k8s-destination:

Prerequisites: Destination
------------------------------------------------

The Collector for Kubernetes requires a destination: Splunk Enterprise or Splunk Cloud Platform (``splunkPlatform``) or Splunk Observability Cloud (``splunkObservability``).

Depending on your destination, you need:

* To send data to ``splunkPlatform``:

   * Splunk Enterprise 8.0 or higher.
   * A minimum of one Splunk platform index ready to collect the log data. This index is used for ingesting logs.
   * An HTTP Event Collector (HEC) token and endpoint. See :new-page:`Set up and use HTTP Event Collector in Splunk Web <https://docs.splunk.com/Documentation/Splunk/8.2.0/Data/UsetheHTTPEventCollector>` and :new-page:`Scale HTTP Event Collector <https://docs.splunk.com/Documentation/Splunk/8.2.0/Data/ScaleHTTPEventCollector>`.
   * ``splunkPlatform.endpoint``. URL to a Splunk instance, for example: ``"http://localhost:8088/services/collector"``.
   * ``splunkPlatform.token``. Splunk HTTP Event Collector token.

* To send data to ``splunkObservability``:

   * ``splunkObservability.accessToken``. Your Splunk Observability Cloud org access token with ingest authorization scope. See :ref:`admin-org-tokens`.
   * ``splunkObservability.realm``. Splunk realm to send telemetry data to. The default is ``us0``. See :new-page:`realms <https://dev.splunk.com/observability/docs/realms_in_endpoints/>`.

.. note:: The default Splunk platform index used by the Collector for Kubernetes is ``main``.

Deploy the Helm chart
--------------------------------

Run the following commands to deploy the Helm chart:

#. Add the Helm repo:

   .. code-block:: bash

      helm repo add splunk-otel-collector-chart https://signalfx.github.io/splunk-otel-collector-chart

#. Determine your destination.

   For Splunk Observability Cloud:

   .. code-block:: bash

      helm install my-splunk-otel-collector --set="splunkObservability.realm=us0,splunkObservability.accessToken=xxxxxx,clusterName=my-cluster" splunk-otel-collector-chart/splunk-otel-collector

   For Splunk Enterprise or Splunk Cloud Platform:

   .. code-block:: bash

      helm install my-splunk-otel-collector --set="splunkPlatform.endpoint=https://127.0.0.1:8088/services/collector,splunkPlatform.token=xxxxxx,splunkPlatform.metricsIndex=k8s-metrics,splunkPlatform.index=main,clusterName=my-cluster" splunk-otel-collector-chart/splunk-otel-collector

   For both Splunk Observability Cloud and Splunk Enterprise or Splunk Cloud Platform:

   .. code-block:: bash

      helm install my-splunk-otel-collector --set="splunkPlatform.endpoint=https://127.0.0.1:8088/services/collector,splunkPlatform.token=xxxxxx,splunkPlatform.metricsIndex=k8s-metrics,splunkPlatform.index=main,splunkObservability.realm=us0,splunkObservability.accessToken=xxxxxx,clusterName=my-cluster" splunk-otel-collector-chart/splunk-otel-collector

#. Specify a namespace to deploy the chart to with the ``-n`` argument:

   .. code-block:: bash

      helm -n otel install my-splunk-otel-collector -f values.yaml splunk-otel-collector-chart/splunk-otel-collector

.. caution::

  The :new-page:`values.yaml <https://github.com/signalfx/splunk-otel-collector-chart/blob/main/helm-charts/splunk-otel-collector/values.yaml>` file lists all supported configurable parameters for the Helm chart, along with a detailed explanation of each parameter. :strong:`Review it to understand how to configure this chart`.

  You can also configure the Helm chart to support different use cases, such as trace sampling and sending data through a proxy server. See :new-page:`Examples of chart configuration <https://github.com/signalfx/splunk-otel-collector-chart/blob/main/examples/README.md>` for more information.

Configure other parameters
--------------------------------

You can configure the following:

* :ref:`otel-kubernetes-config-distro`
* :ref:`otel-kubernetes-config-environment`
* :ref:`otel-upgrade-k8s-access-token`

For example:

.. code-block:: bash

   helm repo add splunk-otel-collector-chart https://signalfx.github.io/splunk-otel-collector-chart
   helm install my-splunk-otel-collector --set="splunkRealm=us0,splunkAccessToken=xxxxxx,clusterName=my-cluster" --set=distribution={value},cloudProvider={value} splunk-otel-collector-chart/splunk-otel-collector

* Read more about :ref:`otel-kubernetes-config` and also :ref:`the advanced Kubernetes config <otel-kubernetes-config-advanced>`.
* See :new-page:`examples of Helm chart configuration <https://github.com/signalfx/splunk-otel-collector-chart/blob/main/examples/README.md>` for additional chart installation examples or upgrade commands to change the default behavior.
* For logs, see :ref:`otel-kubernetes-config-logs`.

Set Helm using a YAML file
--------------------------------

You can also set Helm values as arguments using a YAML file. For example, after creating a YAML file named my_values.yaml, run the following command to deploy the Helm chart:

.. code-block:: bash

   helm install my-splunk-otel-collector --values my_values.yaml splunk-otel-collector-chart/splunk-otel-collector

See :new-page:`an example of a YAML file in GitHub <https://github.com/signalfx/splunk-otel-collector-chart/blob/main/helm-charts/splunk-otel-collector/values.yaml>`. Options include:

* Set ``isWindows`` to ``true`` to apply the Kubernetes cluster with Windows worker nodes.

Set Prometheus metrics
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Set the Collector to automatically scrape any pod emitting Prometheus by adding this property to the Helm chart's values YAML:

.. code-block:: bash

   autodetect:
      prometheus: true

Add this configuration in the resources file for any pods in the deployment:

.. code-block:: bash

   metadata:
      annotations:
         prometheus.io/scrape: "true"
         prometheus.io/path: /metrics
         prometheus.io/port: "8080"


Verify the deployment
--------------------------------

If the chart is deployed successfully, the output displays a message informing that the Splunk Distribution of OpenTelemetry Collector for Kubernetes is being deployed in your Kubernetes cluster, the last deployment date, and the status.

.. _resource-yaml-manifests:

Install the Collector with resource YAML manifests
=======================================================

.. note::

   To specify the configuration, you at least need to know your Splunk realm and base64-encoded access token.

A configuration file can contain multiple resource manifests. Each manifest applies a specific state to a Kubernetes object. The manifests must be configured for Splunk Observability Cloud only and come with all telemetry types activated for the agent, which is the default when installing the Helm chart.

Determine which manifest you want to use
------------------------------------------------

Download the necessary manifest files from :new-page:`the examples repository <https://github.com/signalfx/splunk-otel-collector-chart/tree/main/examples>`. Refer to the ``README`` files for more details on each example.

Determine which :ref:`otel-deployment-mode` you want to use, agent or gateway. By default, host monitoring (agent) mode is configured to send data directly to Splunk SaaS endpoints. Host monitoring (agent) mode can be reconfigured to send to a gateway.

Update the manifest
------------------------------------------------

Once you've decided which manifest suits you better, make the following updates:

#. In the secret.yaml manifest, update the ``splunk_observability_access_token`` data field with your base64-encoded access token.
#. Update any configmap-agent.yaml, configmap-gateway.yaml, and configmap-cluster-receiver.yaml manifest files you use. Search for "CHANGEME" to find the values that must be updated to use the rendered manifests directly.
      #. You need to update "CHANGEME" in exporter configurations to the value of the Splunk realm.
      #. You need to update "CHANGEME" in attribute processor configurations to the value of the cluster name.

Apply the manifest
--------------------------------

After you've updated them, apply the manifests using ``kubectl``, as shown in the following examples.

For host monitoring (agent) mode, download the :new-page:`agent-only manifest directory on GitHub <https://github.com/signalfx/splunk-otel-collector-chart/tree/main/examples/default/rendered_manifests>` for pre-rendered Kubernetes resource manifests that can be applied using the ``kubectl apply`` command after being updated with your token, realm information, and cluster name:

.. code-block:: bash

   kubectl apply -f <agent-manifest-directory> --recursive

For data forwarding (gateway) mode, download the :new-page:`gateway-only manifest directory on GitHub <https://github.com/signalfx/splunk-otel-collector-chart/tree/main/examples/collector-gateway-only/rendered_manifests>` for pre-rendered Kubernetes resource manifests that can be applied using the ``kubectl apply`` command after being updated with your token, realm information, and cluster name:

.. code-block:: bash

   kubectl apply -f <gateway-manifest-directory> --recursive

Use templates
--------------------------------

You can create your own manifest YAML files with customized parameters using ``helm template`` command.

.. code-block:: bash

   helm template --namespace default --set cloudProvider='aws' --set distribution='openshift' --set splunkObservability.accessToken='KUwtoXXXXXXXX' --set clusterName='my-openshift-EKS-dev-cluster' --set splunkObservability.realm='us1' --set gateway.enabled='false' --output-dir <rendered_manifests_dir> --generate-name splunk-otel-collector-chart/splunk-otel-collector

If you prefer, you can update the values.yaml file first.

.. code-block:: bash

   helm template --namespace default --values values.yaml --output-dir <rendered_manifests_dir> --generate-name splunk-otel-collector-chart/splunk-otel-collector

Manifest files will be created in your specified folder ``<rendered_manifests_dir>``.

Manifest examples
--------------------------------

See the following manifest to set security constraints:

.. github:: yaml
   :url: https://raw.githubusercontent.com/signalfx/splunk-otel-collector-chart/main/examples/distribution-openshift/rendered_manifests/securityContextConstraints.yaml


.. _k8s-operator:

Use the Kubernetes Operator in OpenTelemetry
============================================================================================

Using the Splunk Helm chart, you can install the Splunk Collector along with an upstream OpenTelemetry Kubernetes Operator for Auto Instrumentation. For more information, see :ref:`discovery-kubernetes`.

This instance of the Kubernetes Operator is part of the upstream OpenTelemetry Operator project. See the :new-page:`OpenTelemetry GitHub repo <OpenTelemetry GitHub repo <https://github.com/open-telemetry/opentelemetry-operator>` for more information.

.. note:: The upstream Kubernetes Operator is not related to the Splunk Operator for Kubernetes, which is used to deploy and operate Splunk Enterprise deployments in a Kubernetes infrastructure.

Splunk Distribution for the Kubernetes Operator (Alpha)
--------------------------------------------------------

.. caution::

   This project is Alpha. Do not use in production.

The Splunk Distribution of OpenTelemetry Collector for Kubernetes Operator is the Splunk Observability Cloud implementation of a Kubernetes Operator, and it helps deploy and manage the Splunk Distribution of the OpenTelemetry Collector for Kubernetes. See the :new-page:`README file <https://github.com/signalfx/splunk-otel-collector-operator>` in GitHub for installation instructions.

Next steps
==================================
After installing the package, you can:

* :ref:`otel-kubernetes-config`
* :ref:`apm`
