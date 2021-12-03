# Samba - server

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
yum install wsdd
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

now create the directories defined for the share (if they don't exist)

````bash
mkdir /home/samba /home/samba/arquivos /home/samba/musicas 
````

and set the permissions (in this case we allowing all since is just for studies)

````bash
chmod 777 /home/samba/arquivos /home/samba/musicas 
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

test if things are working properly:

````bash
testparm
````

it should looks like:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-file-share-testparm.png)

you can press ENTER and see the current samba definitions:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-first-definitions.png)

test if things are working properly making a local access:

````bash
smbclient -L 127.0.0.1 -U%
````

it should looks like: 

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-local-test.png)

## Accessing a client shared folder 

In order to access a client shared folder, we need a tool that is not directly support. You can install it by running the command:

````bash
yum install cifs-utils
````

Now we can mount the shared folder by running the command *cifs.mount*. As example, we will use the following info:

**Client Ip:** 192.168.200.173

**Shared folder:** Downloads

**Username:** guest

**Password:** -

````bash
mount.cifs //192.168.200.173/Downloads /mnt/ -o username=guest,password= 
````

## Samba as primary domain controller (PDC)

Open the config file 

````bash
vim /etc/samba/smb.conf	
````

set which interfaces will have access. In this case we grant access onty for internal network:

 ![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-network-interfaces.png)

uncomment the lines *domain master* and *domain logons* (~ line 172)

 ![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-domain-master-domain-logon.png)

change the logon script to *smblogon.bat* and set logon path as empty:

 ![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-logon-script-and-path.png)

change the parameters as bellow:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-local-master.png)

In file system options, set the logon drive:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-logon-drive.png)

on home session, uncomment the line *valid users*

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-home-valid-users.png)

uncomment the session *netlogon* and change the lines as bellow:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-net-logon-definitions.png)

save, close the file and then run the command *testparm* to check if everything is working. The result should look like this:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-testparm.png)

now create the directory defined for the netlogon script:

````bash
mkdir /home/samba/netlogon
````

and the script file

````bash
vim home/samba/netlogon/smblogon.bat
````

the content of this file will be:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-smblogon-file-content.png)

we need to change the file codification to work on windows. For this we need to install a tool:

````bash
yum install dos2unix
````

run the command to convert the file:

````bash
unix2dos home/samba/netlogon/smblogon.bat 
````

give permissions to execute:

````bash
chmod 755 home/samba/netlogon/smblogon.bat 
````

restart the service;

````bash
systemctl stop smb
systemctl stop nmb
systemctl start nmb
systemctl start smb
````

### Add samba user

before we can register a user in samba we need him to be registered in the linuizx system, so let's register the root user and two other users in the system. Having him registered in the system, we can use the *smbpasswd* command to include him in the samba.

**Sintax**

````bash
smbpasswd -a USERNAME
````

In this case, we will add the root user, iasmina and lucaschfonseca:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/samba-registering-user.png)

We need to register the machines to limit access only for registered machines. For this, we register the machine as an user of linux and then, register it to samba. Do the following to add a machine;

````bash
useradd -d /dev/null -s /bin/false desktop$
````

lock the login

````bash
passwd -l desktop$
````

add the machine to samba:

````bash
smbpasswd -a -m desktop
````

register the shell in the system's shell relation by opening the file:

````bash
vim /etc/shells
````

and adding the line

````bash
/bin/false
````





