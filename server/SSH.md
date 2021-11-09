## SSH remote service

check if the service is already working:

````bash
ps aux | grep sshd	
````

the result will looks like:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/checking-ssh-status.png)

## Accessing  shh

with default port:

````bash
ssh <username>@<remotIp> 
````

with custom port:

````bash
ssh -p <port> <username>@<remoteIp>
````

## Copying file with ssh

with default port:

````bash
scp <sourcePath> <destinationPath>
````

with custom port:

````
scp -P <port> <sourcePath> <destinationPath>
````

**Note:** The remote path syntax consists of the remote machine's user followed by the '@' character, the remote machine's IP the ':' character and then the file/folder path on the remote machine.

### Examples

copying from the remote machine to the current directory of the local machine:

**remote user: root**
**remote ip: 192.168.200.1**
**path of the remote file: /etc/yum.conf**

with default port:

````bash
scp root@192.168.200.1:/etc/yum.conf ./
````

with custom port 23456:

````bash
scp -P 23456 root@192.168.200.1:/etc/yum.conf ./
````

copy from local machine to the remote machine's '/tmp' directory:

**remote user: root**
**remote ip: 192.168.200.1**
**file path: /etc/yum.conf**

with default port:

````bash
scp /etc/yum.conf root@192.168.200.1:/tmp
````

with custom port 23456:

````bash
scp -P 23456 /etc/yum.conf root@192.168.200.1:/tmp
````

## Making ssh listen only a specific address

open the following file:

````bash
vim /etc/ssh/sshd_config
````

and change the listen address parameter with the desired address:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/ssh-listen-address.png)

**Note:** use the internal network ip from the server for the address

Now save the changes, stop and start the ssh service

````bash
systemctl stop sshd
systemctl start sshd
````

## Permit or not root access on ssh

open the following file:

````bash
vim /etc/ssh/sshd_config
````

and change the listen PermitRootLogin parameter

### Example

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/disable-ssh-root-access.png)

Now stop and start the ssh service

````bash
systemctl stop sshd
systemctl start sshd
````

## Changing ssh port 

open the following file:

````bash
vim /etc/ssh/sshd_config
````

and change the listen Port parameter

### Example

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/ssh-change-port.png)

Now save the changes, stop and start the ssh service:

`````bash
systemctl stop sshd
systemctl start sshd	
`````

