# Dockerizing Laravel applications

This repository builds a dockerized architecture for Laravel applications. It helps developers to quickly set up a development environment without doing any changes on their local machines.

## Requirements
* Linux machine (tested) or MAC OS (tested)
* docker (> 20.10.5)
* docker-compose (> 1.25.0)


## Architecture

The following services are included:
* Web server. An Nginx web server (version: 1.18.0)
* PHP with FPM (PHP version: 7.4.16)
* Database. MySQL database (version: 8.0)
* Redis. Used for storing the PHP sessions (version: 6.2.1)
* MongoDB. (version: 4.4.5)
* NodeJS.  (version:14.16.1)

## How to set up a development environment

* For customizing the domain, add your application's host name under `/etc/hosts`, e.g. `127.0.0.1	myapp.local`
* Clone the repository `https://github.com/xamplifier/laravel-docker.git` and `cd laravel-docker`
* Create the enviromental file that will be used by `docker-compose`:
`cp .env.example .env` and update the properties accordingly
* Update `server_name` in `docker-images/nginx/config/site.conf` if the application's host name is differnet than `myapp.local`
* Add the application's source code under `data/app/<APP_FOLDER>` if the application has already been created: `cd data/app` and then clone the application's repository (TBD).
* Update `data/app/<APP_FOLDER>/.env` (Laravel's enviromental file) based on the properties set on `.env` file above regarding the Mysql/MongoDB settings. Also update `APP_URL` with the application's host name
* Run `docker-compose up` or `docker-compose up --build` to recreate the images. Run `docker-compose down` to shut down all the services
* Open a new terminal 
* Create the Laravel key: `docker-compose exec app php artisan key:generate`
* Cache settings: `docker-compose exec app php artisan config:cache`
* Run composer: `docker-compose exec app composer install`
* Run npm: ` docker-compose exec node npm install `
* Create project's assets: ` docker-compose exec node npm run dev `
* Link storage directory: `docker-compose exec app php artisan storage:link`
* Check if you are properly connected to the database: `docker-compose exec app php artisan tinker` and then `\DB::table('migrations')->get();`
* Visit `http://myapp.local/` or `http://myapp.local:<FORWARD_WEBSERVER_PORT>/` (depending on the `FORWARD_WEBSERVER_PORT` property) to access your application 

## In the Laravel Project
* Run the first 2 steps above
* `cd data/app/{APP_FOLDER}`
* Run `sudo chown -R $USER:$USER <APP_FOLDER>`


## Notes
* Make sure that the ports exposed by containers are not used
* During the first run the `root` and the user specified in `DB_USERNAME` will be created. Grant privileges are set for that user for the database specified in `DB_DATABASE`

## Xdebug
TBD

## Useful commands
* `docker-compose exec <container_name> bash` or `docker exec -it <container_name> bash` to ssh to `<container_name>` container.
* `docker-compose exec app composer <command>` to run a composer command
* `docker-compose exec app php artisan` to run an artisan command
* `docker restart <container_name>` to restart a container
* `docker ps` to see all the running containers
* `docker images` to see all the images

## Accessing the database
* From the host machine if a mysql client is installed: `mysql -u root -p -h 127.0.0.1` or set up a Mysql Workbench connection
* From the container: `docker-compose exec db bash` and then `mysql -u root -p`

 


