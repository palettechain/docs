
## Usage
This document discribe how to setup and join palette chain network.<br>
Compared with quorum, palette uses a fixed istanbul consensus, <br>
and abandons private manager and permission, and the block generation <br>
time is fixed at 15s.

#### palette
Clone palette repo and generate binary file.
```bash
git clone https://github.com/palettechain/palette.git
make
export PATH=$(pwd)/build/bin:$PATH
```

#### istanbul tool
We use [istanbul tool](https://github.com/palettechain/istanbul-tools) to generate files which will be used while setup network.

clone the repo and build bin file with Makefile.
```bash
git clone https://github.com/palettechain/istanbul-tools.git
cd istanbul-tools
make
export PATH=$(pwd)/build/bin:$PATH
```

#### Setup
run comand to generate files as follow:
```bash
istanbul setup --num 5 --nodes --quorum --save --verbose
```
```dtd
$ tree
.
├── 0
│   └── nodekey
├── 1
│   └── nodekey
├── 2
│   └── nodekey
├── 3
│   └── nodekey
├── 4
│   └── nodekey
├── genesis.json
└── static-nodes.json

5 directories, 7 files
```

record the validators:
```bash
validators
{
	"Address": "0x4c3023d9d49147bf8a52d22d526591d38f9c2104",
	"Nodekey": "69d3cbddf4d473443854840196701b1b815d4f1b3b9e3d916206acf9f6fbd9d6",
	"NodeInfo": "enode://d13bf4821b4459d77caaf22e352522750667a440a23946f96c58b150127c0313c228224003016f8c84adbf88576e46b3504ffd9f4f9d84a3de5958ec3d4067f4@0.0.0.0:30303?discport=0"
}
......
{
	"Address": "0x69a28ca167bbf3e8ff8028e4c520f67619173dea",
	"Nodekey": "a4d01e1ab0d46cdaf6c36669ee0e34a56435ea5b033f51654804cc76b5c6f2ad",
	"NodeInfo": "enode://593e50007dc00ae9756f74d2d715b11b6f806d9fe8832353bca111b00cbed42ebc38fcc2a3e382c9f28f0fda6dc304633b050855950c21687b2d4cbff11792c0@0.0.0.0:30303?discport=0"
}
```

###### nodeKey 
nodeKey saved an hex string of private key, this key will be used for istanbul consensus signing and verify.

###### genesis.json
origin genesis file as follow:
```dtd
{
    "config": {
        "chainId": 10,
        "homesteadBlock": 0,
        "eip150Block": 0,
        "eip150Hash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "eip155Block": 0,
        "eip158Block": 0,
        "byzantiumBlock": 0,
        "constantinopleBlock": 0,
        "istanbul": {
            "epoch": 30000,
            "policy": 0,
            "ceil2Nby3Block": 0
        },
        "txnSizeLimit": 64,
        "maxCodeSize": 0,
        "isQuorum": true
    },
    "nonce": "0x0",
    "timestamp": "0x5f8e5eac",
    "extraData": "0x0000000000000000000000000000000000000000000000000000000000000000f8aff869944c3023d9d49147bf8a52d22d526591d38f9c2104948dfd400592cd83db1f260ed97297de8113a61ac6943bdf32d37bc63930f4fefb25905d74c3eb3c92f09493482e8fa60f90b3a1bba3b7431b4a0b9247e00b9469a28ca167bbf3e8ff8028e4c520f67619173deab8410000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c0",
    "gasLimit": "0xe0000000",
    "difficulty": "0x1",
    "mixHash": "0x63746963616c2062797a616e74696e65206661756c7420746f6c6572616e6365",
    "coinbase": "0x0000000000000000000000000000000000000000",
    "alloc": {
        "3bdf32d37bc63930f4fefb25905d74c3eb3c92f0": {
            "balance": "0x446c3b15f9926687d2c40534fdb564000000000000"
        },
        "4c3023d9d49147bf8a52d22d526591d38f9c2104": {
            "balance": "0x446c3b15f9926687d2c40534fdb564000000000000"
        },
        "69a28ca167bbf3e8ff8028e4c520f67619173dea": {
            "balance": "0x446c3b15f9926687d2c40534fdb564000000000000"
        },
        "8dfd400592cd83db1f260ed97297de8113a61ac6": {
            "balance": "0x446c3b15f9926687d2c40534fdb564000000000000"
        },
        "93482e8fa60f90b3a1bba3b7431b4a0b9247e00b": {
            "balance": "0x446c3b15f9926687d2c40534fdb564000000000000"
        }
    },
    "number": "0x0",
    "gasUsed": "0x0",
    "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000"
}
```

add `admin` address in this file, `admin` account used to manage validators.

###### static-nodes.json
```dtd
static-nodes.json
[
	"enode://d13bf4821b4459d77caaf22e352522750667a440a23946f96c58b150127c0313c228224003016f8c84adbf88576e46b3504ffd9f4f9d84a3de5958ec3d4067f4@0.0.0.0:30303?discport=0",
	"enode://e1e704443098036e4a85a317229fb89be0fc8875c01a9edfd15ee38923f960bdcde565bb9b1d9ae7ea85f677fc9fdc0d13e019690ff3bc7ee39a3453985073cf@0.0.0.0:30303?discport=0",
	"enode://6c35bd37f21b9706b3a889a418dec1c3b8debae0d66c555c61d484dd97a4ab8645cabb703a790ba1c220558788c620b913dd05c2e96aafdbe3b9096f421dcded@0.0.0.0:30303?discport=0",
	"enode://7dde2a3b57d3c91860f634ac7070910027ef7bfd0b9a90100694c00be3b92d8dbf62dfb550d11005ed6c7b718fcee1015e3e07178a42fdec8001ba24f7d4b5ec@0.0.0.0:30303?discport=0",
	"enode://593e50007dc00ae9756f74d2d715b11b6f806d9fe8832353bca111b00cbed42ebc38fcc2a3e382c9f28f0fda6dc304633b050855950c21687b2d4cbff11792c0@0.0.0.0:30303?discport=0"
]
```
modify ip address and port.

#### Start up network
create and start up nodes as follow, nodes number should equal or more than 5:
* step 1: <br>
create datadir for nodes, and copy genesis、static-nodes and nodeKey files in your datadir. e.g:
```bash
mkdir -p node0/data/geth;

cp genesis.json node0;
cp static-nodes.json node0/data/;
cp nodekey node0/data/geth;
```
* step 2: <br>
init node, e.g:
```bash
cd node0
geth --datadir data init genesis.json
```

* step 3: <br>
start up node, e.g:
```bash
cd node0
PRIVATE_CONFIG=ignore nohup geth \
--datadir data \
--syncmode full \
--mine --minerthreads 1 \
--verbosity 5 \
--networkid 10 \
--rpc --rpcaddr 0.0.0.0 --rpcport 22000 \
--rpcapi admin,db,eth,debug,miner,net,shh,txpool,personal,web3,quorum,istanbul \
--emitcheckpoints \
--port 30300 2>>node.log &
```

#### Add validator node
* step 1: <br>
Create and start a new node as in the previous steps

* step 2: <br>
Wating for the admin account add the new node as validator.

#### Start sync node
Same instructions as `Start up network` excluding step 3 which proposes the node as validator.

```bash
cd yournodeDir

PRIVATE_CONFIG=ignore geth \
--datadir data \
--syncmode full --verbosity 3 \
--networkid 100 \
--rpcapi db,eth,debug,net,shh,txpool,personal,web3,quorum,istanbul \
--rpcaddr 0.0.0.0 \
--rpcport yourRPCPort \
--port yourP2PPort \
```
