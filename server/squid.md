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

enable it to run on boot:

````bash
systemctl enable squid
````

copy the example file and replace the config file:

````bash
cp /usr/share/doc/squid/squid.conf.documented /etc/squid/squid.conf
````

open the config file

`````bash
vim /etc/squid/squid.conf
`````

Go to line ~ 1297 and comment the ACLs like the picture bellow:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-comment-acl.png)

create an ACL for the local network:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-local-network-acl.png)

go to line ~ 1523 and delete the example http_access directive for localnet:

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

then enable the disk cache and define the disk size reserved for it(~ line 3707):

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-disk-cache.png)

then set the maximum and minimum object to be stored in disk(~ lines 3532 and 3549):

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-maximun-disk-object-size.png)

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-minimum-disk-object-size.png)

setup the cache clearance(~ line 3800):

 ![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-cache-clearance.png)

 ![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-cache-clearance-2.png)

setup the max file size download:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-cache-body-max-size.png)

then restart squid:

````basic
systemctl stop squid
systemctl start squid	
````

## ALC for a specific host

open the squid config file

`````bash
vim /etc/squid/squid.conf
`````

create an ACL with the desired src:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-acl-client.png)

### deny access

create a rule deny:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-acl-client-deny.png)

another way for denying  could be allowing all local network with exception of the client:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-acl-client-deny-way-2.png)

save the changes and restart the squid

````bash
systemctl stop squid
systemctl start squid
````

## ACL URL Regex

open the squid config file

````bash
vim /etc/squid/squid.conf	
````

create the acl as the example bellow:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-acl-regex-facebook.png)

then deny the access:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-deny-facebook.png)

save the changes and restart the service

````bash
systemctl stop squid
systemctl start squid
````

PS.: is possible to denby just addin a '!' before it's name:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-deny-facebook-2.png)

### Multiple regex ACL

create an ACL with the path to file containing all denied URLs:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-acl-multiple-url_regex.png)

then deny it:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-deny-multiple-regex.png)

**Important** before restarting squid, make sure you created the file containing all url regex.

then restart the service:

````bash
systemctl stop squid
systemctl start squid
````

## ACL Time

Open the squid config file

`````bash
vim /etc/squid/squid.conf
`````

Create a time ACL like the example below:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-acl-time.png)

Now you can give or deny access in time. In the example bellow we allowing to access the prohibited sites in the lunch time:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-acl-client-deny-url-regex_except-lunch-time.png)

reboot the service

````bash
systemctl stop squid
systemctl start squid
````

