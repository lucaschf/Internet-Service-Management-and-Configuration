

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

save the changes and  restart the service:

````bash
systemctl stop dhcpd
systemctl start dhcpd
````

