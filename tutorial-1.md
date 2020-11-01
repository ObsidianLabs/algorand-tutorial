# Tutorial One

## Intro

This is the first article of our Algorand Development tutorial series. We will introduce basics and preparation of Algorand Studio, then illustrate a complete Algorand development process with the [Dynamic Fee](#contract-code) smart contract.

## Preparation Work

### Install Algorand Studio and dependencies

You may download the latest build of Algorand Studio from the [Github repo](https://github.com/ObsidianLabs/AlgorandStudio). Algorand Studio currently supports macOS and Linux operating systems while we're working hard to extend it to other platforms.

Download, install and run Algorand Studio on your device. If everything works as it should be, you will be brought to a welcome screen where you shall see a checklist of prerequisites for Algorand Studio. It requires Docker runtime, Algorand Node and PyTeal Compiler to properly function. Click the button at the end of each line to complete installations as per instruction.

<p align="center">
  <img src="./screenshots/welcome.png" width="720px">
</p>

When all the three tools are installed, click the *Get Strated* button and enter Algorand Studio.

### Start Algorand node and connect to the testnet

Click the *Network* button in the header and switch to the network manager. Then click the *New Instance* button in the upper right corner to create a new Algorand node instance. Algorand Studio will automatically download the latest snapshot to save time of synchronizing data with the network. 

<p align="center">
  <img src="./screenshots/create_node.png" width="720px">
</p>

When the snapshot is fully downloaded, an Algorand node instance will show up in the instance list. Click *Start* to start the Algorand node. Notice that Algorand Studio only supports the testnet at this moment.

### Create Algorand keypairs

We also need to create some keypairs for the following tasks such as making transfers and execute the smart contract.

Click the key icon on the bottom left corner of Algorand Studio to open the *Keypair Manager*, then click *Create* button. A popup window will guide you through the process. Put in the name of the keypair, click the *Save* button and you will see the new address you just created.

A keypair is composed of a public key and a private key. Private keys in Algorand Studio are stored in the form of [mnemonic](https://developer.algorand.org/docs/features/accounts/#transformation-private-key-to-25-word-mnemonic). You may export mnemonic phrases by clicking the eye icon at the end of each address, while a popup window will show the address and associated mnemonic.

<p align="center">
  <img src="./screenshots/keypair_manager.png" width="720px">
</p>

In this tutorial, you need to create three keypairs with names *Alice*, *Bob* and *Charlie*. We will use their names to reference the three keypairs in this tutorial.

## Basic Operations

### Using the Explorer

Click the *Account* button in the header to open the *Account Explorer* and paste Alice's address in the address input. You should see Alice's ALGO balance on the left and every transactions on the bottom relating to the her address.

<p align="center">
  <img src="./screenshots/explorer.png" width="720px">
</p>

Different from the screenshot, you may see an empty transaction list since you just created the account. Keep going in this tutorial and you will find how the two transactions come up.

### Claim test tokens using the faucet

In the world of blockchain, people usually refer the tool for claiming test tokens as *faucet*. Click the faucet button next to the address input, a window for *Algorand Dispenser* will pop up. Enter Alice's address and wait some time for the network to process the transaction. Later, you will see her balance go up by 100 ALGO where a new transfer transaction is recorded in the transaction list.

<p align="center">
  <img src="./screenshots/faucet.png" width="720px">
</p>

### Transfer

To start a transfer, open a new tab and type in Bob's address into the address input. Press ENTER and you'll see Bob's current account balance which is 0 ALGO.

Switch back to Alice's tab and press the *Transfer* button next to the address input. Type in 20 as the amount to transfer and the recipient's address, which in our case the address of Bob. Click *Sign and Send* to initiate the transaction.

<p align="center">
  <img src="./screenshots/transfer.png" width="720px">
</p>

Refresh both Alice and Bob's pages after transaction completes and you will see their balances updated to around 80 ALGO for Alice and 20 ALGO for Bob. The discrepency of Alice's amount is the transaction fee deducted from her account as she is the sender of the transaction.

## Algorand smart contract (ASC)

There are two types of smart contracts in Algorand - stateful and stateless. We will mainly talk about stateless contracts in this tutorial. For more info, please refer to the [docs](https://developer.algorand.org/docs/features/asc1/).

### Create new project (Dynamic Fee)

Switch to *Project* in the header to open the project list page. Click the *New* button, put in a project name and select a template you wish to use for the new project. Let's pick *Dynamic Fee* now. Then press *Create Project* to complete this process.

<p align="center">
  <img src="./screenshots/create_project.png" width="720px">
</p>

The Dynamic Fee smart contract allows third-party payment for the transaction fee. The fee in the earlier transfer transaction was paid by Alice. Using the Dynamic Fee contract you can specifiy an external account for paying the fee. The reference for the code walkthrough can be found [here](https://developer.algorand.org/docs/reference/teal/templates/dynamic_fee).

### PyTeal vs TEAL

TEAL (Transaction Execution Approval Language) is the language for ASC development. You may find full description of the language in the [docs](https://developer.algorand.org/docs/reference/teal/specification).

TEAL is a low-level assemply-like language, so Algorand also developed PyTeal which allows developers to write Algorand smart contract using Python syntax. PyTeal serves to inteprete the code to TEAL and then compiles it to binary. You may find more information also in the [official docs](https://developer.algorand.org/docs/features/asc1/teal/pyteal).

### Compilation

Before you compile the Dynamic Fee project, we need to modify a line in the `main.py` file. Put in Charlie's address on Line 5.

<p align="center">
  <img src="./screenshots/main.png" width="720px">
</p>

You may notice there're two hammer buttons at the bottom of the editor window. They represent PyTeal compiler and TEAL compiler. Algorand Studio utilizes both to compile our smart contract. You should already installed both compilers in the Welcome screen. Otherwise, click the buttons and open their version managers to finish the installation.

Press the hammer button in the toolbar (at the top of the file tree) to initiate the compilation process. PyTeal compiler will interpret and convert the `.py` file to `.teal` format. TEAL compiler will further compile it into a `.tok` binary and a `.addr` file that contains the address of the contract. In Algorand it doesn't require deployment right after compilation. You can directly deposit tokens to the smart contract using the address in the `.addr` file as the recipient. The `.tok` binary is only needed when you want to execute the contract program, for example in a withdrawal transaction.

## Construct Algorand transactions

Before going furhter, let's talk about how to contruct a general transaction in Algorand Studio.

Algorand Studio defines transactions through JSON files. You may find some examples in the `test` folder under the project, each reflecting a specific type of transaction.

<p align="center">
  <img src="./screenshots/tests.png" width="300px">
</p>

### Regular transactions

#### Transfer (pay)

Open `1.pay.json`, you shall see all necessary information that defines a basic transfer transaction. It includes information such as the type (`pay`), the sender (`from`), the recipient (`to`), transaction amount, the signer alongside others. Here we defined a transfer of 10 ALGO from Alice to Bob and the transaction is digitally signed by Alice. Notice that you can use names stored in the *Keypair Manager* for addresses as they will be subsititued by Algorand Studio before the final execution.

Click the test-tube button in the toolbar, choose `1.pay.json` in the popup window. Then click *Run Test Transaction* and wait for the transaction to complete.

<p align="center">
  <img src="./screenshots/run_tests.png" width="720px">
</p>

Then switch to the *Explorer* and refresh Alice and Bob's pages, we will see there are another 10 ALGO transfered between them and a new transaction record showed in the transaction list.

#### Atomic transfer

Atomic transfer makes sure multiple operations either all succeed or all fail. Open `2.atomic_transfer.json` to see an example. The whole transaction object consists a `txns` array field where you can give multiple transaction parts. Different parts can have diverse transaction types, contents, and signers. In the example given we constructed an atomic transfer that made sure Alice send Bob 1 ALGO at the same time Bob send Charlie 1 ALGO. This structure is crucial when we contruct a transaction to use the Dynamic Fee smart contract.

For more information about atomic transfer, please refer the [official docs](https://developer.algorand.org/docs/features/atomic_transfers).

#### Multisig

You can also construct multisig transactions in Algorand Studio. See `3.multisig.json` for example and refer to the [docs](https://developer.algorand.org/docs/features/accounts/create/#multisignature) for more information.

### Smart contract transactions

#### Modes of use

Stateless smart contracts are commonly used in [two scenarios](https://developer.algorand.org/docs/features/asc1/stateless/modes/):

- Contract Account: the smart contract is directly involved in the transaction, or specifically, it will be the sender since recipients don't need to do anything;
- Delegated Approval: the smart contract will act as a third-party to verify transactions between other accounts.

#### Call the Dynamic Fee contract

Since in the case of Dynamic Fee, the smart contract will only verify the transactions between other accounts - the sender, the recipient and a third one that is going to pay the fee for the sender - we are using the *delegated approval* here. Open `8.contract_delegrated.json` file which contains an example. The first transaction is a transfer of 0.001 ALGO from Bob (fee payer) to Alice (sender), and the second one is a transfer of 1 ALGO from Alice to Charlie (receiver). Be aware that the sample code has prescribed the sender has to send *1 ALGO* to *Charlie* as the receiver and the fee payer has to send *the same amount of ALGO as the fee* to the *sender*. If we logically combine two transactions into one, it could be intepreted as a transfer transaction of 1 ALGO from Alice to Charlie while Bob pays for the transaction fee of 0.001 ALGO and Alice doesn't pay a fee as a consequence.

You may find the second transaction is not signed by Alice. In fact, no `signers` is given here. However, we have provided the `lsig` which stands for [logic signature](https://developer.algorand.org/docs/features/asc1/stateless/modes/#logic-signatures), the method asking a smart contract to verify the transaction. In our example it contains the raw data of the smart contract code (remember we said before the code is only needed in execution), and Alice's signature to make sure she agrees to withdraw from her account.

Press the test-tube button in the toolbar, choose `8.contract_delegated.json` and click *Run Test Transaction* button. Then wait for a while allowing transaction to complete. Once it completes, move to the explorer and refresh Alice's, Bob's and Charlie's pages respectively. You shall see Alice's balance decreased by 1 ALGO, Bob's decreased by 0.002 ALGO (since he paid fees for both Alice's transfer to Charlie as well as his to Alice), and Charlie's increased by 1 ALGO. In Alice's transaction history you will also see the consequence with one outbound transfer of 1 ALGO to Charlie and 0.001 ALGO inbound transfer from Bob.

You can generate the same group of transactions using atomic transfer mentioned above and have Alice and Bob signing their transactions respectively, but it doesn't gurantee that the combination will act as planned because someone may cheat in the process. For example, the fee payer may pay less than the fee being charged, or the sender doesn't make a transfer to the receiver at all. In the blockchain world, we should act in a *trustless* way - we should not trust any other prople in the system. The Dynamic Fee smart contract can make sure everything goes as planed by codes so no one can manipulate it. That's why we need to use a smart contract to very such transactions.

## Next

Next we will talk about Algorand asset model through examples on LimitOrder in [Tutorial 2](https://github.com/ObsidianLabs/algorand-dapp-tutorial/blob/master/tutorial-2.md)
