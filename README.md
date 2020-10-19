Network Automation


In this assignment, we are to implement a simple web service. The content of that web service is given below

<?php echo date("Y-m-d H:i:s") . " Welcome to " . gethostname() . "|" .$_SERVER['SERVER_ADDR'] .":" . $_SERVER['SERVER_PORT'] ."<br>\n"; ?>


This code we will store  in the main index.php file of a webserver and the server should serve content from the "/var/www/html" folder. The webserver we used is  Nginx, and we will deployed on Ubuntu 20.04LTS. For the service to work properly, we installed the following packages; nginx, php, and php-fpm. 

Example of a generic Nginx configuration, that could act as a template is given bellow 

server {
        listen 80 default_server;
        listen [::]:80 default_server;

        root /var/www/html;
        index index.php index.html index.htm index.nginx-debian.html;

        server_name _;
        location / {
                try_files \$uri \$uri/ =404;
        }
        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/run/php/php7.4-fpm.sock;
        }
}

The basic service is an Nginx web server is to serves one simple PHP page and that  page will renders the hostname, IP, and port of the server running it. 

Here we are expecting this service running on many servers so that we want Nginx server running on three different platform. For accessing the server we add a server called HAproxy which will also act as load balancer.
The site consists of 5 hosts, they are HAproxy,devA,devB,devC and Bastion with internal site local network. HAproxy is the host that runs HAproxy and acts as an entry point for accessing the service. devA,devB and devC are the three web servers. These are only to use private addresses from the site local network. The last host, Bastion, is acting as an SSH entry point to the internal network. I.e. if we connect to Bastion, we will be able  ssh to all of the other hosts within the site local network. 

Hence the ansible-playbook, site.yaml will be deployed to the HAproxy, and Nginx on appropriate existing hosts. Once the hosts have been configured, the The playbook is assumed to run outside the site through the Bastion host.























