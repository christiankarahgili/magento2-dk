# Docker Environment for Developer Magento U Courses and Trainings

This guide will help you set up a local Magento environment for the developer courses and trainings, if you don't have one already.

The goal is to have a local environment with:
- A web server (Apache) with php, starts from 7.1
- A MySQL-like (MariaDB) database server
- Magento Open Source code
- Magento Open Source sample data

## Introduction

We are providing custom docker images so once loaded, you will be ready to start with the developer courses and trainings.

At the end of this guide you will find the instructions for installing the environment with the custom docker images. You don't need to follow those instructions, they are provided for your reference.

## Requirements

- MacOS, Linux or Windows
- [Docker Desktop installed](https://www.docker.com/products/docker-desktop)
- [GIT](https://git-scm.com/downloads)
- [Visual Studio Code](https://code.visualstudio.com/download)

## Step by step guide

- Before start the steps the following services should be switched off: apache2/nginx, mysql.

- Once you have Docker Desktop running, copy docker environment to your working directory. For example this is could be ```YOUR\WORKING\DIRECORY\magento\magento-oms-dk```.

- [Optional - Please read cerefully!] This step is for MAC OS only and really optional, you may skip it. What it about - Setup native NFS for Docker for MAC OS. WARNING: This script will shut down running containers. It will stop your docker and all unused local volumes will be removed. DO NOT use in case if you are do not sure! Use only on your own risk.
```yaml
     sh nfs_setup.sh
```

- Update docker-compose.yml file for regular setup or docker-compose_mac.yml only in case you using MAC plus NFS setup script to set the proper local folders where you want to store the local data.

```yaml
     volumes:
       - /path/to/your/local/www:/var/www/html/shared
```
or for mac plus nfs
```yaml
     device: ":/path/to/local/magento/magento2/oms/instance"
```

Example:

Unix:
```yaml
     volumes:
       - /PATH/TO/YOUR/magento/magento2-oms-dk/instances:/var/www/html/shared
```

Windows:
```yaml
     volumes:
       - C:\PATH\TO\YOUR\magento\magento2-oms-dk\instances:/var/www/html/shared
```
- Your directory for develop purpose:
/PATH/TO/YOUR/magento/magento2-oms-dk/instances

- Once you have Docker Desktop running, [Clone the following from GIT](https://github.com/mike61988/magento2-dk/) to the your working directory. For example this is could be ```YOUR\WORKING\DIRECORY\magento\magento-dk```.
- Open a terminal in that folder and run the following command

```
docker-compose up -d
```
or that command only in case you using MAC plus NFS setup script
```
docker-compose -f docker-compose_mac.yml up -d
```

- After docker images successfully built run

Unix:
```
docker exec -ti <CONTAINER_ID> /bin/bash
```

Windows:
```
docker exec -it <CONTAINER_ID> //bin/sh
```

Example:

Unix:
```
docker exec -ti m23 /bin/bash
```

Windows:
```
docker exec -it m23 //bin/sh
```

### Final step
- Inside the container run

```
mv /var/www/html/magento2ce /var/www/html/shared/
```
After that command Magento instance will be apear in `instances` directory

## Other important information
- In case if you need restart the VM. Rebooting your computer will require restarting the VM.

```
docker-compose stop
```

```
docker-compose start
```

- To run Magento CLI command in a running container
```
sh m23_config/bin/console execute ANY_MAGENTO_CLI_COMMAND
```

Example:
```
sh m23_config/bin/console execute cache:clean
```

- Change your local hosts file to resolve the local magento2u.loc domain
Unix:
```bash
sudo nano /etc/hosts
127.0.0.1       magento2u.loc
```

Windows:
```
Windows host file location is C:\Windows\System32\drivers\etc\hosts
Open file as administrator (or use visual studio code editor), add the following line: 
127.0.0.1       magento2u.loc
then save the file
```

- Now you're able to test it by opening a browser and entering `http://magento2u.loc:82` or `http://magento2u.loc:83`, it depends of installed version.
You also, will be able to login to the admin `http://magento2u.loc:82/admin` or `http://magento2u.loc:83/admin` using the following credentials:

```
username: admin
pass: mageU123
```

- Thats it, you have your VM for learning purposes for Magento 2.2 and Magento 2.3.

Good luck on your training! :)
