## Instalação do Zabbix Server Versão 4.0 no Ubuntu 18.04

Antes de começar a instalação do zabbix deve instalar o editor de texto “vim”
#### apt-get update
#### apt-get install vim

Instalar pacotes de configuração do repositório que contém os arquivos de configuração do apt para zabbix 4.0 para o ubuntu.
Executar o seguinte comando para instalar o pacote de repositório.
#### echo "deb http://ftp.de.ubuntu.org/ubuntu bionic main non-free" >> /etc/apt/sources.list

Criar uma pasta com o nome zabbix:
#### mkdir /opt/zabbix

Entrar na pasta zabbix:
#### cd /opt/zabbix

Realizar a instalação do zabbix 4.0
#### wget http://repo.zabbix.com/zabbix/4.0/ubuntu/pool/main/z/zabbix/zabbix_4.0.3.orig.tar.gz

Baixar o pacote deb do Zabbix:
#### wget http://repo.zabbix.com/zabbix/4.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_4.0-3+bionic_all.deb

Instalar o pacote deb:
#### dpkg -i zabbix-release_4.0-3+bionic_all.deb

Atualizar a lista de pacotes:
#### apt-get update

Instalar o mysql server, apache2, build-essential, snmp-mibs-downloader, snmp:
#### apt-get install mysql-server apache2 build-essential snmp-mibs-downloader snmp

## Obs: No momento da instalação irá pedir para criar uma senha para o usuário root do Mysql:

Instalar os pacotes do zabbix:
#### apt-get install zabbix-server-mysql zabbix-frontend-php zabbix-agent zabbix-get zabbix-sender zabbix-java-gateway

Realizar a configuração do banco de dados, deve-se entrar com o usuário “ROOT”:
#### mysql -u root –p

Criar um usuário para o mysql: Obs: No exemplo foi criado um usuário chamado “zabbix” e criado uma senha “1234”, porém fica a critério de cada um para definir o nome e a senha.
#### CREATE USER ‘zabbix’@’localhost’ IDENTIFIED BY ‘1234';

Criar o banco de dados: Obs: No exemplo foi criado um banco de dados com o nome “zabbix”, porém fica a critério da pessoa para definir o nome do banco.
#### CREATE DATABASE zabbix CHARACTER SET utf8 collate utf8_bin;

Atribuir privilégios para o usuário do banco de dados:
#### GRANT ALL PRIVILEGES ON zabbix.* TO ‘zabbix’@’localhost’ IDENTIFIED BY ‘1234’;

Sair do mysql:
#### quit;

Descompactar o arquivo tar.gz:
#### tar -xzvf zabbix_4.0.3.orig.tar.gz

Entrar no diretório zabbix 4.0.3:
#### cd zabbix-4.0.3/database/mysql

Popular o banco de dados com os arquivos da pasta:
#### mysql -u zabbix -p zabbix < schema.sql
#### mysql -u zabbix -p zabbix < images.sql
#### mysql -u zabbix -p zabbix < data.sql

Editar o arquivo de configuração do zabbix server:
#### vim /etc/zabbix/zabbix_server.conf
procurar pela linha DBPassword e colocar a senha conforme informado quando criou o usuário EX: DBPassword:1234

Editar o arquivo de configuração da interface web:
#### vim /etc/zabbix/apache.conf
procurar pelas linhas php_value date.timezone Europe/Riga e substituir pela região que se encontra Ex: php_value date.timezone America/Sao_Paulo

Reiniciar o apache:
#### systemctl restart apache2.service

Iniciar o zabbix server:
#### systemctl start zabbix-server

Iniciar o Zabbix Agent:
#### systemctl start zabbix-agent

Habilitar o serviços para serem iniciados junto com o sistema operacional:
#### systemctl enable zabbix-server.service
#### systemctl enable zabbix-agent.service








