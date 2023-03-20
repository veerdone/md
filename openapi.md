用户通过 AccessKey 和 SecretKey 访问api

AccessKey 和 SecreKey 通过注册用户后自动生成，一个用户拥有一对访问密钥

管理员可以对接口进行上线和下线处理，查询 Api 调用次数，修改 Api 详细信息，管理用户，限制每天的调用次数

当用户被禁用后，禁用用 AccessKey 和 SecreKey 进行访问



用户

- 注册、登陆
- 查看 AccessKey 和 SecreKey
- 查看 APi 接口及接口详细

管理员

- 登陆
- 管理用户
- 管理 Api



调用 Api 流程

- 用 AccessKey 和 SecreKey 进行签名
- 校验 Api 调用次数是否超出上限
- 校验调用签名是否合法
- 校验参数
- 进行调用，统计次数等调用信息
- 返回结果



如何进行签名和验证 - 参考腾讯云

- 请求头
	- A-Action 接口名称
	- A-Timstamp UNIX 时间戳,精确到秒
	- Authorization 签名

Authorization

`AccessKey,Date,SignedHeaders,Signature`



Signautre

```
SecretDate = HmacSha256(SecretKey,Date)
SecretApi = HmacSha256(SecretDate, "open-api")
Signautre = HexEncode(HmacSha256(SecretApi, CanonicalRequest))
```



CanonicalRequest

HTTPRequestMethod + '\n' + CanonicalHeaders + '\n' + SignedHeaders + \'\n' + HashedRequestPayload

| 字段名称             | 解释                                                         |
| -------------------- | ------------------------------------------------------------ |
| HTTPRequestMethod    | 请求方法                                                     |
| CanonicalHeaders     | 参与签名头部信息，默认为Content-Type和Host，转为<br />头部之间使用 \n 拼接<br />例：`Content-Type:application/json; charset=utf8\nHost:api.xxxx.com` |
| SignedHeaders        | 参与签名的头部信息，说明哪些头部参与了签名，默认为Content-Type和Host<br />多个头部之间使用`;`分隔<br />例：`Content-Type;Host` |
| HashedRequestPayload | 请求体的哈希值，先对请求体进行 SHA256 哈希，然后进行十六进制编码 |

