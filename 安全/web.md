### CSRF 攻击

利用自动携带 cookie 机制，发起请求，执行危险操作。

如何防止：

- 使用 cookie 时设置 samesite strict；
- 使用 jwt，由前端添加在 header 中；

### jwt 认证

认证过程：

1. 发送验明用户身份的信息给后台
2. 后台校验通过后，生成 token 并返回
3. 前端将 token 保存并添加在请求的 header 中
4. 后台取出 header 中的 token 进行验签

优点：

1. 不用存储会话
2. 防止 csrf 攻击
