---
id: testnet
title: Testnet Aggron
---

We have launched Aggron Testnet for developers to test their integration and smart contracts in real environment.The Aggron Testnet’s info has been published via Wiki:
[Chains](https://github.com/nervosnetwork/ckb/wiki/Chains) .Please refer it for more details. Because of the hash rate surge, Aggron will reset regularly.

### Run a CKB Testnet node

**System Requirements**

Any modern computer with macOS/Linux/Windows operating system should be able to run a CKB node and mining programs. Alternatively, you can also use [Docker](https://github.com/nervosnetwork/ckb/blob/develop/docs/run-ckb-with-docker.md) if your operating system is not properly supported by CKB for now.

To run a CKB node, please follow the instructions explained in detail below:

**Step 1: Download the latest ckb binary file from the CKB releases page on GitHub (https://github.com/nervosnetwork/ckb/releases), then check if it works with:**

```
ckb --version 
ckb-cli --version
```

<details>
<summary>(click here to view response)</summary>
```bash
ckb 0.31.1 (94bfa16 2020-04-24)
ckb-cli 0.31.0 (a531b3b 2020-04-17)
```
</details>

**Step 2: Connect to Aggron Testnet**

* Init CKB node with `ckb init --chain testnet`

```
ckb init --chain testnet
```

<details>
<summary>(click here to view response)</summary>
```bash
WARN: mining feature is disabled because of lacking the block assembler config options
Initialized CKB directory in /PATH/ckb_v0.31.1_x86_64-apple-darwin
create ckb.toml
create ckb-miner.toml
```
</details>

**Step 3: Start the CKB Testnet node**

```
ckb run
```
<details>
<summary>(click here to view response)</summary>
```bash
2020-04-29 16:57:20.720 +08:00 main INFO sentry  **Notice**: The ckb process will send stack trace to sentry on Rust panics. This is enabled by default before mainnet, which can be opted out by setting the option `dsn` to empty in the config file. The DSN is now https://48c6a88d92e246478e2d53b5917a887c@sentry.io/1422795
2020-04-29 16:57:20.834 +08:00 main INFO main  Miner is disabled, edit ckb.toml to enable it
2020-04-29 16:57:20.842 +08:00 main INFO ckb-db  Initialize a new database
2020-04-29 16:57:20.935 +08:00 main INFO ckb-db  Init database version 20191127135521
2020-04-29 16:57:20.959 +08:00 main INFO ckb-memory-tracker  track current process: unsupported
2020-04-29 16:57:20.961 +08:00 main INFO main  ckb version: 0.31.1 (94bfa16 2020-04-24)
2020-04-29 16:57:20.961 +08:00 main INFO main  chain genesis hash: 0x63547ecf6fc22d1325980c524b268b4a044d49cda3efbd584c0a8c8b9faaf9e1
2020-04-29 16:57:20.965 +08:00 main INFO ckb-network  Generate random key
2020-04-29 16:57:20.965 +08:00 main INFO ckb-network  write random secret key to "/PATH/ckb_v0.31.1_x86_64-apple-darwin/data/network/secret_key"
2020-04-29 16:57:20.986 +08:00 NetworkRuntime- INFO ckb-network  p2p service event: ListenStarted { address: "/ip4/0.0.0.0/tcp/8115" }
2020-04-29 16:57:20.987 +08:00 NetworkRuntime- INFO ckb-network  Listen on address: /ip4/0.0.0.0/tcp/8115/p2p/QmUBgffV1bAJ47zzKY4M7izLi7KmDNd1ejuT5DsCJ1PaZv
2020-04-29 16:57:21.001 +08:00 main INFO ckb-db  Initialize a new database
2020-04-29 16:57:21.030 +08:00 main INFO ckb-db  Init database version 20191201091330
2020-04-29 16:57:21.201 +08:00 NetworkRuntime- INFO ckb-relay  RelayProtocol(1).connected peer=SessionId(1)
2020-04-29 16:57:21.201 +08:00 NetworkRuntime- INFO ckb-sync  SyncProtocol.connected peer=SessionId(1)
2020-04-29 16:57:21.254 +08:00 NetworkRuntime- INFO ckb-sync  Ignoring getheaders from peer=SessionId(1) because node is in initial block download
2020-04-29 16:57:21.834 +08:00 ChainService INFO ckb-chain  block: 1, hash: 0x016da1edf2776ba642be5417f30fbabde296227025cf643bcc65cd55e378178e, epoch: 0(1/1000), total_diff: 0x1800060, txs: 1

```
</details>
