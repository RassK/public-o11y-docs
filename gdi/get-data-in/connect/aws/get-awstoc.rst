.. _get-started-aws:

************************************************************
Connect to AWS and send data to Splunk Observability Cloud
************************************************************

.. meta::
  :description: Connection planning information and links to the different ways to connect AWS to Splunk Observability Cloud.

.. toctree::
  :hidden:

  AWS authentication and supported regions <aws-prereqs>
  Compare connection options <aws-compare-connect>
  Connect to AWS with the UI guided setup <aws-wizardconfig>
  Connect to AWS with the API <aws-apiconfig>
  Connect to AWS with Terraform <aws-terraformconfig>
  Connect Metric Streams from the AWS console <aws-console-ms>  
  Collect logs from AWS <aws-logs>
  CloudFormation templates <aws-cloudformation>
  Next steps <aws-post-install>
  Troubleshoot your AWS connection <aws-troubleshooting>
  Troubleshoot Metric Streams <aws-ts-metric-streams>
  Troubleshoot logs <aws-ts-logs>
  GetMetricStatistics API deprecation notice <aws-api-notice>

Splunk Observability Cloud offers you several methods to connect and monitor Amazon Web Services (AWS), from a guided UI wizard to an extense API. 

Before you start, see :ref:`aws-prereqs`, and check the :ref:`list of AWS integrations available in Splunk Observability Cloud <aws-integrations>`. 

Read on to learn about data ingest options and available connection methods.

.. _aws-ingest:

.. raw:: html

  <embed>
    <h2>How to ingest AWS data in Splunk Observability Cloud<a name="aws-ingest" class="headerlink" href="#aws-ingest" title="Permalink to this headline">¶</a></h2>
  </embed>

You have two ways to send AWS data to Splunk Observability Cloud:

* Through :ref:`API polling <aws-api-polling>` at specified intervals. 
* Using :ref:`CloudWatch Metric Streams <aws-metricstreams>`. 

.. caution:: CloudWatch Metric Streams supports filtering by namespace and metric name but doesn't support filtering based on resource tags.

.. _aws-api-polling:

.. raw:: html

  <embed>
    <h3>Poll data from AWS using CloudWatch APIs<a name="aws-api-polling" class="headerlink" href="#aws-api-polling" title="Permalink to this headline">¶</a></h3>
  </embed>

You can poll data from AWS at specified intervals using CloudWatch APIs. Due to the CloudWatch metrics instability, for certain namespaces some metrics might be delayed a few minutes. See more in :ref:`Configure API polling <aws-configure-api-polling>`.

.. _aws-data-limits:

.. raw:: html

  <embed>
    <h4>High data volume warning <a name="aws-data-limits" class="headerlink" href="#aws-data-limits" title="Permalink to this headline">¶</a></h4>
  </embed>

After you create an AWS integration, if more than 100,000 metrics are retrieved from CloudWatch, Splunk Observability Cloud automatically deactivates the integration and sends you a warning email.  

This check runs just once per integration. If you activate the integration afterwards, it will work correctly. 

You can deactivate this check by setting the ``enableCheckLargeVolume`` field in the AWS integration to ``false`` :new-page:`using the API <https://dev.splunk.com/observability/reference/api/integrations/latest#endpoint-update-single-integration>`.

.. _tag-filtering-aws:

.. raw:: html

  <embed>
    <h4>Tag filtering<a name="tag-filtering-aws" class="headerlink" href="#tag-filtering-aws" title="Permalink to this headline">¶</a></h4>
  </embed>

If you filter data based on tags, your costs for Amazon CloudWatch and Splunk Infrastructure Monitoring might decrease.

Be careful when choosing tag names: Splunk Observability Cloud only allows alphanumeric characters, and the underscore and minus symbols. Unsupported characters include ``.``, ``:``, ``/``, ``=``, ``+``, ``@``, and spaces, which are replaced by the underscore character. 

.. _aws-metricstreams:

.. raw:: html

  <embed>
    <h3>Use Metric Streams to forward data to Splunk Observability Cloud<a name="aws-metricstreams" class="headerlink" href="#aws-metricstreams" title="Permalink to this headline">¶</a></h3>
  </embed>

Rather than polling for data, Metric Streams continually stream Amazon CloudWatch metrics to Splunk Observability Cloud. Although they're more efficient than API polling, consider the :ref:`cost constraints <collection-interval-aws>`.

.. _aws-data-availability:

.. raw:: html

  <embed>
    <h2>Data availability<a name="aws-data-availability" class="headerlink" href="#aws-data-availability" title="Permalink to this headline">¶</a></h2>
  </embed>

.. caution:: Splunk is not responsible for data availability, and it can take up to several minutes (or longer, depending on your configuration) from the time you connect until you start seeing valid data from your account.

By default, Splunk Observability Cloud brings in data from all :ref:`supported AWS services <aws-integrations>` associated with your account. To limit the amount of data to import, see :ref:`specify-data-metadata`.

.. _collection-interval-aws:

.. raw:: html

  <embed>
    <h2>Data collection interval and costs<a name="collection-interval-aws" class="headerlink" href="#collection-interval-aws" title="Permalink to this headline">¶</a></h2>
  </embed>

In most cases, metrics are reported every minute. However, some services use a different cadence: For example, selected S3 metrics are reported on a daily basis. Check AWS documentation to verify how often your services' metrics are reported.

Collecting Amazon CloudWatch metrics via the polling APIs at the default polling rate of 300 seconds (5 minutes) is usually cheaper than using Metric Streams. On the other hand, if you set polling intervals to one minute, generally you'll see an increase in Amazon CloudWatch usage costs compared to Metric Streams.

Learn more at :ref:`Amazon CloudWatch usage costs <aws-costs>`.

.. _aws-connection-options:

.. raw:: html

  <embed>
    <h2>Connect with AWS: Available options<a name="aws-connection-options" class="headerlink" href="#aws-connection-options" title="Permalink to this headline">¶</a></h2>
  </embed>

You can connect Splunk Observability Cloud to AWS in several ways. See a comparison of the connection options at :ref:`aws-compare-connect`, and choose the connection method that best matches your needs:

.. list-table::
  :header-rows: 1
  :width: 100%
  :widths: 30, 70

  * - :strong:`Connection option`
    - :strong:`Why use this?`

  * - :ref:`Guided setup <aws-wizardconfig>` in Splunk Observability Cloud
    - Guides you step-by-step to set up an AWS connection and default configuration in Splunk Observability Cloud. Guided setup includes links to Amazon CloudFormation templates that you can select to create needed AWS IAM roles.

  * - :ref:`Splunk Observability Cloud API <get-configapi>`
    - Requires knowledge of POST and PUT call syntax, but includes options and automation that are not part of the guided setup. Choose this method if you want to configure many integrations at once. 

  * - :ref:`Metric Streams from the AWS console <aws-console-ms>`
    - Connect and manage Metric Streams from the AWS console.

  * - :ref:`Splunk Terraform <terraform-config>`
    - Use this connection method if you already manage your infrastructure as code by deploying through Terraform.

  * - :new-page:`Splunk add-on for Amazon Kinesis Firehose <https://docs.splunk.com/Documentation/AddOns/latest/Firehose/ConfigureFirehose>`
    - Use the Splunk add-on if you want to configure Amazon Kinesis Firehose to send data to the Splunk platform.

.. note:: If you can't connect AWS to Splunk Observability Cloud, see :ref:`Troubleshoot your AWS connection <aws-troubleshooting>`.

.. _aws-connection-options-more:

.. raw:: html

  <embed>
    <h3>Private connectivity<a name="aws-connection-options-more" class="headerlink" href="#aws-connection-options-more" title="Permalink to this headline">¶</a></h3>
  </embed>  

Splunk Observability Cloud also offers secured connectivity with AWS. For more information, see :ref:`aws-privatelink`.

.. _aws-collector:

.. raw:: html

  <embed>
    <h2>Install the Splunk Distribution of OpenTelemetry Collector<a name="install-splunk-otel-collector" class="headerlink" href="#install-splunk-otel-collector" title="Permalink to this headline">¶</a></h2>
  </embed>

To take advantage of the full benefits of the Splunk Observability Cloud platform, install the :ref:`OpenTelemetry Collector <otel-intro>`, since it offers a higher degree of customization than the AWS integration.

You can track the degree of OpenTelemetry enablement in your AWS integrations by going to :guilabel:`Data Management > AWS`.

..  image:: /_images/gdi/aws-collector-insights.jpg
  :width: 80%
  :alt: Amount of AWS entities with the Collector installed.

Select the :guilabel:`OpenTelemetry Enabled` button to see whether the Collector is installed on each AWS EC2 instance. This will help you identify the instances that still need to be instrumented. For instances that are successfully instrumented, you can see which version of the Collector is deployed.

.. _after-aws-integration:

.. raw:: html

  <embed>
    <h2>Next steps<a name="after-aws-integration" class="headerlink" href="#after-aws-integration" title="Permalink to this headline">¶</a></h2>
  </embed>

* See :ref:`Leverage data from integration with AWS <aws-post-install>` for an overview of what you can do after you connect Splunk Observability Cloud to AWS.
* Learn about :ref:`our AWS Infrastructure Monitoring options <infrastructure-aws>`. You'll find instructions on how to import AWS metrics and metadata, or AWS tag and log information using namespaces and filters. 
* Refer to the AWS official documentation for a list of the available AWS metrics and other data, or read about :ref:`the metadata we provide <aws-infra-metadata>`.
* To collect traces and metrics of your AWS Lambda functions for Splunk APM, see :ref:`splunk-otel-lambda-layer`.

