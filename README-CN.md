# 教程一

## 简介

我们用两篇教程来介绍如何使用 Alogrand Studio 来进行 Alogrand 开发。

本篇为第一篇教程，将简单介绍如何安装使用 Alogrand Studio，然后通过 DynamicFee 项目来展示 Alogrand 开发的完整流程。

## 准备工作

### 安装Algorand Studio

请在 GitHub [下载页面](https://github.com/ObsidianLabs/AlgorandStudio)下载 Algorand Studio。目前 Algorand Studio 支持 macOS 和 Linux 系统，请根据系统下载对应的版本。

正确安装 Algorand Studio Algorand Studio 将显示欢迎页面，根据提示完成 Docker, Algorand Node 以及 PyTeal Compiler 的下载、安装及启动。


### 启动 Algorand node 并连接到测试网

Algorand Studio 在创建 algorand node 的时候会帮助用户自动下载 snapshot，用于快速更新之前的数据。

完成 snapshot 的下载后 algorand node 实例将出现在列表中，点击 start 就可以启动 algorand node。

目前 algorand studio 仅支持测试网。


### 创建Algorand地址

完成 Algorand node 的启动后，首先需要创建钥匙对来完成后续的合约部署以及调用。

有别于一般的密钥对，Algorand 密钥对中的私钥是以 mnemonic （助记词）的形式储存的。

在 Algorand Studio 的任意界面，点击应用左下⻆的钥匙图标，打开密钥管理器。点击 Create 按钮打开新钥匙对弹窗，输入钥匙对的名字并点击 Save 按钮。完成后将在密钥管理器中看到刚刚生成的钥匙对的地址。钥匙对由私钥和公钥组成，公钥在智能合约中也常被称作地址。

导出私钥可以通过点击每个地址后面的眼睛按钮打开查看私钥弹窗，弹窗显示地址以及私钥。后续教程中会需要通过管理器导出私钥。

为了顺利完成接下来教程，首先需要创建三个钥匙对并命名为：Alice，Bob 和 Charlie


## 基础操作

### 使用浏览器

点击顶部 Account 标签打开账户浏览器，并在地址栏粘贴 Alice 的钥匙对地址，可以在左边看到当前地址的 ALGO 余额信息。下方可以看到每一笔交易的信息。


### 通过Faucet申请token

在区块链的世界中，大家通常将申请测试 Token 的方式称为 faucet，目前在测试网络下每次 faucet 申请到的 Token 为 100 ALGO。

点击地址栏旁的水龙头按钮，将弹出一个 Algorand dispenser 页面，在页面中申请 Token。（需要翻墙）

完成申请后稍等片刻，刷新页面后可以看到 Alice 的余额更新为 100 ALGO，并且下方 Transactions 列表中多了一条转账记录。


### 转账

开始转账前我们新增一个 tab，并将 Bob 的地址粘贴到地址栏，回车后可以看到 Bob 的账户余额信息为 0 ALGO。

点击地址栏旁的 Transfer 按钮，输入转账总数和收款人 recipient 地址，这里填 Bob 的地址，点击 Sign and Send。

完成后再次刷新页面，Alice 的余额变为 79.753 ALGO，Bob 的余额为 20 ALGO，这是因为除了给 Bob 转出的 20 ALGO 外，还有从 Alice 账户中扣除的转账手续费。


## Algorand smart contract (ASC)

TEAL是原生写 algorand 智能合约语言，它是一种类似 Assembly 的语言，由 Algorand 官方开发。

https://developer.algorand.org/docs/reference/teal/specification/


### 创建项目（模板DynamicFee）

点击顶部 Project 标签，点击 New 按钮，填入项目名称和模版，这里选择 Dynamic fee 模版，点击 Create 按钮完成项目的创建。

### PyTeal vs TEAL

使用 TEAL 编写代码并不方便，为此 Alogrand 开发了 PyTeal。Pyteal 是通过 python 语法来编写代码，然后通过 Pyteal 编译器编译成 teal，然后编译成二进制。

https://developer.algorand.org/docs/features/asc1/teal/pyteal/


### 编译合约

在编译 Dynamic fee 前，我们还需要修改一下 main.py 的代码，在第 5 行代码中填入接受转账的账户地址。

在编辑器底部有两个锤子按钮，后面跟着版本号，这两个分别是 Pyteal 的编译器和 Teal 的编译器。Algorand Studio会使用这两个编译器一起进行编译。

其中 pyteal 编译器负责将 .py 文件编译成 .teal 文件，Teal 编译器负责将 .teal 文件编译成 .tok 和 .addr 文件

.addr 是合约的地址，相当于合约代码的 hash。

点击工具栏的锤子按钮进行编译，编译完成后的合约不需要部署，只需要在调用时附加合约代码(.tok)，通过地址即可检查代码是否真实。


### 合约代码

Dynamic fee 合约实现了一个代付交易费的功能。上文中的转账交易费是由 Alice 来支付的，如果使用 Dynamic fee 合约来进行转账，是可以由别的账户来支付这笔交易费。

合约代码具体解析参见
https://developer.algorand.org/docs/reference/teal/templates/dynamic_fee/


## 构造交易 & 调用合约

Alogrand Studio 通过 json 文件来定义 transaction。

在项目下的 test 文件夹中可以找到 8 个 json 文件，分别对应着 8 种不同类型的交易。


### 构造普通交易

#### 普通转账 pay

我们将展示如何使用 json 文件来进行转账交易。

打开 test 文件夹下的 pay.json 文件，从这个文件中可以看到定义这笔交易的所有信息，其中包括了方法名称 pay，支付地址，接受地址，支付金额等信息。其中账户地址可以直接使用 keypair manager 中的名字，Alogrand Studio 将使用名字进行索引替换为地址。

从 1.pay.json 文件中可以看出，这笔交易的定义的是从 Alice 转账给 Bob 10个 ALGO，并使用 Alice 进行签名。

点击试管按钮，在弹出的窗口中选择 1.pay.json，然后点击 Run Test Transaction 按钮，稍等片刻等待交易完成。

交易完成后切换至区块浏览器并刷新 Alice 页面，可以看到余额减少了 10+ ALGO，下方的交易记录中也多出了刚刚的那笔转账记录。


#### atomic transfer
atomic transfer 保证多笔交易同时成功和同时失败。
https://developer.algorand.org/docs/features/atomic_transfers


#### multisig
multisig 支持多个账户给这笔交易签名。
https://developer.algorand.org/docs/features/accounts/create/#multisignature


### 调用合约

Algorand 的智能合约有 stateful 和 stateless 两种类型，本教程使用的是 stateless 类型的智能合约。
更多 stateless 相关的合约请查看：
https://developer.algorand.org/docs/features/asc1/stateless


#### Contract Account vs Delegated Approval
Stateless Smart Contracts 使用 LogicSig 作为合约签名的方式。通常 Stateless Smart Contracts 有两种使用场景，分别为 Contract Account 和 Delegated Approval

- Contract Account 合约作为交易中间方，交易方将资金存入合约，当交易双方达到交易条件时由合约本身进行转账
- Delegated Approval 合约作为交易点方，仅负责检查交易，不负责资金流转

更多 Contract Account 和 Delegated Approval 相关的信息请查看：
https://developer.algorand.org/docs/features/asc1/stateless/modes


#### 调用Dynamic Fee合约

打开 8.contract_delegated.json 文件，该 json 文件描述了两笔交易，第一笔是 Bob 给 Alice 转的 0.001 ALGO，第二笔是 Alice 给 Charlie 转的 1 ALGO。

这两笔交易合并起来看是 Alice 给 Charlie 转了 1 ALGO，而 Bob 为这笔交易支付了 0.001 ALGO 的转账手续费，结果就是 Alice 不需要为这笔转账支付手续费。

点击工具栏上的试管按钮，在弹出窗口中选择 8.contract_delegated.json 并点击 Run Test Transaction 按钮，稍等片刻等待交易完成。

交易完成后切换至区块浏览器并刷新 Alice, Bob 和 Charlie 页面，可以看到 Alice 的余额减少了 1 ALGO，Bob 减少了 0.002 ALGO（除了为 Alice 支付的转账费用外，还有自身的转账费用） 而 Charlie 增加了 1 ALGO。

Alice 的交易记录中多了两条交易记录，分别为给 Charlie 的 1 ALGO 转账和收到 Bob 的 0.002 ALGO 转账。




# 教程二

## 简介

本教程将通过 LimitOrder 例子来介绍 Algorand 的资产模型。教程中我们会创建以 LimitOrder 为模版的项目，并将其编译，通过调用对应的交易来完成 ASA 完整的 lifecycle。

## Algorand Standard Asset (ASA)

Algorand Standard Asset 是 Algorand 内置的标准资产模型，包含了 fungible 和 non-fungible 两种类型。

详细说明请参考：https://developer.algorand.org/docs/features/asa/


### 构造 ASA 交易

打开教程一的 Dynamic Fee 项目，在 tests 目录下有 3 个和 ASA 交易相关的文件：

- 4.asset_create.json
- 5.asset_transfer.json
- 6.asset_destory.json

一笔完整的 ASA 交易通常由这几个 lifecycle 组成：

- Create: 创建 ASA
- Opt in: 注册 ASA 的接收人
- Transfer: 转账交易
- Other: freeze, revoke, clawback, destory

更多关于ASA的交易类型，参见 https://developer.algorand.org/docs/features/asa/#asset-functions


## ASC Example: LimitOrder

LimitOrder 是一个 ASA 和 ALGO 交易的合约，通过设置一定的比例来保证 asset 和 ALGO 的兑换。

https://developer.algorand.org/docs/reference/teal/templates/limit_ordera

### 创建项目

使用 Alogorand Studio 新建一个项目，选择 Limit Order 模版，填入项目名后点击 Create Project 按钮。

项目包含了 contract.teal 和 config.json 文件以及一个 tests 目录。

contract.teal 包含了 LimitOrder 合约的所有逻辑，使用 TEAL 语言进行编写。

打开 tests 目录，在这里面可以找到五个文件，它们的名称和作用分别为

- 0.create_asset.json: Bob 创建一种新 asset，总量为 1000
- 1.opt_in.json: 通过 Opt in 来将 Alice 注册为 asset 接收人
- 2.deposit.json: Alice 往合约中存 10 ALGO
- 3.exchange.json: Bob 使用 10 个单位的 asset 来换取合约中的 1 ALGO
- 4.close.json: 将剩余的 ALGO 退回给 Alice

### 合约代码

代码解析参见 https://developer.algorand.org/docs/reference/teal/templates/limit_ordera/#code-overview

### 编译合约

开始编译前，我们需要对代码进行一些修改。

首先点击左下方的 keypair manager，将 Alice 的地址复制一下，然后打开 contract.teal 文件并且定位到第 38 行和第 72 行，将这两行的地址替换为刚刚复制的 Alice 地址。这样设置以后，合约只会允许 Alice 作为本合约唯一的支付 ALGO 的账户。

点击工具栏的锤子按钮进行编译，编译完成后将会在项目目录下生成两个新的文件：contract.teal.tok 和 contract.addr。

### 使用 LimitOrder 合约

完成编译后便可以开始使用 LimitOrder 合约了。

本教程展示的是这样的流程：Bob 创建一种新的 asset，然后通过 LimitOrder 将自己的 asset 按照一定比例兑换 Alice 的 ALOG 资产。

以下将通过 tests 目录下的 json 文件分步实现完整的 LimitOrder 调用：

1. 为 Bob 创建新的 asset My Token，数量为 1000，单位为 MT。点击工具栏的试管按钮并选择 0.create_asset.json 并点击 Run Test Transaction 按钮，查询浏览器可以看到 Bob 的 asset 新增了 1000 MT
2. 将 Alice 将 Bob 的 asset 添加到白名单中。点击工具栏的试管按钮并选择 1.opt_in.json 并点击 Run Test Transaction 按钮，查询浏览器可以看到 Alice 的 asset 中新增了 MT asset，余额为 0
3. Alice 往合约中存 10 ALGO 用以交易 asset。点击工具栏的试管按钮并选择 2.deposit.json 并点击 Run Test Transaction 按钮，查询浏览器可以看到 Alice 的账户上少了 10 ALGO
4. Bob 使用自己的 10 asset 来换取 Alice 存在的 1 ALGO。点击工具栏的试管按钮并选择 3.exchange.json 并点击 Run Test Transaction 按钮，查询浏览器可以看到 Bob 的 asset 减少了 10 MT，但是增加了 1 ALGO
5. Alice 将剩余的 ALGO 从合约中取出剩余的 9 ALGO。点击工具栏的试管按钮并选择 4.close.json 并点击 Run Test Transaction 按钮，查询浏览器可以看到 Alice 的余额多了 9 ALGO
