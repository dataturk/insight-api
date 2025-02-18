# Insight API

## Table of Contents
* [Getting Started](#getting-started)

### Statistics
* [Total 24h](#total-24h-statistic)
* [Transactions](#transactions-statistic)
* [NetHash](#nethash-statistic)
* [Pools](#pools-statistic)
* [Fees](#fees-statistic)
* [Outputs](#outputs-statistic)
* [Difficulty](#difficulty-statistic)
* [Total Supply](#total-supply-statistic)

A Ravencoin blockchain REST and web socket API service for [Ravencore Node](https://github.com/dataturk/ravencore-node).

This is a backend-only service. If you're looking for the web frontend application, take a look at https://github.com/dataturk/insight-ui.

## Getting Started

```bashl
npm install -g ravencore-node
ravencore-node create mynode
cd mynode
ravencore-node install insight-api
ravencore-node start
```

The API endpoints will be available by default at: `http://localhost:3001/api/`

## Prerequisites

- [Ravencore Node](https://github.com/dataturk/ravencore-node)

**Note:** You can use an existing Ravencoin data directory, however `txindex`, `addressindex`, `timestampindex` and `spentindex` needs to be set to true in `raven.conf`, as well as a few other additional fields.

## Query Rate Limit

To protect the server, insight-api has a built it query rate limiter. It can be configurable in `ravencore-node.json` with:
``` json
  "servicesConfig": {
    "insight-api": {
      "rateLimiterOptions": {
        "whitelist": ["::ffff:127.0.0.1"]
      }
    }
  }
```
With all the configuration options available: https://github.com/dataturk/insight-api/blob/master/lib/ratelimiter.js#L10-17

Or disabled entirely with:
``` json
  "servicesConfig": {
    "insight-api": {
      "disableRateLimiter": true
    }
  }
```

**Note:** `routePrefix` can be configurable in `ravencore-node.json` with:

``` json
  "servicesConfig": {
    "insight-api": {
      "routePrefix": "insight-api",
    }
  }
```



## API HTTP Endpoints

## Statistics

### Total 24h Statistic
```
  `GET` /api/statistics/total
```
This would return:
```
    {
        n_blocks_mined: 1268,
        time_between_blocks: 86301,
        mined_currency_amount: 1268000000000000,
        transaction_fees: 633992859997,
        number_of_transactions: 2547,
        outputs_volume: 46920965480,
        difficulty: 981808167.7687966,
    }
```
### Transactions Statistic
```
  `GET` /api/statistics/transactions?days=14
```
This would return:
```
[
    {
      date: "2017-05-30",
        transaction_count: 1087,
        block_count: 541
    },
    ...
]
```

### Fees Statistic
```
  `GET` /api/statistics/fees?days=14
```
This would return:
```
[
   {
       date: "2017-06-06",
       fee: 500000000
   },
   ...
]
```
### Outputs Statistic
```
  `GET` /api/statistics/outputs?days=14
```
This would return:
```
[
   {
       date: "2017-06-06",
       sum: 0
   },
   ...
]
```
### Difficulty Statistic
```
  `GET` /api/statistics/difficulty?days=14
```
This would return:
```
[
    {
        date: "2017-06-06",
        sum: 0
    },
    ...
]
```
### Pools Statistic
```
  `GET` /api/statistics/pools?date=2018-05-14
```
This would return:
```
[
    {
        date: "2018-05-14",
        ...
    }
    
]
```
### Nethash Statistic
```
  `GET` /api/statistics/network-hash?days=14
```
This would return:
```
[
    {
        date: "2017-06-06",
        sum: 0
    },
    ...
]
```
### Total Supply Statistic

```
  `GET` /api/supply
```
or
```
  `GET` /api/supply?format=object
```
This would return:
```
100091264
```
or
```
{
    "supply": "100091264"
}
```

### Assets
Get a listing of assets, optionally with metadata:
```
  /api/assets
  /api/assets?asset=MY_ASSET&verbose=true
  /api/assets?asset=COM*&size=10&skip=10
```
All parameters are optional.  size/skip are to implement pagination.  '*' can be used as a trailing wildcard only.

Sample results:
```
/api/assets?verbose=true&size=2
{
  ASDF: {
    name: "ASDF",
    amount: 10000000,
    units: 0,
    reissuable: 1,
    has_ipfs: 0,
    block_height: 551,
    blockhash: "2f209586e778308833d46ab0df4cd2eab6fd165c38d7221093d389cd0837c8ed"
  },
    ASDG: {
    name: "ASDG",
    amount: 1,
    units: 0,
    reissuable: 1,
    has_ipfs: 0,
    block_height: 552,
    blockhash: "4d5f71c4adeb4c08c46664e220976a29659e8f6431a4156bc56e45a78ac3db4c"
  }
}

/api/assets?size=10
[
  "ASDF",
  "ASDG",
  "ASDH",
  "ASDI",
  "ASDJ",
  "ASDK",
  "ASDL",
  "ASDM",
  "ASDN",
  "ASDO"
]
```

### Restricted Assets
* Global list of restricted assets:

  `/assets/restricted`

  (supports optional `verbose`, `size` and `skip` (pagination) parameters)

* Global list of qualifier assets (tags):

  `/assets/tags`

  OR

  `/assets/qualifiers`

  (supports optional `verbose`, `size` and `skip` (pagination) parameters)

* Global list of restricted assets which have been frozen:

  `/assets/frozen`

* Test a prospective verifier string to see if it's valid (syntax, existing tags, etc.):
  
  `/assets/checkVerifier?verifier=MY_TAG|ANOTHER_TAG`

  (supports POST)

* Retrive the verifier string for a restricted asset:

  `/asset/MY_RESTRICTED/verifier`

* Retrieve a list of all addresses which have been tagged with the specified qualifier asset (tag):

  `/asset/MY_TAG/tagged`

* Find out whether the specified restricted asset has been frozen (returns `true` or `false`):

  `/asset/MY_RESTRICTED/checkFrozen`

* Retrieve a list of all qualifier assets (tags) which have been applied to the specified address:

  `/addr/mqHTNjpK5PaLKY3ZVpRRgsqpmMAtuAiRsR/tags`

* Retrieve a list of all restricted assets which have frozen the specified address:

  `/addr/mqHTNjpK5PaLKY3ZVpRRgsqpmMAtuAiRsR/frozen`

* Find out whether the specified address has been frozen wrt the specified restricted asset (returns `true` or `false`):

  `/addr/mqHTNjpK5PaLKY3ZVpRRgsqpmMAtuAiRsR/checkFrozen/asset/MY_RESTRICTED`

* Find out whether the specified address has been tagged with the specified qualifier asset (tag) (returns `true` or `false`):

  `/addr/mqHTNjpK5PaLKY3ZVpRRgsqpmMAtuAiRsR/checkTag/asset/MY_TAG'`


### Block
```
  /api/block/[:hash]
  /api/block/00000000a967199a2fad0877433c93df785a8d8ce062e5f9b451cd1397bdbf62
```

### Block Index
Get block hash by height
```
  /api/block-index/[:height]
  /api/block-index/0
```
This would return:
```
{
  "blockHash":"000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f"
}
```
which is the hash of the Genesis block (0 height)


### Raw Block
```
  /api/rawblock/[:blockHash]
  /api/rawblock/[:blockHeight]
```

This would return:
```
{
  "rawblock":"blockhexstring..."
}
```

### Block Summaries

Get block summaries by date:
```
  /api/blocks?limit=3&blockDate=2016-04-22
```

Example response:
```
{
  "blocks": [
    {
      "height": 408495,
      "size": 989237,
      "hash": "00000000000000000108a1f4d4db839702d72f16561b1154600a26c453ecb378",
      "time": 1461360083,
      "txlength": 1695,
      "poolInfo": {
        "poolName": "BTCC Pool",
        "url": "https://pool.btcc.com/"
      }
    }
  ],
  "length": 1,
  "pagination": {
    "next": "2016-04-23",
    "prev": "2016-04-21",
    "currentTs": 1461369599,
    "current": "2016-04-22",
    "isToday": true,
    "more": true,
    "moreTs": 1461369600
  }
}
```

### Transaction
```
  /api/tx/[:txid]
  /api/tx/525de308971eabd941b139f46c7198b5af9479325c2395db7f2fb5ae8562556c
  /api/rawtx/[:rawid]
  /api/rawtx/525de308971eabd941b139f46c7198b5af9479325c2395db7f2fb5ae8562556c
```

### Address
```
  /api/addr/[:addr][?noTxList=1][&from=&to=]
  /api/addr/mmvP3mTe53qxHdPqXEvdu8WdC7GfQ2vmx5?noTxList=1
  /api/addr/mmvP3mTe53qxHdPqXEvdu8WdC7GfQ2vmx5?from=1000&to=2000
```

### Address Properties
```
  /api/addr/[:addr]/balance
  /api/addr/[:addr]/totalReceived
  /api/addr/[:addr]/totalSent
  /api/addr/[:addr]/unconfirmedBalance
```
The response contains the value in Satoshis.

### Address Balances (including assets)
```
  /api/addr/[:addr]/balances
```
The response contains RVN and asset balances combined:
```
{
  "MYASSET": {
    "totalReceived":100000000000,
    "totalSpent":0,
    "balance":100000000000
  },
  "RVN": {
    "totalReceived":4200000000,
    "totalSpent":0,
    "balance":4200000000
  }
}
```

### Unspent Outputs
```
  /api/addr/[:addr]/utxo
  /api/addr/2NF2baYuJAkCKo5onjUKEPdARQkZ6SYyKd5/utxo
  /api/addr/[:addr]/asset/[:asset]/utxo
  /api/addr/2NF2baYuJAkCKo5onjUKEPdARQkZ6SYyKd5/asset/MY_ASSET/utxo
  /api/addr/2NF2baYuJAkCKo5onjUKEPdARQkZ6SYyKd5/asset/*/utxo  (all assets)
```
Sample return:
```
[
  {
    "address":"mo9ncXisMeAoXwqcV5EWuyncbmCcQN4rVs",
    "txid":"d5f8a96faccf79d4c087fa217627bb1120e83f8ea1a7d84b1de4277ead9bbac1",
    "vout":0,
    "scriptPubKey":"76a91453c0307d6851aa0ce7825ba883c6bd9ad242b48688ac",
    "amount":0.000006,
    "satoshis":600,
    "confirmations":0,
    "ts":1461349425
  },
  {
    "address": "mo9ncXisMeAoXwqcV5EWuyncbmCcQN4rVs",
    "txid": "bc9df3b92120feaee4edc80963d8ed59d6a78ea0defef3ec3cb374f2015bfc6e",
    "vout": 1,
    "scriptPubKey": "76a91453c0307d6851aa0ce7825ba883c6bd9ad242b48688ac",
    "amount": 0.12345678,
    "satoshis: 12345678,
    "confirmations": 1,
    "height": 300001
  }
]
```

Here's an [extended example response with assets](docs/example_responses/utxo_with_assets.md).

### Unspent Outputs for Multiple Addresses
GET method:
```
  /api/addrs/[:addrs]/utxo
  /api/addrs/2NF2baYuJAkCKo5onjUKEPdARQkZ6SYyKd5,2NAre8sX2povnjy4aeiHKeEh97Qhn97tB1f/utxo
  /api/addrs/[:addrs]/asset/[:asset]/utxo
  /api/addrs/2NF2baYuJAkCKo5onjUKEPdARQkZ6SYyKd5,2NAre8sX2povnjy4aeiHKeEh97Qhn97tB1f/asset/MY_ASSET/utxo
```

POST method:
```
  /api/addrs/utxo
```

POST params:
```
addrs: 2NF2baYuJAkCKo5onjUKEPdARQkZ6SYyKd5,2NAre8sX2povnjy4aeiHKeEh97Qhn97tB1f
```

### Transactions by Block
```
  /api/txs/?block=HASH
  /api/txs/?block=00000000fa6cf7367e50ad14eb0ca4737131f256fc4c5841fd3c3f140140e6b6
```
### Transactions by Address
```
  /api/txs/?address=ADDR
  /api/txs/?address=mmhmMNfBiZZ37g1tgg2t8DDbNoEdqKVxAL
```

### Transactions for Multiple Addresses
GET method:
```
  /api/addrs/[:addrs]/txs[?from=&to=]
  /api/addrs/2NF2baYuJAkCKo5onjUKEPdARQkZ6SYyKd5,2NAre8sX2povnjy4aeiHKeEh97Qhn97tB1f/txs?from=0&to=20
```

POST method:
```
  /api/addrs/txs
```

POST params:
```
addrs: 2NF2baYuJAkCKo5onjUKEPdARQkZ6SYyKd5,2NAre8sX2povnjy4aeiHKeEh97Qhn97tB1f
from (optional): 0
to (optional): 20
noAsm (optional): 1 (will omit script asm from results)
noScriptSig (optional): 1 (will omit the scriptSig from all inputs)
noSpent (option): 1 (will omit spent information per output)
```

Sample output:
```
{ totalItems: 100,
  from: 0,
  to: 20,
  items:
    [ { txid: '3e81723d069b12983b2ef694c9782d32fca26cc978de744acbc32c3d3496e915',
       version: 1,
       locktime: 0,
       vin: [Object],
       vout: [Object],
       blockhash: '00000000011a135e5277f5493c52c66829792392632b8b65429cf07ad3c47a6c',
       confirmations: 109367,
       time: 1393659685,
       blocktime: 1393659685,
       valueOut: 0.3453,
       size: 225,
       firstSeenTs: undefined,
       valueIn: 0.3454,
       fees: 0.0001 },
      { ... },
      { ... },
      ...
      { ... }
    ]
 }
```

Here's an [extended example response with assets](docs/example_responses/tx_with_assets.md).

Note: if pagination params are not specified, the result is an array of transactions.

### Transaction Broadcasting
POST method:
```
  /api/tx/send
```
POST params:
```
  rawtx: "signed transaction as hex string"

  eg

  rawtx: 01000000017b1eabe0209b1fe794124575ef807057c77ada2138ae4fa8d6c4de0398a14f3f00000000494830450221008949f0cb400094ad2b5eb399d59d01c14d73d8fe6e96df1a7150deb388ab8935022079656090d7f6bac4c9a94e0aad311a4268e082a725f8aeae0573fb12ff866a5f01ffffffff01f0ca052a010000001976a914cbc20a7664f2f69e5355aa427045bc15e7c6c77288ac00000000

```
POST response:
```
  {
      txid: [:txid]
  }

  eg

  {
      txid: "c7736a0a0046d5a8cc61c8c3c2821d4d7517f5de2bc66a966011aaa79965ffba"
  }
```

### Historic Blockchain Data Sync Status
```
  /api/sync
```

### Live Network P2P Data Sync Status
```
  /api/peer
```

### Status of the Ravencoin Network
```
  /api/status?q=xxx
```

Where "xxx" can be:

 * getInfo
 * getDifficulty
 * getBestBlockHash
 * getLastBlockHash
 * getMiningInfo
 * getPeerInfo


### Utility Methods
```
  /api/utils/estimatesmartfee[?nbBlocks=6&mode=economical]
```
Sample output:
````
{
  "6":0.00001047
}
````

## Web Socket API
The web socket API is served using [socket.io](http://socket.io).

### `inv` room:

The following are the events published by insight in the `inv` room:

`tx`: new transaction received from network. This event is published in the 'inv' room. Data will be a app/models/Transaction object.
Sample output:
```
{
  "txid":"00c1b1acb310b87085c7deaaeba478cef5dc9519fab87a4d943ecbb39bd5b053",
  "processed":false
  ...
}
```


`block`: new block received from network. This event is published in the `inv` room. Data will be a app/models/Block object.
Sample output:
```
{
  "hash":"000000004a3d187c430cd6a5e988aca3b19e1f1d1727a50dead6c8ac26899b96",
  "time":1389789343,
  ...
}
```
`info`: General Statistics Info
Sample output:
```
{
info:
  {
  balance: 0
  blocks: 245823
  connections: 123
  difficulty: 18231.18159436285
  errors: ""
  keypoololdest: 1528311725
  keypoolsize: 2000
  network: "livenet"
  paytxfee: 0
  protocolversion: 70015
  proxy: ""
  relayfee: 0.00001
  reward: 500000000000
  testnet: false
  timeoffset: 0
  version: 152000
  walletversion: 10000
  }
miningInfo:
  {
  blocks: 245823
  difficulty: 18231.18159436285
  networkhashps: 1311616809584.154
  chain: "main"
  }
supply: "1229115000"
```
`markets_info`: General Price and Market data
Sample output:
```
{
  24h_volume_usd: "280378.0"
  available_supply: "1223775205.0"
  market_cap_usd: "36072365.0"
  percent_change_24h: "-0.25"
  price_btc: "0.00000383"
  price_usd: "0.0294763"
}
```
### `raven` room: 

`raven/tx`: Returns a transformed tx as a json element detailing the transaction
Sample output:
```
{
txid: "4aef2faba5add0a467b4bc636024c7ed2501215a171df7cb7ed7376fe6bfeeca"
valueOut: 5000
vin:(0)
  []
  length: 0
vout: (1)
  [
    {
    value: 500000000000
    address: "RKbh4QopviguAWveJgjmoFKNMxewRFyvAb"
    }
  ] 
isRBF: true
}
```

`raven/block`: Returns a transformed block as a json element detailing the block
Sample output:
```
{
block:
  {
  bits: "1b03983c"
  chainwork: "000000000000000000000000000000000000000000000000a28c9a9e0fbcac74"
  confirmations: 1
  difficulty: 18231.18159436
  hash: "0000000000031522362b3b0d4579ed4bbee206fb7397383e835134ee1c394c2d"
  height: 245845
  isMainChain: true
  merkleroot: "7e036837f82cb526cc44db73504ab9c051c6f37eeb7e64aa543757b4405b0d1b"
  minedBy: "RVG96MbaKEDFzzj9NzbAuxkDt86KAm2Qj5"
  nonce: 1578377516
  poolInfo: {poolName: "f2pool", url: "http://f2pool.com/"}
  previousblockhash: "0000000000037953f3d835fb94e168520b2a7edbb1268ef987048dfa916cd25f"
  reward: 5000
  size: 26990
  time: 1528388943
  tx:(7) 
    [
    "ea4f02a17146117e5961cfe01e7e00067f59b2167d17a415717fadb8a823456e"
    "d9ec249e1a2297610b5af03238f2de34bd1ba739188fd1ea33c464fe77a07ce1"
    "062c951f32f60937c58f009e5c7eaf19db93148e9b677651fe83f6ebfded195e"
    "4d7853c1aa1c9b7722ab7ec34bf053a4661eb82f68075603f69006ece4f21f1c"
    "63b0219ee6d3204dc604660d2aa6eed6fd05dea912777e14322a3ed56886a102"
    "c5e302e3d9b7be5dfab1a219052fb8424071407e978cb57e1a9a0986e9c7f96d"
    "01ee0c906791f9c3e613a4c4b8d87c45b50b9e12058dd69ce5a647e028710b6d"
    ]
  version: 536870912
  }
transactions: (7)
  [
    {
    blockhash: "0000000000031522362b3b0d4579ed4bbee206fb7397383e835134ee1c394c2d"
    blockheight: 245845
    blocktime: 1528388943
    confirmations: 1
    isCoinBase: true
    locktime: 703249321
    size: 250
    time: 1528388943
    txid: "ea4f02a17146117e5961cfe01e7e00067f59b2167d17a415717fadb8a823456e"
    valueOut:5000.0003294
    version:1
    vin: (1)
      [
        {
        coinbase: "0355c0032cfabe6d6d0000000000000000000000000000...0000000000000000000"
        n: 0
        sequence: 3
        }
      ]
    vout: (2)
      [
        {
        n: 0
        scriptPubKey:
          {
          hex: "76a914db2f9b86eb5eae8dae8dfb1da75440584be36daa88ac"
          asm: "OP_DUP OP_HASH160 db2f9b86eb5eae8dae8dfb1da75440584be36daa OP_EQUALVERIFY OP_CHECKSIG"
          addresses: (1)
            [
            "RVG96MbaKEDFzzj9NzbAuxkDt86KAm2Qj5"
            ]
          type: "pubkeyhash"
          }
        spentHeight: null
        spentIndex: null
        spentTxId: null
        value: "5000.00032940"
        }
       ... 
      ]
    }
    ...
  ]
}
```

### Example Usage

The following html page connects to the socket.io insight API and listens for new transactions.

html
```
<html>
<body>
  <script src="http://<insight-server>:<port>/socket.io/socket.io.js"></script>
  <script>
    eventToListenTo = 'tx'
    room = 'inv'

    var socket = io("http://<insight-server>:<port>/");
    socket.on('connect', function() {
      // Join the room.
      socket.emit('subscribe', room);
    })
    socket.on(eventToListenTo, function(data) {
      console.log("New transaction received: " + data.txid)
    })
  </script>
</body>
</html>
```

## License
(The MIT License)

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
