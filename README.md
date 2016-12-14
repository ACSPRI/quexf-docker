# What is queXF?

[queXF](https://quexf.acspri.org.au/) is a free and open source system web based paper form verification and data entry system

# How to use this image

```console
$ docker run --name some-quexf --link some-mysql:mysql -d -v /location-of-forms:/forms acspri/quexf
```

Where location-of-forms is a local path where PDF files will be monitored for importing

The following environment variables are also honored for configuring your queXF instance:

-	`-e QUEXF_DB_HOST=...` (defaults to the IP and port of the linked `mysql` container)
-	`-e QUEXF_DB_USER=...` (defaults to "root")
-	`-e QUEXF_DB_PASSWORD=...` (defaults to the value of the `MYSQL_ROOT_PASSWORD` environment variable from the linked `mysql` container)
-	`-e QUEXF_DB_NAME=...` (defaults to "quexf")
-	`-e QUEXF_ADMIN_PASSWORD=...` (defaults to "password")

If the `QUEXF_DB_NAME` specified does not already exist on the given MySQL server, it will be created automatically upon startup of the `quexf` container, provided that the `QUEXF_DB_USER` specified has the necessary permissions to create it.

If you'd like to be able to access the instance from the host without the container's IP, standard port mappings can be used:

```console
$ docker run --name some-quexf --link some-mysql:mysql -p 8080:80 \
     -v /location-of-forms:/forms -d acspri/quexf
```

Then, access it via `http://localhost:8080` or `http://host-ip:8080` in a browser.

If you'd like to use an external database instead of a linked `mysql` container, specify the hostname and port with `QUEXF_DB_HOST` along with the password in `QUEXF_DB_PASSWORD` and the username in `QUEXF_DB_USER` (if it is something other than `root`):

```console
$ docker run --name some-quexf -e QUEXF_DB_HOST=10.1.2.3:3306 \
	-v /location-of-forms:/forms \
    -e QUEXF_DB_USER=... -e QUEXF_DB_PASSWORD=... -d acspri/quexf
```

## ... via [`docker-compose`](https://github.com/docker/compose)

Example `docker-compose.yml` for `quexf`:

```yaml
version: '2'

services:

  quexf:
    image: quexf
    ports:
      - 8080:80
	volumes:
	  - /location-of-forms:/forms
    environment:
      QUEXF_DB_PASSWORD: example
      QUEXF_ADMIN_PASSWORD: password

  mysql:
    image: mariadb
    environment:
      MYSQL_ROOT_PASSWORD: example
```

Run `docker-compose up`, wait for it to initialize completely, and visit `http://localhost:8080` or `http://host-ip:8080`.

# Supported Docker versions

This image is officially supported on Docker version 1.12.3.

Support for older versions (down to 1.6) is provided on a best-effort basis.

Please see [the Docker installation documentation](https://docs.docker.com/installation/) for details on how to upgrade your Docker daemon.

Notes
-----

A default username is created:

    admin

The password is specified by the QUEXF_ADMIN_PASSWORD environment variable (defaults to: password)

This Dockerfile is based on the [Wordpress official docker image](https://github.com/docker-library/wordpress/tree/8ab70dd61a996d58c0addf4867a768efe649bf65/php5.6/apache)
