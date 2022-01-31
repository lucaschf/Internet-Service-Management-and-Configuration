# DNS - MASTER

Para o DNS usa-se duas VMs servidoras a servidor 1 e a servidor 2, sendo a servidor 1 a master e a servidor 2 a slave



Usa-se IPs fixos nessas duas VMs

pra isso configure o ip em (ambas em modo bridge) 

````bash
vim /etc/sysconfig/network-scripts/ifcfg-enp0s3
````

![image-20220131101412409](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131101412409.png)

Note que mesmo tendo apenas un DNS, o mesmo deve ser configurado como DNS1

Essa configuracao deve ser realizada em ambas as VMs.

Feita essa configuracao, desligue e ligue novamente a placa:

````bash
ifdown enp0s3
ifup enp0s3
````

## Instalando o DNS

Instale os pacotes bind e bind-utils:

````bash
yum install bind bind-utils -y
````

## Configuracao

Acesse o arquivo named.conf em

````bash
/etc/named.conf
````

e altere as inhas a seguir

![image-20220131102139311](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131102139311.png)

para 

![image-20220131102425728](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131102425728.png)

E 

![image-20220131102356424](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131102356424.png)

para

![image-20220131102445135](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131102445135.png)

salve o arquivo e iniciie o servico:

````bash
systemctl start named
````

Habile para que se inicie junto com a maquina

````bash
systemctl enable named
````

Para testar, use o dig (a primeira consulta pode falhar)

````bash
dig wwww.google.com.br @127.0.0.1
````

![image-20220131103222349](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131103222349.png)

Agora é possivel alterar nas configuracoes da placa de rede, o servidor dns para que use o servidor DNS configurado:

`````bash
vim /etc/sysconfig/network-scripts/ifcfg-enp0s3
`````

![image-20220131103504973](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131103504973.png)



## Configuração de Zonas

**No servidor 1**

Será criado duas zonas

para isso vamos criar um arquivo interno e inclui-lo no arquivo named.conf:

````bash
vim /etc/named.conf
````

inclua no final do arquivo:

![image-20220131104936615](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131104936615.png)

e entao crie o arquivo referenciado no include:

![image-20220131105007022](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131105007022.png)

Dentro deste arquivo vamos criar duas zonas (carros.com.br e animais.com.br):

![image-20220131105339879](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131105339879.png)

verifique se o grupo outros tem acesso a leituro desse arquivo criado(named.local.conf)

Vamos usar o named.localhost como base para configurar os arquivos db, para isso copie o para o diretiorio data:

![image-20220131105609776](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131105609776.png)

Agora vamos editar

![image-20220131105705166](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131105705166.png)

configuracao da zona carros.com.br:

![image-20220131111119289](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131111119289.png)

Configuracao da zona carros.com.br:

Copie o arquivo recem configurado e abra-o:

![image-20220131111210933](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131111210933.png)

![image-20220131111341864](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131111341864.png)

verifique as permissoes de leitura:

![image-20220131111427485](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131111427485.png)

Altere o grupo para que seja named:

![image-20220131111501933](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131111501933.png)

reinicie o servico

````bash
systemctl stop named
systemctl start named
````

Consute o servidor:

![image-20220131111647405](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131111647405.png)

![image-20220131111710447](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131111710447.png)

## Consultando o servidor de email:

![image-20220131111900358](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131111900358.png)

![image-20220131111912323](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131111912323.png)

# DNS Reverso

Abra o arquivo named.local.conf:

![image-20220131113243300](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131113243300.png)

E adicione uma zona reversa:

![image-20220131114456140](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131114456140.png)

copie o arquivo named.loopback para que o mesmo seja usado como arquivo definido em file:

![image-20220131113838740](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131113838740.png)

e faca as alteracoes necessarias:

![image-20220131113911701](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131113911701.png)

![image-20220131114403429](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131114403429.png)

Altere o gurpo do arquivo

![image-20220131114843307](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131114843307.png)

reinicie o servico

````bash
systemctl stop named
systemctl start named
````

para consultar o reverso use o comando dig:

![image-20220131114939146](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131114939146.png)

![image-20220131114952917](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131114952917.png)

# Sincronizacao DNS master-slave

no servidor master, altere o arquivo de zonas:

![image-20220131115218266](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131115218266.png)

Adicione em cada zona, a informacao do slave adicionando a diretiva allow master:

![image-20220131115336300](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131115336300.png)

reinicie o servido dns master

````bash
systemctl stop named
systemctl start named
````

# DNS - SLAVE

instale o bind:

````bash
yum install bind bind-utils -y
````

Acesse o arquivo de configuracao

![image-20220131115635970](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131115635970.png)

altere os parametros allow-port e allow-query

![image-20220131115702524](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131115702524.png)

Inclua no final, o arquivo externo 

![image-20220131115748570](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131115748570.png)

crie o arquivo informando na inclusao

![image-20220131115829954](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131115829954.png)

Crie as zonas que ele recebera do master:

![image-20220131120223538](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131120223538.png)

reinice o servico

````
systemctl stop named
systemctl start named
````

## Verificando se foi sincronizado com sucesso:

![image-20220131120342818](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131120342818.png)

## Testando

![image-20220131120421585](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131120421585.png)

![image-20220131120443573](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131120443573.png)

![image-20220131120512513](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131120512513.png)

![image-20220131120518180](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131120518180.png)

aponte o servidor para que olhe seu proprio dns

![image-20220131120610810](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131120610810.png)

![image-20220131120625587](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131120625587.png)

`````bash
ifdown enp0s3
ifup enp0s3
`````

