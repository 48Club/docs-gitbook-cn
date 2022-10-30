---
description: 'Foncé: [法文] 黑暗。 Puissant: [法文] 強力的。'
---

# 進階RPC服務

下列RPC服务实现了不同的高级功能，由BNB48验证节点以及合作的其他验证节点共同提供服务。

反馈问题请到[github](https://github.com/BNB48Club/enhanced\_rpc) ，验证节点商谈合作事宜请联系 _rpc@bnb48.club_

本服务为链下服务，不主动隐藏用户访问记录（包括但不限于IP地址）。

<details>

<summary>koge-rpc-bsc, 向KOGE持有者提供gas费折扣</summary>

持有 1 [$KOGE](https://bscscan.com/token/0xe6df05ce8c8301223373cf5b969afcb1498c5528) 即享GAS费折扣 ！

满足以下条件：&#x20;

1. 发起交易的钱包地址持有KOGE余额。
2. 使用 RPC地址 [http://koge-rpc-bsc.bnb48.club](https://t.co/5859ob3MhI)&#x20;

即可以优惠价格发送BSC交易！持有KOGE数量越多，可享受的折扣越多。

请注意最低1gwei的gasPrice是有条件的，如果发送的tx消耗过多的gas，所需要的gasPrice可能会高于1gwei 。遇到这种情况时，RPC服务会在报错信息中包含推荐的gasPrice，以此重新设置后再发送交易即可。

另外，并不是所有的验证节点都接收低于5gwei的交易（BNB48及合作伙伴支持），所以较低gas的交易打包可能会稍慢，这是正常现象。

**KOGE持仓与gas折扣的关系：**

* 持有至少1 Koge但不超过100，可以1gwei(即比原价5gwei优惠80%)发送gasLimit不超过24万的交易，也可以选择3gwei(即比原价5gwei优惠40%)发送gasLimit不超过48万的交易。即总优惠量不变，优惠越多，允许的gasLimit越小；优惠较少，就可以允许更大的gasLimit。

<!---->

* Koge持仓每扩大10倍，允许的优惠翻倍。例如持有1000Koge，可以1gwei发送gasLimit不超过48万的交易；持有10000Koge，可以1gwei发送gasLimit不超过96万的交易。

</details>

<details>

<summary>fonce-bsc, 適用於普通用戶的隐私交易服務</summary>

Mainnet: `https://fonce-bsc.bnb48.club`

Testnet: `https://testnet-fonce-bsc.bnb48.club`

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

`https://puissant-bsc.bnb48.club //mainnet`

`https://testnet-puissant-bsc.bnb48.club // testnet`

Puissant 服务可以一次接收一组tx，并在保持gasPrice優先排序的前提下，将一组tx按照原子操作打包。Puissant 天生是隐私服务。

必須通過程序調用，不適用於普通錢包。

接口規範參見[api-reference.md](api-reference.md "mention")

</details>
