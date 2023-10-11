Mailhop automatically backs up your email and acts as a queue while your
email server is offline.  Mailhop can be configured to send alerts to
VictorOps.  The following guide will walk you through this integration.

In VictorOps

From the VictorOps web portal, select **Settings** >> **Alert
Behavior** >> **Integrations**

.. image:: images/Integration-ALL-FINAL.png

Select the **Mailhop** integration option.

.. image:: images/Mailhop-final.png

Copy the **Service API Key** to the clipboard.

.. image:: images/Mailhop-2-final.png

In Mailhop
----------

Login to the Mailhop web interface as an admin.

.. image:: images/Screen_Shot_2017-04-03_at_4_45_53_PM.png

From the Mailhop dashboard, click **Services**.

.. image:: images/Dashboard_-_Mailhop.png

Select your “Product/Service”.  In this example, we select **Business
Plus**.

.. image:: images/Dashboard_-_Mailhop-1.png

Scroll down on the “Manage Product” page, then click **Launch Control
Panel** on the “Product Information” tab.

.. image:: images/Dashboard_-_Mailhop-2.png

From the Mailhop Control Panel, select **Monitoring** from the menu of
the left, then select **Alerting**.

.. image:: images/Mailhop.png

In the “ADD NEW RECEIVER” section, select **VictorOps** from the “Alert
type” dropdown menu, then enter a label in the “Receiver label” field.
 Next, paste the “API key” from the “In VictorOps” section into the “API
key” field, then enter the routing key you want to use into the “Routing
key / Team” field.  Finally, select the option you want from the
“Priority” dropdown menu and click **Create Receiver**.

.. image:: images/Mailhop-1.png

Select **SMTP Server** from the menu on the left.

.. image:: images/Mailhop-2.png

Set “Monitoring status” to **Enabled**, then click **Save**.

.. image:: images/Mailhop-3.png

Mailhop alerts should start showing up in your VictorOps timeline as
they are generated after a 10-15 minute delay for the settings to take
effect.

.. image:: images/Timeline_-_vops_davetesting.png

You have completed setting up this integration.  If you have any
questions, please contact `VictorOps
support <mailto:Support@victorops.com?Subject=Mailhop%20VictorOps%20Integration>`__.
