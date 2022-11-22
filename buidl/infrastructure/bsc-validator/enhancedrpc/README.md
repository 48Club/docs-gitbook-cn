---
description: 'Foncé: [法文] 黑暗。 Puissant: [法文] 強力的。'
---

# 進階RPC服務

下列RPC服务实现了不同的高级功能，由BNB48验证节点以及合作的其他验证节点共同提供服务。

反馈问题请到[github](https://github.com/BNB48Club/enhanced\_rpc) ，验证节点商谈合作事宜请联系 _rpc@bnb48.club_

本服务为链下服务，不主动隐藏用户访问记录（包括但不限于IP地址）。

<details>

<summary>koge-rpc-bsc, 向KOGE持有者提供gas费折扣</summary>

持有 [$KOGE](https://bscscan.com/token/0xe6df05ce8c8301223373cf5b969afcb1498c5528) 即享GAS费折扣 ！

满足以下条件：&#x20;

1. 发起交易的钱包地址持有至少1 KOGE余额(包括DAO合約質押部分以及[48er-nft.md](../../../../dao/governance/48er-nft.md "mention"))。
2. 使用 RPC地址 [https://koge-rpc-bsc.bnb48.club](https://t.co/5859ob3MhI)&#x20;

即可以最低1gwei的优惠价格发送BSC交易！

请注意最低1gwei的gasPrice是有条件的，所發送的交易gasLimit越多，要求持有KOGE数量越多。

KOGE持倉計算包括：

1. 錢包中持有的KOGE
2. 質押在[Broken link](broken-reference "mention")的KOGE
3. 每持有一個48er NFT視爲持有100萬KOGE

```
KOGE      可享1gwei的gasLimit上限
<1        0
1~10      240000
100       480000
1000      960000
10000     1920000
...
//持有量每增加10倍，gasLimit翻倍
```

當您發送的交易超過您可以享受的優惠幅度，可以選擇提高gasPrice，或者持倉更多的KOGE。RPC服务会在报错信息中給出所能接受最低的gasPrice，通常參考重新设置后再发送交易即可。

由於並非所有的验证节点都接收低于5gwei的交易（BNB48及合作伙伴支持），所以较低gas的交易打包可能会稍慢，这是正常现象。

</details>

<details>

<summary>fonce-bsc, 適用於普通用戶的隐私交易服務</summary>

`https://fonce-bsc.bnb48.club`

所有通過此服務提交的tx僅會被BNB48及合作驗證節點打包，且被打包前不會對外廣播。

#### 優點:&#x20;

1. 由於交易不會被廣播，因此也不會被搶跑(三明治攻擊).
2. 完全兼容標準RPC協議，不需要編寫程序調用，直接填寫到錢包RPC URL即可使用。

#### 缺點:&#x20;

1. 需要等待BNB48与合作驗證節點打包出塊，確認速度會略慢。
2. 有最低gasprice要求，[#cha-xun-zui-di-gasprice-yao-qiu](api-reference.md#cha-xun-zui-di-gasprice-yao-qiu "mention")

</details>

<details>

<summary>puissant-bsc, 適用於專業人員的群发交易服務</summary>

`https://puissant-bsc.bnb48.club`

Puissant 服务可以一次接收一组tx，并在保持gasPrice優先排序的前提下，将一组tx按照原子操作打包。Puissant 天生是隐私服务。

必須通過程序調用，不適用於普通錢包。

接口規範參見

</details>
