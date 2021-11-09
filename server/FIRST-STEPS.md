## Disable firewall:

````bash
systemctl stop firewalld
systemctl disable firewalld
````

## Disable selinux:

`````bash
vim /etc/sysconfig/selinux
`````

 ![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/linuxse-disabled.png)

## Internal network Ip configuration

````bash
ip addr add 162.126.200.1/24 dev enp0s8
````

change the configuration file to look like the image below:

````bash
vim /etc/sysconfig/network-scripts/ifcfg-enp0s8
````

 ![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/internal-network-interface-config-file.png)

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
