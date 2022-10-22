---
description: 'Foncé: [法文] 黑暗。 Puissant: [法文] 強力的。'
---

# 進階RPC服務

本服务为链下服务，不会隐藏用户访问足迹（包括但不限于IP地址）。

<details>

<summary>適用於終端用戶的Foncé服務</summary>

Mainnet: `https://fonce-bsc.bnb48.club`

Testnet: `https://testnet-fonce-bsc.bnb48.club`

所有通過此服務提交的tx僅會被BNB48驗證節點打包，且被打包前不會對外廣播。

#### 優點:&#x20;

1. 由於交易不會被廣播，因此也不會被搶跑(三明治攻擊).
2. 完全兼容標準RPC協議，不需要編寫程序調用，直接填寫到錢包RPC URL即可使用。

#### 缺點:&#x20;

1. 需要等待BNB48驗證節點打包出塊，確認速度會略慢。
2. 有最低gasprice要求，目前為60gwei。可以通過 eth\_gasPrice 查詢

</details>

<details>

<summary>適用於專業人員的Puissant服務</summary>

`https://puissant-bsc.bnb48.club //mainnet`

`https://testnet-puissant-bsc.bnb48.club // testnet`

保持gasPrice優先排序的前提下，提供Group交易服務。

必須通過程序調用，不適用於普通錢包。

接口規範參見[api-reference.md](api-reference.md "mention")

</details>

Bug追踪 [https://github.com/BNB48Club/puissant\_service](https://github.com/BNB48Club/puissant\_service)
