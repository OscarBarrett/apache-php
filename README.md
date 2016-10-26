apache-php7
================

Base docker image to run PHP applications on Apache.

This is a modified version of https://github.com/tutumcloud/apache-php, using PHP 7 and Ubuntu 16.04 as a base.


Loading your custom PHP application
-----------------------------------

This image can be used as a base image for your PHP application. Create a new `Dockerfile` in your
PHP application folder with the following contents:

    FROM obarrett/apache-php7

After that, build the new `Dockerfile`:

    docker build -t username/my-php-app .

And test it:

    docker run -d -p 80:80 username/my-php-app


Using this image with docker-compose
-----------------------------------

Simply mount your application folder to `/app`, for example:

```yaml
version: '2'

services:
  web:
    image: obarrett/apache-php7

    volumes:
      - ./:/app

    ports:
      - 80:80
```

Building the base image
-----------------------

To create the base image `apache-php7`, execute the following command on the root folder:

    docker build -t apache-php7 .


Loading your custom PHP application with composer requirements
--------------------------------------------------------------

Create a Dockerfile like the following:

    FROM obarrett/apache-php7
    RUN apt-get update && apt-get install -yq git && rm -rf /var/lib/apt/lists/*
    RUN rm -fr /app
    ADD . /app
    RUN composer install

- Replacing `git` with any dependencies that your composer packages might need.
- Add your php application to `/app`
