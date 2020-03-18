This repository is forked and was originally created by David Yeiser as part of his tutorial on how to [setup a local development environment to build a WordPress theme](https://davidyeiser.com/tutorial/docker-wordpress-theme-setup).

## Installation

If you don’t have Docker and Docker Compose installed follow the steps outlined in the blog post linked above.

With Docker installed and running, `cd` into the project directory and run:

```BASH
$ docker-compose up -d
```

Then in your browser:
```
http://localhost:8000/
```

## Rebuild

If you'd like to change the `docker-compose.yml` file after already running the image using the aforementioned command, you're gonna have to start all over by using the following commands:

```BASH
$ docker-compose stop
```
_This stops the current working directory's Docker image from running._

```BASH
$ docker system prune -a --volumes
```
_This removes all unused containers, volumes, networks, and images. Anything not currently in use will be wiped!!!_
* _all stopped containers_
* _all networks not used by at least one container_
* _all volumes not used by at least one container_
* _all images without at least one container associated to them_
* _all build cache_

You can then safely make changes to the `docker-compose.yml` file to then build up again as a fresh installation.

_There's probably a less destructive way to do this, but I'm not yet aware..._

## Development

In the `docker-compose.yml` file, you'll see that there are mapped volumes, `wp-content` and `php/conf.d/uploads.ini`:

```
  wordpress:
    ...
    volumes:
      - ./wp-content:/var/www/html/wp-content
      - ./uploads.ini:/usr/local/etc/php/conf.d/uploads.ini
```

You can write a simple `rsync` command to update your Docker image's files, i.e.:
```BASH
$ rsync -r --exclude=".*" --exclude="composer.*" /path/to/"$MYPLUGIN"/ /path/to/docker-wordpress-setup/wp-content/plugins/"$MYPLUGIN"/
```

## Choosing WordPress & PHP Versions

To change the WordPress or PHP Version used to build the Docker image, simply edit the WordPress image value in `docker-compose.yml`:

```
image: wordpress:5.1.1-php7.3-apache
```
As you can see, this will build an image with WordPress 5.1.1 and PHP 7.3.

Here's another example that I have built with:
```
image: wordpress:4.9.1-php5.6-apache
```

To find WordPress versions, go here: https://wordpress.org/download/releases/

For PHP versions, go here: https://www.php.net/releases/
