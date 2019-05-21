pub-relay
=========

...is a service-type ActivityPub actor that will re-broadcast anything sent to it to anyone who subscribes to it.

forked by [mastodon / pub-relay · GitLab](https://source.joinmastodon.org/mastodon/pub-relay)

![](https://i.imgur.com/5q8db54.jpg)

Endpoints:

- `GET /actor`
- `POST /inbox`
- `GET /.well-known/webfinger`

Operations:

- Send a Follow activity to the inbox to subscribe
  (Object: `https://www.w3.org/ns/activitystreams#Public`)
- Send an Undo of Follow activity to the inbox to unsubscribe
  (Object of object: `https://www.w3.org/ns/activitystreams#Public`)
- Send anything else to the inbox to broadcast it
  (Supported types: `Create`, `Update`, `Delete`, `Announce`, `Undo`)

Requirements:

- All requests must be HTTP-signed with a valid actor
- Only payloads that contain a linked-data signature will be re-broadcast
- Only payloads addressed to `https://www.w3.org/ns/activitystreams#Public` will be re-broadcast

## Installation

※ Require [Crystal](https://crystal-lang.org/) (≦0.27.2), [Redis](https://redis.io/)
1. user add
   ```  
   useradd pub-relay  
   sudo su - pub-relay  
   git clone https://github.com/h3zjp/pub-relay.git  
   cd pub-relay
   ```
1. build
   ```
   shards update
   shards build --release
   ```
1. generate key
   ```
   openssl genrsa 2024 > ~/.ssh/actor.pem
   chmod 600 ~/.ssh/actor.pem
   ```
1. systemctl add  
   ```vi /etc/systemd/system/pub-relay.service```

   ```
   [Unit]
   Description=pub-relay Server
   After=network.target

   [Service]
   Type=simple
   User=pub-relay
   WorkingDirectory=/home/pub-relay/pub-relay
   Environment="RELAY_DOMAIN=Domain (Example: pub-relay.hama3.net)"
   Environment="RELAY_HOST=Host (Default: localhost)"
   Environment="RELAY_PORT=Port (Default: 8085)"
   Environment="RELAY_PKEY_PATH=/home/pub-relay/.ssh/actor.pem"
   Environment="REDIS_URL=redis://Server:Port/DB (Example: 127.0.0.1:6379/0)"
   ExecStart=/home/pub-relay/pub-relay/bin/pub-relay
   TimeoutSec=20
   Restart=always

   [Install]
   WantedBy=multi-user.target
   ```
1. systemctl start
   ```
   systemctl daemon-reload
   systemctl start pub-relay
   systemctl enable pub-relay
   ```
1. Enjoy!

## Usage

TODO

## Contributors

- [RX14](https://source.joinmastodon.org/RX14) creator, maintainer
