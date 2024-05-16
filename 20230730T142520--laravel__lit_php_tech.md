---
title:      "laravel"
date:       2023-07-30T14:25:20-04:00
tags:       ["lit", "php", "tech"]
identifier: "20230730T142520"
---

create project
``` shell
composer create-project laravel\laravel <name-of-project>
```

run server
``` shell
php artisan serve
```
- cd into laravel project

make a model
``` shell
php artisan make:model <name-of-modle> -m
```
- -m stands for migration

run migration
``` shell
php artisan migrate
```
- you can also use this to check if database is conneted properly


make controller
``` shell
php artisan make:controller <name-of-controller>
```

show routes

``` shell
php artisan route:list
```

roleback migrations
``` shell
php artisan migrate:reset
```

refresh migrations

``` shell
php artisan migrate:refresh
```

make a resource controller

``` shell
php artisan make:controller <Controller> --resource --model <model>
```
- create all resource methods

make a API controller

``` shell
php artisan make:controller <controller> --api --model <model>
```

make a migration

``` shell
php artisan make:migration <migration-name>
```

roleback

``` shell
php artisan migrate:rollback
```

run database seed

``` shell
php artisan db:seed
```

make a middleware

``` shell
php artisan make:middleware <name-of-middleware>
```

