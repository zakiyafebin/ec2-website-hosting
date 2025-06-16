#Hosting a Static Website with AWS EC2 Using Nginx
### Step 1 : Configure ec2 with a key pair pem and a security group.
>- Step 1.1 : Configure Security group
 ![image](https://github.com/user-attachments/assets/180c7c26-c9d5-4ac8-b1de-962e6a676981)
### Step 2 : Connect to the server using ssh
>-- Copy downloaded pem file to a folder . go to that folder. the
> - chmod 400 ec2-key-pair.pem
> - Run above command to change the autherization of pem file.
>  -Now copy pem file to ec2 instace home with the following command :
> - ssh-i <path_to_key_pair_file> ec2-user@<public_ip_from_dashbard>   => run this from the bash
> - Now our pem file will be in ec2 home
### step 3
>-  Elevating privileges and updating all the packages on the instance.
>-- sudo su

>--yum update -y
### Step 4 : Creating a static Nginx website
> - systemctl status nginx
> - systemctl start nginx
> - netstat -ntlp => Gives the current network connections and port activity
### Step 5: Update our Nginx web page to a static HTML
>- Using git clone cloned my static web app here it is my_app to ec2 home
>- From there
>- cp -r /home/ec2-user/my_app /usr/share/nginx/html
### Step 6: Configure nginex.
> - Need to tell Nginx where to find website files. This involves modifying the Nginx configuration file.
Locate the Nginx Configuration File:
The main configuration file is often /etc/nginx/nginx.conf
```
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log notice;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    keepalive_timeout   65;
    types_hash_max_size 4096;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80;
        listen       [::]:80;
        server_name  16.176.160.205;
        root         /usr/share/nginx/html/my_app;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
    }

# Settings for a TLS enabled server.
#
#    server {
#        listen       443 ssl;
#        listen       [::]:443 ssl;
#        http2        on;
#        server_name  _;
#        root         /usr/share/nginx/html/my_app;
#
#        ssl_certificate "/etc/pki/nginx/server.crt";
#        ssl_certificate_key "/etc/pki/nginx/private/server.key";
#        ssl_session_cache shared:SSL:1m;
#        ssl_session_timeout  10m;
#        ssl_ciphers PROFILE=SYSTEM;
#        ssl_prefer_server_ciphers on;
#
#        # Load configuration files for the default server block.
#        include /etc/nginx/default.d/*.conf;
#
#        error_page 404 /404.html;
#        location = /404.html {
#        }
#
#        error_page 500 502 503 504 /50x.html;
#        location = /50x.html {
#        }
#    }
}
```
### Step 7: Test nginex configuration
>- sudo nginx -t
### Step 8: Reload Nginx:
> - sudo systemctl reload nginx
### Step 10: open http://your-ec2-public-ip:80 on your browser.

