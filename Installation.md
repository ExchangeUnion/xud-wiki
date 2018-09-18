This page contains detailed instructions on installing `xud`. Instructions for installing via Docker can be found [here](https://github.com/ExchangeUnion/xud/wiki/Docker).

## Requirements

Make sure to have [Node.js](https://nodejs.org/en/download/) installed, v8.11.3 or higher.

## Cloning from GitHub

Testers and developers should clone the repository from GitHub and install dependencies.

```bash
git clone https://github.com/ExchangeUnion/xud
cd xud
npm install
npm run compile
```

## Installing from npm

```bash
sudo npm install xud -g --unsafe-perm
```

## Daemonize `xud`

If you want to daemonize `xud` so it starts on boot without needing its own terminal, you can do this using `systemd`. The following sample `systemd` configuration works on Ubuntu:

```bash
[Unit]
Description= XUD - ExchangeUnion Daemon
ConditionPathExists=/opt/xud/dist/
After=network.target

[Service]
User=xud
Group=xud
WorkingDirectory=/opt/xud/
ExecStart=/usr/bin/node /opt/xud/dist/Xud.js
Type=simple

[Install]
WantedBy=multi-user.target
```