Basic Docker environment for developing Symfony 4 applications.

PHP docker image: php:7.3-fpm-alpine
NGINX docker image: nginx:1.15-alpine
MySQL docker image: mysql:8.0

I did not want to provide a DB tool like phpMyAdmin. 
I recommend setting up a standalone phpMyAdmin. 
If you prefer having everything in one place, 
just add this to docker-compose.yml

```
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: supermare_phpmyadmin
    environment:
      PMA_HOST: mysql
      PMA_PORT: 3306
    ports:
      - "${PHPMYADMIN_PORT}:80"
    links:
      - mysql
```

This setup is tested with Symfony version 4.2.3

Composer is installed so there is no need to install it on your host machine. Just drop into PHP container and run composer commands.

How to install Symfony:

- one way is to do it on your host computer. 
Yuo will need to install Symfony in the ```app``` folder. 
Keep in mind that the command ```composer create-project symfony/website-skeleton my-project``` will create a subfolder which we dont want to do.
Instead just ```cd``` into the ```app``` folder and execute this command ```composer create-project symfony/website-skeleton .```
- second option is to use composer inside the container. 
For this you need to open you're terminal and execute this command 
```docker exec -i -t supermare_php /bin/sh```. 
Out of the box Apine distros dont have bash so that is why 
you need to use ```/bin/sh``` instead of ```/bin/bash```. When you are in youre container ```cd``` into the ```app``` folder and execute ```composer create-project symfony/website-skeleton .```

After you have Symfony installed, go to ```localhost``` and you should see the Symfony welcome screen.
I used standard port for nginx to if you have something running on port ```80```, you might have conflicts.