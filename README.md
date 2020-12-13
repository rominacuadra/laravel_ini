# laravel_ini
# Larvel + Apache + Mysql + Docker

## Webserver image for Laravel
`Laravel Framework 7.9.2`
`php:7.3-apache` 

## Database image 
`mysql:8.0`

## Up and running

1. Create a laravel project:
```
    $ curl -s https://laravel.build/example-app | bash
    
    This option create a laravel project with the last version,
    also run composer.
```    

2. Create a Dockerfile and replace docker-compose.yml:
```    
    $ cp Dockerfile /proyect/example-app
```    

3. Complete variables on .env:
```
    $ vim .env 
```    
    
4. Build the images and start the services:
```
    $ docker-compose build
    $ docker-compose up -d
```

5. Check created containers:
```
    $ docker ps
```

6. Run composer, configure artisan on app container and give storage folder permissions:
```
    $ docker exec -it laravel-app bash
    $ composer install
    $ php artisan key:generate
    $ sudo chmod 777 -R /var/www/html/storage/
```

7. Add new host in hostfile: /etc/hosts
```
    127.0.0.1       example-app.local
```

8. Check URL:
` http://example-app.local:8000`

9. Check Mysql Connection:
``` 
  a. Make sure .env DB_HOST is seted with db container name on .env file `
  DB_HOST=mysql-db 
  DB_PORT=3306 
  
  b. Complete variable on /config/database.php
  
  'mysql' => [
            'driver' => 'mysql',
            'url' => env('DATABASE_URL'),
            'host' => env('DB_HOST','default'),
            'port' => env('DB_PORT','default'),
            'database' => env('DB_DATABASE','default'),
            'username' => env('DB_USERNAME','default'),
            'password' => env('DB_PASSWORD','default'),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8mb4',
            'collation' => 'utf8mb4_unicode_ci',
            'prefix' => '',
            'prefix_indexes' => true,
            'strict' => true,
            'engine' => null,
            'options' => extension_loaded('pdo_mysql') ? array_filter([
                PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
            ]) : [],
        ],
  
  
  c. Run:
    $ docker exec -it laravel-app bash
    root@5073d6744f63:/var/www/html# php artisan migrate
