# Samba

## Installing

perform the command bellow to search the right package to install:

````bash
yum search samba	
````

the result should looks like:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-search-result.png)

install the packages samba and samba-client;

````bash
yum install samba samba-client	
````

install epel:

````
yum install epel-release
````

perform update

````
yum update
````

install the wsdd tool for autodiscovery:

````
yum install 
````

override the config file with the example config file:

````bash
cp /ect/samba/smb.conf.example /ect/samba/smb.conf
````

## File sharing

open the config file:

````
vim /etc/samba/smb.conf
````

In the global section, change the workgroup and server string as you wish:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-workgroup-&-server-string.png)

In case you want allow samba to share directories without authentication, insert in the line bellow in the global section:

````bash
map to guest = bad user
````

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-allow-dir-sharing-without-auth.png)

in the end of the configuration file, create a new session for file sharing:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-directories-sharing.png)

now create the directories defined for sharing(in case they not exists)

````bash
mkdir /home/samba /home/samba/arquivos /home/samba/musicas 
````

start the samba services(in sequence):

````bash
systemctl start nmb
systemctl start smb
systemctl start wsdd
````

set them to start with system:

````bash
systemctl enable nmb
systemctl enable smb
systemctl enable wsdd
````

test if things are working properly;

````bash
testparm
````

it should looks like:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-file-share-testparm.png)

you can press ENTER and see the current samba definitions:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-first-definitions.png)

test if things are working properly making a local access;

````bash
smbclient -L 127.0.0.1 -U%
````

it should looks like: 

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-local-tests.png)

