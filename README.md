# docker-nginx-letsencrypt
This project is made to demonstrate some functionality of Docker and Docker Compose.
Read the source. It's not a lot.

## Prereqs
- docker
- docker-compose > 1.8.1

## Usage
Inspect the `nginx.conf` configuration and edit the placeholder `server_name` and `ssl_*` settings.

Create a volume to hold letsencrypt data and verification files

    $ docker volume create --name letsencrypt

Start the main nginx container

    $ docker-compose up -d
    
Inspect the `certbot/Dockerfile` before building the `certbot` image.

    $ docker build --tag stigok/certbot certbot/.

Retrieve a test certificate from the staging server

    $ docker run --rm -v letsencrypt:/letsencrypt stigok/certbot --test-cert -d www.example.com -d example.com

Restart the web server

    $ docker-compose restart

Verify that the cert is working. It is expected that curl returns an invalid certificate because the
certificate was acquired with the `--test-cert` flag.

    $ curl -vIL example.com
    
Remove the test certificate

    $ docker run -v letsencrypt:/letsencrypt -it --rm --entrypoint certbot delete --config-dir /letsencrypt --test-cert --cert-name stigok/certbot

Fetch a real certificate

    $ docker run --rm -v letsencrypt:/letsencrypt stigok/certbot -d www.example.com -d example.com

Restart the web server again and verify

    $ docker-compose restart
    $ curl -vIL example.com

## TODO
- Find a way to ease the pain of having to restart the container on `nginx.conf` changes.
