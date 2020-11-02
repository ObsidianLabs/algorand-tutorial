# Tutorial Two

## Intro

In this tutorial, we will talk about Algorand asset model through examples on LimitOrder. You will have the chance to create a LimitOrder-based project, and go through a full ASA lifecycle by compiling and then calling transactions defined in the project.

## Algorand Standard Asset (ASA)

Algorand Standard Asset is an Algorand built-in Standard Asset model, including both Fungible and Non-fungible types of assets.

For full information, please refer to

https://developer.algorand.org/docs/features/asa

### Construct ASA transaction

Open the Dynamic Fee project from Tutorial One, you may find 3 ASA-related files under `tests` folder.

- `4.asset_create.json`
- `5.asset_transfer.json`
- `6.asset_destory.json`

A complete ASA transction usually contains theses elements in its lifecycle.

- Create: Create ASA
- Opt in: Register ASA recipients
- Transfer: the Transfer Transaction
- Other: freeze, revoke, clawback, destroy

For more ASA transaction classification, please refer to

https://developer.algorand.org/docs/features/asa/#asset-functions

## ASC Example: LimitOrder

LimitOrder is a ASA and ALGO transaction contract, which guarantees exchange between asset and ALGO by setting a specified exchange rate.

https://developer.algorand.org/docs/reference/teal/templates/limit_ordera

### Create Project

Create a new Project in Algorand Studio. Select *Limit Order* template, put in project name and click *Create Project* button.

<p align="center">
  <img src="./screenshots/create_limit_order.png" width="720px">
</p>

The project contains `contract.teal`, `config.json` file as well as a `tests` folder.

`contract.teal` contains all logics to execute LimitOrder contract, which was written in TEAL.

<p align="center">
  <img src="./screenshots/limit_order.png" width="300px">
</p>

Navigate to `tests` folder, you shall find 5 files named as below.

- `0.create_asset.json`: Bob creates a new asset of 1000 units.
- `1.opt_in.json`: Registers Alice as Asset Recipient through Opt-in
- `2.deposit.json`: Alice deposits 10 ALGO into the contract
- `3.exchange.json`: Bob exchanges 10 unites of asset for 1 ALGO in the contract.
- `4.close.json`: Returns remaining ALGO to Alice.

### Contract Code

Full overview of code can be found below:

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
