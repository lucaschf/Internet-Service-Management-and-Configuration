# FTP

## Instalando

````bash
yum install vsftpd -y
````

Acesse o arquivo de configuracao

````bash
vim /etc/vsftpd/vsftpd.conf 
````

Altere o umask para 027 // leitura escrita e ou execucao

![image-20220202212211593](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220202212211593.png)

adicione a linha (linha 101)

````
allow_writeable_chroot=YES
````

![image-20220202212313000](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220202212313000.png)

descomente a linha abaixo

![image-20220202212338703](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220202212338703.png)

salve e saia do arquivo

inicie o servico

````bash
systemctl stop vsftpd
systemctl start vsftpd
systemctl enable vsftpd
````

## Criando usuarios e suas senhas

````bash
useradd -d /var/www/html/animais webanimais
useradd -d /var/www/html/carros webcarros

passwd webanimais
passwd webcarros
````

## Configure as permissoes

````bash
chown -R webcarros:apache /var/www/html/carros
chown -R webanimais:apache /var/www/html/animais
````

Ajuste a permissao

````
chmod g+s  /var/www/html/carros
chmod g+s  /var/www/html/animais
````

check as permissoes:

![image-20220202213022129](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220202213022129.png)

Faca um acesso atraves do filezila

## Adicionando criptografia

````bash
vim /etc/vsftpd/vsftpd.conf 
````

Adicione uma sessao de criptografia no final do arquivo

````bash
#Criptografia
ssl_enable=YES # habilita a criptografia
allow_anon_ssl=NO # bloqueia o acesso com usuario anonimo
force_local_data_ssl=YES # ativa a criptografia dos dados
force_local_logins_ssl=YES # ativa a criptografia na comunicacao de controle
ssl_tlsv1=YES # forca o uso da versao tlsv1
ssl_sslv2=NO # desabilita o uso 
ssl_sslv3=NO # desabilita o uso 
rsa_cert_file=/etc/ssl/mycerts/cert-ftp.meuprovedor.com.pem
rsa_private_key_file=/etc/ssl/mycerts/priv-ftp.meuprovedpr.combr.pem
````

