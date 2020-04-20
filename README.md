# nginx-php7.2
Install and configure nginx and PHP-FastCGI on Ubuntu 16.04
Install nginx, PHP for Processing, and Required PackagesPermalink

Install the nginx web server and PHP dependencies:

sudo apt-get install nginx php7.0-cli php7.0-cgi php7.0-fpm
Configure nginx Virtual Hosting and the PHP ProcessorPermalink
tail /etc/nginx/sites-available/default -n 13 | cut -c 2- | sudo tee /etc/nginx/sites-available/example.com 1> /dev/null
/etc/nginx/sites-available/example.com

     1
     2
     3
     4
     5
     6
     7
     8
     9
    10
    11
    12
    13
    14
    15
    16
    17
    18
    19

    	

    server {
        listen 80;
        listen [::]:80;

        server_name example.com;

        root   /var/www/html/example.com/public_html;
        index  index.html index.php;

        location / {
            try_files $uri $uri/ =404;
        }
        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                include fastcgi_params;
                fastcgi_pass unix:/run/php/php7.0-fpm.sock;
                fastcgi_param SCRIPT_FILENAME /var/www/html/example.com/public_html$fastcgi_script_name;
        }
    }
    Create the root directory referenced in this configuration, replacing example.com with your domain name:

sudo mkdir -p /var/www/html/example.com/public_html

Enable the site, disable the default host, and restart the web server:

sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled
sudo rm /etc/nginx/sites-enabled/default
sudo systemctl restart php7.0-fpm nginx

To deactivate a site, simply delete the symbolic link:

sudo rm /etc/nginx/sites-enabled/example.com
sudo systemctl restart nginx
