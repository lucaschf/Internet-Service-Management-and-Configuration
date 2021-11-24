# Samba - client

first of all, assure that the windows client is using your internal network:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-client-network-config-1.png)

then, turn on the network discovery:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-turn-on-network-discovery-step-1.png)

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-turn-on-network-discovery-step-2.png)

then, refresh the explorer. Now the shared server folders should be appearing:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-network-sharing-1.png)

***Note:** Ensure that the samba services and the wsdd service are started on server or only the local machine will be displayed.*

if you open the "SERVIDOR" machine, you shall see the two folders that we previously shared on server:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-server-shared-folders.png)

now you can share files and folder inside these folders both from/to file server.

## Sharing from client to server

Suppose we want to share a folder from samba client to samba server, in this case, the downloads folder. We can can do this, by following the steps:

Go to folder's properties, in the sharing tab:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-sharing-from-client-step-1.png)

Click in share and the popup should be displayed:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-sharing-from-client-step-2.png)

We can restrict for am user but in this case we will share for everyone, so select everyone and click add:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-sharing-from-client-step-3.png)

Change the permissions if needed:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-sharing-from-client-step-4.png)

Then click in share. Now your folder is shared:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-sharing-from-client-step-5.png)

But we need to do some more adjustments. So click in advanced sharing and this dialog will appear:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-sharing-from-client-step-6.png)

Here we can configure a few things but we change only option *share this folder* and *permissions*:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-sharing-from-client-step-7.png)

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-sharing-from-client-step-10.png)

Back in the folder's properties, at the bottom we can see ***People must have a user account and password for this computer to access shared folders***. Since we want a public sharing, we will change that. Click in *Network and sharing center* and this window should appear:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-sharing-from-client-step-8.png)

Go to the section *All networks* and change the *Password protect sharing* turning it off:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-sharing-from-client-step-9.png)

Save the changes and close the properties window