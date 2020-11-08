# SeanMVC

This is a PHP framework that uses MVC design pattern.

Before starting, please setup the framework in `app/config/config.php`.

## Hosting this framework

### Apache

If you are using Apache as webserver, please configure RewriteBase in `public/.htaccess` into your directory.

### Nginx

If you are using Nginx as webserber, the following is the recommended configurations. Remember to change the site name in the example configuration.

```
server {
    listen 80;
    listen 443 ssl http2;
    server_name .seanmvc.test;
    root "/home/vagrant/code/seanmvc/public";

    index index.html index.htm index.php;

    charset utf-8;

    

    location / {
        try_files $uri $uri/ /index.php?$query_string;
        
    }

    

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    access_log off;
    error_log  /var/log/nginx/seanmvc.test-error.log error;

    sendfile off;

    client_max_body_size 100m;

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        

        fastcgi_intercept_errors off;
        fastcgi_buffer_size 16k;
        fastcgi_buffers 4 16k;
        fastcgi_connect_timeout 300;
        fastcgi_send_timeout 300;
        fastcgi_read_timeout 300;

        if ($request_uri ~* "(?<=^.)(.*)"  ) {
            set  $last_path_component  $1;
            rewrite (?<=^.)(.*)  /index.php?url=$last_path_component  break;
        }

        try_files $uri $uri/ =404;
    }

    location ~ ^/(css|js|img) {
        try_files $uri $uri/ =404;
    }

    location ~ /\.ht {
        deny all;
    }

    ssl_certificate     /etc/nginx/ssl/seanmvc.test.crt;
    ssl_certificate_key /etc/nginx/ssl/seanmvc.test.key;
}
```