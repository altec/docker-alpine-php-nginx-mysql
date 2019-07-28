Basic Docker environment for developing Symfony 4 applications.

PHP docker image: php:7.3-fpm-alpine
NGINX docker image: nginx:1.16-alpine
MySQL docker image: mysql:8.0

I did not want to provide a DB tool like phpMyAdmin. 
I recommend setting up a standalone phpMyAdmin. 
If you prefer having everything in one place, 
just add this to docker-compose.yml

```
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: prototype_phpmyadmin
    environment:
      PMA_HOST: mysql
      PMA_PORT: 3306
    ports:
      - "${PHPMYADMIN_PORT}:80"
    links:
      - mysql
```

This setup is tested with Symfony version 4.3.0

Composer is installed so there is no need to install it on your host machine. Just drop into PHP container and run composer commands.

How to install Symfony:

- one way is to do it on your host computer. 
You will need to install Symfony in the ```app``` folder. 
Keep in mind that the command ```composer create-project symfony/website-skeleton my-project``` will create a subfolder which we don’t want to do.
Instead just ```cd``` into the ```app``` folder and execute this command ```composer create-project symfony/website-skeleton .```
- second option is to use composer inside the container. 
For this you need to open you're terminal and execute this command 
```docker exec -i -t prototype_php /bin/sh```. 
Out of the box Apine distros don’t have bash so that is why 
you need to use ```/bin/sh``` instead of ```/bin/bash```. When you are in youre container ```cd``` into the ```app``` folder and execute ```composer create-project symfony/website-skeleton .```

After you have Symfony installed, go to ```localhost:8001``` and you should see the Symfony welcome screen.

Setting up DB connection:

- rename `/.env.dist` to `/.env` and put in your data (generate new password!)
- in the `/app` folder create a `/app/.env.local` file and copy the content from `/app/.env` to your new `/app/.env.local` file
- edit the `DATABASE_URL` line. Replace `db_user` with your username that you specified in `/.env` file (the default username is `prototype`). Then replace `db_password` with your password from `/.env`. Then replace `db_name` with your database name from `/.env`.
- the final step is to get the IP address (in the future I will make it static but for now read on). Start up your containers with `docker-compose up` (the first time it will be slow), after list out your conteners with `docker ps`, find the `prototype_mysql` container and use its ID with this command `docker inspect <container ID>`. At the bottom, under "NetworkSettings", you can find "IPAddress" or just execute `docker inspect <container id> | grep "IPAddress"` (if you are using Windows `grep` is not a valid command, I use GitBash on windows where this command executes). When you get the IP address replace `127.0.0.1` with what you get. I WILL MAKE THIS EASYER IN THE FUTURE!
