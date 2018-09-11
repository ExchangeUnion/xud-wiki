# Configuration

An *optional* configuration file uses [TOML](https://github.com/toml-lang/toml) and by default should be saved at  `~/.xud/xud.conf` on Linux or `AppData\Local\Xud\xud.conf` on Windows (run `xud` at least once for this folder to be created). Options with default values are shown below.

```toml
initDb = true # Whether to initalize a new database with default values

[rpc]
port = 8886
host = "localhost"

[webproxy]
disable = false
port = 8080

[db]
username = "xud"
password = ""
database = "xud"
port = 3306
host = "localhost"

[p2p]
listen = true
port = 8885 # The port to listen for incoming peer connections when listen = true
# A string array of reachable socket addresses for this node, port defaults to p2p.port if unspecified
addresses = [ "4.8.15.16:8885" ]

[lndbtc]
disable = false
host = "localhost"

[lndltc]
disable = false
host = "localhost"
certpath = "" # The default value is platform-specific
macaroonpath = "" # The default value is platform-specific

[raiden]
disable = false
host = "localhost"
port = 5001
```