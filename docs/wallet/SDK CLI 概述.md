---
sidebar_position: 2
---

# SDK CLI 概述

`Findora SDK` 包含一个CLI命令工具，它使开发人员能够使用单一的命令语法快速执行经常使用的操作。  

### **安装和配置 `Findora SDK`**

要安装 `Findora SDK CLI` ，需要克隆它的Github repo并安装它的依赖项。  
 
首先，在终端中运行以下命令下载repo: 

```bash
git clone https://github.com/FindoraNetwork/findora-sdk.git
```

接下来，将目录更改为 `Findora SDK` 的克隆版本:  

```bash
cd findora-sdk
```

然后，安装所有SDK依赖项:

```bash
yarn
```

最后，检查“CLI”工具是否正确安装:

```bash
yarn cli
```

如果安装正确，你会看到:

```bash
 ~/t/t/findora-sdk $ yarn cli
yarn run v1.22.11
$ yarn cli:build && yarn cli:run "$npm_config"
$ tsc
$ nodemon dist/cli.js --ignore cache/ "$npm_config" ''
[nodemon] 2.0.15
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node dist/cli.js "" ""`
"2021-12-14, 11:36:39 a.m." - please run as "yarn cli fund --address=fraXXX --amountToFund=1 "
"2021-12-14, 11:36:39 a.m." - please run as "yarn cli createWallet"
"2021-12-14, 11:36:39 a.m." - please run as "yarn cli restoreWallet --mnemonicString='XXX ... ... XXX'"
[nodemon] clean exit - waiting for changes before restart
```

### **CLI 命令**

**1.** **查看所有Findora SDK CLI命令(和选项) **

要查看所有命令的列表(和使用说明),运行以下终端命令:

```bash
yarn cli
```

在输出信息中,你会看到所有可用的CLI命令和相关选项的列表:

```bash
"2021-12-08, 12:07:09 p.m." - please run as "yarn cli fund --address=fraXXX --amountToFund=1 "
"2021-12-08, 12:07:09 p.m." - please run as "yarn cli createWallet"
"2021-12-08, 12:07:09 p.m." - please run as "yarn cli restoreWallet --mnemonicString='XXX ... ... XXX'"
```

正如你所看到的,目前有三个可用的命令:

- fund
- createWallet
- runRestoreWallet

**2.** **"Fund"命令**

通过运行 `yarn cli fund --address=fraXXX --amountToFund=1` 你会资助一个地址  `fraXXX`  数量为  `1 FRA`.

请注意,资金的**来源**是**另一个账户**(其私钥由您控制),并且必须有一些资金，就像你自己的水龙头。

该帐户将从**私钥**恢复,这需要提供在'.Env '文件，使用以下格式:

```env
PKEY_LOCAL_FAUCET="XAsFsKosjY8J=XXXXXXXXXX";
```

因此，在运行此命令之前，您需要创建一个`.env`文件,并将上述信息放入其中。在此之后，您将能够执行一个快速的,一行命令发送FRA到其他地址(为他们提供资金)。

**2.** **"创建钱包" 命令**

如果需要快速创建一个新的钱包，你可以使用' yarn cli createWallet `命令来创建一个新的钱包，以及获取它的助记词、地址、私钥和公钥。

该信息将显示为命令输出(类似于下面的内容):

```bash
[nodemon] starting `node dist/cli.js "" "" createWallet`
"2021-12-08, 12:22:06 p.m." - 🚀 ~ new mnemonic: "output insect settle weather spray lava seven day rice swamp captain upgrade layer ocean century kitten feel crunch fly huge power divert amused fitness"
"2021-12-08, 12:22:06 p.m." - 🚀 ~ new wallet info:  [
  {
    keyStore: Uint8Array(104) [
       16,  51,  89, 254,  22,  53, 133,  79, 253, 182,  55, 161,
      213,  15, 157, 231,  66,   7, 255, 188,   8, 122,  16, 124,
      223,  86, 162, 198, 211,  51, 251,  14,   2, 140, 195, 238,
      235, 184, 250, 169, 162,  97, 217,  99,  62, 117,  61,  57,
       75, 167, 208,  16, 100, 199, 215, 147, 166,  40,  81,   6,
      180, 194, 114,  10, 180,  77, 159,  65,  60, 254, 166, 212,
       92, 246, 202, 137, 255, 243, 236, 127, 123, 228,  95,  23,
       65, 224,   6, 208, 104, 131, 156,  76, 255, 201,  90,  92,
       48, 147, 144, 222,
      ... 4 more items
    ],
    publickey: 'C_0GxCvI8OBYZRO9mNZOhh8MykvOrOLx7F7U-ug8vUM=',
    address: 'fra1p07sd3ptercwqkr9zw7e34jwsc0sejjte6kw9u0vtm2046puh4pszj0rs4',
    keypair: XfrKeyPair { ptr: 2425208 },
    privateStr: 'epG6XtjssaZdyCyRwijTQM92ptqyScZRtMz1lpRC-O8='
  }
]

```

输出显示了你的新助记词(见下文):

```
output insect settle weather spray lava seven day rice swamp captain upgrade layer ocean century kitten feel crunch fly huge power divert amused fitness
```

以及地址和2个密钥(见下文):

```js
    publickey: 'C_0GxCvI8OBYZRO9mNZOhh8MykvOrOLx7F7U-ug8vUM='
    address: 'fra1p07sd3ptercwqkr9zw7e34jwsc0sejjte6kw9u0vtm2046puh4pszj0rs4'
    privateStr: 'epG6XtjssaZdyCyRwijTQM92ptqyScZRtMz1lpRC-O8='
```

记得将这些内保保存起来，以备以后使用。

**3.** **"恢复钱包" 命令**

要**恢复**之前创建的钱包(例如将其用作**交易发起者**,或检查其私钥)，运行` yarn restoreWallet——mnemonicString='XX XXXX `，并提供一个有效的记忆字符串。

以恢复上例中创建的钱包为例，运行如下命令:

```bash
yarn restoreWallet --mnemonicString="output insect settle weather spray lava seven day rice swamp captain upgrade layer ocean century kitten feel crunch fly huge power divert amused fitness"
```

该命令的输出如下:

```bash
[nodemon] starting `node dist/cli.js "" "" restoreWallet "--mnemonicString=output insect settle weather spray lava seven day rice swamp captain upgrade layer ocean century kitten feel crunch fly huge power divert amused fitness"`
"2021-12-08, 12:29:45 p.m." - 🚀 ~ mnemonic to be used: "output insect settle weather spray lava seven day rice swamp captain upgrade layer ocean century kitten feel crunch fly huge power divert amused fitness"
"2021-12-08, 12:29:45 p.m." - 🚀 ~ restored wallet info:  [
  {
    keyStore: Uint8Array(104) [
      207, 167, 239, 154, 208, 138, 170, 153, 134,  69,  99, 225,
       26, 112, 166,  57, 218,  63, 155,  38, 124, 243, 207, 226,
      169, 247,  70,  67, 254,  42, 232,  28,  48, 112,  78,  83,
      118,  26, 218,  25,  84,  13, 196, 172, 127,  97,  98, 100,
      216, 129,  62,  98,  99, 135,  99,  25, 113,  99,  36, 134,
      155, 184, 254, 153,   1, 115,  38, 213,  30, 202,  68, 166,
       30,  99, 121, 118, 219, 111, 250, 202, 135, 116, 151,  66,
      189,  50, 242, 140, 209, 202,  89, 252,   2, 184, 102, 205,
      110, 219, 225,  60,
      ... 4 more items
    ],
    publickey: 'C_0GxCvI8OBYZRO9mNZOhh8MykvOrOLx7F7U-ug8vUM=',
    address: 'fra1p07sd3ptercwqkr9zw7e34jwsc0sejjte6kw9u0vtm2046puh4pszj0rs4',
    keypair: XfrKeyPair { ptr: 2424840 },
    privateStr: 'epG6XtjssaZdyCyRwijTQM92ptqyScZRtMz1lpRC-O8='
  }
]


```

在这里，您可以验证来自恢复的钱包的数据(其地址、私钥和公钥)是否与来自` createWallet `命令输出的值相同——因为我们使用了相同的助记词，该助记词是自动生成的，并用于创建一个新的钱包。
