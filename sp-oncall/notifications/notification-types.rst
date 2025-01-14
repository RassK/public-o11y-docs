
.. _notif-types:

************************************************************************
Notification types
************************************************************************

.. meta::
   :description: Splunk On-Call offers many notification options including email, SMS, phone, and push notifications. This topic highlights each of the different options.


.. toctree::
    :hidden:

    snooze
    delayed-notifications
    desktop-notifications
    call-notification-numbers
    paging-policy-setup





Splunk On-Call offers many notification options including email, SMS, phone, and push notifications. This topic highlights each of the different options.

.. note:: A maximum amount of four separate phone numbers for SMS and Phone notifications can be implemented into any given Splunk On-Call user profile.

You can also configure delayed notifications for alerts that may auto-resolve within a set time frame. For details, see 


.. raw:: html
  
    <embed>
      <h2>Push notification<a name="push-notification" class="headerlink" href="#push-notification" title="Permalink to this headline">¶</a></h2>
    </embed>


Push notifications are sent through the application. We use push for:

-  Paging
-  On-call changes
-  Chats (timeline and private)
-  Control Call

When a push notification is used to deliver a page, you will have the option to acknowledge, reroute, or snooze the incident straight from the notification.

.. image:: /_images/spoc/notif-types1.png
    :width: 35%
    :alt: Splunk On-Call push notification.

.. raw:: html
  
    <embed>
      <h2>SMS<a name="sms" class="headerlink" href="#sms" title="Permalink to this headline">¶</a></h2>
    </embed>

SMS notifications can be used in your personal paging policy. The message you receive is, at most, 160 characters, and it displays the
incident number, entity_display_name, and response code if two-way SMS is supported. When you receive an SMS notification, two codes are included in the message so you can acknowledge aor resolve the alert by responding with the correct five-digit code. These response codes expire after 1 hour.

.. image:: /_images/spoc/notif-types2.png
    :width: 35%
    :alt: Splunk On-Call SMS notification.

.. raw:: html
  
    <embed>
      <h3>SMS Subscription Management<a name="sms-subscription" class="headerlink" href="#sms-subscription" title="Permalink to this headline">¶</a></h3>
    </embed>

You may stop and start our SMS notification subscription by replying to the message with STOP or START. Although, it is best to manage your notifications from the personal profile page in Splunk On-Call.

.. raw:: html
  
    <embed>
      <h2>WhatsApp<a name="whatsapp" class="headerlink" href="#whatsapp" title="Permalink to this headline">¶</a></h2>
    </embed>

You can use WhatsApp notifications in your personal paging policy. To enable WhatsApp, download and configure the WhatsApp application from the Apple App Store or Google Play Store. Next, access your user profile in Splunk On-Call and enter and verify your mobile number. After verification, an :guilabel:`Enable WhatsApp` button will appear next to the number and you'll be able to use WhatsApp in your Paging Policy.

.. raw:: html
  
    <embed>
      <h2>Email<a name="email" class="headerlink" href="#email" title="Permalink to this headline">¶</a></h2>
    </embed>

Emails can be used for pages. Emails can also be used as reminders that your Splunk On-Call instance is in :ref:`maintenance-mode`, or that you have a gap in your schedule due to a :ref:`scheduled-overrides` that is not covered.

.. image:: /_images/spoc/notif-types3.png
    :width: 75%
    :alt: Splunk On-Call email notification.

Scheduled Override:

.. image:: /_images/spoc/notif-types4.png
    :width: 75%
    :alt: Splunk On-Call scheduled override email.

Maintenance Mode:

.. image:: /_images/spoc/notif-types5.png
    :width: 75%
    :alt: Splunk On-Call scheduled maintenance override.

.. raw:: html
  
    <embed>
      <h2>Phone<a name="phone" class="headerlink" href="#phone" title="Permalink to this headline">¶</a></h2>
    </embed>

Phone calls are used for paging. The “entity_display_name” field is read
aloud and then an option to acknowledge or resolve the alert is offered.

-  Press 4 to acknowledge
-  Press 6 to resolve

For a list of phone numbers used by Splunk On-Call, see :ref:`mobile-get-started`.
