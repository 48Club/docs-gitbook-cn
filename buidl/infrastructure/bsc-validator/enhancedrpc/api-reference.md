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

{% swagger-response status="200: OK" description="Success" %}
```javascript
{
    "id": 1,
    "jsonrpc": "2.0",
    "result": "0xdeadbeef883764809a94a5320e4557102f5a3fdd98dabd8cd48773b0eca00666" // tx hash
}
```
{% endswagger-response %}

{% swagger-response status="200: OK" description="Fail" %}
```javascript
{
    "id": 1,
    "jsonrpc": "2.0",
    "result": "0x" // error for hex
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="/" baseUrl="" summary="發送捆綁交易" %}
{% swagger-description %}
發送一組捆綁交易，即Bundle。

Bundle被打包時，其中交易會按照發送時的順序被打包，但僅其中gasPrice完全相同的交易保證position連續。

Bundle中所有交易的平均gasPrice必須滿足最低GasPrice要求。注意計算平均gasPrice時，所有gasPrice大於最低gasPrice要求的tx其gasLimit一律按照21000計算。詳細計算公式如下：

$$\begin{equation} average\_gasPrice = \frac{\sum(gasPrice_i \times e\_gasLimit_i)}{\sum(e\_gasLimit_i)} \end{equation}$$

其中

The trust-relay bundle has the higher priority than the normal bundle.

$$\begin{equation} e\_gasLimit_i= \left\{  \begin{aligned} gasLimit_i\qquad if\quad gasPrice_i \le gasPriceFloor   \\ min(21000,gasLimit_i)\qquad if\quad gasPrice_i \gt gasPriceFloor   \\ \end{aligned} \right. \end{equation}$$

發生競爭時，平均gasPrice更高者優先。
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
Signed transactions (eth_sendRawTransaction style, signed and RLP-encoded)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="params[0].maxTimestamp" type="uint64" required="true" %}
bundle valid until timestamp reaches. No more than 2 minutes from now
{% endswagger-parameter %}

{% swagger-parameter in="body" name="params[0].acceptReverting" type="[]TxHash" required="false" %}
The array of hash indicated which transaction(s) are allowed to revert
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
