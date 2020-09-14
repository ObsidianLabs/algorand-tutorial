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
