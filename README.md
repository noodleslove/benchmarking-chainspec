# Exercise 6

- Runtime benchmarking
- Use runtime benchmarking to provide the weights for extrinsic calls.
- Create a custom Chainspec and change to raw chainspec, and deploy a test net.

## 1. Benchmarking

### 1.1 Build

```sh
cargo build --release --features runtime-benchmarks
```

### 1.2 Test

```sh
cargo test -p pallet-template --all-features
```

Results:
![The Result Picture](https://i.loli.net/2021/09/27/wNzBIE2im3dVRKO.jpg)

```sh
./target/release/node-template benchmark --chain dev --execution=wasm --wasm-execution=compiled --pallet pallet_template --extrinsic do_something --steps 50 --repeat 20
```

Results:
```bash
2021-09-28 11:35:08 Running Benchmark:  pallet_template do_something    18/20   0/1    
Pallet: "pallet_template", Extrinsic: "do_something", Lowest values: [], Highest values: [], Steps: 20, Repeat: 50
Raw Storage Info
========
Storage: TemplateModule Something (r:0 w:1)

Median Slopes Analysis
========
-- Extrinsic Time --

Model:
Time ~=    580.3
    + s        0
              µs

Reads = 0 + (0 * s)
Writes = 1 + (0 * s)

Min Squares Analysis
========
-- Extrinsic Time --

Data points distribution:
    s   mean µs  sigma µs       %
    0     571.1     25.02    4.3%
   50     597.9     35.91    6.0%
  100     571.5     20.46    3.5%
  150     595.5     35.31    5.9%
  200     616.4     61.87   10.0%
  250     578.7     31.34    5.4%
  300     573.3     27.29    4.7%
  350     608.8     38.55    6.3%
  400     569.8     16.69    2.9%
  450     586.3     21.63    3.6%
  500     537.3     17.26    3.2%
  550     543.2     23.19    4.2%
  600     577.6     13.02    2.2%
  650     546.9     21.58    3.9%
  700     575.5     15.14    2.6%
  750     572.8     11.01    1.9%
  800     581.6     33.02    5.6%
  850     553.2     15.91    2.8%
  900     577.3     16.11    2.7%
  950     575.8     22.03    3.8%
 1000     565.3     25.11    4.4%

Quality and confidence:
param     error
s         0.004

Model:
Time ~=    587.4
    + s        0
              µs

Reads = 0 + (0 * s)
Writes = 1 + (0 * s)

```

## 2. Creating chain specs

```sh
# Export the local chain spec to json
./target/release/node-template build-spec --disable-default-bootnode --chain local > my-staging-spec.json
```

[my-staging-spec.json](./my-staging-spec.json).

```sh
./target/release/node-template build-spec --chain=my-staging-spec.json --raw --disable-default-bootnode > my-staging-spec-Raw.json
```

[my-staging-spec-Raw.json](./my-staging-spec-Raw.json).

## 3. Launching

```sh
# purge chain (only required for new/modified dev chain spec)
./target/release/node-template purge-chain --base-path /tmp/node01 --chain local -y
```

```sh
# start node01
./target/release/node-template \
  --base-path /tmp/node01 \
  --chain ./my-staging-spec-Raw.json \
  --port 30333 \
  --ws-port 9945 \
  --rpc-port 9933 \
  --telemetry-url 'wss://telemetry.polkadot.io/submit/ 0' \
  --validator \
  --rpc-methods Unsafe \
  --name MyNode01
```

```sh
# purge chain (only required for new/modified dev chain spec)
./target/release/node-template purge-chain --base-path /tmp/node02 --chain local -y
```

```sh
# start node02
./target/release/node-template \
  --base-path /tmp/node02 \
  --chain ./my-staging-spec-Raw.json \
  --port 30334 \
  --ws-port 9946 \
  --rpc-port 9934 \
  --telemetry-url 'wss://telemetry.polkadot.io/submit/ 0' \
  --validator \
  --rpc-methods Unsafe \
  --name MyNode02 \
  --bootnodes /ip4/127.0.0.1/tcp/30333/p2p/12D3KooWS1Veq29c5xTFDgYS4GbdV9c7MVBP5nWzaA9rR8aLYapx
  # you MUST fill the correct info in the line above:
  # --bootnodes /ip4/<IP Address>/tcp/<p2p Port>/p2p/<Peer ID>
```

Node 01

```bash
edwins-air: ~/Dev/substrate/Benchmarking-Weights-and-Chainspec-main [git:master] 
🦁 > ./target/debug/node-template   --base-path /tmp/node01   --chain ./my-staging-spec-Raw.json   --port 30333   --ws-port 9945   --rpc-port 9933   --telemetry-url 'wss://telemetry.polkadot.io/submit/ 0'   --validator   --rpc-methods Unsafe   --name MyNode01
2021-09-28 12:03:55 Substrate Node    
2021-09-28 12:03:55 ✌️  version 3.0.0-monthly-2021-09+1-unknown-x86_64-macos    
2021-09-28 12:03:55 ❤️  by Substrate DevHub <https://github.com/substrate-developer-hub>, 2017-2021    
2021-09-28 12:03:55 📋 Chain specification: Local Testnet    
2021-09-28 12:03:55 🏷 Node name: MyNode01    
2021-09-28 12:03:55 👤 Role: AUTHORITY    
2021-09-28 12:03:55 💾 Database: RocksDb at /tmp/node01/chains/local_testnet/db    
2021-09-28 12:03:55 ⛓  Native runtime: node-template-100 (node-template-1.tx1.au1)    
2021-09-28 12:03:56 🔨 Initializing Genesis block/state (state: 0x7ef5…5733, header-hash: 0x08b0…d264)    
2021-09-28 12:03:56 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.    
2021-09-28 12:04:00 ⏱  Loaded block-time = 6s from block 0x08b01156cae5ba1e59161ee07a938e8872031e8a31ff44426293782c3349d264    
2021-09-28 12:04:00 Using default protocol ID "sup" because none is configured in the chain specs    
2021-09-28 12:04:00 🏷 Local node identity is: 12D3KooWSs4if6JyNmGwNsbSXZcCFYLcf1sxtdBAy3PmxJAHknr3    
2021-09-28 12:04:01 📦 Highest known block at #0    
2021-09-28 12:04:01 〽️ Prometheus exporter started at 127.0.0.1:9615    
2021-09-28 12:04:01 Listening for new connections on 127.0.0.1:9945.    
2021-09-28 12:04:06 💤 Idle (0 peers), best: #0 (0x08b0…d264), finalized #0 (0x08b0…d264), ⬇ 0 ⬆ 0    
2021-09-28 12:04:11 💤 Idle (0 peers), best: #0 (0x08b0…d264), finalized #0 (0x08b0…d264), ⬇ 0 ⬆ 0    
2021-09-28 12:04:16 💤 Idle (0 peers), best: #0 (0x08b0…d264), finalized #0 (0x08b0…d264), ⬇ 0 ⬆ 0    
2021-09-28 12:04:21 💤 Idle (0 peers), best: #0 (0x08b0…d264), finalized #0 (0x08b0…d264), ⬇ 0 ⬆ 0
...
2021-09-28 12:08:21 🔍 Discovered new external address for our node: /ip4/127.0.0.1/tcp/30333/p2p/12D3KooWSs4if6JyNmGwNsbSXZcCFYLcf1sxtdBAy3PmxJAHknr3    
2021-09-28 12:08:21 🔍 Discovered new external address for our node: /ip4/192.168.0.107/tcp/30333/p2p/12D3KooWSs4if6JyNmGwNsbSXZcCFYLcf1sxtdBAy3PmxJAHknr3    
2021-09-28 12:08:21 💤 Idle (1 peers), best: #0 (0x08b0…d264), finalized #0 (0x08b0…d264), ⬇ 0.7kiB/s ⬆ 0.7kiB/s    
2021-09-28 12:08:26 💤 Idle (1 peers), best: #0 (0x08b0…d264), finalized #0 (0x08b0…d264), ⬇ 64 B/s ⬆ 52 B/s    
2021-09-28 12:08:31 💤 Idle (1 peers), best: #0 (0x08b0…d264), finalized #0 (0x08b0…d264), ⬇ 32 B/s ⬆ 27 B/s    
2021-09-28 12:08:36 💤 Idle (1 peers), best: #0 (0x08b0…d264), finalized #0 (0x08b0…d264), ⬇ 81 B/s ⬆ 83 B/s    
2021-09-28 12:08:41 💤 Idle (1 peers), best: #0 (0x08b0…d264), finalized #0 (0x08b0…d264), ⬇ 0 ⬆ 0    
2021-09-28 12:08:46 💤 Idle (1 peers), best: #0 (0x08b0…d264), finalized #0 (0x08b0…d264), ⬇ 0 ⬆ 0  
```

Node 02

````bash
edwins-air: ~/Dev/substrate/Benchmarking-Weights-and-Chainspec-main [git:master] 
🐹 > ./target/debug/node-template \
  --base-path /tmp/node02 \
  --chain ./my-staging-spec-Raw.json \
  --port 30334 \
  --ws-port 9946 \
  --rpc-port 9934 \
  --telemetry-url 'wss://telemetry.polkadot.io/submit/ 0' \
  --validator \
  --rpc-methods Unsafe \
  --name MyNode02 \
  --bootnodes /ip4/127.0.0.1/tcp/30333/p2p/12D3KooWSs4if6JyNmGwNsbSXZcCFYLcf1sxtdBAy3PmxJAHknr3
2021-09-28 12:08:15 Substrate Node    
2021-09-28 12:08:15 ✌️  version 3.0.0-monthly-2021-09+1-unknown-x86_64-macos    
2021-09-28 12:08:15 ❤️  by Substrate DevHub <https://github.com/substrate-developer-hub>, 2017-2021    
2021-09-28 12:08:15 📋 Chain specification: Local Testnet    
2021-09-28 12:08:15 🏷 Node name: MyNode02    
2021-09-28 12:08:15 👤 Role: AUTHORITY    
2021-09-28 12:08:15 💾 Database: RocksDb at /tmp/node02/chains/local_testnet/db    
2021-09-28 12:08:15 ⛓  Native runtime: node-template-100 (node-template-1.tx1.au1)    
2021-09-28 12:08:15 🔨 Initializing Genesis block/state (state: 0x7ef5…5733, header-hash: 0x08b0…d264)    
2021-09-28 12:08:15 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.    
2021-09-28 12:08:20 ⏱  Loaded block-time = 6s from block 0x08b01156cae5ba1e59161ee07a938e8872031e8a31ff44426293782c3349d264    
2021-09-28 12:08:20 Using default protocol ID "sup" because none is configured in the chain specs    
2021-09-28 12:08:20 🏷 Local node identity is: 12D3KooWFbW9rDTe635V9w31PKjjjn9Ykrce9j1q29dc2H7hqBpL    
2021-09-28 12:08:20 📦 Highest known block at #0    
2021-09-28 12:08:20 Listening for new connections on 127.0.0.1:9946.    
2021-09-28 12:08:21 🔍 Discovered new external address for our node: /ip4/127.0.0.1/tcp/30334/p2p/12D3KooWFbW9rDTe635V9w31PKjjjn9Ykrce9j1q29dc2H7hqBpL    
2021-09-28 12:08:21 🔍 Discovered new external address for our node: /ip4/192.168.0.107/tcp/30334/p2p/12D3KooWFbW9rDTe635V9w31PKjjjn9Ykrce9j1q29dc2H7hqBpL    
2021-09-28 12:08:25 💤 Idle (1 peers), best: #0 (0x08b0…d264), finalized #0 (0x08b0…d264), ⬇ 0.8kiB/s ⬆ 0.8kiB/s    
2021-09-28 12:08:30 💤 Idle (1 peers), best: #0 (0x08b0…d264), finalized #0 (0x08b0…d264), ⬇ 27 B/s ⬆ 32 B/s    
2021-09-28 12:08:35 💤 Idle (1 peers), best: #0 (0x08b0…d264), finalized #0 (0x08b0…d264), ⬇ 0 ⬆ 0    
2021-09-28 12:08:40 💤 Idle (1 peers), best: #0 (0x08b0…d264), finalized #0 (0x08b0…d264), ⬇ 83 B/s ⬆ 81 B/s
````
