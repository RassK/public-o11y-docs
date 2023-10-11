.. _aws-privatelink:

*********************************************************************
Private Connectivity using AWS PrivateLink
*********************************************************************

.. meta::
  :description: Connect to AWS using PrivateLink.

You can use Amazon Web Services (AWS) PrivateLink to secure your traffic from your AWS environment to your Splunk Observability Cloud environment without exposing it to the Internet. 

AWS PrivateLink connects your Virtual Private Cloud (VPC) to your AWS services, treating them as if they were in your VPC. You can create and use VPC endpoints to securely access AWS services and control the specific API endpoints and sites. To learn more, see the AWS PrivateLink documentation at :new-page:`https://docs.aws.amazon.com/vpc/latest/privatelink/what-is-privatelink.html <https://docs.aws.amazon.com/vpc/latest/privatelink/what-is-privatelink.html>`.

The following diagram shows an overview of how AWS PrivateLink works: 

.. source in Lucidchart: https://lucid.app/lucidchart/21f1cd02-7b2c-4654-a1b8-18c80a903fee/edit?shared=true&page=0_0&invitationId=inv_2f660037-6a85-4b98-9025-212b16c6b5a2#

.. image:: /_images/gdi/aws-privatelink-schema2.png
  :width: 80%
  :alt: AWS Private Link schema.
  

Prerequisites
==================================================

To connect Splunk Observability Cloud to AWS using AWS PrivateLink, you need the following:

* An active AWS account
* A basic understanding of VPC concepts and networking principles

.. _aws-privatelink-regions-names:

AWS PrivateLink availability and service name
==================================================

The following tables show the AWS PrivateLink endpoint URLs and service names for each AWS region:

.. _aws-privatelink-endpoint-urls:

AWS PrivateLink endpoint URLs
--------------------------------------------------

.. list-table::
  :header-rows: 1
  :width: 100
  :widths: 20, 20, 20, 20, 20

  * - AWS region
    - Ingest endpoint URL
    - API endpoint URL
    - Backfill endpoint URL
    - Stream endpoint URL

  * - ap-northeast-1
    - :new-page:`private-ingest.jp0.signalfx.com <http://private-ingest.jp0.signalfx.com/>`
    - :new-page:`private-api.jp0.signalfx.com <http://private-api.jp0.signalfx.com/>`
    - Coming soon
    - Coming soon

  * - ap-southeast-2
    - :new-page:`private-ingest.au0.signalfx.com <http://private-ingest.au0.signalfx.com/>`
    - :new-page:`private-api.au0.signalfx.com <http://private-api.au0.signalfx.com/>`
    - Coming soon
    - Coming soon

  * - eu-west-1
    - :new-page:`private-ingest.eu0.signalfx.com <http://private-ingest.eu0.signalfx.com/>`
    - :new-page:`private-api.eu0.signalfx.com <http://private-api.eu0.signalfx.com/>`
    - Coming soon
    - Coming soon

  * - us-east-1
    - :new-page:`private-ingest.us0.signalfx.com <http://private-ingest.us0.signalfx.com/>`
    - :new-page:`private-api.us0.signalfx.com <http://private-api.us0.signalfx.com/>`
    - Coming soon
    - Coming soon

  * - us-west-2
    - :new-page:`private-ingest.us1.signalfx.com <http://private-ingest.us1.signalfx.com/>`
    - :new-page:`private-api.us1.signalfx.com <http://private-api.us1.signalfx.com/>`
    - Coming soon
    - Coming soon

.. _aws-privatelink-service-names:

AWS PrivateLink service names
--------------------------------------------------

.. list-table::
  :header-rows: 1
  :width: 100
  :widths: 20, 20, 20, 20, 20

  * - AWS region
    - Ingest endpoint service name
    - API endpoint service name
    - Backfill endpoint service name
    - Stream endpoint service name

  * - ap-northeast-1
    - com.amazonaws.vpce.ap-northeast-1.vpce-svc-086c8167a74323e5a
    - com.amazonaws.vpce.ap-northeast-1.vpce-svc-06e1951072fcabaaa
    - Coming soon
    - Coming soon

  * - ap-southeast-2
    - com.amazonaws.vpce.ap-southeast-2.vpce-svc-01e4e31c294754b6e
    - com.amazonaws.vpce.ap-southeast-2.vpce-svc-0d1d69a0b1bf003cd
    - Coming soon
    - Coming soon

  * - eu-west-1
    - com.amazonaws.vpce.eu-west-1.vpce-svc-01c194b2265ecb86e
    - com.amazonaws.vpce.eu-west-1.vpce-svc-07b08296ff84e17a0
    - Coming soon
    - Coming soon

  * - us-east-1
    - com.amazonaws.vpce.us-east-1.vpce-svc-0336437d577075951
    - com.amazonaws.vpce.us-east-1.vpce-svc-089b68950f5be1c22
    - Coming soon
    - Coming soon

  * - us-west-2
    - com.amazonaws.vpce.us-west-2.vpce-svc-06376c4a9be288ee9
    - com.amazonaws.vpce.us-west-2.vpce-svc-0da2bbb45fa4c3a6b
    - Coming soon
    - Coming soon

Configure your AWS PrivateLink VPC endpoints
=================================================================

Follow these steps to create, use, and manage your AWS PrivateLink VPC endpoint:

* :ref:`aws-privatelink-step1`
* :ref:`aws-privatelink-step2`
* :ref:`aws-privatelink-step3`
* :ref:`aws-privatelink-step4`

.. _aws-privatelink-step1:

Step 1: Request to add your AWS Account ID to the allow list
--------------------------------------------------------------------

Reach out to Splunk Customer Support with the following information to include your AWS Account ID to the allow list:

* AWS Account ID

* AWS region

* Endpoint type
  
  * Ingest
  * API

Review the ways you can contact Splunk Customer Support at :ref:`Splunk Observability Cloud support <support>`.

.. _aws-privatelink-step2:

Step 2: Verify AWS Account ID is added to allow list
-----------------------------------------------------------

.. caution:: Wait for Splunk Customer Support's confirmation that your AWS Account ID was added to the allow list before performing these steps. Support might take up to 24 hours.

To verify your AWS Account ID has been allowed, follow these steps:

1. Log in to the AWS Management Console, and open the :guilabel:`Amazon VPC service` in the specific region where you intend to set up AWS PrivateLink.

2. On the left navigation pane, select :guilabel:`Endpoints`.

3. Select :guilabel:`Endpoint`, and then :guilabel:`Other endpoint services`.

4. Enter and verify the service name based on the AWS region where you're configuring the VPC endpoint. Identify the appropriate service name using the :ref:`AWS PrivateLink service names table <aws-privatelink-service-names>`.

  * If you see the "Service name verified" message, proceed with :ref:`aws-privatelink-step3`. 
  * If you see the "Service name could not be verified" error message, your account ID is not yet allowed for the given service name. Reach out to Splunk Customer Support to check the status of your request from :ref:`aws-privatelink-step1`. 

.. _aws-privatelink-step3:

Step 3: Create a VPC endpoint
--------------------------------------------------

To create a VPC endpoint, follow these steps:

1. Log in to the AWS Management Console, and open :guilabel:`Amazon VPC service` within the specific region where you intend to set up AWS PrivateLink. If you have a VPC peering configuration, keep in mind the destination region of VPC peering.

2. On the left navigation pane, select :guilabel:`Endpoints`.

3. Select :guilabel:`Create Endpoint`, and then :guilabel:`Other endpoint` services.

4. Enter and verify the service name based on the AWS region where you're configuring the VPC endpoint. Identify the appropriate service name using the :ref:`AWS PrivateLink service names table <aws-privatelink-service-names>`.

5. Select the VPC in which you want to create the endpoint. 

6. Choose the subnet or subnets within the VPC where the endpoint will reside. Make sure to select the subnets from the appropriate availability zones.

7. Set the IP address type to ``IPv4``.

8. Specify the security group or groups controlling inbound and outbound traffic for the endpoint, and set the outbound rule for the selected security groups open for port ``443``.

  The following image shows the security options for AWS PrivateLink: 

  .. image:: /_images/gdi/aws-privatelink-secgroups2.png
      :width: 80%
      :alt: Specify security groups that control traffic.

9. Review the configuration details and select :guilabel:`Create Endpoint`.

10. Wait for Splunk Customer Support's confirmation before proceeding to :ref:`aws-privatelink-step4`. 

.. _aws-privatelink-step4:

Step 4: Modify the endpoint to enable a Private DNS Name
----------------------------------------------------------------

To modify the endpoint to enable a Private DNS Name, follow these steps:

#. Log in to the AWS Management Console.
#. Navigate to the :guilabel:`Amazon VPC service` in the region where you have created the VPC endpoint.
#. On the left navigation pane, select :guilabel:`Endpoints`.
#. Select the VPC endpoint you want to modify.
#. Select :guilabel:`Actions`, and then :guilabel:`Modify Endpoint`.
#. Enable the private DNS names under the :guilabel:`Modify private DNS name` settings.
#. After the process is completed, select :guilabel:`Save Changes`.

You can now start using the AWS PrivateLink URL mentioned in the :ref:`AWS PrivateLink endpoint URLs table <aws-privatelink-endpoint-urls>`.

Delete a VPC endpoint
--------------------------------------------------

You can list, modify, tag, or delete your VPC endpoints.

To delete an endpoint, follow these steps:

#. Log in to the AWS Management Console and open the :guilabel:`Amazon VPC service`.
#. On the left navigation pane, select :guilabel:`Endpoints`.
#. Select the VPC endpoint you want to delete.
#. Confirm the deletion when prompted.

Advanced configuration: AWS PrivateLink with VPC peering 
==========================================================================

Examine a scenario where your source region, or region generating your data, is ``ap-south-1`` and your destination region, or region where you have established your VPC connection and want to receive data at, is ``us-east-1``. 

In this context, you need to establish a VPC endpoint within your destination region ``us-east-1``. By activating AWS PrivateLink in this region, you obtain a seamless, secure, and private channel to access AWS services available in the your source region, ``ap-south-1``. This arrangement ensures that communication between the two VPCs occurs through an internal network, removing the necessity of routing traffic over the public Internet.

This enhancement bolsters data integrity and security, aligning with the goal of optimizing inter-region communication while upholding stringent data protection standards.

Learn more in the AWS documentation at :new-page:`https://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html#two-vpcs-full-access <https://docs.aws.amazon.com/vpc/latest/peering/peering-configurations-full-access.html#two-vpcs-full-access>`.

.. Next steps
.. ================

.. After you connect Splunk Observability Cloud with AWS, you can use Observability Cloud to track a series of metrics and analyze your AWS data in real time. 

.. - See the AWS official documentation for a list of the available AWS resources.
..  - See :ref:`how to leverage data from integration with AWS <aws-post-install>` for more information.

.. _aws-privatelink-support:

.. include:: /_includes/report-issue.rst
