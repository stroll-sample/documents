cockroachdb [跳转连接](./cockroachdb/index.md)

mongodb [跳转连接](./mongodb/index.md)

postgreSQL [跳转连接](./postgreSQL/index.md)

redis [跳转连接](./redis/index.md)

graph使用 [跳转连接](./graph.md)

http请求 [跳转连接](./HTTP.md)

webRTC [跳转连接](./webRTC.md)

发布订阅 [跳转连接](./pubSub.md)

socket [跳转连接](./socket.md)

worker [跳转连接](./worker.md)

引入

```js
import { Token } from "@stroll/node-data";
// or
import Token from "@stroll/node-data/dist/token";
```

调用
```js
const token = new Token();
// or
const token = Token.init();


// 生成 jwt 令牌
const jwt = await token.jwt('用户信息', '设备信息')

// 数据加密
const encryptData = await token.encrypt({name: '张三'})

// 数据解密 如果是 jwt 令牌只解密用户信息
const decryptData = await token.decrypt(encryptData)

// MD5 编码
const md5 = await token.md5('123456')

// 验证设备
const verifyResult = await token.verifyDevice(jwt, '设备信息')

// 是否初始化完成
token.Done().then(async () => {
  // 初始化完成
  // ...
})


token.

```


