---
description: 按照標準RPC接口風格設計
---

# Puissant API

{% swagger method="post" path="/" baseUrl="" summary="查詢最低GasPrice要求" %}
{% swagger-description %}
Override of original eth\_gasPrice endpoint.

重載標準eth\_gasPrice 接口.

查詢Puissant服務接受的最低GasPrice要求. 發送到Puissant服務的Tx如果GasPrice低於此要求會被拒絕.
{% endswagger-description %}

{% swagger-parameter in="header" name="Content-Type" type="String" required="true" %}
`"application/json"`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="id" type="uint64" required="true" %}
`73`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="jsonrpc" type="String" required="true" %}
`"2.0"`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="method" type="String" required="true" %}
`"eth_gasPrice"`
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    "jsonrpc":"2.0",
    "id":1337,
    "result":"0x37e11d600"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="/" baseUrl="" summary="發送隱私交易" %}
{% swagger-description %}
提交隱私交易。

所有通過此服務提交的tx僅會被BNB48驗證節點打包，且被打包前不會對外廣播。
{% endswagger-description %}

{% swagger-parameter in="header" name="Content-Type" type="String" required="true" %}
`"application/json"`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="id" type="uint64" required="true" %}
1
{% endswagger-parameter %}

{% swagger-parameter in="body" name="jsonrpc" required="true" type="String" %}
`"2.0"`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="method" required="true" type="String" %}
`"eth_sendPrivateRawTransaction"`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="params" required="true" type="[]Tx" %}
Signed transaction (eth_sendRawTransaction style, signed and RLP-encoded)
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="成功" %}
```javascript
{
    "id": 1,
    "jsonrpc": "2.0",
    "result": "0xdeadbeef883764809a94a5320e4557102f5a3fdd98dabd8cd48773b0eca00666" // tx hash
}
```
{% endswagger-response %}

{% swagger-response status="200: OK" description="失敗" %}
```javascript
{
    "id": 1,
    "jsonrpc": "2.0",
    "result": "0x" // error for hex
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="/" baseUrl="" summary="發送Puissant" %}
{% swagger-description %}
發送一組交易，即Puissant。

Puissant中的交易必須按照gasPrice降序排列。tx最終在block中的打包順序與在Puissant中排列順序一致，但僅保證gasPrice完全相同的交易連續。

Puissant中首個tx的gasPrice必須滿足[#cha-xun-zui-di-gasprice-yao-qiu](api-reference.md#cha-xun-zui-di-gasprice-yao-qiu "mention")。

若首個tx使用gas不足21000，整個Puissant會被撤回。

發生競爭時，首個tx gasPrice更高者優先。

若多個Puissant的首個tx來自同一個sender，則僅保留該sender gas Price最高的tx所在的Puissant，其餘丟棄。
{% endswagger-description %}

{% swagger-parameter in="body" name="id" required="true" type="uint64" %}
48
{% endswagger-parameter %}

{% swagger-parameter in="body" name="jsonrpc" type="String" required="true" %}
`"2.0"`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Content-Type" required="true" %}
`"application/json"`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="method" type="String" required="true" %}
`"eth_sendPuissant"`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="params[0].txs" type="[]Tx" required="true" %}
簽名交易。(與eth_sendRawTransaction 接口參數要求一致, signed and RLP-encoded)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="params[0].maxTimestamp" type="uint64" required="true" %}
Puissant 有效時間，過期後將不被打包. 需在請求時間兩分鐘內。
{% endswagger-parameter %}

{% swagger-parameter in="body" name="params[0].acceptReverting" type="[]TxHash" required="false" %}
可接受revert的交易hash。與默認行為不同，這些交易revert將不會撤回整個puissant。
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```javascript
{
    "jsonrpc": "2.0",
    "id": 48,
    "result": null
}
```
{% endswagger-response %}

{% swagger-response status="200: OK" description="Fail" %}
```javascript
{
    "jsonrpc":"2.0",
    "id":48,
    "result": "0x" // error for hexnull
}
```
{% endswagger-response %}
{% endswagger %}
