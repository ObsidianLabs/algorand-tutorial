# [Algorand Studio Tutorial 3] Smart Contract Limit Order

In this tutorial, we will talk about the [Algorand Standard Asset](https://developer.algorand.org/docs/features/asa) (ASA) model and work through the Limit Order smart contract. You will learn

- The asset model of Algorand
- Issuing your own asset
- Use the Limit Order smart contract to implement a simple exchange feature

## Algorand Standard Asset

Algorand Standard Asset is an Algorand built-in features to issue customized tokens, including both fungible and non-fungible types of assets.

### ASA transactions

Create a new project from the Limit Order template. Put in project name and click *Create Project* button.

<p align="center">
  <img src="./screenshots/create_limit_order.png" width="720px">
</p>

You will find some ASA related transactions under the `tests` folder.

- `0.create_asset.json`
- `1.opt_in.json`
- `3.exchange.json` (the second item in the `txns` array)

A complete lifecycle for an ASA usually contains

- Create: create the ASA and give the asset parameters
- Opt in: register to allow receiving the ASA
- Transfer: transfer the ASA between accounts
- Other: freeze, revoke, clawback, and destroy

For more ASA transaction types, please refer to [the docs](https://developer.algorand.org/docs/features/asa/#asset-functions)

### Creat an ASA

Open the file `0.create_asset.json`. This is a transaction that creates a new asset named `My Token` for Bob. This asset has `MT` as its symbol and the total amount is `1000 MT`. Click the test-tube button on the toolbar and run this transaction. Once the transaction completes, switch to the explorer and open Bob's page, you shall see `My Token` is created and `1000 MT` is issued to Bob's account. Take a note for the asset ID which we will use later.

### Opt in

The file `1.opt_in.json` described a transaction which is initiated by Alice to allow receiving an asset. Copy and paste the asset ID we just created into the file, and run this transaction. You shall see in Alice's page in the explorer that she also has `0 MT` added to her asset list. From now on, other people can send `My Token` asset to Alice.

### Transfer

The project doesn't provide a separate asset transfer transaction. However, you can look at the second part in `3.exchange.json` for an example. You can run it individually by creating a new JSON object. We leave this as an exercise.

## Limit Order

[Limit Order](https://developer.algorand.org/docs/reference/teal/templates/limit_ordera) is a smart contract to exchange ALGO for an ASA, which guarantees the exchange rate between ALGO and the asset.

Limit Order is written in native TEAL language. The project contains the source code `contract.teal`, a config file `config.json` as well as example transactions in the `tests` folder. 

<p align="center">
  <img src="./screenshots/limit_order.png" width="300px">
</p>

The five files in `tests` are actually the steps to use Limit Order. We justed executed the first two transactions. The following three are all related to the smart contract

- `2.deposit.json`: Alice deposits 10 ALGO into the smart contract;
- `3.exchange.json`: Bob exchanges 10 unites of `My Asset` for 1 ALGO;
- `4.close.json`: Return the remaining ALGO to Alice.

https://developer.algorand.org/docs/reference/teal/templates/limit_ordera/#code-overview

### Compile Contract

Before you compile the contract, we need you to modify a few lines in to contract code.

First, click *Keypair Manager* icon on the bottom of left corner, copy Alice's address. Then open `contract.teal` file, and navigate to Line 38 and Line 72, respectively. Overwrite both addresses with Alice's address as they specify Alice as the only recipient allowed in this contract.

<p align="center">
  <img src="./screenshots/replace_address.png" width="720px">
</p>

Click hammer button to start compilation. Upon completion, you will find two outputs, `contract.teal.tok` and `contract.addr` created under project folder.

### Call LimitOrder Contract

Once compilation completes, you shall start to make use of LimitOrder contract.

In this tutorial, we're showcasing a lifecycle in the following steps, Bob created a new asset, and exchange this new asset for ALGO at a specified rate from Alice.

Following achieves a full LimitOrder call lifecycle through multiple step-by-step json file call.

1. Create a new asset named My Token for Bob. Inititate 1000 units and takes MT as its symbol. Click Tube button on the toolkit bar, select `0.create_asset.json` and click *Run Test Transaction* button. You shall see 1000 MT created in Bob's asset through Explorer once the transaction completes.
2. Alice adds Bob into its whilelist. Click Tube button on the toolkit bar, select `1.opt_in.json` and click *Run Test Transaction* button, then you shall see 0 MT asset added into Alice's asset through the Explorer.
3. Alice deposits 10 ALGO for exchange transaction with asset. Click Tube button on the toolkit bar, select `2.deposit.json` and click *Run Test Transaction* button, then you shall see 10 ALGO deducted from Alice's.
4. Bob exchanges 10 asset for Alice's 1 ALGO. Click Tube button on the toolkit bar, select `3.exchange.json` and click *Run Test Transaction* button, then you shall see 10 MT deducted while 1 ALGO added in Bob's account.
5. Alice withdrawed 9 remaining ALGO from the contract. Click Tube button on the toolkit bar, select `4.close.json` and click *Run Test Transaction* button, then you shall see 9 ALGO added on Alice's balance.
