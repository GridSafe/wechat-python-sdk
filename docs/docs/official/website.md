# 官方接口 - 微信网页开发

## 生成授权页URL

**调用方法：**`.generate_oauth2_authorize_url(redirect_uri, response_type="code", scope="snsapi_userinfo", state="")`

**参数：**

* `redirect_uri`：授权后重定向的回调连接地址
* `response_type`：返回类型
* `scope`：应用授权作用域, 目前必须为code
* `state`：重定向后会带上state参数，开发者可以填写a-zA-Z0-9的参数值，最多128字节

**返回值：**字符串对象, 为生成的授权页URL：

```
https://open.weixin.qq.com/connect/oauth2/authorize?appid=wxf0e81c3bee622d60&redirect_uri=http%3A%2F%2Fnba.bluewebgame.com%2Foauth_response.php&response_type=code&scope=snsapi_userinfo&state=STATE#wechat_redirect
```

**异常：**当发生失败时抛出 [`exceptions.OfficialAPIError`](/api/exception.md#officialapierror) 异常，该异常包含了错误的代号与原因信息。

**对应的官方文档：**[用户同意授权, 获取code](http://mp.weixin.qq.com/wiki/17/c0f37d5704f0b64713d5d2c37b468d75.html#.E7.AC.AC.E4.B8.80.E6.AD.A5.EF.BC.9A.E7.94.A8.E6.88.B7.E5.90.8C.E6.84.8F.E6.8E.88.E6.9D.83.EF.BC.8C.E8.8E.B7.E5.8F.96code)

## 一步网页授权获取用户基本信息

**调用方法：**`.get_oauth2_userinfo_one_step(code)`

**参数：**

* `code`：通过OAuth2 Authorization获取的code参数

**调用前检查：**App ID / App Secret

**返回值：**正常返回官方接口的 JSON 数据：

```json
{
    "openid":" OPENID",
    " nickname": NICKNAME,
    "sex":"1",
    "province":"PROVINCE"
    "city":"CITY",
    "country":"COUNTRY",
    "headimgurl":    "http://wx.qlogo.cn/mmopen/g3MonUZtNHkdmzicIlibx6iaFqAc56vxLSUfpb6n5WKSYVY0ChQKkiaJSgQ1dZuTOgvLLrhJbERQQ4eMsv84eavHiaiceqxibJxCfHe/46", 
    "privilege":[
        "PRIVILEGE1"
        "PRIVILEGE2"
    ],
    "unionid": "o6_bmasdasdsad6_2sgVt7hMZOPfL"
}
```

**异常：**当发生失败时抛出 [`exceptions.OfficialAPIError`](/api/exception.md#officialapierror) 异常，该异常包含了错误的代号与原因信息。

**对应的官方文档：**[网页授权获取用户基本信息](http://mp.weixin.qq.com/wiki/17/c0f37d5704f0b64713d5d2c37b468d75.html)

## 通过code换取网页授权access_token

**调用方法：**`.get_oauth2_access_token(code, grant_type="authorization_code")`

**参数：**

* `code`：通过OAuth2 Authorization获取的code参数
* `grant_type`：授权类型, 在此必须为authorization_code

**调用前检查：**App ID / App Secret

**返回值：**正常返回官方接口的 JSON 数据：

```json
{
   "access_token":"ACCESS_TOKEN",
   "expires_in":7200,
   "refresh_token":"REFRESH_TOKEN",
   "openid":"OPENID",
   "scope":"SCOPE",
   "unionid": "o6_bmasdasdsad6_2sgVt7hMZOPfL"
}
```

**异常：**当发生失败时抛出 [`exceptions.OfficialAPIError`](/api/exception.md#officialapierror) 异常，该异常包含了错误的代号与原因信息。

**对应的官方文档：**[通过code换取网页授权access_token](http://mp.weixin.qq.com/wiki/17/c0f37d5704f0b64713d5d2c37b468d75.html#.E7.AC.AC.E4.BA.8C.E6.AD.A5.EF.BC.9A.E9.80.9A.E8.BF.87code.E6.8D.A2.E5.8F.96.E7.BD.91.E9.A1.B5.E6.8E.88.E6.9D.83access_token)

## 刷新access_token（如果需要）

**调用方法：**`.refresh_oauth2_acccess_token(refresh_token, grant_type="refresh_token")`

**参数：**

* `refresh_token`：通过access_token获取到的refresh_token参数
* `grant_type`：授权类型, 在此必须为refresh_token

**调用前检查：**App ID / App Secret

**返回值：**正常返回官方接口的 JSON 数据：

```json
{
   "access_token":"ACCESS_TOKEN",
   "expires_in":7200,
   "refresh_token":"REFRESH_TOKEN",
   "openid":"OPENID",
   "scope":"SCOPE"
}
```

**异常：**当发生失败时抛出 [`exceptions.OfficialAPIError`](/api/exception.md#officialapierror) 异常，该异常包含了错误的代号与原因信息。

**对应的官方文档：**[刷新access_token（如果需要）](http://mp.weixin.qq.com/wiki/17/c0f37d5704f0b64713d5d2c37b468d75.html#.E7.AC.AC.E4.B8.89.E6.AD.A5.EF.BC.9A.E5.88.B7.E6.96.B0access_token.EF.BC.88.E5.A6.82.E6.9E.9C.E9.9C.80.E8.A6.81.EF.BC.89)

## 拉取用户信息(需scope为 snsapi_userinfo)

**调用方法：**`.get_oauth2_userinfo(access_token, openid, lang='zh_CN')`

**参数说明：**

* `access_token`：网页授权接口调用凭证, 注意：此access_token与基础支持的access_token不同
* `user_id`：用户的OpenID
* `lang`：返回国家地区语言版本，zh_CN 简体，zh_TW 繁体，en 英语

**调用前检查：**App ID / App Secret

**返回值：**正常返回官方接口的 JSON 数据：

```json
{
    "openid":" OPENID",
    " nickname": NICKNAME,
    "sex":"1",
    "province":"PROVINCE"
    "city":"CITY",
    "country":"COUNTRY",
    "headimgurl":    "http://wx.qlogo.cn/mmopen/g3MonUZtNHkdmzicIlibx6iaFqAc56vxLSUfpb6n5WKSYVY0ChQKkiaJSgQ1dZuTOgvLLrhJbERQQ4eMsv84eavHiaiceqxibJxCfHe/46", 
    "privilege":[
        "PRIVILEGE1"
        "PRIVILEGE2"
    ],
    "unionid": "o6_bmasdasdsad6_2sgVt7hMZOPfL"
}
```

**异常：**当发生失败时抛出 [`exceptions.OfficialAPIError`](/api/exception.md#officialapierror) 异常，该异常包含了错误的代号与原因信息。

**对应的官方文档：**[拉取用户信息(需scope为 snsapi_userinfo)](http://mp.weixin.qq.com/wiki/17/c0f37d5704f0b64713d5d2c37b468d75.html#.E7.AC.AC.E5.9B.9B.E6.AD.A5.EF.BC.9A.E6.8B.89.E5.8F.96.E7.94.A8.E6.88.B7.E4.BF.A1.E6.81.AF.28.E9.9C.80scope.E4.B8.BA_snsapi_userinfo.29)

## 检验授权凭证（access_token）是否有效

**调用方法：**`check_oauth2_access_token(access_token, user_id)`

**参数：**

* `access_token`：网页授权接口调用凭证, 注意：此access_token与基础支持的access_token不同
* `user_id`：用户的OpenID

**返回值：**正确时返回True, 错误时返回False

**异常：**本方法不抛出 [`exceptions.OfficialAPIError`](/api/exception.md#officialapierror) 异常

**对应的官方文档：**[检验授权凭证（access_token）是否有效](http://mp.weixin.qq.com/wiki/17/c0f37d5704f0b64713d5d2c37b468d75.html#.E9.99.84.EF.BC.9A.E6.A3.80.E9.AA.8C.E6.8E.88.E6.9D.83.E5.87.AD.E8.AF.81.EF.BC.88access_token.EF.BC.89.E6.98.AF.E5.90.A6.E6.9C.89.E6.95.88)

## 获取 jsapi ticket

**该方法仅为自行维护单机版 jsapi_ticket 使用。**

获取 jsapi_ticket 及 jsapi_ticket 过期日期, 仅供缓存使用。

**调用方法：**`.get_jsapi_ticket()`

**调用前检查：**App ID / App Secret

**返回值：**正常返回官方接口的 JSON 数据：

```json
{
    "jsapi_ticket":"bxLdikRXVbTPdHSM05e5u8EoHz_JA7Re-noqE0ZAnxk3XzAntyhT4_k272aJ4LprCM68rVXv6DDydT7JW1Mwsw",
    "jsapi_ticket_expires_at": 1454476720
}
```

**异常：**当发生失败时抛出 [`exceptions.OfficialAPIError`](/api/exception.md#officialapierror) 异常，该异常包含了错误的代号与原因信息。

**对应官方文档：**[获取jsapi ticket](http://mp.weixin.qq.com/wiki/11/74ad127cc054f6b80759c40f77ec03db.html#.E8.8E.B7.E5.8F.96api_ticket)


