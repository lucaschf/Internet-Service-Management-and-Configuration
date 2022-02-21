# Servidor de Email - POSTFIX



Antes de comecar, faca um teste para saber qual a maquina esta respondendo pelos email

````bash
dig -t mx www.animais.com.br
dig -t mx www.lucas.com.br
````

Instale o postfix

````bash
yum install postfix -y
````

````bash
vim /etc/postfix/main.cf
````

Altere o nome do servidor de acordo com o desejado:

![image-20220203110111988](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203110111988.png)

Defina o dominio responsavel por esse email:

![image-20220203110142598](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203110142598.png)

Defina o modo de enderecamento dos emails em myorigin:

![image-20220203110231030](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203110231030.png)

Defina que ele escute todas as interfaces 

![image-20220203110310654](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203110310654.png)

use todos protocolos

![image-20220203110333934](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203110333934.png)

Defiina o mydestination

![image-20220203110446350](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203110446350.png)

Defina a rede confiavel apenas como o localhost

![image-20220203110511806](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203110511806.png)

defina o relayhost como vazio para que ninguem terceirize o servidor de email:

![image-20220203110621742](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203110621742.png)

Defina o formato das mensagens:

 ![image-20220203110730342](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203110730342.png)

Para forcar a autenticacao, insira no final do arquivo:

````bash
# autenticacao
smtpd_sasl_type = dovecot
smtpd_sasl_path = private/auth
smtpd_sasl_auth_enable = yes
smtpd_sasl_security_options = noanonymous
smtpd_sasl_local_domain = 
smtpd_recipient_restrictions= permit_sasl_authenticated, reject_unauth_destination, reject 
````

Salve o arquivo

abra o arquivo :

````bash
vim /etc/postfix/master.cf
````

E habilite o uso da porta 587 descomentando a linha abaixo:

![image-20220203111833681](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203111833681.png)



## Dovecote

````bash
yum install dovecot -y
````

````bash
vim /etc/dovecote/dovecot.conf 
````

Defina com quais protocolos vai trabalhar

![image-20220203112154847](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203112154847.png)



defina com qual versao do IP vai estar trabalhando (* = 4, :: = 6):

![image-20220203112233023](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203112233023.png)



salve o arquivo

````bash
vim /etc/dovecot/conf.d/10-auth.conf 
````

![image-20220203112401695](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203112401695.png)

![image-20220203112431784](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203112431784.png)

salve o arquivo

````bash
vim /etc/dovecot/conf.d/10-mail.conf
````

Descomente a linha para dizer o formato da mensagem:

![image-20220203112559440](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203112559440.png)

`````bash
vim /etc/dovecot/conf.d/10-ssl.conf
`````

desabilite a criptografia

![image-20220203112657719](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203112657719.png)

````bash
vim /etc/dovecot/conf.d/10-master.conf
````

descomente a linha 107 e adicione as linhas conforme abaixo:

![image-20220203112905831](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203112905831.png)



````bash
systemctl stop dovecot
systemctl start dovecot
systemctl enable dovecot

systemctl stop postfix
systemctl start postfix
systemctl enable postfix
````

verifique se os servicos foram inicializados corretamente:

````bash
ps aux | grep dovecot
ps aux | grep postfix
````



## Testando o envio de email

Registre o usuario no thunderbird

![image-20220203113450602](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203113450602.png)

![image-20220203113502240](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203113502240.png)



![image-20220203113853619](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203113853619.png)

![image-20220203113931651](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203113931651.png)

Tente enviar um email

![image-20220203114031123](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203114031123.png)

![image-20220203114045699](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203114045699.png)

![image-20220203114108011](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203114108011.png)

![image-20220203114122898](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203114122898.png)



## Criptografia SMTP

````bash
vim /etc/postfix/main.cf
````

Comente as seguintes linhas:

![image-20220203130637392](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203130637392.png)

![image-20220203130653293](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203130653293.png)

e adicione as seguintes linhas no fim do arquivo

````bash
# criptografia
smtpd_tls_security_level = may
smtpd_tls_key_file = /etc/ssl/mycerts/priv-mail.carros.com.br.pem
smtpd_tls_cert_file = /etc/ssl/mycerts/cert-mail.carros.com.br.pem
smtpd_tls_loglevel = 1
smtpd_tls_session_cache_timeout = 3600s
smtpd_tls_session_cache_database = btree:/var/lib/postfix/smtpd_tls_cache
tls_random_source = dev:/dev/urandom
tls_random_exchange_name = /var/lib/postfix/prng_exch
smtpd_tls_auth_only = yes
````

salve e reinicie o servico

````bash
systemctl stop postfix
systemctl start postfix		
````

No thunderbird, ative a criptografia:

![image-20220203131439378](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203131439378.png)

![image-20220203131452976](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203131452976.png)

Envie um novo email 

![image-20220203131715800](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203131715800.png)

![image-20220203131724854](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203131724854.png)

![image-20220203131731887](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203131731887.png)

![image-20220203131737707](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203131737707.png)

reenvie depois de acitar o certificado

![image-20220203131833257](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203131833257.png)

Tente sem criptografia

![image-20220203131941673](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203131941673.png)

![image-20220203132021640](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203132021640.png)

![image-20220203132042098](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203132042098.png)

![image-20220203132051161](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203132051161.png)



![image-20220203132409512](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203132409512.png)

volte as configuracoes para

![image-20220203132507383](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203132507383.png)

![image-20220203132556665](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203132556665.png)

## Criptografia dovecot

````bash
vim /etc/dovecot/conf.d/10-ssl.conf
````

![image-20220203132914265](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203132914265.png)

![image-20220203132953424](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203132953424.png)

![image-20220203133021800](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203133021800.png)

salve e reinicie o servico

````bash
systemctl stop dovecot
systemctl start dovecot
````

check a inicializacao

![image-20220203133123128](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203133123128.png)

![image-20220203133139602](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203133139602.png)

No thunderbird

![image-20220203133243779](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203133243779.png)



## Envio de um servidor para outro

````bash
vim /etc/postfix/main.cf
````



````bash
na autenticacao altere a ultima linha
smtpd_relay_restrictions= permit_sasl_authenticated, reject_unauth_destination 
````

````bash
na criptografia adicione a linha
smtpd_sasl_tls_security_options = noanonymous
````

````
vim /etc/postfix/master.cf
````

![image-20220203224805407](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220203224805407.png)

reinicie o postfix







