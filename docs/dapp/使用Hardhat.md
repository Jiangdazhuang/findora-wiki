# 使用 Hardhat

### 概览[#](https://wiki.findora.org/docs/dapp/hardhat#overview)

Hardhat is an Ethereum development environment that helps developers manage and automate repetitive tasks for smart contract and DApp development, and can be used in the truffle project。Hardhat是一个以太坊开发环境，可帮助开发者管理和自动化智能合约和DApp开发的重复性任务，可以在truffle项目中使用。

### 先决条件[#](https://wiki.findora.org/docs/dapp/hardhat#prerequisites)

本教程需要安装Node.js。你可以通过本网址[Node.js](https://nodejs.org/)下载，或运行以下代码来完成安装。你可以通过请求每个安装包版本来验证安装的正确性：

   `node -v`

   `npm -v`

### 创建Hardhat项目[#](https://wiki.findora.org/docs/dapp/hardhat#create-hardhat-project)

这里我们使用truffle项目（请参考truffle文档，使用truffle创建项目模块，完成步骤1、2、3），然后配置package.json文件，在devDependencies中添加dependencies。

`"devDependencies": {   "@nomiclabs/hardhat-ethers": "^2.0.2",   "@nomiclabs/hardhat-etherscan": "^2.1.0",   "hardhat": "^2.0.3",   "hardhat-gas-reporter": "^1.0.1",   "ts-node": "^9.0.0",   "typescript": "^4.0.5"}`

需要重新安装dependencies

`npm install`

这将在我们的项目目录下创建一个Hardhat配置文件（hardhat.config.js），并在项目根目录下创建一个.secret来存储我们在操作合同时需要使用的钱包地址私钥。

`import '@nomiclabs/hardhat-ethers';import '@nomiclabs/hardhat-etherscan';import 'hardhat-gas-reporter';import * as fs from 'fs';
const mnemonic = fs.readFileSync('.secret').toString().trim();
module.exports = {  defaultNetwork: "hardhat",  networks: {    hardhat: {    },    findora: {      url: "https://prod-forge.prod.findora.org:8545",      chainId:525,      accounts: [mnemonic]    }  },  solidity: {    version: "0.6.12",    settings: {      optimizer: {        enabled: true,        runs: 200      }    }  },  paths: {    sources: "./contracts",    tests: "./test",    cache: './build/cache',    artifacts: './build/artifacts',  },  mocha: {    timeout: 20000  },  gasReporter: {    currency: 'USD',    enabled: true,  },  etherscan: {    // Your API key for Etherscan    // Obtain one at https://etherscan.io/    apiKey: '**',  }}`

### 创建Hardhat脚本文件[#](https://wiki.findora.org/docs/dapp/hardhat#create-hardhat-script-file)

在与合约相同的级别创建一个脚本目录，以存储Hardhat脚本文件（如deploy.ts），然后使用ethers编写部署脚本。首先，通过getContractFactory()方法创建一个合约的本地实例。再使用该实例中包含的deploy()方法来启动智能合约。最后，使用deploy()等待部署完成。合约部署完成后，可以在MyContract实例中获得合约地址。该脚本是本教程中使用的一个简化版本。

`import { network, ethers} from 'hardhat';
// scripts/deploy.tsasync function main() {   // We get the contract to deploy   const MyContract = await ethers.getContractFactory('MyContract');   console.log('Deploying MyContract...');
   // Instantiating a new MyContract smart contract   const myContract = await MyContract.deploy();
   // Waiting for the deployment to resolve   await myContract.deployed();   console.log('MyContract deployed to:', myContract.address);}
main()   .then(() => process.exit(0))   .catch((error) => {      console.error(error);      process.exit(1);   });`

### 部署合约[#](https://wiki.findora.org/docs/dapp/hardhat#deploy-contract)

使用运行命令，将MyContract合约部署到Findora devnet上。

`npx hardhat run scripts/deploy.js --network findora` 
合同可以在几秒钟内部署完毕，之后您就可以看到终端上打印出的合约地址：

![https://wiki.findora.org/assets/images/hardhat-deploy-b93b49af9322cf82a5426d3af2ea120a.jpg](https://wiki.findora.org/assets/images/hardhat-deploy-b93b49af9322cf82a5426d3af2ea120a.jpg)

### 与合约交互[#](https://wiki.findora.org/docs/dapp/hardhat#interact-with-the-contract)

创建MyContract.sol合约的本地实例，然后输入部署合约时获得的地址，将此实例连接到现有实例，连接到合约后与之交互。当控制台命令还在运行时，调用合约中的方法并传入正确的参数（如果该方法涉及ERC20类型的转账，请先完成授权）。

`const myContract = await ethers.getContractAt('MyContract', '0x8D94133ddF3A6Cc451653Cd4B21Dc8b65c3383B0');   const tx = await myContract.connect(operator).setValue(88, override);   console.log('hash is:', tx.hash)
   const value = await myContract.getValue();   console.log('value is:', value.toString())`

![https://wiki.findora.org/assets/images/hardhat-value-c1ce1dcf083d6e0178fef1bcca75a5b6.jpg](https://wiki.findora.org/assets/images/hardhat-value-c1ce1dcf083d6e0178fef1bcca75a5b6.jpg)

恭喜你！你已完成了 Hardhat运营的基本教程！
