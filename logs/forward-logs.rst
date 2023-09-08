.. _forward-logs:


*****************************************************************
Forward Log Observer logs data to the Splunk platform
*****************************************************************

.. meta::
  :description: Learn how you can forward Log Observer logs to the Splunk platform as part of the Log Observer transition.


The Log Observer transition allows customers to analyze their Log Observer logs in the Splunk platform while still maintaining the ability to analyze them in Log Observer. Current Log Observer customers can forward their Log Observer logs data to a single index in a single instance of the Splunk platform. Splunk Observability Cloud uses an HEC token to forward new incoming Log Observer logs data to the Splunk platform in addition to storing it in Log Observer. 

To forward logs data from Log Observer to the Splunk platform, you must do the following:

1. Create an HEC ingest token in the Splunk platform

2. Authenticate the connection to HEC in Splunk Observability Cloud


.. _create-hec-token:

Create an HEC ingest token in the Splunk platform
================================================================================
Follow the steps below to create an HEC ingest token in the Splunk platform:

1. Log in to your Splunk platform instance, select :guilabel:`Settings`, then select :guilabel:`Add Data`.

2. Select :guilabel:`Monitor`.

3. Select :guilabel:`HTTP Event Collector`.

4. In the :guilabel:`Name` field, enter a name for the token.

5. (Optional) In the :guilabel:`Source name override` field, you can enter a custom name for the event data's source. The source field contains automatically generated information such as the log file path, network device, or application generating the log event, but you can override the value of the source name.

6. (Optional) In the :guilabel:`Description` field, enter a description for the logs data.

7. (Optional) If you want to activate indexer acknowledgment for this token, select :guilabel:`Enable indexer acknowledgment`, then select :guilabel:`Next`. Indexer acknowledgement can impact the timeliness of data forwarding and Splunk does not recommend it for the Log Observer forwarding use case. 

8. Ensure that the source type and index are correct. It is best practice to create a new index for Log Observer logs data. Ensure that the index for Log Observer logs data is in the :guilabel:`Allowed indexes` list and select it as the default index. For more information on source types and indexes, see :new-page:`Modify input settings <https://docs.splunk.com/Documentation/Splunk/9.0.4/Data/Modifyinputsettings>`.

9. Select :guilabel:`Review` and confirm that all settings for the HEC endpoint are what you want.

10. If all settings are what you want, select :guilabel:`Submit`. Otherwise, select :guilabel:`Back` to make changes.

11. Copy the token value that Splunk Web displays and paste it in the :guilabel:`HEC Ingest Token` field. You must use it in the next section.

12. (Optional) If you want to see the progress of the new token’s deployment to your Splunk platform instance, select :guilabel:`Track deployment progress` on the Splunk platform token settings page. When you see a status of "Done", you can then use the token to send your Log Observer data.

To learn more about HEC Ingest tokens for Splunk Cloud or Splunk Enterprise, see :new-page:`Set up and use HTTP Event Collector in Splunk Web <https://docs.splunk.com/Documentation/Splunk/9.1.0/Data/UsetheHTTPEventCollector>`.


.. _authenticate-hec:

Authenticate the connection to HEC in Splunk Observability Cloud
================================================================================
Follow these steps to authenticate your connection to HEC:

1. Log in to Splunk Observability Cloud and select :guilabel:`Settings`, then select :guilabel:`Forward Logs Data`.

2. In the :guilabel:`Enter your HEC details` section, enter the URL and port of your Splunk platform instance, and the value of the HEC Ingest token that you created in the previous section.

3. Select :guilabel:`Save and Activate` to start forwarding new incoming Log Observer logs data to the Splunk platform in addition to storing it in Log Observer.

To verify that your HEC ingest token is functional and that Splunk Observability Cloud is forwarding logs successfully, go to :guilabel:`Search & Reporting` in your Splunk platform instance and verify that logs from Splunk Observability Cloud appear in the index you selected in step 8 of the previous section. To continue working with the forwarded logs in Splunk Observability Cloud, use Log Observer Connect to connect to the Splunk platform index that contains the forwarded logs.


Troubleshooting
================================================================================
If you do not see Log Observer logs in your Splunk platform instance, check the Splunk platform instance URL and HEC token you provided and try again, or contact Customer Support for help. To update an HEC token, select :guilabel:`Deactivate Forwarding`, update the token, then select :guilabel:`Reactivate Forwarding`. When you select :guilabel:`Deactivate Forwarding`, Log Observer no longer forwards logs to the Splunk platform.