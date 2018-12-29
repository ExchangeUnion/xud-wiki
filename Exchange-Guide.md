# Exchange Guide (WIP)

## Goal

This guide describes how to setup `xud` with an existing exchange system and testing its trading functionalities with the `xud-simnet`.

`xud` is in alpha stage and this guide aims to give exchange operators a 'feel' of how things will work. `xud` in it's current stage should never be connected to a production system or be configured for mainnet usage. The `xud-simnet` uses test coins at all times. We are working on an improved setup using docker, which will also be the preferred way to use `xud` for exchanges in production.

### XUD SimNet

Follow [this guide](SimNet-Guide.md) to set up the XUD SimNet. Once the setup of `xud-simnet` is completed, you will be able to query pending orders, place orders, and experiment with `xud`'s rich set of commands. This guide is written for Linux & MacOS operating systems and geared towards IT administrators of exchanges.


### API Integration

`xud` uses [gRPC](https://grpc.io/). The latest overview of available calls with descriptions can be found in [this proto file](https://github.com/ExchangeUnion/xud/blob/master/proto/xudrpc.proto). Once the gRPC is more stable, we'll provide a nicer looking documentation.

From a high-level perspective, `xud` supports two modes: `matching` and `nomatching`

Default is `matching` mode, which means that `xud` acts as order book & matching engine for all of your orders. Choose this mode if your exchange is not live yet, comparably small or you simply want to rather rely on the matching engine of `xud` instead of using your own. You will need to design your exchange in a way to add orders to `xud`'s order book and listen to order events, in order to display real-time order information to your users and update user account balances. The following API calls are relevant:
* `SubscribeAddedOrders` this streaming call subscribes to orders being added to the order book. Together with `SubscribeRemovedOrders`, this allows the client to maintain an up-to-date view of the order book. Use this call to show a real-time order book to users via your exchange front-end and update account balances for your users.
* `SubscribeRemovedOrders` this streaming call subscribes to orders being  removed - either in full or in part - from the order book. Together with `SubscribeAddedOrders`, this allows the client to maintain an up-to-date view of the order book. Use this call to show a real-time order book to users via your exchange front-end and update account balances for your users.
* `PlaceOrder` use this to add an order to `xud`'s order book, which was triggered by a user of your platform. You can choose the sync version of the call, which returns the final result. This can take some seconds if the order gets swapped with a remote peer. The async version updates with events in the course of the swap via `SubscribeSwaps`. Currently buy/sell as market/limit order are supported. 
* `RemoveOrder` removes an order previously placed via `PlaceOrder`, which is triggered by a user of your platform.
* `SubscribeSwaps` informs about successful/failed swaps with other peers in an event stream. Use this call to track order executions, update balances, and inform users when one of their orders was settled.

`nomatching` mode, which can be enabled via [xud.conf](https://github.com/ExchangeUnion/xud/blob/master/sample-xud.conf), is designed for all order matching being handled by your existing matching engine and `xud` mainly a) informs about new & removed orders from other peers and b) executes swaps with peers when a match in your matching engine was found. The following API calls are relevant:
* `SubscribeAddedOrders` informs about new `peerOrder`s from other peers in an event stream. Use this call to include these `peerOrder`s in your internal order book & matching engine. Adjust your system to call `ExecuteSwap`, if a match is found for a specific `peerOrder`.
* `SubscribeRemovedOrders` informs about `peerOrder`s being removed - either in full or in part - in an event stream. Use this call to remove these `peerOrder`s from your internal order book & matching engine. These orders were invalidated by the peer and are no longer available for trading.
* `PlaceOrder` broadcasts your orders to other peers. This call is async in `nomatching` mode and you'll get updates of peers swapping your orders via `SubscribeSwaps`. Currently buy/sell as market/limit order are supported.
* `SubscribeSwaps` informs about successful/failed swaps with other peers in an event stream and is to be used with `PlaceOrder`. Use this call to get real-time swap events from placed orders to track order executions, update your user's balances, and inform users when one of their orders was settled.
* `ExecuteSwap` executes a swap for an match your internal matching engine found with a remote peer. This call requires `orderID`, `pairID` & `quantity`. You can continue using your internal `orderID`s, `xud` takes care of mapping such with `orderID`s in the Exchange Union network. This is a sync call, returning the final result of a swap. It might take several seconds for the swap to complete.

### Help us to improve!

`xud` is in alpha stage, as well as this guide. Please help us to improve by opening issues (or even better PRs) for [xud](https://github.com/ExchangeUnion/xud/issues), the [simnet](https://github.com/ExchangeUnion/xud-simnet/issues) and this [wiki](https://github.com/ExchangeUnion/xud-wiki/issues).

Feel like talking? Chat with us on [Gitter](https://gitter.im/exchangeunion/xud-testing)!