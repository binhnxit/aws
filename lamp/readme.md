# Install Apache php
## Remove current apache & php
```sh
sudo yum remove httpd* php*
```
  
## Install Apache 2.4
```sh
sudo yum install httpd24
```
 
## Install PHP
```sh
sudo yum install php71
sudo yum install php71-cli php71-common php71-json php71-process php71-pdo php71-mysqlnd php71-mbstring php71-mcrypt php71-opcache php71-xml
```
 
## Install mysql client
```sh
sudo yum install mysql57
```
 
## Install git
```sh
sudo yum install git
```
 
## Install composer
```sh
sudo curl -sS https://getcomposer.org/installer | sudo php
sudo mv composer.phar /usr/local/bin/composer
sudo ln -s /usr/local/bin/composer /usr/bin/composer
composer -V
 ```
## Make Apache start automatically on boot
```sh
sudo chkconfig httpd on
sudo service httpd start
```
  
## Change owner
```sh
sudo usermod -a -G apache ec2-user
exit
groups
sudo chown -R ec2-user:apache /var/log/httpd
sudo chown -R ec2-user:apache /var/www
```
  
## Change AllowOverride from None -> All (/var/www/html)
```sh
sudo nano /etc/httpd/conf/httpd.conf  
```
 
 ```
<Directory /var/www/html>
  
    ...
    AllowOverride None -> All
</Directory>
 ```
 
## Restart apache
```sh
sudo service httpd restart
```
 
## Install node & gulp
```sh
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```

```sh
  
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```
```sh  
source ~/.bashrc
nvm --version
nvm install v6.9.5
nvm use 6.9.5
```
 
## Install yarn
```sh
sudo wget https://dl.yarnpkg.com/rpm/yarn.repo -O /etc/yum.repos.d/yarn.repo

curl --silent --location https://rpm.nodesource.com/setup_6.x | sudo bash -

sudo yum install yarn

yarn --version

```



## Disabled SELINUX
```sh
sudo nano /etc/selinux/config
SELINUX=disabled

```


## Prepare deploy

```sh
cd /var/www/html
 
mkdir -p ampi/shared
cd ampi/shared
nano .env
 
mkdir -p storage/framework/cache storage/framework/sessions storage/framework/views storage/framework/testing

```
 
## Uploads folder
```sh
mkdir -p storage/app/uploads
sudo chmod -R ug+rwx storage
sudo chown -RH ec2-user:apache storage
```
