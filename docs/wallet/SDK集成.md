---
sidebar_position: 1
---

## SDK集成

本指南将使开发者能够将Findora协议特性集成到他们的钱包中，并讨论了这个过程的两个关键部分:

- 安装 `Findora SDK`
- 设置 `Findora SDK`
- 使用最常见的钱包功能

通过遵循本指南，开发者应该能够将Findora功能（如将FRA发送到另一个钱包、检查FRA余额等）集成到他们的加密钱包、加密交易所或DApp中。

## **安装 `Findora SDK`**

要安装' Findora SDK '，我们只需要运行一个命令:  

```bash
yarn add @findora-network/findora-sdk.js
```

## 设置 **`Findora SDK`**[#](https://wiki.findora.org/docs/wallet/sdk_integration#setting-up-the-findora-sdk)

- **1.设置 Findora SDK**
    
  ```jsx
  // First, we need to import the findora sdk package
  const findoraSdk = await import("@findora-network/findora-sdk.js");

  // Before using the sdk, we need to configure it so it knows which server to connect to
  const sdkEnv = { hostUrl: "http://127.0.0.1" };
  const { Sdk } = findoraSdk;
  Sdk.default.init(sdkEnv);

  // That is it! after that Findora sdk is ready to use!
  ```

## **使用最常用的钱包功能**[#](https://wiki.findora.org/docs/wallet/sdk_integration#using-the-most-commonly-called-wallet-features)

- **2. 创建Findora 钱包**
    
  ```jsx
  // Note: The SDK must be setup before you can proceed with the instructions below (i.e. see step #1)

  // To create an account we would use one of the Findora SDK APIs.
  // In this example, we use Keypair API to create a Findora wallet
  const { Keypair: KeypairApi } = findoraSdk.Api;

  // This wallet info below will be used for any operation related to the SDK
  const walletInfo = await KeypairApi.createKeypair(password);
  ```
  
- **3. 恢复以前创建的Findora钱包**
    
  以前创建的钱包可以使用助记符或私钥恢复。 可参考以下两个例子。

- Restore from a mnemonic

  ```jsx
  // Note: The SDK must be setup before you can proceed with the instructions below (i.e. see step #1)
  const walletInfo = await KeypairApi.restoreFromMnemonic(
    yourMnenomic,
    password
  );
  ```

- Restore from a private key

  ```jsx
  // Note: The SDK must be setup before you can proceed with the instructions below (i.e. see step #1)
  const anotherWalletInfo = await KeypairApi.restoreFromPrivateKey(
    pkey,
    password
  );
  ```

- **4. 向另一钱包发送FRA**
    
  ```jsx
  // Note: The SDK must be setup before you can proceed with the instructions below (i.e. see step #1)

  const {
    Keypair: KeypairApi,
    Asset: AssetApi,
    Transaction: TransactionApi,
  } = findoraSdk.Api;

  // First step is to restore your wallet using a mnemonic phrase or, alternatively, a private key
  // a. Restore from mnemonic
  const walletInfo = await KeypairApi.restoreFromMnemonic(
    yourMnenomic,
    password
  );
  // b. Or, alternatively, restore from private key
  const anotherWalletInfo = await KeypairApi.restoreFromPrivateKey(
    pkey,
    password
  );

  // We are going to use a native FRA asset code since we are sending FRA
  const assetCode = await AssetApi.getFraAssetCode();

  // Next, we create an instance of the transaction builder - that is what is going to be broadcast to the network
  const transactionBuilder = await TransactionApi.sendToAddress(
    walletInfo,
    address,
    numbers,
    assetCode,
    assetBlindRules
  );

  // Finally, we will broadcasting this transaction to the network
  const resultHandle = await TransactionApi.submitTransaction(
    transactionBuilder
  );
  ```
    
- **5. 向多个钱包发送FRA**
    
```jsx
  // Note: The SDK must be setup before you can proceed with the instructions below (i.e. see step #1)

  const {
    Keypair: KeypairApi,
    Asset: AssetApi,
    Transaction: TransactionApi,
  } = findoraSdk.Api;

  // The first step is to restore your wallet using a mnemonic phrase, or, alternatively, a private key
  // a. Restore from mnemonic
  const walletInfo = await KeypairApi.restoreFromMnemonic(
    yourMnenomic,
    password
  );
  // b. Or, alternatively, restore from private key
  const anotherWalletInfo = await KeypairApi.restoreFromPrivateKey(
    pkey,
    password
  );

  // We are going to use a native FRA asset code, since we are sending FRA
  const assetCode = await AssetApi.getFraAssetCode();

  // Specify how much FRA to send to each recipient
  const numbersForAlice = "0.1";
  const numbersForPeter = "0.2";

  // Use `getAddressPublicAndKey` helper to prepare an object with an address and public key,
  // as both values would be needed by the transaction api
  const aliceWalletInfo = await KeypairApi.getAddressPublicAndKey(aliceAddress);
  const peterWalletInfo = await KeypairApi.getAddressPublicAndKey(peterAddress);

  // Prepare the receipient data
  const recieversInfo = [
    { reciverWalletInfo: aliceWalletInfo, amount: numbersForAlice },
    { reciverWalletInfo: peterWalletInfo, amount: numbersForPeter },
  ];

  // Create an instance of the transaction builder - that is what is going to be broadcast to the network
  const transactionBuilder = await TransactionApi.sendToMany(
    walletInfo,
    recieversInfo,
    assetCode,
    assetBlindRules
  );

  // Finally, broadcast this transaction to the network
  const resultHandle = await TransactionApi.submitTransaction(
    transactionBuilder
  );
  ```

    
- **6. 创建一个自定义的资产**
    
  ```jsx
  // Note: The SDK must be setup before you can proceed with the instructions below (i.e. see step #1)

  const {
    Keypair: KeypairApi,
    Asset: AssetApi,
    Transaction: TransactionApi,
  } = findoraSdk.Api;

  // First step is to restore your wallet using a mnemonic phrase or, alternatively, a private key
  // a. Restore from mnemonic
  const walletInfo = await KeypairApi.restoreFromMnemonic(
    yourMnenomic,
    password
  );
  // b. Or, alternatively, restore from private key
  const anotherWalletInfo = await KeypairApi.restoreFromPrivateKey(
    pkey,
    password
  );

  // Generate a random asset code to ensure it is unique and it does not yet exist
  // It should return something like 'W4-AkZy73UGA6z8pUTv4YOjRNuFwD03Bpk0YPJkKPzs=' (i.e. it will be random)
  // but it follows a certain format and that is why we need to use that helper
  const tokenCode = await AssetApi.getRandomAssetCode();

  // Then we create an instance of the transaction builder - that is what is going to be broadcast to the network
  const assetBuilder = await AssetApi.defineAsset(walletInfo, tokenCode);

  // Finally, broadcast this transaction to the network
  const resultHandle = await TransactionApi.submitTransaction(assetBuilder);
  ```
    
- **7. 发行资产（注意：创建新资产后不会有余额，你必须先 "发行"该资产)**

  ```jsx
  // Note: The SDK must be setup before you can proceed with the instructions below (i.e. see step #1)

  const {
    Keypair: KeypairApi,
    Asset: AssetApi,
    Transaction: TransactionApi,
  } = findoraSdk.Api;

  // First, restore your wallet using a mnemonic phrase or, alternatively, a private key
  // a. Restore from mnemonic
  const walletInfo = await KeypairApi.restoreFromMnemonic(
    yourMnenomic,
    password
  );
  // b. Or, alternatively, restore from private key
  const anotherWalletInfo = await KeypairApi.restoreFromPrivateKey(
    pkey,
    password
  );

  // Pass the asset code which you want to issue
  const tokenCode = "W4-AkZy73UGA6z8pUTv4YOjRNuFwD03Bpk0YPJkKPzs=";

  // Specify how much of the asset to issue?
  const amountToIssue = "5000";

  // Optionally, you can hide the transaction amount (i.e. the issued amount would be hidden on the block explorer)
  // For that we use AssetBlindRules - but again, it is completely optional!
  const assetBlindRules = { isAmountBlind: true };

  // Create an instance of the transaction builder - this is what will be broadcast to the network
  const issueAssetBuilder = await AssetApi.issueAsset(
    walletInfo,
    tokenCode,
    amountToIssue,
    assetBlindRules
  );

  // Finally, broadcasting the transaction to the network
  const resultHandle = await TransactionApi.submitTransaction(
    issueAssetBuilder
  );
  ```

- **8. 发送自定义资产到另一个钱包**
    
  ```jsx
  // Note: The SDK must be setup before you can proceed with the instructions below (i.e. see step #1)

  // This is nearly identical to sending FRA but use the custom asset code instead
  // (i.e. 'W4-AkZy73UGA6z8pUTv4YOjRNuFwD03Bpk0YPJkKPzs=') instead of using FRA asset code from AssetApi.getFraAssetCode()
  // See details below:

  const {
    Keypair: KeypairApi,
    Asset: AssetApi,
    Transaction: TransactionApi,
  } = findoraSdk.Api;

  // First, restore your wallet using a mnemonic phrase or, alternatively, a private key
  // a. Restore from mnemonic
  const walletInfo = await KeypairApi.restoreFromMnemonic(
    yourMnenomic,
    password
  );
  // b. Or, alternatively, restore from private key
  const anotherWalletInfo = await KeypairApi.restoreFromPrivateKey(
    pkey,
    password
  );

  // Below is the only difference from sending FRA - the rest is exactly the same
  // const assetCode = await AssetApi.getFraAssetCode(); // <- that is to send FRA
  const assetCode = "W4-AkZy73UGA6z8pUTv4YOjRNuFwD03Bpk0YPJkKPzs="; // <- that is to send this particular asset

  // Create an instance of the transaction builder - that is what will be broadcast to the network
  const transactionBuilder = await TransactionApi.sendToAddress(
    walletInfo,
    address,
    numbers,
    assetCode,
    assetBlindRules
  );

  // Finally, broadcasting this transaction to the network
  const resultHandle = await TransactionApi.submitTransaction(
    transactionBuilder
  );
  ```
    
- **9. 秘密发送代币**
    
    ```jsx
  // Note: The SDK must be setup before you can proceed with the instructions below (i.e. see step #1)

  const {
    Keypair: KeypairApi,
    Asset: AssetApi,
    Transaction: TransactionApi,
  } = findoraSdk.Api;

  // First, restore your wallet using a mnemonic phrase or, alternatively, a private key
  // a. Restore from mnemonic
  const walletInfo = await KeypairApi.restoreFromMnemonic(
    yourMnenomic,
    password
  );
  // b. Or, alternatively, restore from private key
  const anotherWalletInfo = await KeypairApi.restoreFromPrivateKey(
    pkey,
    password
  );

  // Use the native FRA asset code
  const assetCode = await AssetApi.getFraAssetCode(); // <- that is to send FRA
  // or, if we want to send custom asset, we would use smth like this
  // const assetCode = 'W4-AkZy73UGA6z8pUTv4YOjRNuFwD03Bpk0YPJkKPzs='; // <- that is to send this particular asset

  // Specify how much of this asset to send to recipients
  const numbersForAlice = "0.1";
  const numbersForPeter = "0.2";

  // We use `getAddressPublicAndKey` helper to prepare an object with an address and public key
  // as both values would be needed by the transaction api
  const aliceWalletInfo = await KeypairApi.getAddressPublicAndKey(aliceAddress);
  const peterWalletInfo = await KeypairApi.getAddressPublicAndKey(peterAddress);

  // Preparing receipients' data
  const recieversInfo = [
    { reciverWalletInfo: aliceWalletInfo, amount: numbersForAlice },
    { reciverWalletInfo: peterWalletInfo, amount: numbersForPeter },
  ];

  // Define this transaction to have a confidential value, or asset type, or both
  // For that we are creating an object with AssetBlindRules.
  // In this example we are only "hiding" the amount, and keeping the type visible.
  const assetBlindRulesForSend = { isTypeBlind: false, isAmountBlind: true };
  // If you do not want to enable a particular asset blind rule, (i.e. you are not hiding asset type)
  // then you can drop this option and keep as below:
  // const assetBlindRulesForSend = { isAmountBlind: true };

  // Create an instance of the transaction builder - that is what will be broadcast to the network
  const transactionBuilder = await TransactionApi.sendToMany(
    walletInfo,
    recieversInfo,
    assetCode,
    assetBlindRules
  );

  // Finally, broadcast this transaction to the network
  const resultHandle = await TransactionApi.submitTransaction(
    transactionBuilder
  );
  ```
    
- **10.检查代币余额**
    
    ```jsx
  // Note: The SDK must be setup before you can proceed with the instructions below (i.e. see step #1)

  const {
    Keypair: KeypairApi,
    Asset: AssetApi,
    Transaction: TransactionApi,
  } = findoraSdk.Api;

  // First, restore your wallet using a mnemonic phrase or, alternatively, a private key
  // a. Restore from mnemonic
  const walletInfo = await KeypairApi.restoreFromMnemonic(
    yourMnenomic,
    password
  );
  // b. Or, alternatively, restore from private key
  const anotherWalletInfo = await KeypairApi.restoreFromPrivateKey(
    pkey,
    password
  );

  // By default, `getBalance` checks for FRA balance,
  // but we can pass an additional argument to check a custom asset balance
  const balance = await AccountApi.getBalance(walletInfo);

  const assetCode = "W4-AkZy73UGA6z8pUTv4YOjRNuFwD03Bpk0YPJkKPzs=";
  const anotherBalance = await AccountApi.getBalance(walletInfo, assetCode);
  ```
