# Exchange Guide (WIP)

## Goal
This guide describes how to setup `xud` with an existing exchange system and how to connect to the XUD Simulation Network or `xud-simnet`. `xud` is in alpha stage and the simnet aims to give exchange operators a 'feel' of how things will work. `xud` in it's current stage should never be connected to a production system nor be used on mainnet. You'll be using test coins at all times. This guide purpusely is kept relatively manual to make it understandable how `xud` works. In parallel we are working on improved automated setup using docker.

Once the setup is completed, you will be able to query pending orders, place orders, and experiment with `xud`'s rich set of commands. Once a match is found in the network, your orders will be executed via a cross-chain atomic swap. `xud-simnet` currently supports atomic swaps between BTC and LTC. This guide will show how to to install GO, bitcoin and litecoin daemons, setup Lightning daemons (LND) for bitcoin (BTC) and litecoin (LTC), setup `xud` and basic instructions of how to integrate its API and finally connect to `xud-simnet`. This guide is written for Linux & MacOS operating systems and geared towards IT administrators of exchanges.

## Describing the Setup
To connect to `xud-simnet` you'll need to run several components which together provide support for exchanging order information and atomic swaps between BTC and LTC on the lightning network. You will setup the following components:
- `BTCD` - full node, connected to the BTC chain
- `LTCD` - full node, connected to the LTC chain
- `LND-BTC` - lightning network daemon for the BTC network
- `LND-LTC` - lightning network daemon for the LTC network
- `XUD` - Exchange Union's decentralized exchange layer

## Installation 
### Prerequisites
Make sure you have the below programs installed:
- [git](https://gist.github.com/derhuerst/1b15ff4652a867391f03#file-linux-)
- [make](https://www.cyberciti.biz/faq/debian-linux-install-gnu-gcc-compiler/)
- [node.js + npm](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions)
- killall (`sudo apt-get install psmisc`)

### Installing GO
if GO is not yet installed on your system please use the following command to install it:
```
sudo apt-get install golang-1.10-go
```
Note that golang-1.10-go puts binaries in /usr/lib/go-1.10/bin. If you want them on your PATH, you need to make that change yourself. Alternatively, you can create a softlink as follows:
```
sudo ln -s /usr/lib/go-1.10/bin/go /usr/local/bin/go 
```

### Repository Checkout
From your home directory run
```
git clone https://github.com/ExchangeUnion/xud-simnet.git
```
This should create the `xud-simnet` directory under your home directory.

### Setup ~/.bashrc
To enable access to the scripts please update and source your `.bashrc`:
```
source ~/xud-simnet/setup.bash
source ~/.bashrc
```
Please note that the `setup.bash` script set your `$GOPATH` to `~/xud-simnet/go`. All changes from `setup.bash`are temporary and only active for the current terminal session. You need to repeat the sourcing above or add things to `PATH` to make these permanent.

### Verify Setup
Before you move on lets verify that we are ready:
```
xud-simnet-check
```

### Installation
Install all components with
```
xud-simnet-install
```
It could take several minutes depending on the speed of your machine & connection until you see `Ready!`.

### Starting
You can start all components with
```
xud-simnet-start
```
Since `btcd` and `ltcd` need to sync the blocks of the XUD simnet when started for the first time, it could take a minute or two until you see `Ready!`.

### Payment Channels
To setup payment channels run
```
xud-simnet-channels
```
Payment channels are used to instantly settle trades via cross chain atomic swaps with peers. It should take about 10 minutes for your payment channels to be ready. The `xud-simnet` features one minute block times and we could speed this up arbitrarily, but later on mainnet this will involve an even more significant waiting time. We decided to keep it close to reality without being to annoyingly long.

### Final check
Once you see `Xud system is ready!`, run
```
xucli getinfo
```
and you should see something similar to
```
{
  "version": "1.0.0-alpha.1",
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

with one active channel for both, `lndbtc` and `lndltc`. If so, you are finally ready to rumble. Yay - let's do some test trades!

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
xucli getorders LTC/BTC
```
Let's execute a test order to trigger a match and execution via atomic swap by e.g. buying 0.0005 litecoin with bitcoin
```
xucli buy 0.0005 LTC/BTC 1.13
```
which should result in
```
**TODO**
```

Currently the trading bots of xud1, 2 & 3 are configured to simply replace successfully filled and swapped orders. A more realistic, market dynamic behavior will be deployed soon.

### API Integration
`xud`'s gRPC API overview can be found [here](https://exchangeunion.github.io/xud-typedoc/classes/_grpc_grpcservice_.grpcservice.html).

From a high-level perspective, `xud` supports two modes:
1. `matching`
2. `nomatching`

Default is `matching` mode, which means that `xud` acts as order book & matching engine for all of your orders. Choose this mode if your exchange is not live yet, comparably small or you simply want to rather rely on the battle-tested matching engine of `xud` instead of using your own. You will need to change your current system in a way to *forward* orders to and listen to order events from `xud`. The following API calls are relevant:
* `placeOrder` use this to forward an order to `xud`, e.g. triggered by a user of your platform. You can choose the sync version, which returns the final result in the call, which can take some seconds if a swap with a remote peer is involved. The async version updates with events in the course of the trade. Currently buy/sell as market/limit order are supported. 
* `cancelOrder` cancel an order previously placed via `placeOrder`, e.g. triggered by a user of your platform
* `getOrders` use this to e.g. display order information in your front-end

In `nomatching` mode, which can be enabled via [xud.conf](https://github.com/ExchangeUnion/xud/blob/master/sample-xud.conf), `xud` informs about orders from other peers and executes swaps with peers.
* `subscribePeerOrders` informs about orders from other peers in an event stream, use this call to receive peerOrders from the network and include these in your internal order book & matching engine. You will need to adjust your matching engine to call *executeSwap* if a match is found for a specific `peerOrder`.
* `executeSwap` executes an atomic swap in a matter of with a remote peer. You can continue using your internal `orderID`s, `xud` takes care of mapping such with `orderID`s in the network. The swap might take up to several seconds to complete.
* `subscribeSwaps` informs about successful/failed swaps with other peers in an event stream, use this call to determine when to remove an order from your internal order book.

### Upgrade
To upgrade your existing `xud-simnet` setup with active channels run
```
xud-simnet-stop
cd ~/xud-simnet
git fetch
git pull
xud-simnet-start
xucli getinfo
```
### Help us to improve!
`xud` is in alpha stage, as well as this guide. Please help us to improve by opening issues for [xud](https://github.com/ExchangeUnion/xud/issues) and the [simnet](https://github.com/ExchangeUnion/xud-simnet/issues).

Feel like talking? Chat with us in our [Gitter channel](https://gitter.im/exchangeunion/xud-testing)!