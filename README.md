# Docker-LEMP

Basic Docker LEMP setup + composer + nodejs.
I'm using it to quickly spin up local dev environment for all my PHP projects, mostly Laravel based.

It consists of following services (more to come soon):
- nginx (1.9)
- php (8.0-fpm)
- mysql (8.0)
- composer, a dependency manager for PHP
- nodejs with npm, a package manager for Node.js packages

## Requirements
- git (only to be able to clone the repo)
- docker desktop

## Configuration
- in .env file you can update few options like shared folder location, mysql creds etc
- nginx configuration is in /nginx/conf.d folder
- nginx logs are in nginx/logs and they are persistent
- mysql data is in /mysql/data folder and they are persistent as well
- in php folder there is php.ini file. Edit if needed to fine tune your set up.

## Installation
- Create new directory and navigate to it:
```shell script0
  mkdir docker-lemp
  cd docker-lemp
```
- Clone this repo:
  `git clone git@github.com:pawco/docker-lemp.git .`
- configure .env file to fit your needs (optional)
- Install laravel using composer by running following command:
  `docker-compose run --rm composer composer create-project laravel/laravel .`
- Install packages using npm by running following command:
  `docker-compose run --rm nodejs npm install` (optional)
- Compile assets by running following commands:
  `docker-compose run --rm nodejs npm run dev`
- Start all services with:
  `docker-compose up -d`
- Verify installation:
  `docker exec -it php php artisan -V`
- Edit Laravel's .env file and update mysql creds, host and database
- execute all migrations:
  `docker exec -it php php artisan migrate`

and your LAMP stack is ready, just type `127.0.0.1` or `localhost`.


In order to bind custom domain to your localhost, edit your `/etc/hosts` file and add following (applies for Mac and Linux):
```shell script
127.0.0.1   domain.intra
```


## Fun Facts
- default document root is `./source` folder
- default MySQL root password is `toor`
- default MySQL database is `docker`
- default http port is 80
- if you change `composer.json` and you need to install/update packages, you have to run`composer update` manually:
  `docker-compose run --rm composer composer update`
- in order to create database dump from mysql container run following:
```shell script
docker exec mysql sh -c 'exec mysqldump --all-databases -uroot -p"$MYSQL_ROOT_PASSWORD"' > /some/path/on/your/host/all-databases.sql
```
- in order ro restore database dump into mysql container run following:
```shell script
docker exec -i mysql sh -c 'exec mysql -uroot -p"$MYSQL_ROOT_PASSWORD"' < /some/path/on/your/host/all-databases.sql
```

All default values mentioned above are configurable in .env file

## ToDo (in this order)
- vendor and node_modules are under root user. It should be under user with lower privileges
- Add support for redis
- better documentation with examples