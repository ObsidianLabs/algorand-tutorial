# 教程一
# Tutorial One

## 简介
## Intro
最后写完再总结

## 准备工作
## Preparation Work

### 安装Algorand Studio 
### Installing Algorand Studio

请在 GitHub [下载页面](https://github.com/ObsidianLabs/AlgorandStudio)下载 Algorand Studio。目前 Algorand Studio 支持 macOS 和 Linux 系统，请根据系统下载对应的版本。

You may download latest build of Algorand Studio from our Github release page [Link](https://github.com/ObsidianLabs/AlgorandStudio). Algorand Studio currently supports macOS and Linux operating systems while we're working hard to extend it to other platforms. 

正确安装 Algorand Studio Algorand Studio 将显示欢迎页面，根据提示完成 Docker, Algorand Node 以及 PyTeal Compiler 的下载、安装及启动。

Download, install and run Algorand Studio on your device, if everything works as it should be, you will be brought to a welcome screen where you shall see a checklist of prerequisites for Algorand Studio. Algorand Studio requires Docker runtime, Algorand Node and PyTeal Compiler to proper function, therefore we put it here to simplify your preparation work. Click the button at the end of each of the items and complete installation as per instruction. 


### 启动 Algorand node 并连接到测试网
### Start Algorand Node and connect to the test network

Algorand Studio 在创建 algorand node 的时候会帮助用户自动下载 snapshot，用于快速更新之前的数据。
Algorand Studio automatically downloads snapshot when you creates a new algorand node. 

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

为了顺利完成接下来教程，首先需要创建三个钥匙对并命名为：Alice，Bob 和 Charlie
In this tutorial, you need to create three keypairs after names, Alice, Bob and Charlie. Alice, Bob and Charlie will be referenced wherever we need a keypair in this tutorial.


## 基础操作
## the Basics

### 使用浏览器
### Using the Explorer

点击顶部 Account 标签打开账户浏览器，并在地址栏粘贴 Alice 的钥匙对地址，可以在左边看到当前地址的 ALGO 余额信息。下方可以看到每一笔交易的信息。
Click the Account Label on the top to open Account Explorer and paste Alice's keypair address in the address column. You should see its ALGO balance on the left and every transaction on the bottom relating to the specified address.


### 通过Faucet申请token
### Application of Token through Faucet

在区块链的世界中，大家通常将申请测试 Token 的方式称为 faucet，目前在测试网络下每次 faucet 申请到的 Token 为 100 ALGO。
In the world of Blockchain, people usually refer the method to apply for test token as Faucet. Currently, you will be granted 100 ALGO each time when you faucet for test tokens.

点击地址栏旁的水龙头按钮，将弹出一个 Algorand dispenser 页面，在页面中申请 Token。（需要翻墙）
Click the faucet button next to the address column, a window named Algorand Dispenser will pop up where you can apply for test token. (You might need VPN to complete this if you are located in Mainland China)

完成申请后稍等片刻，刷新页面后可以看到 Alice 的余额更新为 100 ALGO，并且下方 Transactions 列表中多了一条转账记录。
Minutes following your applcation, you will see Alice's balance go up by 100 ALGO where a new transfer transaction is recorded in the Transactions List.


### 转账
### Transfer

开始转账前我们新增一个 tab，并将 Bob 的地址粘贴到地址栏，回车后可以看到 Bob 的账户余额信息为 0 ALGO。
To start a transfer, you need to create a new tab, and type in Bob's address into the address column. Press ENTER and you'll see Bob's current account balance, which is 0 ALGO.

点击地址栏旁的 Transfer 按钮，输入转账总数和收款人 recipient 地址，这里填 Bob 的地址，点击 Sign and Send。
Then press Transfer button next to the address column. Type in the amount to be transferred and the recipient's address, which in our case, is the address of Bob. Click Sign and Send to initiate the transaction.

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


# 教程二
# Tutorial Two

## 简介
## Intro

本教程将通过 LimitOrder 例子来介绍 Algorand 的资产模型。教程中我们会创建以 LimitOrder 为模版的项目，并将其编译，通过调用对应的交易来完成 ASA 完整的 lifecycle。
In this tutorial, We will talk about Algorand asset model through examples on LimitOrder. You will have the chance to create a LimitOrder-based project, and go through a full ASA lifecycle by compiling and then calling transactions defined in the project.

## Algorand Standard Asset (ASA)

Algorand Standard Asset 是 Algorand 内置的标准资产模型，包含了 fungible 和 non-fungible 两种类型。
Algorand Standard Asset is an Algorand built-in Standard Asset model, including both fungible and non-fungible types of assets.

For full information, please refer to below.
详细说明请参考：https://developer.algorand.org/docs/features/asa/


### 构造 ASA 交易
### Construct ASA transaction

打开教程一的 Dynamic Fee 项目，在 tests 目录下有 3 个和 ASA 交易相关的文件：
Open the Dynamic Fee project from Tutorial One, you may find 3 ASA-related files under tests folder.

- 4.asset_create.json
- 5.asset_transfer.json
- 6.asset_destory.json

一笔完整的 ASA 交易通常由这几个 lifecycle 组成：
A complete ASA transction usually contains theses elements in its lifecycle.

- Create: 创建 ASA
- Create: Create ASA
- Opt in: 注册 ASA 的接收人
- Opt in: Register ASA recipients
- Transfer: 转账交易
- Transfer: the Transfer Transaction
- Other: freeze, revoke, clawback, destory
- Other: freeze, revoke, clawback, destroy

For more ASA transaction classification, please refer to
更多关于ASA的交易类型，参见 https://developer.algorand.org/docs/features/asa/#asset-functions


## ASC Example: LimitOrder

LimitOrder 是一个 ASA 和 ALGO 交易的合约，通过设置一定的比例来保证 asset 和 ALGO 的兑换。
LimitOrder is a ASA and ALGO transaction contract, which guarantees exchange between asset and ALGO by setting a specified exchange rate.

https://developer.algorand.org/docs/reference/teal/templates/limit_ordera

### 创建项目
### Create Project

使用 Alogorand Studio 新建一个项目，选择 Limit Order 模版，填入项目名后点击 Create Project 按钮。
Create a new Project in Algorand Studio. Select LimitOrder template, put in project name and click Create button.

项目包含了 contract.teal 和 config.json 文件以及一个 tests 目录。
The project contains contract.teal, config.json file as well as a tests folder.

contract.teal 包含了 LimitOrder 合约的所有逻辑，使用 TEAL 语言进行编写。
contract.teal contains all logics to execute LimitOrder contract, which was written in TEAL.

打开 tests 目录，在这里面可以找到五个文件，它们的名称和作用分别为
Navigate to tests folder, you shall find 5 files named as below.

- 0.create_asset.json: Bob 创建一种新 asset，总量为 1000
- 0.create_asset.json: Bob creates a new asset of 1000 units.
- 1.opt_in.json: 通过 Opt in 来将 Alice 注册为 asset 接收人
- 1.opt_in.json: Registers Alice as Asset Recipient through Opt-in
- 2.deposit.json: Alice 往合约中存 10 ALGO
- 2.deposit.json: Alice deposits 10 ALGO into the contract
- 3.exchange.json: Bob 使用 10 个单位的 asset 来换取合约中的 1 ALGO
- 3.exchange.json: Bob exchanges 10 unites of asset for 1 ALGO in the contract.
- 4.close.json: 将剩余的 ALGO 退回给 Alice
- 4.close.json: Returns remaining ALGO to Alice.

### 合约代码
### Contract Code

Full Overview of code can be found below
代码解析参见 https://developer.algorand.org/docs/reference/teal/templates/limit_ordera/#code-overview

### 编译合约
### Compile Contract

开始编译前，我们需要对代码进行一些修改。
Before you compile the contract, we need you to modify a few lines in to contract code.

首先点击左下方的 keypair manager，将 Alice 的地址复制一下，然后打开 contract.teal 文件并且定位到第 38 行和第 72 行，将这两行的地址替换为刚刚复制的 Alice 地址。这样设置以后，合约只会允许 Alice 作为本合约唯一的支付 ALGO 的账户。
First, click Keypair Manager icon on the bottom left corner, copy Alice's address. Then open contract.teal file, and navigate to Line 38 and Line 72, respectively. Overwrite both addresses with Alice's address as they specify Alice as the only recipient allowed in this contract.

点击工具栏的锤子按钮进行编译，编译完成后将会在项目目录下生成两个新的文件：contract.teal.tok 和 contract.addr。
Click hammer button to start compilation. Upon completion, you will find two outputs, contract.teal.tok and contract.addr created under project folder.

### 使用 LimitOrder 合约
### Call LimitOrder Contract

完成编译后便可以开始使用 LimitOrder 合约了。
Once compilation completes, you shall start to make use of LimitOrder contract.

本教程展示的是这样的流程：Bob 创建一种新的 asset，然后通过 LimitOrder 将自己的 asset 按照一定比例兑换 Alice 的 ALOG 资产。
In this tutorial, we're showcasing a lifecycle in the following steps, Bob created a new asset, and exchange this new asset for ALGO at a specified rate from Alice.

以下将通过 tests 目录下的 json 文件分步实现完整的 LimitOrder 调用：
Following achieves a full LimitOrder call lifecycle through multiple step-by-step json file call.

1. 为 Bob 创建新的 asset My Token，数量为 1000，单位为 MT。点击工具栏的试管按钮并选择 0.create_asset.json 并点击 Run Test Transaction 按钮，查询浏览器可以看到 Bob 的 asset 新增了 1000 MT
1. Create a new asset named My Token for Bob. Inititate 1000 units and takes MT as its symbol. Click Tube button on the toolkit bar, select 0.create_asset.json and click Run Test Transaction button. You shall see 1000 MT created in Bob's asset through Explorer once the transaction completes.
2. 将 Alice 将 Bob 的 asset 添加到白名单中。点击工具栏的试管按钮并选择 1.opt_in.json 并点击 Run Test Transaction 按钮，查询浏览器可以看到 Alice 的 asset 中新增了 MT asset，余额为 0
2. Alice adds Bob into its whilelist. Click Tube button on the toolkit bar, select 1.opt_in.json and click Run Test Transaction button, then you shall see 0 MT asset added into Alice's asset through the Explorer.
3. Alice 往合约中存 10 ALGO 用以交易 asset。点击工具栏的试管按钮并选择 2.deposit.json 并点击 Run Test Transaction 按钮，查询浏览器可以看到 Alice 的账户上少了 10 ALGO
3. Alice deposits 10 ALGO for exchange transaction with asset. Click Tube button on the toolkit bar, select 2.deposit.json and click Run Test Transaction button, then you shall see 10 ALGO deducted from Alice's.
4. Bob 使用自己的 10 asset 来换取 Alice 存在的 1 ALGO。点击工具栏的试管按钮并选择 3.exchange.json 并点击 Run Test Transaction 按钮，查询浏览器可以看到 Bob 的 asset 减少了 10 MT，但是增加了 1 ALGO
4. Bob exchanges 10 asset for Alice's 1 ALGO. Click Tube button on the toolkit bar, select 3.exchange.json and click Run Test Transaction button, then you shall see 10 MT deducted while 1 ALGO added in Bob's account.
5. Alice 将剩余的 ALGO 从合约中取出剩余的 9 ALGO。点击工具栏的试管按钮并选择 4.close.json 并点击 Run Test Transaction 按钮，查询浏览器可以看到 Alice 的余额多了 9 ALGO
5. Alice withdrawed 9 remaining ALGO from the contract. Click Tube button on the toolkit bar, select 4.close.json and click Run Test Transaction button, then you shall see 9 ALGO added on Alice's balance.
