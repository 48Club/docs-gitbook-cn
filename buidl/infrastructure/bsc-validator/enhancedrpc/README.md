---
description: 'Puissant: [法文] 強力的。'
---

# 進階RPC服務

下列RPC服务实现了不同的高级功能，由BNB48验证节点以及合作的其他验证节点共同提供服务。同样全部以隐私模式处理。

反馈问题请到[github](https://github.com/BNB48Club/enhanced\_rpc) ，验证节点商谈合作事宜请联系 _rpc@bnb48.club_

本服务为链下服务，不主动隐藏用户访问记录（包括但不限于IP地址）。

<details>

<summary>koge-rpc-bsc, 向KOGE持有者提供gas费折扣</summary>

持有 [$KOGE](https://bscscan.com/token/0xe6df05ce8c8301223373cf5b969afcb1498c5528) 即享GAS费折扣 ！

满足以下条件：&#x20;

1. 发起交易的钱包地址持有至少1 KOGE余额(包括钱包余额、DAO質押中余额以及[48er-nft.md](../../../../dao/governance/48er-nft.md "mention"))。
2. 使用 RPC地址 [https://koge-rpc-bsc.bnb48.club](https://t.co/5859ob3MhI)&#x20;

即可以最低1gwei的优惠价格发送BSC交易！

请注意最低1gwei的gasPrice是有条件的，所發送的交易gasLimit越多，要求持有KOGE数量越多。

KOGE有效持倉计算以下三部分之和：

1. 錢包中持有的KOGE，超过10 KOGE按10 KOGE计算。
2. 質押在[Broken link](broken-reference "mention")的KOGE
3. 每持有一個48er NFT視爲持有100萬KOGE

```
KOGE有效持仓 = (Koge余额，最大10 Koge) + DAO质押中Koge + 48erNFT数量 * 1,000,000

KOGE有效持仓   可享1gwei的gasLimit上限
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

<summary>puissant-bsc, 適用於專業人員的群发交易服務</summary>

`https://puissant-bsc.bnb48.club`

Puissant 服务可以一次接收一组tx，并在保持gasPrice優先排序的前提下，将一组tx按照原子操作打包。Puissant 天生是隐私服务。

必須通過程序調用，不適用於普通錢包。

接口規範參見

</details>
