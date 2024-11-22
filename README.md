# Ethereum Proof-of-Stake 

This repository provides a docker-compose file to run a fully-functional, local development network for Ethereum with proof-of-stake enabled. This configuration uses [Lighthouse](https://github.com/sigp/lighthouse) as a consensus client and [go-ethereum](https://github.com/ethereum/go-ethereum) for execution. **It starts from proof-of-stake** and does not go through the Ethereum merge.

This sets up a single node development network with 64 deterministically-generated validator keys to drive the creation of blocks in an Ethereum proof-of-stake chain. Here's how it works:

1. We initialize a go-ethereum, proof-of-work development node from a genesis config
2. We initialize a Lighthouse beacon chain, proof-of-stake development node from a genesis config

The development net is fully functional and allows for the deployment of smart contracts and all the features that also come with the Prysm consensus client such as its rich set of APIs for retrieving data from the blockchain. This development net is a great way to understand the internals of Ethereum proof-of-stake and to mess around with the different settings that make the system possible.

## Using

First, install Docker. Then, run:

```
git clone https://github.com/thaichain/eth-pos/ && cd eth-pos
./generate_genesis_data.sh
docker compose up -d
```

You will see the following:

```
$ docker compose up -d
[+] Running 5/5
 ✔ Network tch-pos_default        Created 
 ✔ Container tch-pos-init-geth-1  Started 
 ✔ Container tch-pos-geth-1       Started 
 ✔ Container tch-pos-beacon-1     Started 
 ✔ Container tch-pos-validator-1  Started 

```

Each time you restart, you can wipe the old data using `./clean.sh`.

Next, you can inspect the logs of the different services launched. 

```
 docker compose logs -f --tail 30 geth
```

and see:

```
geth-1  | INFO [11-22|14:37:38.006] Starting work on payload                 id=0x033ba9e8a8188679
geth-1  | INFO [11-22|14:37:38.006] Updated payload                          id=0x033ba9e8a8188679 number=1 hash=0ddc82..58e984 txs=0 withdrawals=0 gas=0 fees=0 root=777f07..351570 elapsed="183.32µs"
geth-1  | INFO [11-22|14:37:38.006] Stopping work on payload                 id=0x033ba9e8a8188679 reason=delivery
geth-1  | INFO [11-22|14:37:38.035] Imported new potential chain segment     number=1 hash=0ddc82..58e984 blocks=1 txs=0 mgas=0.000 elapsed="405.87µs"  mgasps=0.000 snapdiffs=249.00B triediffs=2.16KiB triedirty=0.00B
geth-1  | INFO [11-22|14:37:38.036] Chain head was updated                   number=1 hash=0ddc82..58e984 root=777f07..351570 elapsed="35.13µs"
geth-1  | ERROR[11-22|14:37:38.037] Nil finalized block cannot evict old blobs
geth-1  | INFO [11-22|14:37:38.037] Indexed transactions                     blocks=2 txs=0 tail=0 elapsed="162.15µs"
geth-1  | INFO [11-22|14:37:46.003] Starting work on payload                 id=0x03a52de39ca72b52
geth-1  | INFO [11-22|14:37:46.003] Updated payload                          id=0x03a52de39ca72b52 number=2 hash=7309e7..8fd40e txs=0 withdrawals=0 gas=0 fees=0 root=92f131..a92993 elapsed="320.5µs"
geth-1  | INFO [11-22|14:37:50.002] Stopping work on payload                 id=0x03a52de39ca72b52 reason=delivery
geth-1  | INFO [11-22|14:37:50.045] Imported new potential chain segment     number=2 hash=7309e7..8fd40e blocks=1 txs=0 mgas=0.000 elapsed="654.96µs"  mgasps=0.000 snapdiffs=498.00B triediffs=4.43KiB triedirty=0.00B
geth-1  | INFO [11-22|14:37:50.051] Chain head was updated                   number=2 hash=7309e7..8fd40e root=92f131..a92993 elapsed="71.72µs"
```

# Available Features

- Starts from the deneb Ethereum hard fork
- The network launches with a [Validator Deposit Contract](https://github.com/ethereum/consensus-specs/blob/dev/solidity_deposit_contract/deposit_contract.sol) deployed at address `0x5454545454545454545454545454545454545454`. This can be used to onboard new validators into the network by depositing 32 ETH into the contract
- The default account used in the go-ethereum node is address `0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266` which comes seeded with ETH for use in the network. This can be used to send transactions, deploy contracts, and more
- The default account, `0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266` is also set as the fee recipient for transaction fees proposed validators in Lighthouse. This address will be receiving the fees of all proposer activity
- The go-ethereum JSON-RPC API is available at http://geth:9545
- The Lighthouse client also exposes a gRPC API at http://beacon:5052
![Screenshot 2567-11-22 at 21 42 10](https://github.com/user-attachments/assets/46dd4a21-d51d-4c62-87e9-88e22cac1b77)


