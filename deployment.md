# Deployment with Docker

You can easily deploy and monitor your own instance online using [Docker](https://docker.com)

#### Install Docker

To install docker, please refer to the [official website](https://docs.docker.com/engine/installation/). For OSX users, you can just [install it from brew](https://stackoverflow.com/questions/40523307/brew-install-docker-does-not-include-docker-engine).

### Build the Docker image


You need to build the docker image from the `Dockerfile`. From within the github rep, execute the following command :

```sh
docker build -t docker-proofx .
```

### Start the image

For the server to start, you will need to pass your config to the Docker image by using a list of environment variables. You will find an example in `.sample-env`.  Please report to the [config](./config) doc for more info about environment variables.

```sh
cp .sample-env .env-docker
```

You can now start docker-node as daemon.

```sh
docker run -d -i --rm --name docker-proofx --env-file .env-docker -p 3003:3003 docker-proofx
```

You can check that the node is running using `docker ps`.

#### Stop the image using

```sh
docker stop docker-proofx
```

#### Attach to the docker-node

```sh
docker exec --user node -w /home/node -ti docker-node bash
```

## Systemd Service

The app can be run as a systemd service with a startup script such as:

```
[Unit]
Description=Proof of Existence prod
After=network.target

[Service]
WorkingDirectory=/home/ubuntu/proofofexistence/
Environment="NODE_ENV=production"
Environment="PORT=3003"
Type=simple
User=ubuntu
ExecStart=/usr/bin/node /home/ubuntu/proofofexistence/lib/server.js
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
