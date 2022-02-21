# Apache Web server - SERVER

para configurar os nomes com os quais a maquina deve ser reconhecida, acesse o arquivo hosts. (Vc deve saber qual o ip da maquina para isso - no nosso caso 192.168.0.117)

````bash
vim /etc/hosts
````

insira uma linha indicando qual o ip e os nomes reconhecidos:

![image-20220131152257030](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131152257030.png)

para fazer um teste, execute um ping em um dos nomes informados no arquivo:

![image-20220131152344863](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131152344863.png)

## Instalação do servidor web

````bash
yum install httpd -y
````

O arquivo de configuracao principal fica localizado em /etc/httpd/conf

Para inicializar o servico:

````bash
systemctl start httpd
systemctl enable httpd
````

Para testar, abra o navegador e insira o endereco do servidor:

![image-20220131154539015](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131154539015.png)

A mesma pagina sera exibida se navegarmos para um dos nomes mapeados no servidor:

![image-20220131154657817](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131154657817.png)

## Configurando os sites hospedados:

Crie um arquivo conf para o site a ser configurado no diretorio /etc/httpd/conf.d. Neste caso faremos para o animais.com.br e carros.com.br:

![image-20220131155006558](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131155006558.png)

![image-20220131155049573](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131155049573.png)

![image-20220131155307901](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131155307901.png)

![image-20220131155338181](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131155338181.png)

reinicie o servico

````bash
systemctl start httpd
systemctl enable httpd
````

Crie um conteudo qualquer para os sites: 

![image-20220131155448805](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131155448805.png)

![image-20220131155516380](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131155516380.png)

![image-20220131155551803](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131155551803.png)

![image-20220131155630100](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131155630100.png)

Navegue novamente e vera o resultado: 

![image-20220131155704716](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131155704716.png)

![image-20220131155711692](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131155711692.png)

## Estrutura dos sites hospedados

para configurar o site animais, navegue /var/www/html/animais:

![image-20220131162435538](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131162435538.png)

caso exista um index.html no diretório, apague-o 

***Crie sua estrutura com paginas e imagens***

Um Exemplo

![image-20220131162948499](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131162948499.png)



![image-20220131162959475](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131162959475.png)



![image-20220131163231754](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131163231754.png)



![image-20220131163242132](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131163242132.png)

![image-20220131163508837](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131163508837.png)

![image-20220131163530323](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131163530323.png)

![image-20220131163920477](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131163920477.png)

![image-20220131164129993](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131164129993.png)

  

## Removendo pagina de teste

comente todo o conteudo do arquivo welcome.conf localizado em /etc/httpd/conf.d

![image-20220131164617819](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131164617819.png)



![image-20220131164536851](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131164536851.png)

reinicie o serviço

`````bash
systemctl stop httpd
systemctl start httpd
`````

Crie uma estrutura para o provedor 

![image-20220131164806947](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131164806947.png)

![image-20220131174007503](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131174007503.png)

crie um arquivo 000-default.conf no diretorio conf.d

![image-20220131174120518](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131174120518.png)

e defina o seguinte conteudo

![image-20220131174240174](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131174240174.png)

Reinicie o servico 

````bash
systemctl start httpd
systemctl enable httpd
````

Agora, ao digitarmos o ip do servidor, sera carregada a pagina do provedor

![image-20220131174338934](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131174338934.png) 

## Gerando chaves de criptografia

para permitir que qualquer usuario crie suas chaves, abra o arquivo openssl.cnf:

![image-20220131175630333](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131175630333.png)

e altere a diretiva HOME:

![image-20220131175723973](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131175723973.png)

Na sessao CA_default altere a diretiva dir :

![image-20220131175811772](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131175811772.png)

salve e feche o arquivo.

## Gerando  as chaves 

https://drive.google.com/file/d/112S-Y2gbs0r8dH2eugfgvZ3PFidQxZ3T/view

![image-20220131175940390](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131175940390.png)

### montando o cartorio

![image-20220131180153677](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131180153677.png)



![image-20220131180314851](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131180314851.png)

![image-20220131180426597](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131180426597.png)

**criacao de chaves** 

![image-20220131181023226](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131181023226.png)

![image-20220131181035316](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131181035316.png)

![image-20220131181134812](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131181134812.png)

mesma coisa pra: 

![image-20220131181151012](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131181151012.png)

chaves criadas:

![image-20220131181313593](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131181313593.png)

**Assinatura das chaves**

![image-20220131181426916](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131181426916.png)

mesma coisa para o restante das chaves

**Relacao de chaves/certificados**

![image-20220131181450236](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131181450236.png)

local de armazenamento dos certificados criados: 

````bash
/etc/ssl/mycerts
````

copie todos os certificados gerados anteriormente para este diretorio ( oexemplo abaixo usa o ssh uma vez que os certificados foram gerados em uma maquina diferente da que vai ser usada)

![image-20220131181904911](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131181904911.png)

copie tambem todas as chaves privadas

![image-20220131181843397](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131181843397.png)

Dê permissao de leitura ao grupo e adicione o apache ao grupo das chves privadas

![image-20220131182035430](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131182035430.png)

![image-20220131182100983](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131182100983.png)



## Criptografia 

### Instalacao 

````bash
yum install mod_ssl -y
````

reinicie o servico

````bash
systemctl start httpd
systemctl enable httpd
````

Execute a copia dos arquivos de configuracao transformando-os em ssl:

![image-20220131161112908](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131161112908.png)

Abra o arquivo recem criado

![image-20220131161139331](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131161139331.png)

e configure como abaixo
![image-20220131161247684](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131161247684.png)

Para ativar o uso obrigatorio do https, altere o arquivo de configuracao http

![image-20220131161459107](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131161459107.png)

![image-20220131162048313](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131162048313.png)

reinicie o servico: 

````bash
systemctl start httpd
systemctl enable httpd
````

Acesse e vera a seguinte mensagem

![image-20220131161643000](C:\Users\lucas\AppData\Roaming\Typora\typora-user-images\image-20220131161643000.png)