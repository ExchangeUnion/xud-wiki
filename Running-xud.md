# Running `xud`

`xud` can be launched via `npm start` or the `xud` executable in the `bin` folder. If `xud` is installed globally, it can be launched from any directory.

Use `xud --help` to get up-to-date, optional command line arguments to override defaults and settings in the [configuration](https://github.com/ExchangeUnion/xud/wiki/Configuration) file for a specific `xud` instance.

```
$ xud --help
Options:
  --help                 Show help                                     [boolean]
  --version              Show version number                           [boolean]
  --initDb               Whether to initialize the db with data        [boolean]
  --xudir, -x            Data directory for xud                         [string]
  --logLevel, -l         Verbosity of the logger                        [string]
  --logPath              Path to the log file                           [string]
  --db.database, -d      SQL database name                              [string]
  --db.host              Hostname for SQL database                      [string]
  --db.password          Password for SQL database                      [string]
  --db.port              Port for SQL database                          [number]
  --db.username          User for SQL database                          [string]
  --lndbtc.certpath      Path to the SSL certificate for lndBtc         [string]
  --lndbtc.disable       Disable lndBtc integration                    [boolean]
  --lndbtc.host          Host of the lndBtc gRPC interface              [string]
  --lndbtc.macaroonpath  Path of the admin macaroon for lndBtc          [string]
  --lndbtc.port          Port of the lndBtc gRPC interface              [number]
  --lndltc.certpath      Path to the SSL certificate for lndLtc         [string]
  --lndltc.disable       Disable lndLtc integration                    [boolean]
  --lndltc.host          Host of the lndLtc gRPC interface              [string]
  --lndltc.macaroonpath  Path of the admin macaroon for lndLtc          [string]
  --lndltc.port          Port of the lndLtc gRPC interface              [number]
  --p2p.addresses        String array of reachable addresses             [array]
  --p2p.listen           Listen for incoming peers                     [boolean]
  --p2p.port, -p         Port to listen for incoming peers              [number]
  --raiden.disable       Disable raiden integration                    [boolean]
  --raiden.port          Port for raiden REST service                   [number]
  --rpc.host             gRPC service host                              [string]
  --rpc.port, -r         gRPC service port                              [number]
  --webproxy.disable     Disable web proxy server                      [boolean]
  --webproxy.port, -w    Port for web proxy server                      [number]
```