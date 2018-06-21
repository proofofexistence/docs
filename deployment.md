# Deployment with Docker

You can easily deploy and monitor your own instance online using [Docker](https://docker.com)

### Get the image

A [ready-to-use docker build](https://hub.docker.com/r/proofofexistence/proofofexistence/) is available online to download at [hub.docker.com](https://hub.docker.com/r/proofofexistence/proofofexistence/).

```sh
docker pull proofofexistence/proofofexistence
```

### Configure your wallet

For the server to start, you will need to pass your config to the Docker image by using a list of environment variables. You will find an example in `.sample-env`.  Please report to the [config](./config) doc for more info about environment variables.

```sh
cp .sample-env .env-docker
```

### Start/stop the image

You can now start your Docker image as daemon.

```sh
docker run -d -i --rm --name proofofexistence --env-file .env-docker -p 3003:3003 proofofexistence:VERSION
```

You can check that the node is running using `docker ps`.

#### Stop the image

```sh
docker stop proofofexistence
```

#### Attach to the docker-node

```sh
docker exec --user node -w /home/node -ti proofofexistence bash
```

## Deployment with [Amazon Elastic Container Service](https://aws.amazon.com/ecs/)

If you don't know how to use Amazon ECS, you can read the [example](https://medium.com/boltops/gentle-introduction-to-how-aws-ecs-works-with-example-tutorial-cea3d27ce63d) first.

This is the task defination:

```json
{
    "containerDefinitions": [
        {
            "name": "proofofexistence",
            "image": "poexio/proofofexistence",
            "essential": true,
            "portMappings": [
                {
                    "hostPort": "3003",
                    "containerPort": "3003",
                    "protocol": "tcp"
                }
            ],
            "environment": [
                {
                    "name": "DB_PATH",
                    "value": "/tmp/proofx-database-test"
                },
                {
                    "name": "PORT",
                    "value": "3003"
                }
            ],
            "mountPoints": [
                {
                    "sourceVolume": "proofx-db",
                    "containerPath": "/tmp/proofx-database-test",
                    "readOnly": ""
                }
            ],
            "volumesFrom": null,
            "hostname": null,
            "user": null,
            "workingDirectory": null,
            "extraHosts": null,
            "logConfiguration": null,
            "ulimits": null,
            "dockerLabels": null,
            "healthCheck": {
                "interval": "60",
                "timeout": "20",
                "startPeriod": "20",
                "retries": "2",
                "command": [
                    "cd /tmp;ls"
                ]
            }
        }
    ],
    "volumes": [
        {
            "host": {
                "sourcePath": "/proofx/proofx-database-test"
            },
            "name": "proofx-db"
        }
    ],
    "networkMode": "bridge",
    "memory": "1024",
    "cpu": "1 vcpu",
    "placementConstraints": [],
    "family": "proofx-ecs",
    "taskRoleArn": "arn:aws:iam::611798417555:role/ecsTaskExecutionRole"
}
```

WARN: (MUST DO IT IF YOU CARE ABOUT YOUR DATABASE)

- /proofx/proofx-database-test is the default volume in the instance, you can mount a large one if you need. and you must make it exists in the instance before you run the task.

## Build the Docker image yourself

You can build the docker image from the `Dockerfile` from within the github rep by using the following command:

```sh
docker build -t proofofexistence .
```

## Install Docker

To install docker, please refer to the [official website](https://docs.docker.com/engine/installation/). For OSX users, you can just [install it from brew](https://stackoverflow.com/questions/40523307/brew-install-docker-does-not-include-docker-engine).



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
