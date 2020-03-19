This repository is forked and was originally created by David Yeiser as part of his tutorial on how to [setup a local development environment to build a WordPress theme](https://davidyeiser.com/tutorial/docker-wordpress-theme-setup).

## Installation

If you don’t have Docker and Docker Compose installed follow the steps outlined in the blog post linked above.

With Docker installed and running, `cd` into the project directory and run:

```BASH
$ docker-compose up -d
```
> Create and start containers

To access the WordPress installation, visit: http://localhost:8000/

To access PHPMyAdmin, visit: http://localhost:8080/

## Rebuild

If you'd like to change the `docker-compose.yml` file after already running the image using the aforementioned command, take the images `down` and then build them `up` again. Here are useful commands related to stopping services and images:

```BASH
$ docker-compose stop
```
> Stop services (Docker Desktop shows the stack is available but not running)

```BASH
$ docker-compose down
```
> Stop and remove containers, networks, images, and volumes (Docker Desktop does not list the stack)

If you'd like to wipe everything clean to start over, use this command:
```BASH
$ docker system prune -a --volumes
```
> This removes all unused containers, volumes, networks, and images. Anything not currently in use will be wiped, that is:
> * all stopped containers
> * all networks not used by at least one container
> * all volumes not used by at least one container
> * all images without at least one container associated to them
> * all build cache
```
docker system prune [OPTIONS]

Remove unused data

Options:
  -a, --all             Remove all unused images not just dangling ones
      --filter filter   Provide filter values (e.g. 'label=<key>=<value>')
  -f, --force           Do not prompt for confirmation
      --volumes         Prune volumes
```

## Development

In the `docker-compose.yml` file, you'll see that there are mapped volumes, `wp-content` and `php/conf.d/uploads.ini`:

```YAML
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

### Useful Links

Here are some links you might find useful when determining what versions to test:
* [WordPress Releases](https://wordpress.org/download/releases/)
* [WordPress System Statistics](https://wordpress.org/about/stats/)
* [PHP Releases](https://www.php.net/releases/)
