## 1. Instalação Zabbix  
```
# wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-1+ubuntu20.04_all.deb
# dpkg -i zabbix-release_6.0-1+ubuntu20.04_all.deb
# apt update
```
## 2. Instale o servidor, o frontend o Agent e o Banco  
```
# apt install zabbix-server-mysql zabbix-frontend-php zabbix-nginx-conf zabbix-sql-scripts zabbix-agent mysql-server
```
## 3. Criar o banco de dados inicial. (Tenha certeza que o banco de dados está UP e running):  
Execute os seguintes passos em seu host de banco de dados.
```
# mysql -uroot -p
password
mysql> create database zabbix character set utf8mb4 collate utf8mb4_bin;
mysql> create user zabbix@localhost identified by 'password';
mysql> grant all privileges on zabbix.* to zabbix@localhost;
mysql> quit;
```
- No servidor do Zabbix, importe o esquema inicial e os dados. Vocá será solicitado a inserir a senha que foi criada anteriormente.

` # zcat /usr/share/doc/zabbix-sql-scripts/mysql/server.sql.gz | mysql -uzabbix -p zabbix` 
## 4. Configure o banco de dados para o servidor Zabbix  
- Editar arquivo /etc/zabbix/zabbix_server.conf
`DBPassword=password`  

## 4.1 Configure o PHP para o frontend Zabbix  
- Editar arquivo /etc/zabbix/nginx.conf, descomente e defina as diretivas 'listen' e 'server_name'.
```
# listen 80;
# server_name example.com;
```
## 4.2 Inicie o servidor Zabbix e os processos do agente
Inicie o servidor Zabbix e os processos do agente e configure-os para que sejam iniciados durante o boot do sistema.
```
# systemctl restart zabbix-server zabbix-agent nginx php7.4-fpm
# systemctl enable zabbix-server zabbix-agent nginx php7.4-fpm
```
## 4.3 Configure o frontend do Zabbix
Conecte-se ao frontend Zabbix instalado: `http://server_ip_or_name`  
Siga as etapas descritas na documentação do Zabbix: Instalando frontend




