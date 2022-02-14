# 使用 Remix

### 概述[#](https://wiki.findora.org/docs/dapp/remix#overview)

开发人员也可以使用Remix IDE与Findora进行交互。Remix IDE是Ethereum智能合约最常用的开发环境之一。它可以提供基于网络的解决方案，在本地虚拟机或外部Web3供应商（如MetaMask）上快速编译和部署Solidity和Vyper代码。通过结合使用这两个工具，开发者可以快速在Findora上开始部署。

### 如何开始使用Remix[#](https://wiki.findora.org/docs/dapp/remix#how-to-start-using-remix)

现在我们可以启动Remix，使用Findora的更多高级功能。

首先，打开一个新标签，输入 [https://remix.ethereum.org/](https://remix.ethereum.org/) ，打开Remix。在主界面点击Environments，选择Solidity以配置Remix用于Solidity开发，最后打开File Explorers界面，我们需要创建一个新文件夹来存储Solidity智能合约。点击File Explorers下的 "+"按钮，在弹出的窗口中输入 "MyContract.sol"，然后在弹出的编辑窗口中粘贴以下智能合约

`pragma solidity ^0.7.5;
contract MyContract {  //Your logic code}`

### 部署合约[#](https://wiki.findora.org/docs/dapp/remix#deploy-contract)

我们将通过以下基本合约来展示如何使用Remix在Findora上部署智能合约：

编译完成后，我们可以点击"Deploy & Run Transactions" （部署和运行交易）标签。首先需要将环境设置为 "Injected Web3."，需要使用MetaMask导入的提供者，并将合约部署到通过提供者连接的网络中。在本例中是Findora Devnet的测试网。

我们将使用一个有资产余额的MetaMask账户来部署该合同。它可以通过我们的测试网水龙头充值，然后部署到Findora Devnet上。接下来在constructor中输入测试合约，点击 "Deploy"（部署）。MetaMask的弹出窗口将显示交易相关信息，我们需要点击 "confirm" （确认）来签署。

![https://wiki.findora.org/assets/images/remix-confirm-bf5a3440e27b358ae8f3112f689d9178.png](https://wiki.findora.org/assets/images/remix-confirm-bf5a3440e27b358ae8f3112f689d9178.png)

交易确认后，合约将出现在Remix的 "已部署的合约"栏中。在这里你可以与合同功能进行互动。
