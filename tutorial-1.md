# 教程一
# Tutorial One

## 简介
## Intro

本篇为第一篇教程，将简单介绍如何安装使用 Alogrand Studio，然后通过 DynamicFee 项目来展示 Alogrand 开发的完整流程。

## 准备工作
## Preparation Work

### 安装Algorand Studio 
### Installing Algorand Studio

请在 GitHub [下载页面](https://github.com/ObsidianLabs/AlgorandStudio)下载 Algorand Studio。目前 Algorand Studio 支持 macOS 和 Linux 系统，请根据系统下载对应的版本。

You may download latest build of Algorand Studio from our Github release page [Link](https://github.com/ObsidianLabs/AlgorandStudio). Algorand Studio currently supports macOS and Linux operating systems while we're working hard to extend it to other platforms. 

正确安装 Algorand Studio Algorand Studio 将显示欢迎页面，根据提示完成 Docker, Algorand Node 以及 PyTeal Compiler 的下载、安装及启动。

Download, install and run Algorand Studio on your device, if everything works as it should be, you will be brought to a welcome screen where you shall see a checklist of prerequisites for Algorand Studio. Algorand Studio requires Docker runtime, Algorand Node and PyTeal Compiler to proper function, therefore we put it here to simplify your preparation work. Click the button at the end of each of the items and complete installation as per instruction. 

<p align="center">
  <img src="./screenshots/welcome.png" width="720px">
</p>

### 启动 Algorand node 并连接到测试网
### Start Algorand Node and connect to the test network

Algorand Studio 在创建 algorand node 的时候会帮助用户自动下载 snapshot，用于快速更新之前的数据。
Algorand Studio automatically downloads snapshot when you creates a new algorand node. 

<p align="center">
  <img src="./screenshots/create_node.png" width="720px">
</p>

完成 snapshot 的下载后 algorand node 实例将出现在列表中，点击 start 就可以启动 algorand node。
When snapshot is fully downloaded, an algorand node will show up on the list. Click start to start the algorand node.

目前 algorand studio 仅支持测试网。
Algorand Studio only supports test network at this moment.  


### 创建Algorand地址
### Create Algorand address

完成 Algorand node 的启动后，首先需要创建钥匙对来完成后续的合约部署以及调用。
After you successfully start up Algorand node, you need to create a keypair to support contract deployment and contract call.

有别于一般的密钥对，Algorand 密钥对中的私钥是以 mnemonic （助记词）的形式储存的。
Unlike ordinary keypairs, the private keys in Algorand keypairs are stored in the form of mnemonic.

在 Algorand Studio 的任意界面，点击应用左下⻆的钥匙图标，打开密钥管理器。点击 Create 按钮打开新钥匙对弹窗，输入钥匙对的名字并点击 Save 按钮。完成后将在密钥管理器中看到刚刚生成的钥匙对的地址。钥匙对由私钥和公钥组成，公钥在智能合约中也常被称作地址。
Click the key icon on the bottom left corner on Algorand Studio to open Keypair Manager, then click Create button. A popup window will guide you through the process by putting in the name of the keypair then click Save button. Once it completes, you shall see the keypair addresses we you just created in the Manager, A keypair are composed of public key and private key, while the public key is also commonly named the address in smart contracts.

导出私钥可以通过点击每个地址后面的眼睛按钮打开查看私钥弹窗，弹窗显示地址以及私钥。后续教程中会需要通过管理器导出私钥。
You may also export private keys by clicking the eye icon at the end of each address, when a popup window showing address and private key will guide you through the export process. We will showcase this process later in this tutorial.

<p align="center">
  <img src="./screenshots/keypair_manager.png" width="720px">
</p>

为了顺利完成接下来教程，首先需要创建三个钥匙对并命名为：Alice，Bob 和 Charlie
In this tutorial, you need to create three keypairs after names, Alice, Bob and Charlie. Alice, Bob and Charlie will be referenced wherever we need a keypair in this tutorial.


## 基础操作
## the Basics

### 使用浏览器
### Using the Explorer

点击顶部 Account 标签打开账户浏览器，并在地址栏粘贴 Alice 的钥匙对地址，可以在左边看到当前地址的 ALGO 余额信息。下方可以看到每一笔交易的信息。
Click the Account Label on the top to open Account Explorer and paste Alice's keypair address in the address column. You should see its ALGO balance on the left and every transaction on the bottom relating to the specified address.

<p align="center">
  <img src="./screenshots/explorer.png" width="720px">
</p>


### 通过Faucet申请token
### Application of Token through Faucet

在区块链的世界中，大家通常将申请测试 Token 的方式称为 faucet，目前在测试网络下每次 faucet 申请到的 Token 为 100 ALGO。
In the world of Blockchain, people usually refer the method to apply for test token as Faucet. Currently, you will be granted 100 ALGO each time when you faucet for test tokens.

点击地址栏旁的水龙头按钮，将弹出一个 Algorand dispenser 页面，在页面中申请 Token。
Click the faucet button next to the address column, a window named Algorand Dispenser will pop up where you can apply for test token.

<p align="center">
  <img src="./screenshots/faucet.png" width="720px">
</p>

完成申请后稍等片刻，刷新页面后可以看到 Alice 的余额更新为 100 ALGO，并且下方 Transactions 列表中多了一条转账记录。
Minutes following your applcation, you will see Alice's balance go up by 100 ALGO where a new transfer transaction is recorded in the Transactions List.


### 转账
### Transfer

开始转账前我们新增一个 tab，并将 Bob 的地址粘贴到地址栏，回车后可以看到 Bob 的账户余额信息为 0 ALGO。
To start a transfer, you need to create a new tab, and type in Bob's address into the address column. Press ENTER and you'll see Bob's current account balance, which is 0 ALGO.

点击地址栏旁的 Transfer 按钮，输入转账总数和收款人 recipient 地址，这里填 Bob 的地址，点击 Sign and Send。
Then press Transfer button next to the address column. Type in the amount to be transferred and the recipient's address, which in our case, is the address of Bob. Click Sign and Send to initiate the transaction.

<p align="center">
  <img src="./screenshots/transfer.png" width="720px">
</p>

完成后再次刷新页面，Alice 的余额变为 79.753 ALGO，Bob 的余额为 20 ALGO，这是因为除了给 Bob 转出的 20 ALGO 外，还有从 Alice 账户中扣除的转账手续费。
Refresh the page after transaction completes, you'll see Alice's balance updated to 79.753 ALGO while Bob's is 20 ALGO. The 0.753 ALGO discrepency here is the transaction fee deducted from Alice account (aka. the Sender).


## Algorand smart contract (ASC)

TEAL是原生写 algorand 智能合约语言，它是一种类似 Assembly 的语言，由 Algorand 官方开发。
TEAL is a native ASC stack language developed by Algorand Foundation. It's similar to Assembly language in some sense. You may find full description of the language in its official documentation below.

https://developer.algorand.org/docs/reference/teal/specification/


### 创建项目（模板DynamicFee）
### Create New Project (from Dynamic Fee template)

点击顶部 Project 标签，点击 New 按钮，填入项目名称和模版，这里选择 Dynamic fee 模版，点击 Create 按钮完成项目的创建。
Switch to Project label from the top, click New button and put in project name and the template name you wish to create it from. Let's pick Dynamic Fee as our template here and press Create to complete the project creation process.

<p align="center">
  <img src="./screenshots/create_project.png" width="720px">
</p>

### PyTeal vs TEAL

使用 TEAL 编写代码并不方便，为此 Alogrand 开发了 PyTeal。Pyteal 是通过 python 语法来编写代码，然后通过 Pyteal 编译器编译成 teal，然后编译成二进制。

Since TEAL is not a programmer-friendly language, Algorand developed PyTeal which allows developers to write Algorand smart contract with Python syntax. PyTeal serves to inteprete these code to Teal then further compiles it to binary. You may find more information below if you're interested.

https://developer.algorand.org/docs/features/asc1/teal/pyteal/


### 编译合约
### Compile Smart Contract

在编译 Dynamic fee 前，我们还需要修改一下 main.py 的代码，在第 5 行代码中填入接受转账的账户地址。
Before you compile our Dynamic Fee project, we need to modify a few lines in our main.py file. Put in the recipient's address on Line 5.

在编辑器底部有两个锤子按钮，后面跟着版本号，这两个分别是 Pyteal 的编译器和 Teal 的编译器。Algorand Studio会使用这两个编译器一起进行编译。
You may notice there're two hammer buttons at the bottom of the editor window. These represent PyTeal compiler and Teal compiler. Algorand Studio utilizes both to compile our smart contract.

<p align="center">
  <img src="./screenshots/main.png" width="720px">
</p>

其中 pyteal 编译器负责将 .py 文件编译成 .teal 文件，Teal 编译器负责将 .teal 文件编译成 .tok 和 .addr 文件
Precisely, PyTeal compiler intepretes .py file into .teal file where Teal compiler further compiles the file into .tok and .addr file.

.addr 是合约的地址，相当于合约代码的hash。
.addr contains the address of the contract, or in other words, hash of the smart contract code.

点击工具栏的锤子按钮进行编译，编译完成后的合约不需要部署，只需要在调用时附加合约代码(.tok)，通过地址即可检查代码是否真实。
Press the hammer button to initiate compilation process. The binary doesn't require deployment right after compilation as the code is verified by the address when you have the code calling the .tok file during smart contract execution.


### 合约代码
### Contract Code

Dynamic fee 合约实现了一个代付交易费的功能。上文中的转账交易费是由 Alice 来支付的，如果使用 Dynamic fee 合约来进行转账，是可以由别的账户来支付这笔交易费。
Dynamic Fee contract allows third-party payment for transaction fee. The fee in the example transaction earlier was paid for by Alice. Using Dynamic Fee contract to make transaction allows you to specifiy if you wish an external account to pay for this fee.

合约代码具体解析参见
Full reference documentation on contract code can be found below.
https://developer.algorand.org/docs/reference/teal/templates/dynamic_fee/


## 构造交易 & 调用合约
## Construct Transaction & Call Contract

Alogrand Studio 通过 json 文件来定义 transaction。
Algorand Studio defines transaction through json files.

在项目下的 test 文件夹中可以找到 8 个 json 文件，分别对应着 8 种不同类型的交易。
You may find eight json files in test folder under the project, each reflects to a specific type of transaction.

<p align="center">
  <img src="./screenshots/tests.png" width="300px">
</p>

### 构造普通交易
### Construct regular transaction

#### 普通转账 pay
#### Regular transfer pay

我们将展示如何使用 json 文件来进行转账交易。
We'll guide you through the process of making transfers, using json files.

打开 test 文件夹下的 pay.json 文件，从这个文件中可以看到定义这笔交易的所有信息，其中包括了方法名称 pay，支付地址，接受地址，支付金额等信息。其中账户地址可以直接使用 keypair manager 中的名字，Alogrand Studio 将使用名字进行索引替换为地址。
Open pay.json file in the test folder, you shall see all necessary information that defines this specific transaction. It includes information like, the method (pay), sender address, recipient address, transaction amount, alongside other information. You can use the names in the keypair manager when writing addresses, as they will be subsititued by Algorand Studio to the actual addresses before compilation.

从 1.pay.json 文件中可以看出，这笔交易的定义的是从 Alice 转账给 Bob 10个 ALGO，并使用 Alice 进行签名。
If you take a close look at 1.pay.json, you may see the transaction is defined as a transfer of 10 ALGO from Alice to Bob and the transaction is digitally signed by Alice.

点击试管按钮，在弹出的窗口中选择 1.pay.json，然后点击 Run Test Transaction 按钮，稍等片刻等待交易完成。
Click the tube button, choose 1.pay.json in the popup window. Then click Run Test Transaction and wait for the transaction to complete.

<p align="center">
  <img src="./screenshots/run_tests.png" width="720px">
</p>

交易完成后切换至区块浏览器并刷新 Alice 页面，可以看到余额减少了 10+ ALGO，下方的交易记录中也多出了刚刚的那笔转账记录。
Then switch to Block Explorer and refresh Alice's page, we will see 10+ ALGO was deducted and a new transaction record showed up in the bottom part where shows all transaction history.


#### atomic transfer
atomic transfer 保证多笔交易同时成功和同时失败。
Atomic Transfer makes sure multiple transactions either all succeed or all fail.
https://developer.algorand.org/docs/features/atomic_transfers


#### multisig
multisig 支持多个账户给这笔交易签名。
multisig allows a transaction signed by a ordered set of addresses.
https://developer.algorand.org/docs/features/accounts/create/#multisignature


### 调用合约
### Call Contracts

Algorand 的智能合约有 stateful 和 stateless 两种类型，本教程使用的是 stateless 类型的智能合约。
There are two types of Algorand Smart Contract, stateful and stateless. In this tutorial, all contracts are deemed stateless.

更多 stateless 相关的合约请查看：
For more information on stateless contracts, please refer to 
https://developer.algorand.org/docs/features/asc1/stateless


#### Contract Account vs Delegated Approval
Stateless Smart Contracts 使用 LogicSig 作为合约签名的方式。通常 Stateless Smart Contracts 有两种使用场景，分别为 Contract Account 和 Delegated Approval
Stateless Smart Contracts signs contracts using LogicSig method. Stateless Smart Contracts are commonly used in two scnearios, Contract Account and Delegated Approval.

- Contract Account 直接从合约取钱
- Contract Account Direct Debit from a Contract
- Delegated Approval 合约作为第三方来检查交易
- Delegated Approval Contract works as third-party to verify transaction.

更多 Contract Account 和 Delegated Approval 相关的信息请查看：
For more information on Contract Account and Delegated Approval, please refer to
https://developer.algorand.org/docs/features/asc1/stateless/modes


#### 调用Dynamic Fee合约
#### Call Dynamic Fee contract

打开 8.contract_delegated.json 文件，该 json 文件描述了两笔交易，第一笔是 Bob 给 Alice 转的 0.001 ALGO，第二笔是 Alice 给 Charlie 转的 1 ALGO。
Open 8.contract_delegrated.json file, which contains description of two transactions. The first transaction is a transfer of 0.001 ALGO from Bob to Alice, then the second is a transfer of 1 ALGO from Alice to Charlie.

这两笔交易合并起来看是 Alice 给 Charlie 转了 1 ALGO，而 Bob 为这笔交易支付了 0.001 ALGO 的转账手续费，结果就是 Alice 不需要为这笔转账支付手续费。

If we logically combines two transaction into one, it could be intepreted as a transfer transaction of 1 ALGO from Alice to Charlie while Bob pay for the transaction fee of 0.001 ALGO and Alice doesn't pay for transaction fee as a consequence.

点击工具栏上的试管按钮，在弹出窗口中选择 8.contract_delegated.json 并点击 Run Test Transaction 按钮，稍等片刻等待交易完成。

Press the Tube button on the Toolkit bar, choose 8.contract_delegated.json and click Run Test Transaction button. Then wait for a while allowing transaction to complete.

交易完成后切换至区块浏览器并刷新 Alice, Bob 和 Charlie 页面，可以看到 Alice 的余额减少了 1 ALGO，Bob 减少了 0.002 ALGO（除了为 Alice 支付的转账费用外，还有自身的转账费用） 而 Charlie 增加了 1 ALGO。
Once it completes, move to block explorer and refresh Alice's , Bob's and Charlie's pages respectively. You shall see Alice's balance shrinked by 1 ALGO, Bob's have 0.002 ALGO less than before (Since it paid the fees both for Alice's transfer to Charlie as well as his to Alice), and Charlie, gets 1 more ALGO then before.

Alice 的交易记录中多了两条交易记录，分别为给 Charlie 的 1 ALGO 转账和收到 Bob 的 0.002 ALGO 转账。

Alice's transaction history will see two new records, that are outbound transfer of 1 ALGO to Charlie and 0.002 ALGO inbound transfer from Bob, repsectively.
