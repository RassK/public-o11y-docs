Splunk On-Call was designed to make communication between key personnel
easy and efficient when dealing with a crisis.  This article describes
the different methods Splunk On-Call users can employ to collaborate
with colleagues to solve problems faster and more efficiently.

.. raw:: html

   <script type="text/javascript" id="vidyard_embed_code_dCp8p2E6SZqojehsiws2bz" src="//play.vidyard.com/dCp8p2E6SZqojehsiws2bz.js?v=3.1.1&amp;type=inline"></script>

*\* In order to receive the push notifications mentioned in this
article, users are required to have installed the Splunk On-Call mobile
app on their mobile device (Android or iOS).*

`@Mentions <#at>`__ and `@@Mentions <#atat>`__ can be used to contact
other users directly via push notification to the user’s registered
mobile device.  These communications are recorded in the main timeline
and visible to all users, which means they can also be used for
reporting and analysis.  This function exists both in the main timeline
and the `Incident Timeline <#incident>`__, with slightly different
behavior in either case.

`Private chats <#private>`__ occur between two users and are not
recorded in the timeline. They cannot be used for reporting and are not
visible to any other users in the platform.

**@ Mentions**
~~~~~~~~~~~~~~

Within the main timeline, there is a text bar located at the top.
 Typing a single @ symbol into the text bar will bring up a list of
users.  You can either click on a user to populate the name, or begin
typing out the username to narrow the list down to the specific user you
want to contact.  This can be done for a single user or multiple users
(each requires a @ symbol in front of their username). Once
the \_@username\_ has been entered, you simply type the message you want
to convey and press ENTER. This will result in a single push
notification to that user’s mobile device informing them that they have
been mentioned in the Splunk On-Call timeline.

It is not required that you use the @ symbol before every chat.  Once a
conversation has begun and the users are engaged, it may not be
necessary for users to receive a push notification for each chat.  At
that point, you can simply chat back and forth with the assumption that
the other user is already engaged and paying attention.  If a long time
passes, you may want to use another @mention to draw that user’s
attention back to the timeline.

.. _mentions-1:

**@@ Mentions**
~~~~~~~~~~~~~~~

Within the main timeline, there is a text bar located at the top.
 Typing two @ symbols (@@) into the text bar will bring up a list of the
teams you have configured in Splunk On-Call.  You can either click on a
team to populate the team name or begin typing out the team name to
narrow the list down to the specific team you want to contact. This can
be done for a single team or multiple teams (each requires the @@ symbol
in front of the team name). Once the @@team-name has been entered, you
simply type the message you want to convey and press ENTER. This will
result in a single push notification to the mobile device of every
member of that team (regardless of their current on-call status)
informing them that they have been mentioned in the Splunk On-Call
timeline.

It is not required that you use the @@ symbol before every chat. Once a
conversation has begun and the users are engaged, it may not be
necessary for users to receive a push notification for each chat. At
that point, you can simply chat back and forth with the assumption that
the other user is already engaged and paying attention. If a long time
passes, you may want to use another @@mention to draw the team’s
attention back to the timeline.

**Chatting in the Main Timeline vs. the Incident Timeline**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In the Splunk On-Call timeline, you can use the @ or @@ mention function
in both the main timeline or in the incident timeline. Clicking on a
specific incident in the incident timeline will bring up the details of
that incident along with all of the timeline events (taken from the main
timeline) that are related to that specific incident. Using the @ or @@
mention function embedded in this view will cause the chat communication
to be automatically appended with the specific incident # at the end.
This is a great way for users with whom you are communicating to quickly
get caught up on events when they receive the chat notification.
