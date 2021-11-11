## Squid Proxy

***Each file change must be saved before restarting the service. For squid, we can use the reconfigure option instead of stopping and starting the service:***

````bash
squid -k reconfigure
````

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

restart the service

````bash
systemctl stop squid
systemctl start squid
````

## User authentication

first of all install the httpd-tolls package;

````bash
yum install httpd-tools
````

then create an empty file that will contain the passwords(in production you should hide it)

````bash
touch /etc/squid/senhas.txt
````

Now you can create users with the command htpaswd

````
htpasswd USERS_FILE username 
````

**Example**

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-hpasswd-example.png)

We need to use a third party program to enable squid authenticatio. You can check if the authentication is working by runnig the command:

`````bash
/usr/lib64/squid/basic_ncsa_auth SQUID_PASSWORD_FILE
`````

and inform a registered user and its password.

**Example**

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-auth-test-example.png)

Open the squid config file:

`````bash
vim /etc/squid/squid.conf
`````

Now set the third party auth and the message (~line 583):

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-third-party-auth-config.png)

Now, create an ACL for authentication

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-acl-auth.png)

And change the access for only authenticated

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-allow-only-auth.png)

restart the service

````bash
systemctl stop squid
systemctl start squid
````

Now the client must authenticate to access.

You can check the access logs by accessing the file:

`````bash
/var/log/squid/access.log
`````

**Example**

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-access-logs.png)

##  User-Specific ACL

Open the squid config file:

`````bash
vim /etc/squid/squid.conf
`````

Create an ACL for the desired user (we gonna use maria as example)

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-user-acl-maria.png)

now we can deny the facebook access for maria for example:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-deny-user-maria-facebook.png)

or allow the facebook access for maria during a certain time:

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-acl-noite.png)

![](https://github.com/lucaschf/Internet-Service-Management-and-Configuration/blob/main/images/server/squid-deny-user-maria-facebook-except-night.png)

restart the service

`````bash
systemctl stop squid
systemctl start squid
`````

## Preventing users from bypassing the proxy

run the commands bellow, it will add an rule in firewall to reject any http/https outside proxy.

````bash
iptables -A FORWARD -0 enp0s3 -p tcp --dport 80 REJECT
iptables -A FORWARD -0 enp0s3 -p tcp --dport 443 REJECT
````

***
