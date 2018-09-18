`xucli` is a command line tool that handles much of the basic interaction with a running `xud` instance. If `xud` is installed globally, it can be launched from any directory.

```
$ xucli --help
xucli <command>

Commands:
  xucli addcurrency <currency>              add a currency
  <swap_client> [decimal_places]
  [token_address]
  xucli addpair <base_currency>             add a trading pair
  <quote_currency>
  xucli cancelorder <pair_id> <order_id>    cancel an order
  xucli channelbalance <currency>           get total channel balance for a
                                            given currency
  xucli connect <node_uri>                  connect to an xu node
  xucli disconnect <node_pub_key>           disconnect from an xu node
  xucli executeSwap <role>                  execute an atomic swap
  <sending_amount> <sending_token>
  <receiving_amount> <receiving_token>
  <node_pub_key>
  xucli getinfo                             get general info from the xud node
  xucli getorders <pair_id> [max_results]   get orders from the order book
  xucli listpairs                           get order book's available pairs
  xucli listpeers                           list connected peers
  xucli buy <quantity> <pair_id> <price>    place a buy order
  [order_id]
  xucli sell <quantity> <pair_id> <price>   place a sell order
  [order_id]
  xucli removecurrency <currency>           remove a currency
  xucli removepair <pair_id>                remove a trading pair
  xucli shutdown                            gracefully shutdown the xud node
  xucli subscribepeerorders                 subscribe to peer order events
  xucli subscribeswaps                      subscribe to executed swaps

Options:
  --help          Show help                                            [boolean]
  --version       Show version number                                  [boolean]
  --rpc.port, -p  RPC service port                      [number] [default: 8886]
  --rpc.host, -h  RPC service hostname           [string] [default: "localhost"]
```

Calling any one of the commands above with the `--help` flag will output additional instructions for that particular command.

Examples:
```
./xucli connect xud1.test.exchangeunion.com 8885
# Manually connect to another xud instance

./xucli buy 2 LTC/BTC 0.01
# Places a new limit order BUYING 2 LTC for a price of 0.01 BTC

./xucli sell 2 LTC/BTC 1338 market
# Places a new limit order SELLING 2 LTC for the best market price

./xucli buy 5 LTC/BTC 0.012 1337
# Places a new market order BUYING 5 LTC for a price of 0.012 BTC, assigning alocal order id of 1337
```