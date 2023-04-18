# Integrate AD - Roundcube Address Book

Integrating the AD sync to the address book requires us to change the Roundcube config file.

```vim
#
# "sql" is personal address book stored in roundcube database.
# "global_ldap_abook" is the new LDAP address book for AD, we will create it below.
#
$config['autocomplete_addressbooks'] = array("sql", "global_ldap_abook");

# Enable setting below if Roundcube returns 'user@127.0.0.1' as email address
#$config['mail_domain'] = '%d';

#
# Global LDAP Address Book with AD.
#
$config['ldap_public']["global_ldap_abook"] = array(
    'name'          => 'Global Address Book',
    'hosts'         => array("ad.pandora.local"), // <- Set AD hostname or IP address here.
    'port'          => 389,
    'use_tls'       => false,   // <- Set to true if you want to use LDAP over TLS.
    'ldap_version'  => '3',
    'network_timeout' => 10,
    'user_specific' => false,

    'base_dn'       => "cn=users,dc=pandora,dc=local", // <- Set base dn in AD
    'bind_dn'       => "vmail",             // <- bind dn
    'bind_pass'     => "********", // <- bind password
    'writable'      => false,               // <- Do not allow mail user write data back to AD.

    'search_fields' => array('mail', 'cn', 'sAMAccountName', 'displayname', 'sn', 'givenName'),

    // mapping of contact fields to directory attributes
    'fieldmap' => array(
        'name'          => 'cn',
        'displayname'   => 'displayName',
        'surname'       => 'sn',
        'firstname'     => 'givenName',
        'jobtitle'      => 'title',
        'department'    => 'department',
        'company'       => 'company',
        'email'         => 'mail:*',
        'phone:work'    => 'telephoneNumber',
        'phone:home'    => 'homePhone',
        'phone:mobile'  => 'mobile',
        'phone:workfax' => 'facsimileTelephoneNumber',
        'phone:pager'   => 'pager',
        'phone:other'   => 'ipPhone',
        'street:work'   => 'streetAddress',
        'zipcode:work'  => 'postalCode',
        'locality:work' => 'l',
        'region:work'   => 'st',
        'country:work'  => 'c',
        'notes'         => 'description',
        'photo'         => 'jpegPhoto',   // Might be 'thumbnailPhoto' for
                                          // compatibility with some other
                                          // Microsoft software
        'website'       => 'wWWHomePage',
    ),
    'sort'          => 'cn',
    'scope'         => 'sub',
    'filter'        => "(&(|(objectclass=person)(objectclass=group))(!(userAccountControl:1.2.840.113556.1.4.803:=2)))",
    'fuzzy_search'  => true,
    'vlv'           => false,   // Enable Virtual List View to more
                                // efficiently fetch paginated data
                                // (if server supports it)
    'sizelimit'     => '0',     // Enables you to limit the count of
                                // entries fetched. Setting this to 0
                                // means no limit.
    'timelimit'     => '0',     // Sets the number of seconds how long
                                // is spend on the search. Setting this
                                // to 0 means no limit.
    'referrals'     => false,   // Sets the LDAP_OPT_REFERRALS option.
                                // Mostly used in multi-domain Active
                                // Directory setups
);
```
