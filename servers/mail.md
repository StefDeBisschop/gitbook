---
description: Setting up the Mail with iRedMail
---

# Mail

I wanted to have a reliable and secure mailing system and hosted on-prem. Again, I choose a Ubuntu environment. This is a more complex setup than a normal M365 exchange online setup but far more interesting as setup and debugging goes.

## Setting up the VM

The VM for running a Ubuntu 22.04 server that hosts iRedMail has following hardware;

* 1 CPU
* 2 GB RAM
* 10GB HARD DISK

iRedMail is an open-source email server solution that provides a complete mail system that includes mail server, webmail client, and other components necessary for mail communication. It provides a simplified process of setting up and managing an mail server.

iRedMail consist out of 3 major components that work together:

* Postfix: It is the mail transfer agent (MTA) responsible for sending and receiving email messages.
* Dovecot: It is the email delivery agent (MDA) that stores and manages user mailboxes.
* Roundcube: It is a webmail client that allows users to access and manages user mailboxes.

These are the 3 main components that are present. But it's important to know it's not all, you also have Amavisd-new, ClamAV, SpanAssassin and OpenLDAP. The will not be mentioned in this documentation and will not be used in my project (except for OpenLDAP).
