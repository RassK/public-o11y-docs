This article discusses roles in Splunk On-Call and provides a detailed
breakdown of the specific permissions allocated to each role. It will
also review how to change the roles for users.

There are five roles in Splunk On-Call:

-  Global Admin: Global access, no restrictions
-  Team Admin: Manages people and scheduling on a team basis
-  Alert Admin: Manages the technical aspects of creating and optimizing
   alerts
-  User: Alert response
-  Stakeholder: Read-only awareness

Global Admin, Alert Admin, User, and Stakeholder are **global** roles.
The Team Admin role is assigned on a team basis. This means that a user
that is a Team Admin for one team has permissions to manage people and
schedules for only that team.

This means that a user can hold **two** roles. Here are the possible
combinations:

-  User & Team Admin
-  Alert Admin & Team Admin

Stakeholders can be considered separate from all other roles as these
users cannot be placed in any on-call schedules nor take action on
incidents.  They can simply be added to existing incidents and notified
through their defined contact methods for awareness.  For comprehensive
information on stakeholders, please see `this
article <https://help.victorops.com/knowledge-base/stakeholders/>`__.

The next sections provides a detailed breakdown of the specific
permissions for each role.

User Permissions
----------------

User Management
~~~~~~~~~~~~~~~

[table id=10 /]

Team Management
~~~~~~~~~~~~~~~

[table id=11 /]

Alert/Incident Management
~~~~~~~~~~~~~~~~~~~~~~~~~

[table id=12 /]

On-call Actions
~~~~~~~~~~~~~~~

[table id=13 /]

Billing
~~~~~~~

[table id=14 /]

Reporting
~~~~~~~~~

[table id=15 /]

How to Change Global Roles
==========================

Only Global Admins can change the user roles. This includes other Global
Admins.

To change a user’s global role, navigate to *Users*, and click on the
name of the user to access their profile page.

You may change the role of that user by selecting a new role from the
Role dropdown. Your changes will save automatically.

For information on how to manage Team Admin permissions, please visit
the `How to set up Team
Admins <https://help.victorops.com/knowledge-base/how-to-set-up-team-admins/>`__
article.

For more information regarding overall Admin permissions, including the
Alert Admin role, please follow the link our `How to Manage Admin
Permissions <https://help.victorops.com/knowledge-base/manage-admin-permissions/>`__
Knowledge Base article.

How to change Stakeholder Roles
===============================

Users can be converted to or from a stakeholder role with the assistance
of the support team.  Please note that Stakeholders are priced
differently from all other user roles so additional charges may be
incurred if converting someone from a stakeholder to another role.

If interested in a stakeholder conversion, please `contact the support
team <https://help.victorops.com/knowledge-base/how-to-contact-splunk-on-call-support/>`__
and include the specific user(s) you’d like to convert and what role
you’d like them converted to.  If converting from one of the user role
types to a stakeholder, please ensure that the user is removed from all
rotations and escalation policies and isn’t actively being paged for any
incidents.
