# BSC 驗證節點

BNB48 是BNB Smart Chain上最早的社區驗證節點。[質押給BNB48](https://www.bnbchain.org/en/staking/validator/bva1ygrhjdjfyn2ffh5ha5llf5g6l3wxjt29hz9q4s)

我們提供BNB Smart Chain RPC服務：

```
https://rpc-bsc.bnb48.club \\ mainnet
wss://rpc-bsc.bnb48.club
https://testnet-rpc-bsc.bnb48.club \\testnet
```

发送至主网RPC服务的交易默认以隐私模式处理，所有通過此服務提交的tx僅會被BNB48及合作驗證節點打包，且被打包前不會對外廣播。

#### 優點:&#x20;

1. 由於交易不會被廣播，因此也不會被搶跑(三明治攻擊).
2. 完全兼容標準RPC協議，不需要編寫程序調用，直接填寫到錢包RPC URL即可使用。

#### 缺點:&#x20;

1. 需要等待BNB48与合作驗證節點打包出塊，確認速度會略慢。
2. 有最低gasprice要求，[#cha-xun-zui-di-gasprice-yao-qiu](enhancedrpc/api-reference.md#cha-xun-zui-di-gasprice-yao-qiu "mention")
