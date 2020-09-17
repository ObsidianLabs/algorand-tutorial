# 教程二

## 简介

本教程将通过 LimitOrder 例子来介绍 Algorand 的资产模型。教程中我们会创建以 LimitOrder 为模版的项目，并将其编译，通过调用对应的交易来完成 ASA 完整的生命周期。

## Algorand Standard Asset (ASA)

Algorand Standard Asset 是 Algorand 内置的标准资产模型，包含了 Fungible 和 Fon-fungible 两种类型。

详细说明请参考：

https://developer.algorand.org/docs/features/asa

### 构造 ASA 交易

打开教程一的 Dynamic Fee 项目，在 `tests` 目录下有 3 个和 ASA 交易相关的文件：

- `4.asset_create.json`
- `5.asset_transfer.json`
- `6.asset_destory.json`

一笔完整的 ASA 交易通常由这几个生命周期组成：

- Create: 创建 ASA
- Opt in: 注册 ASA 的接收人
- Transfer: 转账交易
- Other: freeze, revoke, clawback, destory

更多关于ASA的交易类型，参见：

https://developer.algorand.org/docs/features/asa/#asset-functions

## ASC Example: LimitOrder

LimitOrder 是一个 ASA 和 ALGO 交易的合约，通过设置一定的比例来保证 asset 和 ALGO 的兑换。

https://developer.algorand.org/docs/reference/teal/templates/limit_ordera

### 创建项目

使用 Alogorand Studio 新建一个项目，选择 *Limit Order* 模版，填入项目名后点击 *Create Project* 按钮。

项目包含了 `contract.teal` 和 `config.json` 文件以及一个 `tests` 目录。

`contract.teal` 包含了 LimitOrder 合约的所有逻辑，使用 TEAL 语言进行编写。

打开 `tests` 目录，在这里面可以找到五个文件，它们的名称和作用分别为

- `0.create_asset.json`: Bob 创建一种新 asset，总量为 1000
- `1.opt_in.json`: 通过 Opt in 来将 Alice 注册为 asset 接收人
- `2.deposit.json`: Alice 往合约中存 10 ALGO
- `3.exchange.json`: Bob 使用 10 个单位的 asset 来换取合约中的 1 ALGO
- `4.close.json`: 将剩余的 ALGO 退回给 Alice

### 合约代码

代码解析参见

https://developer.algorand.org/docs/reference/teal/templates/limit_ordera/#code-overview

### 编译合约

开始编译前，我们需要对代码进行一些修改。

首先点击左下方的 *Keypair Manager*，将 Alice 的地址复制一下，然后打开 `contract.teal` 文件并且定位到第 38 行和第 72 行，将这两行的地址替换为刚刚复制的 Alice 地址。这样设置以后，合约只会允许 Alice 作为本合约唯一的支付 ALGO 的账户。

点击工具栏的锤子按钮进行编译，编译完成后将会在项目目录下生成两个新的文件：`contract.teal.tok` 和 `contract.addr`。

### 使用 LimitOrder 合约

完成编译后便可以开始使用 LimitOrder 合约了。

本教程展示的是这样的流程：Bob 创建一种新的 asset，然后通过 LimitOrder 将自己的 asset 按照一定比例兑换 Alice 的 ALOG 资产。

以下将通过 `tests` 目录下的 json 文件分步实现完整的 LimitOrder 调用：

1. 为 Bob 创建新的 asset My Token，数量为 1000，单位为 MT。点击工具栏的试管按钮并选择 `0.create_asset.json` 并点击 *Run Test Transaction* 按钮，查询浏览器可以看到 Bob 的 asset 新增了 1000 MT
2. 将 Alice 将 Bob 的 asset 添加到白名单中。点击工具栏的试管按钮并选择 `1.opt_in.json` 并点击 *Run Test Transaction* 按钮，查询浏览器可以看到 Alice 的 asset 中新增了 MT asset，余额为 0
3. Alice 往合约中存 10 ALGO 用以交易 asset。点击工具栏的试管按钮并选择 `2.deposit.json` 并点击 *Run Test Transaction* 按钮，查询浏览器可以看到 Alice 的账户上少了 10 ALGO
4. Bob 使用自己的 10 asset 来换取 Alice 存在的 1 ALGO。点击工具栏的试管按钮并选择 `3.exchange.json` 并点击 *Run Test Transaction* 按钮，查询浏览器可以看到 Bob 的 asset 减少了 10 MT，但是增加了 1 ALGO
5. Alice 将剩余的 ALGO 从合约中取出剩余的 9 ALGO。点击工具栏的试管按钮并选择 `4.close.json` 并点击 *Run Test Transaction* 按钮，查询浏览器可以看到 Alice 的余额多了 9 ALGO
