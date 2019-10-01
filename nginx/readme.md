# Install env for mac os

## Homebrew
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

brew install git
```

## Zsh (can skip)
```
brew install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

brew install nginx

sudo brew services start nginx

brew install php

brew services start php

brew install php@7.1

brew services start php@7.1

brew install php@5.6

brew services start php@5.6

brew install mysql

brew services start mysql

mysql -u root
```

## Change password for root mysql
```
USE mysql;
```
### mysql 5.7
```
UPDATE user SET authentication_string=PASSWORD("secret") WHERE User='root';
```
#$$ mysql 8.0
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'secret';

FLUSH PRIVILEGES;

quit;
```
## DNS Resolver (don't need add more domain to /etc/host)
```
brew install dnsmasq

echo 'address=/.test/127.0.0.1\naddress=/.service/127.0.0.1\naddress=/.package/127.0.0.1\naddress=/.learn/127.0.0.1' > /usr/local/etc/dnsmasq.conf

sudo brew services start dnsmasq

sudo mkdir -v /etc/resolver

sudo bash -c 'echo "nameserver 127.0.0.1" > /etc/resolver/test'

sudo bash -c 'echo "nameserver 127.0.0.1" > /etc/resolver/service'

sudo bash -c 'echo "nameserver 127.0.0.1" > /etc/resolver/package'

sudo bash -c 'echo "nameserver 127.0.0.1" > /etc/resolver/learn'
```
## Root resource code (can skip)
sudo mkdir /www

sudo chown binhnx:staff /www

## Visual studio code (can skip)
brew cask install visual-studio-code

## Config php
```
code /usr/local/etc/php
```

X.X/php-fpm.d/www.conf
```
user = binhnx
group = staff
listen = /usr/local/var/run/php72.sock
listen.owner = binhnx
listen.group = staff
listen.mode = 0660
```

touch /usr/local/etc/nginx/php72.conf
```
location ~ \.php$ {
    try_files $uri =404;
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    include fastcgi_params;
    fastcgi_pass unix:///usr/local/var/run/php72.sock;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param SCRIPT_NAME $fastcgi_script_name;
    fastcgi_param SERVER_NAME $host;
    #fastcgi_param HTTP_X_REQUEST_ID $x_request_id;
    fastcgi_index index.php;
}

```

## Config nginx
```
code /usr/local/etc/nginx/nginx.conf
```
```
user binhnx staff;

    server {
        listen       80;
        server_name  localhost local.test;
        index index.php;

        root /Code;
        location / {
            try_files $uri $uri/ /index.php$is_args$args;
        }

        include php72.conf;
    }
    
    server {
        listen       80;
        server_name  control.drone.test;
        index index.php;

        root /www/drone/web/control/public;
        location / {
            try_files $uri $uri/ /index.php$is_args$args;
        }

        include php71.conf;
    }    

```
```
brew services restart php
brew services restart php@7.1
brew services restart php@5.6

sudo brew services restart nginx
```
## Multi git acc
```
ssh-keygen -t rsa -C "binh.nx@abc.com"
```
/Users/binhnx/.ssh/id_rsa_bitbucket
```
ssh-keygen -t rsa -C "binhnx.it@gmail.com"
```
/Users/binhnx/.ssh/id_rsa_github

```
cd ~/.ssh/
touch config
```
```
Host github.com
  HostName github.com
  IdentityFile ~/.ssh/id_rsa

Host bitbucket.org-nl
  HostName bitbucket.org
IdentityFile ~/.ssh/id_rsa_nl

Host bitbucket.org-wh
  HostName bitbucket.org
IdentityFile ~/.ssh/id_rsa_binh_github

Host *
  UseKeychain yes
  AddKeysToAgent yes

```
```
ssh-add -D

ssh-add ~/.ssh/id_rsa_bitbucket
ssh-add ~/.ssh/id_rsa_github

ssh-add -l

cat ~/.ssh/id_rsa_bitbucket.pub | pbcopy
cat ~/.ssh/id_rsa_github.pub | pbcopy
```
```
brew install composer

brew install node

brew install nvm

```
```
code ~/.zshrc

  export NVM_DIR="$HOME/.nvm"
  . "/usr/local/opt/nvm/nvm.sh"

```

## https generate

```
cd /usr/local/ec/nginx

cat > openssl.cnf <<-EOF
[req]
distinguished_name = req_distinguished_name
x509_extensions = v3_ca
prompt = no
[req_distinguished_name]
CN = local.test
[v3_ca]
keyUsage = digitalSignature, keyEncipherment
extendedKeyUsage = serverAuth
subjectAltName = @alternate_names
[alternate_names]
DNS.1 = local.test
DNS.2 = *.local.test
EOF

openssl req -new -x509 -newkey rsa:2048 -sha256 -days 3650 -nodes -keyout /usr/local/etc/nginx/ssl/ssl.key -out /usr/local/etc/nginx/ssl/ssl.crt -config /usr/local/etc/nginx/openssl.cnf

Add ssl.crt to Keychain Access > SYSTEM

```

## Others app
```
brew cask install google-chrome vlc postman docker phpstorm visual-studio-code
datagrip webstorm pycharm skype chatwork slack telegram-desktop gotiengviet mac2imgur
```
