# Copy of Integrate AD - Postfix

Because we are using OpenLDAP as the backend for iRedMail we still need to configure the LDAP settings. For this, we are just using the documentation from iRedMail itself.

First, we create a read-only user vmail in ADUC. This is read-only because the mail server doesn't need to create users.

### Enable LDAP query with AD in Postfix <a href="#enable-ldap-query-with-a-d-in-postfix" id="enable-ldap-query-with-a-d-in-postfix"></a>

Disable unused iRedMail special settings

```vim
postconf -e virtual_alias_maps=''
postconf -e sender_bcc_maps=''
postconf -e recipient_bcc_maps=''
postconf -e relay_domains=''
postconf -e relay_recipient_maps=''
postconf -e sender_dependent_relayhost_maps=''
```

Add your mail domain name in `smtpd_sasl_local_domain` and `virtual_mailbox_domains`

```vim
postconf -e smtpd_sasl_local_domain='pandora.local'
postconf -e virtual_mailbox_domains='pandora.local'
```

Change transport maps setting

```vim
postconf -e transport_maps='hash:/etc/postfix/transport'
```

Verifying SMTP senders, local mail users and mail groups.

```vim
postconf -e smtpd_sender_login_maps='proxy:ldap:/etc/postfix/ad_sender_login_maps.cf'
postconf -e virtual_mailbox_maps='proxy:ldap:/etc/postfix/ad_virtual_mailbox_maps.cf'
postconf -e virtual_alias_maps='proxy:ldap:/etc/postfix/ad_virtual_group_maps.cf'
```

Creating the ad\_sender\_login\_maps.cf file. This file is a configuration file used by the Postfix mail transfer agent (MTA) to map sender email addresses to login names for authentication purposes.

```vim
server_host     = pandora.local
server_port     = 389
version         = 3im
bind            = yes
start_tls       = no
bind_dn         = vmail
bind_pw         = ********
search_base     = cn=users,dc=pandora,dc=local
scope           = sub
query_filter    = (&(userPrincipalName=%s)(objectClass=person)(!(userAccountControl:1.2.840.113556.1.4.803:=2)))
result_attribute= userPrincipalName
debuglevel      = 0
```

Creating the ad\_virtual\_mailbox\_maps.cf file. This is a configuration file used by the Postfix mail transfer agent (MTA) to map email addresses to virtual mailboxes.

```vim
server_host     = pandora.local
server_port     = 389
version         = 3
bind            = yes
start_tls       = no
bind_dn         = vmail
bind_pw         = ********
search_base     = cn=users,dc=pandora,dc=local
scope           = sub
query_filter    = (&(objectclass=person)(userPrincipalName=%s))
result_attribute= userPrincipalName
result_format   = %d/%u/Maildir/
debuglevel      = 0
```

Creating the ad\_virtual\_group\_maps.cf. This is a configuration file used by the Postfix mail transfer agent (MTA) to map email addresses to virtual groups.

```vim
server_host     = pandora.local
server_port     = 389
version         = 3
bind            = yes
start_tls       = no
bind_dn         = vmail
bind_pw         = ********
search_base     = cn=users,dc=pandora,dc=local
scope           = sub
query_filter    = (&(objectClass=group)(mail=%s))
special_result_attribute = member
leaf_result_attribute = mail
result_attribute= userPrincipalName
debuglevel      = 0
```

We first need to test if LDAP works by creating a test account in the AD and verifying sync to the iRedMail server.

```vim
postmap -q test@pandora.local ldap:/etc/postfix/ad_virtual_mailbox_maps.cf
postmap -q test@pandora.local ldap:/etc/postfix/ad_sender_login_maps.cf
```

The expected output should be the email address if the setup is correct. The first time I set this up, it didn't work, so debugging was the solution.
