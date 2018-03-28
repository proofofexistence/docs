# Deployment

Deploy and monitor your own instance online

#### Install Docker

- Install Docker CE: https://docs.docker.com/engine/installation/
- Build docker-node (`Only build it once`)
- Alternative is to install from brew: https://stackoverflow.com/questions/40523307/brew-install-docker-does-not-include-docker-engine

Download [Dockerfile](./Dockerfile), then build it in the same path.

```sh
wget https://raw.githubusercontent.com/proofofexistence/proofofexistence/master/Dockerfile
```

```sh
docker build -t docker-node .
```

- Start docker-node as daemon
```sh
docker run -d -i --rm --name docker-node -p 3003:3003 docker-node
```
- Attach to the docker-node
```
docker exec --user node -w /home/node -ti docker-node bash
```

### Service

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


### News

The `/news` route should be handled by [proofofexistence/news]. This can be
configured in Nginx with a directive like:

```
location /news {
        alias /home/ubuntu/news/_site;
        error_page 404 /news/404.html;
}
```
