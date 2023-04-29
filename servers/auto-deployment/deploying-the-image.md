# Deploying the image

Deploying the image is straight forward. We start up a new client and boot it via PXE (network boot). This will give us the same options as when we captured the image.

<div>

<figure><img src="../../.gitbook/assets/Deploy_FOG_Image_13.png" alt=""><figcaption><p>Deployment CLI</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/Deploy_FOG_Image_12.png" alt=""><figcaption><p>Deployment Questions</p></figcaption></figure>

</div>

<figure><img src="../../.gitbook/assets/Deploy_FOG_Image_14.png" alt=""><figcaption><p>FOG image transfer</p></figcaption></figure>

After the full installation, the pc will automaticly join the domain. This is the part of the FOG log where the pc succesfully joined the domain.

```
------------------------------------------------------------------------------
--------------------------------HostnameChanger-------------------------------
------------------------------------------------------------------------------
 4/29/2023 2:38:51 PM Client-Info Client Version: 0.13.0
 4/29/2023 2:38:51 PM Client-Info Client OS:      Windows
 4/29/2023 2:38:51 PM Client-Info Server Version: 1.5.10
 4/29/2023 2:38:51 PM Middleware::Response Success
 4/29/2023 2:38:51 PM HostnameChanger Checking Hostname
 4/29/2023 2:38:51 PM HostnameChanger Renaming host to 000c295691aa
 4/29/2023 2:38:51 PM HostnameChanger Joining domain
 4/29/2023 2:38:55 PM HostnameChanger Success, code =  0
 4/29/2023 2:38:55 PM Power Creating shutdown command in 60 seconds
 4/29/2023 2:38:55 PM Bus Emmiting message on channel: Power
------------------------------------------------------------------------------
```
