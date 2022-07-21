---
description: 按照標準RPC接口風格設計
---

# Puissant API

### 查詢最低GasPrice要求

```
POST /eth_gasPrice
```

#### 功能描述

重載標準eth\_gasPrice 接口.

查詢Puissant服務接受的最低GasPrice要求. 發送到Puissant服務的Tx如果GasPrice低於此要求會被拒絕.

#### 示例

```
#Request
curl -H 'Content-Type: application/json' --data '{
    "id": 1337,
    "jsonrpc": "2.0",
    "method": "eth_gasPrice"
}' https://testnet-puissant-bsc.bnb48.club/
```

```
#Response
{
    "jsonrpc":"2.0",
    "id":1337,
    "result":"0x37e11d600" //in WEI
}
```

### 發送隱私交易

```
POST eth_sendPrivateRawTransaction
```

#### 功能描述

提交隱私交易。

所有通過此服務提交的tx僅會被BNB48驗證節點打包，且被打包前不會對外廣播。

#### 參數

| 名稱    | 類型           | 描述                                                                          |
| ----- | ------------ | --------------------------------------------------------------------------- |
| input | Array\<Data> | Signed transaction (`eth_sendRawTransaction` style, signed and RLP-encoded) |

#### 響應

```
"result" : Data - txhash or error
```

#### 示例

```
# Request
curl -X POST -H 'Content-Type: application/json' --data '{
    "id": 1337,
    "jsonrpc": "2.0",
    "method": "eth_sendPrivateRawTransaction",
    "params": [
        "0x02f8b30181b18502cb417800853a352944008307a12094b893a8049f250b57efa8c62d51527a22404d7c9a80b844095ea7b300000000000000000000000093e17e368e82dd24bed931091f831b5bed3f711effffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffc080a027148354c23bb016147ed68014b2aa13c43a4feef36274be88ef58d25f91e20fa05ccc423d4e9e1de88515adf3245df69db8c05b1ce345a738c75b06c87a96f878"
    ]
}' https://testnet-puissant-bsc.bnb48.club/

# Response
{
    "id": 1337,
    "jsonrpc": "2.0",
    "result": "0xdeadbeef883764809a94a5320e4557102f5a3fdd98dabd8cd48773b0eca00666" //tx hash
}
```

### 發送捆綁交易

```
POST eth_sendPuissant
```

#### 功能描述

發送一組捆綁交易，即Bundle。

Bundle被打包時，其中交易會按照發送時的順序被打包，但僅其中gasPrice完全相同的交易保證position連續。

Bundle中所有交易的平均gasPrice必須滿足[#cha-xun-zui-di-gasprice-yao-qiu](api-reference.md#cha-xun-zui-di-gasprice-yao-qiu "mention")。注意計算平均gasPrice時，所有gasPrice大於最低gasPrice要求的tx其gasLimit一律按照21000計算。詳細計算公式如下：

$$
平均\ gasPrice = \frac{\sum(gasPrice_i
\times e\_gasLimit_i)}{\sum(e\_gasLimit_i)}
$$

其中

$$
\begin{equation}
e\_gasLimit_i= \left\{ 
\begin{aligned}
gasLimit_i\qquad 當\quad gasPrice_i \le gasPriceFloor   \\
21000\qquad 當\quad gasPrice_i \gt gasPriceFloor   \\
\end{aligned}
\right.
\end{equation}
$$

#### 參數

| 參數名稱            | 參數類型                | 描述                                                                           |
| --------------- | ------------------- | ---------------------------------------------------------------------------- |
| txs             | Array\<Transaction> | Signed transactions (`eth_sendRawTransaction` style, signed and RLP-encoded) |
| maxTimestamp    | uint64              | bundle valid until timestamp reaches. No more than 2 minutes from `now`      |
| acceptReverting | Array\<Hash>        | The array of hash indicated which transaction(s) are allowed to revert       |

#### 示例

```
#Request
curl -X POST -H 'Content-Type: application/json' --data '{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "eth_sendPuissant",
    "params": [
        {
            "txs": [
                "0xf86e6b85037e11d600830493e094b799602dde4f07869da75e795c266aab9b923c908904563918244f4000008081e5a0d68fb22cf14969482c6a9f40b4f782bc15141f5447a48060aa31d610060209a89febba553a3f06c74294e12233071427306d4e4942e2b0df0b1d545382179ab4",
                "0xf86f6c85037e11d600830493e094b799602dde4f07869da75e795c266aab9b923c908904563918244f4000008081e5a021ef8e1499c2fa2b4ed59eee86bb52196185d88b223a39e3d39d525789217433a05d3dd194887e4e6a2950dfd2b2322d56613d640f28316f07cab6f4e19dd50641"
            ],
            "maxTimestamp": 1886667078,
            "acceptReverting": ["0xefc87cd8b4eb302f6ea88502d655a184b32a41f44bcbdb744a392f032097e91e"]
        }
    ]
}' https://testnet-puissant-bsc.bnb48.club/

#Response
{
    "jsonrpc":"2.0", //echo
    "id":1, //echo
    "result":null //success
}
```
