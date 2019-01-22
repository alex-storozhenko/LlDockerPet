# Laravel - Lumen Docker Pet Project

Contains lightweight infrastructure for quick launch of Laravel - Lumen and other php projects using Docker containers.

### Requirements:
 
 - [docker](http://www.docker.com) (17.06+)
 - [docker-compose](https://docs.docker.com/compose/install/) (1.18+)
 
### Quick start guide for Lumen:

Make src directory for the store project source code:

```sh
mkdir src
```

Copy ".env-example" and set name ".env" for new file change ENV.

Will be good set "container_name" in docker-compose.yml.

run:

```sh
docker-compose up {?-d} --build
```

-d arg:
```sh
-d Detached mode: Run containers in the background,
                  print new container names. Incompatible with
```

after that "fail" into "workspace" container:

 ```sh
 docker-compose exec workspace bash 
 ```
 
and go to standard installation flow for lumen:
 
 ```sh
 cd src/
 ```
 
```sh
composer create-project --prefer-dist laravel/lumen .
```

### Quick start guide for Laravel:

The same with Lumen guide except last step of installation:
 
 ```sh
 cd src/
 ```
 
```sh
composer create-project --prefer-dist laravel/laravel .
```
