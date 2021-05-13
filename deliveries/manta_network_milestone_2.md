<!-- # Guildlines

> Only the GitHub account, which is responsible for the pull request of the accepted application is allowed to submit milestones. Don't remove any of the mandatory parts presented in bold letters or as headlines!

**The [invoice form :pencil:](https://forms.gle/8Wx7nxtq8fKrsuEz8) has been filled out correctly for this milestone and the delivery is according to the official [milestone delivery guidelines](https://github.com/w3f/General-Grants-Program/blob/master/grants/milestone-deliverables-guidelines.md).**  

* **PR Link:** Please, provide a link to the initial accepted pull request of your application to the [Web3 Foundation Open Grants Program repository](https://github.com/w3f/Open-Grants-Program). 
* **Milestone Number:** The number of the milestone

Please provide a list of all deliverables of the milestone extracted from the initial application and a link to the deliverable itself. Ideally all links inside the below table should include a commit hash, which should be used for testing.

| Number | Deliverable | Link | Notes |
| ------------- | ------------- | ------------- |------------- |
| 1. | ... |...| ...| 
| 2.  | ... |...| ...| 
 -->

* Invoice form has been filled
* Original [pull request](https://github.com/w3f/Open-Grants-Program/pull/117)
* Milestone number 2

> Please provide a list of all deliverables of the milestone extracted from the initial application and a link to the deliverable itself. Ideally all links inside the below table should include a commit hash, which should be used for testing.


| Number | Deliverable | Outcome |
| ------------- | ------------- | ------------- |
| 0a. | License | GPL 3.0 |
| 1. | Manta Substrate Node Prototype | [Manta](https://github.com/Manta-Network/Manta) node and [pallet-manta-pay](https://github.com/Manta-Network/pallet-manta-pay) pallet |
| 2. | Manta Client Prototype | [manta-client](https://github.com/Manta-Network/manta-client) and [manta-subxt](https://github.com/Manta-Network/manta-subxt) | 
| 3. | Benchmark | See [benchmark](#benchmark)   |
| 4. | Docker | A updated [dockerfile](#Docker) is provided |
| 5. | Community & Partner Test | See [Community Engagement](#Community-engagement)  |


__Note__: in milestone 1 we have already shown most of the ZKP functionality of MantaPay (formerly known as manta dap). In 
milestone 2 we focus on the following:
* get the `manta` code production-ready and spin off the testnet
    * In Milestone 1, the server end is build from `2.0.0` version of node template. In Milestone 2, we 
    rebuild the entire node from the bare bones of the substrate, version `3.0.0`.
* build the `client end` 
    * In Milestone 1, the client end is not provided, and the ZKP transactions are hardcoded; in this Milestone deliverable, users can invoke our client end to generate real-time ZKP transactions on the fly

# Milestone 2 Delivery

## Code Organization

* [pallet-manta-pay](https://github.com/Manta-Network/pallet-manta-pay): the implementation of manta DAP protocol. It implement the following functions:
  - `init_asset`: create a public asset (a.k.a. base token), which is public
  - `transfer_asset`: transfer the base token from one polkadot address to another
  - `mint_private_asset`: mint a private token from the public token
  - `private_transfer`: transfer a private token to a new shielded address
  - `reclaim`: reclaim a public token from a private token
* [Manta](https://github.com/Manta-Network/Manta): Manta substrate node that integrate `pallet-manta-pay`. You can start a manta local testnet using this code.
* [manta-front-end](https://github.com/Manta-Network/manta-front-end): A minimal web front-end for Manta that is forked from `substrate-front-end-template`.
* [manta-subxt](https://github.com/Manta-Network/manta-subxt): A `substrate-subxt` based client end that sends and 
rpc to the manta nodes.
* [manta-client](https://github.com/Manta-Network/manta-client): A client end command line interface that generate 
transaction data for the manta node.


## Using Manta

Detailed instructions can be found [here](https://github.com/Manta-Network/Manta/blob/manta/README.md).
Alternative you may access the manta testnet via the [Apps](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fws.f1.testnet.manta.network#/explorer) front-end.


Examples of running the client-end are given [here](https://github.com/Manta-Network/manta-subxt/tree/manta-client/examples).
To run those examples:

1. setup the local nodes
    1. `git clonne https://github.com/Manta-Network/Manta.git`
    2. `cargo build --release`
    3. spawn two nodes with 
    ```
    ./target/release/manta --chain dev --ws-port 9944 --port 30333 --alice --tmp --rpc-cors all --unsafe-ws-external --unsafe-rpc-external --rpc-methods=Unsafe
    ./target/release/manta --chain dev --ws-port 9955 --port 30343 --bob --tmp --rpc-cors all --unsafe-ws-external --unsafe-rpc-external --rpc-methods=Unsafe
    ```
2. build the client
    1. `git clone https://github.com/Manta-Network/manta-subxt.git`
    2. `git checkout manta-client`
    3. `cargo build --release`
3. run the example: `cargo run --example [EXAMPLE] --release` for the following examples:
    * `fetch_all_accounts`: get all the account information from the local Manta node
    * `fetch_remote`: get block hashes from our testnet at `ws.f1.testnet.manta.network`
    * `manta_transfer_ma`: transfer the governance token `$MA`
    * `manta_init_asset`: initializing a customized, public fungible asset
    * `manta_transfer_public_asset`: transfer this public asset from one account to another
    * `manta_mint_asset`: mint the pub asset into a private asset
    * `manta_transfer_private_asset`: transfer the private asset from two old UTXOs to two new UTXOs; 
    requires that the sums of old and new UTXOs match.
    * `manta_reclaim_private_asset`: transfer the private asset from two old UTXOs to one new UTXOs, converting
    the remaining number of private asset to public, and credit to the sender's account.


<!-- ## 

![step3](https://user-images.githubusercontent.com/720571/110532076-3b84a100-80d1-11eb-9c7b-ab7f98350a0b.png)

## Run from docker

1. download and run the manta node:
   1. `docker pull mantalab/manta-node:w3f-milestone-1`
   2. `docker container run -p 9944:9944 mantalab/manta-node:w3f-milestone-1`
2. setup the front end 
    1. `git clone https://github.com/Manta-Network/manta-front-end`
    2. `cd manta-front-end`
    3. `yarn install`
    4. `yarn start` -->
<!-- 
## Test Coverage

Detailed instructions to run test coverage can be found [here](https://github.com/Manta-Network/pallet-manta-pay#test-coverage).

![Result](https://github.com/Manta-Network/pallet-manta-pay/blob/calamari/coverage/coverage.png) -->


## Benchmark

Detailed instructions to run Benchmark can be found [benchmark](https://github.com/Manta-Network/pallet-manta-pay#benchmark).

Summary of Benchmark Result:
| Function      | init_asset |  transfer_asset | mint_asset | private_transfer | reclaim |
| ----------- |:-----------:|:-----------:|:-----------:|:-----------:|:-----------:|
| Rust       |    640 us   |  13 us | 1.9 ms | 10.1 ms | 8.8 ms |
| Wasm |    2.8 ms    |  111  us | 13.1 ms | 130 ms | 107 ms |


## Docker

See [here](https://github.com/Manta-Network/Manta#using-docker) for instructions.

## Community engagement

We have build a large community. See our [telegram](https://t.me/mantanetworkofficial), [medium](https://mantanetwork.medium.com/) and [discord](https://discord.gg/n4QFj4n5vg) channels.
We will engage with the community about our product once we become a parachain.