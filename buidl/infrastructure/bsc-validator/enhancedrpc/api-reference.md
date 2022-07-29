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
一次發送一組tx，即Puissant。

Puissant中的tx必須按照gasPrice降序排列。Puissant中首個tx的gasPrice必須滿足[#cha-xun-zui-di-gasprice-yao-qiu](api-reference.md#cha-xun-zui-di-gasprice-yao-qiu "mention")，且GasLimit必须至少为21000，否则请求立刻失败。

若发生以下情时，puissant中所有交易均被丢弃。否则puissant中所有交易按顺序被打包进下一个由我们的验证节点出的块。

**CASE EXPIRED**

当前时间已经超过puissant请求时携带的`maxTimestamp`参数，或者puissant中的任一tx已经被其他块打包，则puissant失效。

**CASE INVALID**

尝试打包puissant首个tx时gasUsed不足21000，则认为puissant由于付款组不足无效。

**CASE REVERTED**

打包Puissant中任一tx失败(reverted，无论具体原因如何)，且该txHash不在puissant请求的acceptReverting参数中出现，则认为puissant整体执行失败。

**CASE BEATEN**

多个Puissant包含同个tx时，优先服务首个tx gasPrice最高的puissant，其他puissant竞价失败。



puissant中gasPrice完全一致的tx，打包时会按顺序在区块中连续的位置。gasPrice不同的tx则不保证。

若多個Puissant的首個tx來自同一個sender，视作对puissant发动攻击，僅保留該sender gas Price最高的tx所在的Puissant，其餘忽略。
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
