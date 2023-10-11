Apteligent is a mobile application performance management solution.

The Splunk On-Call (formerly VictorOps) integration with Apteligent
opens an incident in Splunk On-Call every time an alert is triggered in
Apteligent. Below is a short walkthrough.

1. From the Apteligent UI, select **Alert Center**, then **Alert
   Integrations**, then  **Add Integration**. |crittercism1|
2. Select **VictorOps** for the Integration, and give it a name
   (“VictorOps Integration” in the example) |crittercism2|
3. To obtain the URL for this integration, go into **Integrations**.
4. Select **Apteligent**, then copy the link for the “Service API
   Endpoint\ **.”** |image1|
5. Paste in the “Service API Endpoint” as a “URL.” |crittercism5|
6. Make sure to switch out **$routing_key** for the appropriate key that
   will route to the team you want notified for all Apteligent alerts.
   |crittercism6|
7. Click “Create Integration”, now you can configure any alert to notify
   the Splunk On-Call timeline by assigning the alert to “VictorOps
   Integration” |crittercism7|
8. If you have any questions contact support.

.. |crittercism1| image:: images/Crittercism1.png
.. |crittercism2| image:: images/Crittercism2.png
.. |image1| image:: images/apteligent_button.png
.. |crittercism5| image:: images/Crittercism5.png
.. |crittercism6| image:: images/Crittercism6.png
.. |crittercism7| image:: images/Crittercism7.png
