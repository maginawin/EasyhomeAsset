# EasyHome API For App
---

### 版本信息
- version 1.0 -- 2016/07/06
    * host: <i>http://easyhome.rgbw.hk/api/1.0</i>
    * init


- version 1.0 -- 2016/07/13
    * host: <i>http://easyhome.rgbw.hk/api/1.0</i>
    * 新增
        1. 更改密码
    * 修改
        1. 更新用户信息 - 移除更改密码
        2. 找回密码 - 更新 url

- version 1.0 -- 2016/07/15
    * host: <i>http://easyhome.rgbw.hk/api/1.0</i>
    * 新增
        1. 找回密码邮箱内容  

- version 1.0 -- 2016/07/21
    * host: <i>http://easyhome.rgbw.hk/api/1.0</i>
    * 新增
        1. 房间、场景、灯、事件信息同步
### 注意事项
- 所有字符串均使用 UTF-8 编码

---
# 一、用户

## 1. 用户注册
- **Base**
    * url: host/user/register
    * type: post
	* content-type: json


- **Request**
<dl>
<dd>{</dd>
  <dd>"version" : "1.0",</dd>
  <dd>"email" : "username@email.com",</dd>
  <dd>"password" : "123456",</dd>
  <dd>"nickname" : "Nickname"</dd>
<dd>}</dd>
</dl>
     * version: version
     * email: user email
         + _唯一，不能为空_
     * password: user password
         + _6 ~ 32 位 ASCII 字符_
     * nickname: user nickname
         + _可以为空，若为空则默认 nickname 为 "Nickname"_


- **Response**
<dl>
<dd>{</dd>
<dd>"version" : "1.0",</dd>
<dd>"status" : "0",</dd>
<dd>"email" : "username@email.com",</dd>
<dd>"nickname" : "Nickname"</dd>
<dd>}</dd>
</dl>
    * version: version
    * status: status
        + _"0": 成功_
        + _"1": 失败，邮箱已经存在_
        + _"2": 失败，服务器超时_
        + _"3": 失败，未知错误_
    * email: user email
        + _注册使用的邮箱_
    * nickname: user nickname
        + _注册使用的昵称_

## 2. 登录
- **Base**
    * url: host/user/login
    * type: post
    * content-type: json


- **Request**
<dl>
<dd>{</dd>
<dd>"version" : "1.0",</dd>
<dd>"email" : "username@email.com",</dd>
<dd>"password" : "123456"</dd>
<dd>}</dd>
</dl>
    * version: version
    * email: user email
    * password: user password


- **Response**
<dl>
<dd>{</dd>
<dd>"version" : "1.0",</dd>
<dd>"token" : "JJWT token",</dd>
<dd>"status" : "0",</dd>
<dd>"nickname" : "Nickname",</dd>
<dd>}</dd>
</dl>
    * version: version
    * token: JJWT token
        + _默认 token 有效期为三十天_
    * status: status
        + _"0": 登录成功_
        + _"1": 登录失败， 用户名不存在_
        + _"2": 登录失败， 用户密码错误_
        + _"3": 服务器错误_


## 3. 更新用户信息
- **Base**
    * url: host/user/update
    * type: post
    * content-type: json


- **Request**
<dl>
<dd>{</dd>
<dd>"version" : "1.0",</dd>
<dd>"token" : "JJWT token",</dd>
<dd>"type" : "update type",</dd>
<dd>"content" : "update content"</dd>
<dd>}</dd>
</dl>
    * version: version
    * token: JJWT token
    * type: update type
        + _"0": update nickname_
    * content: update content
        + _用于更新的内容，比如 type = 0，则此处的 content 应该是需要更新的nickname_


- **Response**
<dl>
<dd>{</dd>
<dd>"version" : "1.0",</dd>
<dd>"type" : "update type",</dd>
<dd>"status" : "update status"</dd>
<dd>}</dd>
</dl>
    * version:
    * type:
        + _"0": update nickname_
    * status:
        + _"0": 更新成功_
        + _"1": 用于更改的 token 失效，需要 App 重新登录并登录获取 token_
        + _"2": 更新失败，未知错误_


## 4. 更改密码
- **Base**
    * url: host/user/alterpassword
    * type: post
    * content-type: json


- **Request**
<dl>
<dd>{</dd>
<dd>"version" : "1.0",</dd>
<dd>"email" : "username@email.com",</dd>
<dd>"oldpassword" : "123456",</dd>
<dd>"newpassword" : "abcdefg"</dd>
<dd></dd>
<dd>}</dd>
</dl>
    * version: version
    * email: user email
    * oldpassword:
        + _旧密码_
    * newpassword:
        + _新密码_


- **Response**
<dl>
<dd>{</dd>
<dd>"version" : "1.0",</dd>
<dd>"status" : "0"</dd>
<dd>}</dd>
</dl>
    * version: version
    * status: alter user new password status
        + _"0": 更新成功_
        + _"1": 更新失败，Email 不存在，_
        + _"2": 更新失败，oldpassword 错误_
        + _"3": 更新失败，未知错误_

## 5. 找回密码
- **Base**
    * url: host/user/retrievepassword
    * type: post
    * content-type: json


- **Request**
<dl>
<dd>{</dd>
<dd>"version" : "1.0",</dd>
<dd>"email" : "username@email.com"</dd>
<dd>}</dd>
</dl>
    * version: version
    * email: user email


- **Response**
<dl>
<dd>{</dd>
<dd>"version" : "1.0",</dd>
<dd>"status" : "0"</dd>
<dd>}</dd>
</dl>
    * version: version
    * status: retrieve status
        + _"0": 已经发送找回密码的邮件到注册邮箱，下一步：登录注册邮箱_
        + _"1": 邮箱尚未注册_

## 6. 找回密码内容
- **邮件信息**
    * 发件箱 sunricher_ios@sunricher.com
    * 密码 security
    * 标题 [EasyHome] Password reset request
    * 发件人 Sunricher
    * 内容
        <p>You're receiving this email because you requested a password reset for the user <i style="color: orange">"user email"</i></p>
        <p style="color: blue; text-decoration: underline; font-weight: bold">Reset password ></p>
    * Reset password 超链 url
        + [host/user/resetpassword?email=username@email.com&token=usertoken](http://easyhome.rgbw.hk/api/1.0/user/passwordreset?email=username@email.com&key=reestkey)
        + 其中 key 为根据 email 生成的 token，有效期为 12 小时；有效期内跳转至直接更改密码的页面，有效期外跳转至失效页面；通过此 token 修改密码成功后，token 失效，再次使用此 token 更改密码无效。

    * Reset 页面 Submit url
        + [host/user/resetnewpassword]
        + type: post
        + content: json

        + **Requset**
            - {"version" : "1.0", "token" : "reset passowrd token", "newpassword" : "New password"}

        + **Response**
            - 更新成功，请用新密码登录；
            - 更新失败，请重新找回密码；

---

# 二、房间、场景、灯、事件

## 1. App 发送信息至服务器
- **Base**
    * url: host/easyhome/uploadinfo
    * type: post
    * content: xml
    * 内容：easyhome_sync.xml
    * PS: 发送信息至服务器时 easyhome_sync 的 token 不能为空，需要为登录后保存的token
- **Response**
    * content: json
    * {"version" : "1.0", "status" : "0"}
    * status:
        + 0: 处理成功
        + 1: token 无效（可能是过期或者用户名不存在）
        + 2: 传输的文件格式不对（没有相应的字段，或者根本不是 xml 字符串等）

## 2. App 请求删除服务器信息
- **Base**
    * url: host/easyhome/deleteinfo
    * type: post
    * content: xml
    * 内容：easyhome_delete.xml
    * PS：发送至服务器时 easyhome_delete 的 token 不能为空，需要为登录后保存的 token
- **Response**
    * content: json
    * {"version" : "1.0", "status" : "0"}
    * status:
        + 0: 处理成功
        + 1: token 无效（可能是过期或者用户名不存在）
        + 2: 传输的文件格式不对（没有相应的字段，或者根本不是 xml 字符串等）

## 3. App 向服务器请求信息同步
- **Base**
    * url: host/easyhome/syncinfo
    * type: post
    * content: json
- **Request**
    * {"version" : "1.0", "token" : "user token"}
- **Response**
    * content: xml
    * 内容：easyhome_sync.xml
    * PS: 返回的信息中 token 可以为空
