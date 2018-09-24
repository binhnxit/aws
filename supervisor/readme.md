# Install Supervisord

## Default is python 2.7
sudo alternatives --set python /usr/bin/python2.7
 
## Enabled python
scl enable python27 bash

## Install pip
```sh
yum -y install python27-pip
```
or 
```sh
sudo yum -y install python2-pip
```
 
## Run pip anywhere
```sh
sudo cp /usr/local/bin/pip /usr/bin/
```
 
## Upgrade pip
```sh
sudo pip install --upgrade pip
```
 
## Check if supervisor is available in yum repository
```sh
yum info supervisor
```
 
## Install supervisor with pip
```sh
sudo pip install supervisor
```
 
## Download init script
```sh
curl -O https://raw.githubusercontent.com/Supervisor/initscripts/master/redhat-init-mingalevme
```
 
## Supervisor was installed in /usr/local/bin, change PREFIX from /usr to /usr/local. Rename initscript
```sh
sed -e "s/^PREFIX=\/usr$/PREFIX=\/usr\/local/" redhat-init-mingalevme > supervisord
```
 
## Set permissions on the initscript
```sh
chmod 755 supervisord
 
sudo chown root.root supervisord
```
 
## Put initscript into /etc/init.d
```sh
sudo mv supervisord /etc/init.d
```
 
## Create sample config file
```sh
echo_supervisord_conf | sudo tee /etc/supervisord.conf
```
 
## Edit some config
```sh
sudo nano /etc/supervisord.conf
```
 
```
[unix_http_server]
file=/var/tmp/supervisor.sock   ; the path to the socket file
 
[supervisord]
logfile=/var/log/supervisor/supervisord.log ; main log file; default $CWD/supervisord.log
 
pidfile=/var/run/supervisord.pid ; supervisord pidfile; default supervisord.pid
 
[supervisorctl]
serverurl=unix:///var/tmp/supervisor.sock ; use a unix:// URL  for a unix socket
 
[include]
files = /etc/supervisord.d/*.conf
```
 
## Create folder for store program configs
```sh
sudo mkdir -p /etc/supervisord.d
```
 
## Create folder for log
```sh
sudo mkdir -p /var/log/supervisor
```
 
## Create ampi batch program config file
```sh
sudo nano /etc/supervisord.d/dmm_price_update_batch.conf
```
 
```
[program:laravel-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /var/www/html/dmm_backend/current/artisan queue:work database --sleep=3 --tries=1 --timeout=60000
autostart=true
autorestart=true
user=centos
numprocs=8
redirect_stderr=true
stdout_logfile=/var/log/supervisor/supervisord.log
```
 
#Apply config
```sh
sudo supervisord -c /etc/supervisord.conf
```
 
# Start Supervisord
```sh
sudo cp /usr/bin/supervisord /usr/local/bin/
sudo cp /usr/bin/supervisorctl /usr/local/bin/
sudo service supervisord restart
```
 
# or
```sh
sudo /etc/init.d/supervisord restart
```
 
# Add Supervisord as a service for chkconfig
```sh
sudo chkconfig --add supervisord
```
 
# Make Supervisord turn on at boot
```sh
sudo chkconfig supervisord on

sudo service supervisord restart
sudo service supervisord stop
sudo service supervisord start
sudo service supervisord status
 
sudo /usr/local/bin/supervisorctl status

```