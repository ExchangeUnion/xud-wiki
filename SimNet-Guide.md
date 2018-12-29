# SimNet Guide

## Goal

This guide describes how to setup `xud` and connect to and trade on the XUD Simulation Network (`xud-simnet`). It should take about 30 minutes to complete all steps.

`xud` is in alpha stage and the `xud-simnet` aims to give exchange operators and testers an early 'look & feel' of how things will work. `xud` in it's current stage should never be connected to a production system or be configured for mainnet usage. The `xud-simnet` uses test coins at all times.

Once the setup of `xud-simnet` is completed, you will be able to query pending orders, place orders, and experiment with `xud`'s rich set of commands. When a match is found in the network, your orders will automatically be executed via cross-chain atomic swaps. `xud-simnet` currently supports atomic swaps between BTC and LTC. This guide will show how to install dependencies like Go and how to use `xud-simnet` to automatically setup bitcoin and litecoin daemons, lightning daemons (LND) for bitcoin (BTC) and litecoin (LTC), `xud` and finally connect to the `xud-simnet`. This guide is written for Linux & MacOS and suitable for everyone that knows how to use a command line.

## Describing the Setup

`xud-simnet` spins up several components on your machine which together provide support for exchanging order information and executing atomic swaps between BTC and LTC on the lightning network. It will setup the following components:

- `BTCD` - full node, connected to the BTC chain
- `LTCD` - full node, connected to the LTC chain
- `LND-BTC` - lightning network daemon for the BTC network
- `LND-LTC` - lightning network daemon for the LTC network
- `XUD` - Exchange Union's decentralized exchange layer

## Installation 

### Prerequisites

Make sure you have the below programs installed:
- [git](https://gist.github.com/derhuerst/1b15ff4652a867391f03#file-linux-md)
- [make](https://www.cyberciti.biz/faq/debian-linux-install-gnu-gcc-compiler/)
- [node.js + npm](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions) (use node 10 - we will drop support for node 8 soon)
- python 2.7 (preinstalled on most systems, check with `python --version`)
- killall (`sudo apt-get install psmisc`)

### Installing Go

If Go is not yet installed on your system, use the following command to install it:

```
sudo apt-get install golang-1.10-go
```

If Go is not included in your package manager, follow the official [install instructions](https://golang.org/doc/install). Note that `golang-1.10-go` puts binaries in `/usr/lib/go-1.10/bin`. If you want them on your PATH, you need to make that change yourself. Alternatively, you can create a softlink with

```
sudo ln -s /usr/lib/go-1.10/bin/go /usr/local/bin/go 
```

### Repository Checkout

```
git clone https://github.com/ExchangeUnion/xud-simnet.git ~/xud-simnet
```

This should create the `xud-simnet` directory under your home directory.

### Setup ~/.bashrc

To enable access to the scripts please update and source your `.bashrc`:

```
source ~/xud-simnet/setup.bash
source ~/.bashrc
```

Please note that the `setup.bash` script set your `$GOPATH` to `~/xud-simnet/go`. All changes from `setup.bash`are temporary and only active for the current terminal session. You can run

```
echo "source ~/xud-simnet/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

to make these changes permanent.

### Installation

Install all components with

```
xud-simnet-install
```

It could take several minutes, depending on the speed of your machine & connection, until you see `Ready!`.

### Starting

You can start all components with

```
xud-simnet-start
```

Since `btcd` and `ltcd` need to sync the blocks of the XUD simnet when started for the first time, it could take some time until you see `Ready!`. As long as you see the process advancing in the terminal, be patient and keep the process running.

### Stopping

Gracefully stop the environment with `xud-simnet-stop`.

### Payment Channels

To setup payment channels run

```
xud-simnet-channels
```

Payment channels are used to instantly settle trades via cross chain atomic swaps with peers. It can take 10 minutes or more for your payment channels to be ready. The `xud-simnet` features one minute block times and we could speed this up arbitrarily, but later on mainnet this will involve an even more significant waiting time. We decided to keep it close to reality without being too annoyingly long.

### Final check

Once you see `Xud system is ready!`, run

```
xucli getinfo
```

and you should see something similar to

```
{
  "version": "1.0.0-alpha.6",
  "nodePubKey": "0361b8cb3dc599564f68ac651df4b002280bd8fa790ccc0aacc9113880e8b5e8b8",
  "urisList": [],
  "numPeers": 3,
  "numPairs": 1,
  "orders": {
    "peer": 1,
    "own": 0
  },
  "lndbtc": {
    "error": "",
    "channels": {
      "active": 1,
      "inactive": 0,
      "pending": 0
    },
    "chainsList": [
      "bitcoin"
    ],
    "blockheight": 18797,
    "urisList": [],
    "version": "0.5.0-beta commit=v0.4.2-beta-1247-ge22ae9985b696924d5934004c269c930d3ccba43",
    "alias": "BTC@exchange-install-test-machine"
  },
  "lndltc": {
    "error": "",
    "channels": {
      "active": 1,
      "inactive": 0,
      "pending": 0
    },
    "chainsList": [
      "litecoin"
    ],
    "blockheight": 22362,
    "urisList": [],
    "version": "0.5.0-beta commit=v0.4.2-beta-1247-ge22ae9985b696924d5934004c269c930d3ccba43",
    "alias": "LTC@exchange-install-test-machine"
  }
}
```

with one active channel for both, `lndbtc` and `lndltc`. If so, you are finally ready to rumble. Let's do some trades!

### Trading

`xud` is now connected to `xud-simnet` which includes three permanent nodes operated by Exchange Union. You can view these by running

```
xucli listpeers
```

The output of connected peers should contain

```
"address": "xud1.test.exchangeunion.com:8885",
"nodePubKey": "02b66438730d1fcdf4a4ae5d3d73e847a272f160fee2938e132b52cab0a0d9cfc6",

"address": "xud2.test.exchangeunion.com:8885",
"nodePubKey": "028599d05b18c0c3f8028915a17d603416f7276c822b6b2d20e71a3502bd0f9e0a"

"address": "xud3.test.exchangeunion.com:8885",
"nodePubKey": "03fd337659e99e628d0487e4f87acf93e353db06f754dccc402f2de1b857a319d0"
```

You already received existing orders from the network. You can view these with

```
xucli listorders LTC/BTC
```

Let's execute a test order to trigger a match and execution via atomic swap by e.g. buying 0.5 litecoin with bitcoin

```
xucli buy 0.5 LTC/BTC 0.0079
```

which should result in sth. like

```
swapped 0.5 LTC with peer order c0de48a0-0b90-11e9-97c6-95f0014144a4
```

Currently the trading bots of xud1, 2 & 3 are configured to simply replace successfully filled and swapped orders. A more realistic, market dynamic behavior will be deployed soon. In a recent upgrade, `xud-simnet` now features larger BTC and LTC lightning channels to support real-world order sizes.

### Updates & useful commands

`xud-simnet-start` automatically checks for updates and installs these. If an update fails or for any other unexpected errors, please [report the issue](https://github.com/ExchangeUnion/xud-simnet/issues) and use `xud-simnet-stop`, `xud-simnet-clean`, `rm -rf xud-simnet` and install from scratch.

For always up-to-date CLI commands check `xucli --help`. 

Restart the simnet environment with `xud-simnet-restart`. If you want to control the underlying `lnd`,`btcd` or `ltcd` clients type `alias` to see aliases set by the `xud-simnet` scripts. to e.g. call `getInfo` for the BTC LND this would be `lndbtc-lncli getinfo`.

### Help us to improve!

`xud` is in alpha stage, as well as this guide. Please help us to improve by opening issues (or even better PRs) for [xud](https://github.com/ExchangeUnion/xud/issues), the [simnet](https://github.com/ExchangeUnion/xud-simnet/issues) and this [wiki](https://github.com/ExchangeUnion/xud-wiki/issues).

Feel like talking? Chat with us on [Gitter](https://gitter.im/exchangeunion/xud-testing)!