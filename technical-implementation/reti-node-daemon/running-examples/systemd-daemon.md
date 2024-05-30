# systemd daemon

Here's a submitted example systemd daemon file.\
For your own configuration, note the user / group parameters, and the validator, node, and network arguments in ExecStart which will need adjusted for your validator id (and later, network)

```systemd
[Unit]
Description=Reti daemon
After=network.target

[Service]
Type=simple
WorkingDirectory=/home/pi
ExecStart=/bin/sh -c 'exec /home/pi/reti/nodemgr/reti --validator 4 --node 1 --network testnet daemon'
User=pi
Group=pi
Restart=always
RestartSec=10s
ProtectSystem=false

[Install]
WantedBy=multi-user.target
```
