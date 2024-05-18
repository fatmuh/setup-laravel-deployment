# Laravel Deployment Ubuntu by Fatmuh

## Install Nginx:

- sudo apt update
- sudo apt install nginx -y
- systemctl status nginx

## Install PHP:

- sudo apt install software-properties-common -y
- sudo add-apt-repository ppa:ondrej/php
- sudo apt install php8.1-fpm php8.1-common php8.1-dom php8.1-intl php8.1-mysql php8.1-xml php8.1-xmlrpc php8.1-curl php8.1-gd php8.1-imagick php8.1-cli php8.1-dev php8.1-imap php8.1-mbstring php8.1-soap php8.1-zip php8.1-bcmath -y
- systemctl status php8.1-fpm

## Install Composer:

- sudo apt install composer

#### Notes : Setup Domain A ke IP Server

## Install Laravel

- cd /var/www/
- mkdir nama_domain
- cd nama_domain
- git clone link_project_repository.git
- sudo vim /etc/nginx/sites-available/nama_domain

#### Setup Nginx
```
server {
    listen 80;
    listen [::]:80;
    server_name example.com;
    root /var/www/example.com/git_folder_name/public;
 
    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";
 
    index index.php;
 
    charset utf-8;
 
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
 
    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }
 
    error_page 404 /index.php;
 
    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }
 
    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

- ln -s /etc/nginx/sites-available/nama_domain /etc/nginx/sites-enabled/
- nginx -t
- systemctl restart nginx
- cd /var/www/nama_domain/clone_folder_git/
- composer install --optimize-autoloader --no-dev
- php artisan cache:clear
- php artisan view:clear
- php artisan route:clear
- php artisan config:cache
- php artisan config:clear
- cp .env.example .env
- sudo vim .env (Setting ENVnya)
- php artisan key:generate
- cd storage/
- sudo chmod -R 777 logs
- cd framework/
- sudo chmod -R 777 sessions/
- sudo chmod -R 777 views/

## Install SSL (Certbot)

- sudo snap install core; sudo snap refresh core
- sudo snap install --classic certbot
- sudo ln -s /snap/bin/certbot /usr/bin/certbot
- sudo certbot --nginx

## Install Firewall UFW

- sudo ufw status verbose
- sudo ufw enable
- sudo ufw status verbose
- sudo ufw app list
- sudo ufw allow 'Nginx Full'
- sudo ufw allow 'Nginx HTTP'
- sudo ufw allow 'Nginx HTTPS'
- sudo ufw allow 'OpenSSH'
- sudo ufw allow ftp
- sudo ufw allow mysql
