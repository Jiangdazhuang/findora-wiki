# 使用 Waffle

### 概览[#](https://wiki.findora.org/docs/dapp/waffle-mars#overview)

Waffle是一个用于编译和测试智能合约的库，而Mars是一个部署管理器。Waffle和Mars可以共同用于编写、编译、测试和部署以太坊智能合约。由于Findora的以太坊兼容性，可以使用Waffle和Mars将智能合约部署到Findora Devnet测试网。

Waffle使用最小的依赖（dependencies），有一个易于学习和扩展的编写语法，并在编译和测试智能合约时提供快速的执行时间。此外，Waffle和TypeScript的兼容以及Chai匹配器的使用使得查看和编写测试变得容易。

Mars有一个兼容TypeScript的简单框架，用于创建高级部署脚本，使其与状态变化保持同步。Mars专注于 "基础设施即代码"，允许开发者指定部署智能合约的方式，使用这些规范来自动处理状态变化和部署。

在本教程中，您需要先创建一个TypeScript项目，然后使用Waffle来编写、编译和测试智能合约，再用Mars将其部署到Findora Devnet测试网。

### 先决条件[#](https://wiki.findora.org/docs/dapp/waffle-mars#prerequisites)

本教程需要安装Node.js。你可以通过本网址[Node.js](https://nodejs.org/)下载，或运行以下代码来完成安装。你可以通过请求每个安装包版本来验证安装的正确性：

   `node -v`

   `npm -v`

### 使用Waffle创建TypeScript项目[#](https://wiki.findora.org/docs/dapp/waffle-mars#use-waffle-to-create-a-typescript-project)

这里我们使用truffle项目（请参考truffle文档，使用truffle创建项目模块，完成步骤1、2、3），然后配置package.json文件，在devDependencies中添加依赖：

`"devDependencies": {   "@types/chai": "^4.2.21",    "@types/mocha": "^9.0.0",    "chai": "^4.3.4",    "ethereum-mars": "^0.1.5",    "ethereum-waffle": "^3.4.0",    "ethers": "^5.4.6",    "mocha": "^9.1.1",    "ts-node": "^10.2.1",    "typescript": "^4.4.3",    "node-fetch": "^3.0.0"}`

[Waffle](https://github.com/EthWorks/Waffle) - 用于编写、编译和测试智能合约

[Ethers](https://github.com/ethers-io/ethers.js/) - 用于与Findora的以太坊API进行交互

[OpenZeppelin Contracts](https://github.com/OpenZeppelin/openzeppelin-contracts) - 你要创建的合约将使用OpenZeppelin的ERC20基础实现

[TypeScript](https://github.com/microsoft/TypeScript) - 该项目将是一个TypeScript项目

[TS Node](https://github.com/TypeStrong/ts-node) - 用于执行你将在本指南后创建的部署脚本

[Chai](https://github.com/chaijs/chai) - 一个与Waffle共同使用的断言库，用于编写测试

[@types/chai](https://github.com/DefinitelyTyped/DefinitelyTyped/tree/HEAD/types/chai) - 包含chai的类型定义

[Mocha](https://github.com/mochajs/mocha) - 一个测试框架，与Waffle一起编写测试

[@types/mocha](https://github.com/DefinitelyTyped/DefinitelyTyped/tree/HEAD/types/mocha) - 含mocha的类型定义

需要重新安装依赖（dependencies）

`npm install`

### 创建一个TypeScript配置文件：[#](https://wiki.findora.org/docs/dapp/waffle-mars#create-a-typescript-configuration-file%EF%BC%9A)

`touch tsconfig.json`

### 添加基本的TypeScript配置：[#](https://wiki.findora.org/docs/dapp/waffle-mars#add-basic-typescript-configuration%EF%BC%9A)

`{  "compilerOptions": {    "strict": true,    "target": "ES2019",    "moduleResolution": "node",    "resolveJsonModule": true,    "esModuleInterop": true,    "module": "CommonJS",    "composite": true,    "sourceMap": true,    "declaration": true,    "noEmit": true  }}`

现在你应该有一个基本的TypeScript项目，其中包括使用Waffle和Mars构建所需的依赖。

### 用Waffle编译[#](https://wiki.findora.org/docs/dapp/waffle-mars#compile-with-waffle)

1.回到项目的根目录，创建一个waffle.json文件来配置Waffle：

`cd .. && touch waffle.json`

2.编辑waffle.json以指定编译器配置，包括合同目录等。在这个例子中，我们将使用solcjs和你用于合约的Solidity版本，即0.6.12：

`{  "compilerType": "solcjs", // Specifies compiler to use  "compilerVersion": "0.6.12", // Specifies version of the compiler  "compilerOptions": {    "optimizer": { // Optional optimizer settings      "enabled": true, // Enable optimizer      "runs": 20000 // Optimize how many times you want to run the code    }  },  "sourceDirectory": "./contracts", // Path to directory containing smart contracts  "outputDirectory": "./build", // Path to directory where Waffle saves compiler output  "typechainEnabled": true // Enable typed artifact generation}`

3.在package.json中添加一个脚本，用于运行Waffle：

`"scripts": {  "build": "waffle"}`

以上就是配置Waffle的所有步骤，现在你可以使用构建脚本来编译MyContract合约了：

`npm run build`

![https://wiki.findora.org/assets/images/wallfe-build-ad7fab85ee69bc672ae67e04f003b98e.jpg](https://wiki.findora.org/assets/images/wallfe-build-ad7fab85ee69bc672ae67e04f003b98e.jpg)

4.在测试目录下创建一个文件（MyContract.test.ts），用于测试你的MyContract合约：

`import { use, expect } from 'chai';import { Provider } from '@ethersproject/providers';import { solidity } from 'ethereum-waffle';import { ethers, Wallet } from 'ethers';import { MyContract, MyContractFactory } from '../build/types';import * as fs from 'fs';
const mnemonic = fs.readFileSync('.secret').toString().trim();
// Tell Chai to use Waffle's Solidity pluginuse(solidity);
describe ('MyContract', () => {  // Use custom provider to connect to Moonbase Alpha  let provider: Provider = new ethers.providers.JsonRpcProvider('https://prod-forge.prod.findora.org:8545');  let wallet: Wallet;  let walletTo: Wallet;  let contract: MyContract;
  beforeEach(async () => {    // Create a wallet instance using your private key & connect it to the provider    wallet = new Wallet(mnemonic).connect(provider);
    // Create a random account to transfer tokens to & connect it to the provider    walletTo = Wallet.createRandom().connect(provider);
    // Use your wallet to deploy the MyToken contract    contract = await new MyContractFactory(wallet).deploy();
    let contractTransaction = await contract.setValue(88);
    // Wait until the transaction is confirmed before running tests    await contractTransaction.wait();  });    // 测试结果  it('current value', async () => {    expect(await contract.getValue()).to.equal(88); // This should fail  });})`

![https://wiki.findora.org/assets/images/wallfe-test-005b99467a31c16c1e334bf8124b8163.jpg](https://wiki.findora.org/assets/images/wallfe-test-005b99467a31c16c1e334bf8124b8163.jpg)

### 使用Mars来配置部署脚本[#](https://wiki.findora.org/docs/dapp/waffle-mars#use-mars-to-configure-the-deployment-script)

现在你需要为Findora Devnet测试网配置MyContract合约的部署。

你需要为Mars生成artifact，以便在部署脚本中启用类型检查。

1.更新现有脚本，在package.json中运行Waffle，以囊括Mars：

`"scripts": {  "build": "waffle && mars",  "test": "mocha",}`

2.生成artifacts并创建部署所需的artifacts.ts文件

`npm run build`

如果你打开构建目录，可以看到一个artifacts.ts文件，它包含了部署所需的artifact数据。在进行部署之前，你需要写一个部署脚本。部署脚本将用于解释Mars部署哪些合约，部署到哪个网络，以及哪个账户用于触发部署。

TODO: insert /img/evm/mars-build.png

### 创建部署脚本[#](https://wiki.findora.org/docs/dapp/waffle-mars#create-deploy-script)

在这一步，你将创建一个部署脚本，将定义合约的部署方式。Mars提供了一个部署函数，你可以向其传递一些选项，如用于部署合约的账户私钥、要部署的网络等等。部署功能用于定义要部署的合约。Mars有一个合约功能，接受name、artifact和constructorArgs。

1.创建一个包含部署脚本的src目录，创建一个脚本来部署MyContract合约：

`mkdir src && cd src && touch deploy.ts`

2.在deploy.ts中，使用Mars的部署功能创建一个脚本，并用你的账户私钥将其部署到Findora Devnet：

`import { deploy } from 'ethereum-mars';
const privateKey = "<insert-your-private-key-here>";deploy({network: 'https://prod-forge.prod.findora.org:8545', privateKey},(deployer) => {  // Deployment logic will go here});`

3.设置部署功能，以部署在上述步骤中创建的MyContract合约：

`import { deploy, contract } from 'ethereum-mars';import { MyToken } from '../build/artifacts';
const privateKey = "<insert-your-private-key-here>";deploy({network: 'https://prod-forge.prod.findora.org:8545', privateKey}, () => {  contract('myContract', MyContract);});`

4.将部署脚本添加到package.json中的scripts对象中。

`"scripts": {    "build": "waffle && mars",    "test": "mocha",    "deploy": "ts-node src/deploy.ts"  }`

So far, you should have created a deployment script in deploy.ts to deploy the MyContract contract to Findora Devnet, and added the ability to easily call the script and deploy the contract。到目前为止，你应该已经在deploy.ts中创建了一个部署脚本，用于将MyContract合约部署到Findora Devnet，添加了轻松调用脚本和部署合约的功能。

### 用Mars部署[#](https://wiki.findora.org/docs/dapp/waffle-mars#use-mars-for-deploy)

如果你已经配置了部署，现在就可以实际部署到Findora Devnet上。

1.使用你刚刚创建的脚本部署合约：

`npm run deploy`

恭喜你！你已经通过Waffle和Mars成功地在Findora Devnet上部署了合约!
