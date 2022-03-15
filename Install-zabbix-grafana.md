# Instalação Zabbix Server  
## 1. Instalação Zabbix  
```
# wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-1+ubuntu20.04_all.deb
# dpkg -i zabbix-release_6.0-1+ubuntu20.04_all.deb
# apt update
```
- Instale o servidor, o frontend o Agent e o Banco  
```
# apt install zabbix-server-mysql zabbix-frontend-php zabbix-nginx-conf zabbix-sql-scripts zabbix-agent mysql-server
```
-  Criar o banco de dados inicial. (Tenha certeza que o banco de dados está UP e running):  
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
-  Configure o banco de dados para o servidor Zabbix  
Editar arquivo /etc/zabbix/zabbix_server.conf
`DBPassword=password`  

- Configure o PHP para o frontend Zabbix  
Editar arquivo /etc/zabbix/nginx.conf, descomente e defina as diretivas 'listen' e 'server_name'.
```
# listen 80;
# server_name example.com;
```
- Inicie o servidor Zabbix e os processos do agente
Inicie o servidor Zabbix e os processos do agente e configure-os para que sejam iniciados durante o boot do sistema.
```
# systemctl restart zabbix-server zabbix-agent nginx php7.4-fpm
# systemctl enable zabbix-server zabbix-agent nginx php7.4-fpm
```
- Configure o frontend do Zabbix
Conecte-se ao frontend Zabbix instalado: `http://server_ip_or_name`  
Siga as etapas descritas na documentação do Zabbix: Instalando frontend

# Instalação Grafana
## 1. Para instalar a última versão OSS release:
```
sudo apt-get install -y apt-transport-https
sudo apt-get install -y software-properties-common wget
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
``` 
- Add this repository for stable releases:

`echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list`

- Add this repository if you want beta releases:

`echo "deb https://packages.grafana.com/oss/deb beta main" | sudo tee -a /etc/apt/sources.list.d/grafana.list`

- After you add the repository:
```
sudo apt-get update
sudo apt-get install grafana
``` 
- Start the server with systemd
To start the service and verify that the service has started:
```
sudo systemctl daemon-reload
sudo systemctl start grafana-server
sudo systemctl status grafana-server
sudo systemctl enable grafana-server.service
``` 
- Acesse http://ip do servidor:3000  
User: admin
Pass: admin
