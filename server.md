## Disable firewal:

````bash
systemctl stop firewalld
systemctl disable firewalld
````

## Disable selinux:

`````bash
vim /etc/sysconfig/selinux
`````

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/linuxse-disabled.png)

## Internal network Ip configuration

````bash
ip addr add 162.126.200.1/24 dev enp0s8
````

change the configuration file to look like the image below:

````bash
vim /etc/sysconfig/network-scripts/ifcfg-enp0s8
````

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/internal-network-interface-config-file.png)

turn the interface down, then turn it up again

````bash
ifdown enp0s8
ifup enp0s8
````

then setup the masking:

````bash
iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE
echo 1 > /proc/sys/net/ipv4/ip_forward
````

### Making settings permanent

open the following file:

````bash
vim /etc/sysctl.d/99-sysctl.conf
````

add the line below: 

````bash
net.ipv4.ip_forward=1
````

It will look like:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/internal-networking-forwarding.png)

then open the following file:

````bash
vim /etc/rc.d/rc.local
````

and add the iptables command.

````bash
iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE
````

 It will looks like:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/internal-networking-forwarding-2.png)

give it permission to execute:

````bash
chmod +x /etc/rc.d/rc.local
````

And is done! reboot the system and check if is connecting from the client side.

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

**Note:** The remote path syntax consists of the remote machine's user followed by the '@' character, the remote machine's ip the ':' character and then the file/folder path on the remote machine.

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

Now stop and start the ssh service

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

Now stop and start the ssh service:

````bash
systemctl stop sshd
systemctl start sshd	
````

## DHCP

check for the service.

`````bash
ps aux | grep dhcpd
`````

it should return nothing:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/dhcp-intial-service-check-result.png)

search for dhcp:

````bash
yum search dhcp
````

install:

`````bash
yum install dhcp-server	
`````

open config file

````bash
vim /etc/dhcp/dhcpd.conf
````

use the example file:

````bash
cp /usr/share/doc/dhcp-server/dhcpd.conf.example /etc/dhcp/dhcpd.conf
````

open config file again

`````bash
vim /etc/dhcp/dhcpd.conf
`````

comment out all lines below the subnet line:

 ![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/dhcp-comment-lines.png)

change the domain name:

 ![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/dhcp-domain-name.png)

change the dns domain:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/dhcp-dns-domain-server.png)

If you want to change the ip checking for ociosity and the max time of an ip in a machine, change these values:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/dhcp-lease-time.png)

setup the subnet, the range of ip reserved for DHCP and the subnet gateway:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/dhcp-subnet-config.png)

restart the service:

````bash
systemctl stop dhcpd
systemctl start dhcpd	
````

enable dhcp to start on boot:

`````bash
systemctl enable dhcpd
`````

Test in the client side.

### Fixing an IP to a machine

open the dhcp config file:

`````bash
vim /etc/dhcp/dhcpd.conf
`````

create a host with an identifier, MAC and the desired IP address(You can retrieve the mac address by running the command *ip addr* in the client):

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/dhcp-fixing-ip-to-host.png)

restart the service:

`````bash
systemctl stop dhcpd
systemctl start dhcpd
`````

## Squid Proxy

check package name

`````bash
yum search squid
`````

install 

`````bash
yum install squid
`````

check squid file config

`````bash
vim /etc/squid/squid.conf
`````

copy the example file and replace the config file:

````bash
cp /usr/share/doc/squid/squid.conf.documented /etc/squid/squid.conf
````

open the config file

`````bash
vim /etc/squid/squid.conf
`````

Go to line 1297 and comment the ACLs like the picture bellow:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-comment-acl.png)

create an ACL for the local network:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-local-network-acl.png)

go to line 1523 and delete the example http_access directive for localnet:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-directive-local-network-custom-add.png)

then add a directive for your local network:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-directive-local-network-custom-add.png)

then save the file and start the service

````bash
systemctl start squid
````

### Cache

open the squid config file

````bash
vim /etc/squid/squid.conf
````

go to line 3435 and uncomment it:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-cache-enable.png)

change the cached objects size :

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-cache-objects-size.png)

then enable the disk cache and define the disk size reserved for it(line 3707):

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-maxi-disk-object-size.png)

then set the maximum and minimum object to be stored in disk(lines 2544 and 3527):

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-maximum-disk-object-size.png)

