# uniswap_notes
对接uniswap 相关事情

## 内容

    本文分成两部分内容，第一部分是对接uniswap 的工程，第二部分是将token发布到uniswap上面，所以接下来会分成这两部分和读者介绍。


### 对接uniswap 

首先前置内容是有关于token的知识，这里就不再解释，不熟悉的同学可以谷歌一下相关的内容。本部分主要内容主要谈论如何利用智能合约，利用hardhat 将智能合约部署到uniswap 上。首先我们聚焦于uniswap 的机制，并从中窥探其中的商业价值【说的有点大】。

    Uniswap是一种用于交换加密货币的分散金融协议。Uniswap也是最初构建Uniswap协议的公司的名称。该协议通过使用智能合约促进以太坊区块链上的加密货币令牌之间的自动化交易。截至2020年10月，按每日交易量计算，Uniswap估计是最大的分散交易所和第四大加密货币交易所。

以上的文字摘自维基百科，uniswap 既是一种协议，又是交易所的名称，所以这里读者需要注意。这一部分对应工程：`deploy-uniswapv3-template`。该工程使用hardhat 作为技术栈，hardhat可以理解为一套解决方案，集成对应的配置进而使得有能力对接智能合约。

文件夹目录中，在contracts中首先写合约相关的代码， 配置项在config系列文件中，举例说：
```js
const config: HardhatUserConfig = {
  defaultNetwork: "hardhat",
  networks: {
    hardhat: {
      accounts: {
        mnemonic: MNEMONIC,
      },
      chainId: chainIds.hardhat,
    },
    mainnet: createTestnetConfig("mainnet"),
    goerli: createTestnetConfig("goerli"),
    kovan: createTestnetConfig("kovan"),
    rinkeby: createTestnetConfig("rinkeby"),
    ropsten: createTestnetConfig("ropsten"),
  },
  solidity: {
    compilers: [
      {
        version: '0.7.6',
        settings: {
          optimizer: {
            enabled: true,
            runs: 800,
          },
          metadata: {
            // do not include the metadata hash, since this is machine dependent
            // and we want all generated code to be deterministic
            // https://docs.soliditylang.org/en/v0.7.6/metadata.html
            bytecodeHash: 'none',
          },
        }
      },
    ],
  },
  etherscan: {
    apiKey: ETHERSCAN_API_KEY,
  },
};
```

network 是要部署的网络，部署的网络是智能合约要发布的地址。

### interface 搭建