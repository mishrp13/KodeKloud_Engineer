Below are clear, production-ready steps to meet all the requirements on app server 1 (stapp01).
These steps assume a RHEL/CentOS/Alma/Rocky-based system, which is what Nautilus infra typically uses.

1. Install Nginx on App Server 1
sudo yum install -y nginx

Configure Nginx to use port 8099 and document root /var/www/html

Edit the default server configuration:

sudo vi /etc/nginx/conf.d/default.conf


Replace the content with:

server {
    listen 8099;
    server_name stapp01;

    root /var/www/html;
    index index.php index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass unix:/var/run/php-fpm/default.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
}


Enable and start nginx:

sudo systemctl enable nginx
sudo systemctl start nginx

2. Install PHP-FPM 8.3

Enable Remi repository (required for PHP 8.3):

sudo yum install -y epel-release
sudo yum install -y https://rpms.remirepo.net/enterprise/remi-release-9.rpm


Enable PHP 8.3 module:

sudo yum module reset php -y
sudo yum module enable php:remi-8.3 -y


Install PHP-FPM:

sudo yum install -y php php-fpm

3. Configure PHP-FPM to Use Unix Socket

Create socket directory if it doesn’t exist:

sudo mkdir -p /var/run/php-fpm


Edit PHP-FPM pool config:

sudo vi /etc/php-fpm.d/www.conf


Update the following values:

listen = /var/run/php-fpm/default.sock
listen.owner = nginx
listen.group = nginx
listen.mode = 0660

user = nginx
group = nginx


Save and exit.

Enable and start PHP-FPM:

sudo systemctl enable php-fpm
sudo systemctl start php-fpm

4. Verify Permissions

Ensure correct ownership of the web directory:

sudo chown -R nginx:nginx /var/www/html

5. Restart Services
sudo systemctl restart php-fpm
sudo systemctl restart nginx

6. Test from Jump Host

From the jump host, run:

curl http://stapp01:8099/index.php


✅ If configured correctly, PHP output will be displayed.
You can also test:

curl http://stapp01:8099/info.php
