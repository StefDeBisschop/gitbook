---
description: 'Step 7: Setting up the CA for HTTPS traffic'
---

# Certificate Authority

As you might have noticed in the previous step, the site didn't have a secure SSL connection to the workstation. Time to fix that with a CA; run on Linux in OpenSSL

## Setting up the VM

The VM for running a Ubuntu 22.04 server that hosts OpenSSL has following hardware;

* 1 CPU
* 2 GB RAM
* 10GB HARD DISK

It is better to organize the new CA with dedicated folders; so I first made those. First we made an empty file and stored '1000' in it. This indicates the next (first) serial number for the certificate.

```shell
sudo mkdir /root/ca/
sudo mkdir /root/ca/certs/
sudo mkdir /root/ca/crl/
sudo mkdir /root/ca/newcerts/
sudo mkdir /root/ca/private/
sudo mkdir /root/ca/requests/

sudo touch /root/ca/index.txt
echo ‘1000’ > /root/ca/serial
```

### Self-signed certificate

At first we create the self-signed certificate for the CA itself (so the CA trusts itself). We generate a AES256 4096 bit key that we will output to the private folder.

```shell
cd /root/ca/
openssl genrsa -aes256 -out private/cakey.pem 4096
```

Next, we create a certificate that expires after 3650 days or 10 years, uses the previous created key and outputs this certificate to cacert.pem.

```shell
openssl req -new -x509 -key /root/ca/private/cakey.pem -out cacert.pem -days 3650
```

When we execute this, we are asked a few questions:

Country Name (2 letter code) \[AU]:**BE**\
State or Province Name (full name) \[Some-State]:**Oost-Vlaanderen**\
Locality Name (eg, city) \[]:**Aalst**\
Organization Name (eg, company) \[Internet Widgits Pty Ltd]:**PANDORA**\
Organizational Unit Name (eg, section) \[]:\
Common Name (e.g. server FQDN or YOUR name) \[]:**ca.pandora.local**\
Email Address \[]:**admin@pandora.local**

We need to edit the configuration file of OpenSSL

```shell
vim /usr/lib/ssl/openssl.cnf
```

Because of the creation of the new directories before, we need to change this in the config file. The root directory is now /root/ca. In this directory we can find the child dir certs, crl\_dir, newcerts, serial file, ... . Changing the private key file name is also needed.

```vim
[ CA_default ]

dir             = /root/ca              # Where everything is kept
certs           = $dir/certs            # Where the issued certs are kept
crl_dir         = $dir/crl              # Where the issued crl are kept
database        = $dir/index.txt        # database index file.
#unique_subject = no                    # Set to 'no' to allow creation of
                                        # several certs with same subject.
new_certs_dir   = $dir/newcerts         # default place for new certs.

certificate     = $dir/private/cacert.pem       # The CA certificate
serial          = $dir/serial           # The current serial number
crlnumber       = $dir/crlnumber        # the current crl number
                                        # must be commented out to leave a V1 CRL
crl             = $dir/crl.pem          # The current CRL
private_key     = $dir/private/cakey.pem# The private key

```

We're keeping the other default setting of OpenSSL

```
default_days    = 365                   # how long to certify for
default_crl_days= 30                    # how long before next CRL
default_md      = default               # use public key default MD
preserve        = no                    # keep passed DN ordering

# For the CA policy
[ policy_match ]
countryName             = match
stateOrProvinceName     = match
organizationName        = match
organizationalUnitName  = optional
commonName              = supplied
emailAddress            = optional
```

