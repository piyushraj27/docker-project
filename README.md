# Docker-Project
This project is to setup JOOMLA (open-source content management system for publishing web content) using *Docker* under the guidance and mentorship of **Mr. Vimal Daga** sir.

## Project Description
## (1) Start a Docker
* Use `#systemctl start docker`,to start DOCKER because it's a PRODUCT. ![MySQL server](screenshots_docker/start%20docker.png)

## (2) Download required docker images
* Pull image of MySQL
  * Use `#docker pull mysql:5.7` to download **mysql version 5.7** image to use as database server.
* Pull image of JOOMLA
  * Use `#docker pull joomla:3.9-php7.2-apache` to download **joomla image** with php and apache web server already configured.
   * to know more go to page: https://hub.docker.com/ 
## (3) Create a seperate volume for storage and start MySQL in one container
* Use `#docker volume create <name of volume1>` .
* Use `#docker run -dit -e MYSQL_ROOT_PASSWORD=<password of your root account>  -e MYSQL_USER=<user name>  -e MYSQL_PASSWORD=<password for user account>  -e MYSQL_DATABASE=<name of folder inside MySQL which will be your database>  -v <name of volume1>:/var/lib/mysql  --name <hostname of container having mysql>  mysql:5.7` .
![MySQL server](screenshots_docker/volume%20create%20run%20mysql.png)
* To see databases created you need to install MySQL client which will send request to MySQL Server . For installing, use `#yum install mysql`.
![install mysql client](screenshots_docker/ip%20%26%20mysql.png)
![mysql database](screenshots_docker/sql%20database.png)
## (4) Create a seperate volume and start webapp in different container
* Use `#docker volume create <name of volume2>` .
![create storage for joomla](screenshots_docker/volume%20create.png)
* Use `#docker run -dit  -e JOOMLA_DB_HOST=<hostname of container having mysql>  -e JOOMLA_DB_USER=<user name>  -e JOOMLA_DB_PASSWORD=<password for user account>  -e JOOMLA_DB_NAME=<name of folder inside MySQL which will be your database>  -v <name of volume2>:/var/www/html  --name <container name>  -p <port no. of router i.e. exposed port>:<port no. of services running inside your server>  --link <hostname of container having mysql>  joomla:3.9-php7.2-apache` .
![run joomla](screenshots_docker/run%20webapp.png)
## Type`<ip address>:<port no. that you have exposed>` to see in browser
![joomla on chrome browser](screenshots_docker/bowse%20pic.png)
![data added in MySQL database](screenshots_docker/data%20add.png)
## (5) Docker compose
*  Install the software before using Docker-Compose you should install the software. Link refrence- https://docs.docker.com/compose/install/
* You can create and edit this file using vim editor. For that use `#vim docker-compose.yml`. Remember the file name should always be **docker-compose.yml**.
![yml file](screenshots_docker/yml%20file1.png)
![yml file continued](screenshots_docker/yml%20file2.png)
## (6) Docker-compose
## UP/DOWN or START/STOP
* Copied entire infrastructure in one single file.Now, you will come to know about the role of Docker. Why it's so fast?
* Use `#docker-compose up` and `#docker-compose down` to start and stop services.
 ![docker compose up](screenshots_docker/docker%20compose.png)
 ![docker compose up output](dscreenshots_docker/output%20.png)
 ## 7. Finally , your WebApp can be seen:
 ![final result](screenshots_docker/12_final%20output.png)
 ## TROUBLESHOOT (some possible error solution)
 * There might be possible it is not able to connect that might be due to FIREWALL . Use `#systemctl stop firewalld`. Again you want to start the service use `#systemctl start firewalld`. but, it is not good practice to stop firewall. Instead do this,
   * Masquerading allows for docker ingress and egress 
    `#firewall-cmd --zone=public --add-masquerade --permanent`
   * Specifically allow incoming traffic on port 80/443 (nothing new here)
    `#firewall-cmd --zone=public --add-port=80/tcp`
    `#firewall-cmd --zone=public --add-port=443/tcp`
   * Reload firewall to apply permanent rules
    `#firewall-cmd --reload`
   * Restart docker 
    `#systemctl restart docker`
 * If JOOMLA WebApp is not opening on your desired port , maybe due to **iptables** . Use `#iptables -P FORWARD ACCEPT` 

