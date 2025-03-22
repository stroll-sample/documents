mongodb [跳转连接](./mongodb/index.md)

postgreSQL [跳转连接](./postgreSQL/index.md)

redis [跳转连接](./redis/index.md)

graph使用 [跳转连接](./graph.md)

http请求 [跳转连接](./HTTP.md)

webRTC [跳转连接](./webRTC.md)

发布订阅 [跳转连接](./pubSub.md)

socket [跳转连接](./socket.md)

token [跳转连接](./token.md)

worker [跳转连接](./worker.md)

引入
```js
import {Redis} from '@stroll/node-data'
// or
import Redis from '@stroll/node-data/dist/redis'
```
调用
```js

/**
 * redis 操作类
 * @param port 端口 默认6379
 * @param host 地址 默认127.0.0.1
 * @param options 配置项
 * @returns Redis
 * @example
 * const redis = new Redis(6379, "127.0.0.1", {})
 * @example options 常用属性
 * - host‌：Redis服务器地址，默认为localhost。
‌ * - port‌：Redis服务器端口，默认为6379。
‌ * - password‌：连接Redis的密码，默认为空。
‌ * - db‌：数据库索引，默认为0。
‌ * - connectTimeout‌：连接超时时间，单位为毫秒，默认为10000毫秒。
‌ * - retryStrategy‌：重试策略函数，返回重试延迟时间（毫秒），默认为无重试策略。
‌ * - keyPrefix‌：为所有键添加前缀，默认为空。
‌ * - enableOfflineQueue‌：是否启用离线队列，默认为true。
‌ * - socketNoDelay‌：是否禁用Nagle算法，默认为true。
‌ * - autoResubscribeCommands‌：是否自动重新订阅PUB/SUB频道，默认为true。
‌ * - showFriendlyErrorStack‌：是否显示友好的错误堆栈信息，默认为true。
‌ * - enableBufferCommands‌：是否启用缓冲命令，默认为true。
 * - maxRetriesPerRequest：对每个请求的最大重试次数。如果未设置，则使用全局的 retryStrategy。默认值: undefined
 * - enableReadyCheck：在连接建立后，是否发送一个 PING 命令来检查连接是否就绪。这有助于在连接建立后立即发现问题。默认值: false
 * - lazyConnect：是否延迟连接直到首次执行命令时才进行连接。这对于某些场景下减少启动时的延迟很有用。默认值: false
 * - name：为当前连接设置一个名称，这在管理多个 Redis 连接时非常有用。默认值: undefined
 * - tls：这些配置项用于启用 TLS/SSL 加密连接，特别是在连接到需要安全连接的 Redis 服务时非常有用。
 * - - tls.ca: CA证书路径。例：fs.readFileSync('path/to/ca_cert.pem') 
 * - - tls.cert：客户端证书路径。例：fs.readFileSync('path/to/client_cert.pem'), 
 * - - tls.key：客户端私钥路径。例：fs.readFileSync('path/to/client_key.pem')
 */
const redis = new Redis();
```
连接成功回调
```js
/** 连接成功回调
 * @param fn 回调函数
 * @returns Promise
 */
redis.connect((result) => {}).then(res => {})
```
关闭连接
```js
redis.close().then(res => {})
```
添加或编辑数据
```js
/** 添加或编辑 集合以外的数据，集合使用 sadd 添加、编辑
 * @param key 建
 * @param value 值
 * @param seconds 过期时间 单位秒
 * @returns Promise
 */
redis.add('key', 'value').then(res => {})

/** 添加或编辑集合类型数据
 * @param key 建
 * @param value 值
 * @param seconds 过期时间 单位秒
 * @returns Promise
 */
redis.sadd('key', ['value']).then(res => {})

/** 数据类型：string、hash、list、set、zset */ 

// string（字符串）: 基本的数据存储单元，可以存储字符串、整数或者浮点数。
redis.add('key', 'value').then(res => {})

// hash（哈希）:一个键值对集合，可以存储多个字段。
redis.add('key', {field: 'value', field1: 'value'}).then(res => {})

// list（列表）:一个简单的列表，可以存储一系列的字符串元素。
redis.add('key', ['value', 'value1', 'value2']).then(res => {})

// set（集合）:一个无序集合，可以存储不重复的字符串元素。
redis.sadd('key', ['value', 'value1', 'value2']).then(res => {})

// zset(sorted set：有序集合): 类似于集合，但是每个元素都有一个分数（score）与之关联。
redis.sadd('key', {field: 1, field2: 2, field3: 3}).then(res => {})
```
获取数据
```js
/** 获取数据 全类型数据
 * @param key 建
 * @param field key
 * @returns Promise
 */
redis.get('key').then(res => {})
```
编辑数据
```js
/** 编辑数据 全类型数据
 * @param key 建
 * @param value 值
 * @param seconds 过期时间 单位秒
 * @returns Promise
 */
redis.set('key', 'field').then(res => {})
```
删除数据
```js
/** 删除数据 全类型数据
 * @param key 建
 * @param field key
 * @returns Promise
 */
redis.del('key').then(res => {})
```
查看是否存在
```js
/** 查看是否存在 全类型数据
 * @param key 建
 * @param field key
 * @returns Promise
 */
redis.is('key').then(res => {})
```
设置或重置过期时间，不传时间则是查看过期时间， -1 是不过期
```js
/** 设置或重置过期时间
 * @param key 建
 * @param seconds 过期时间 单位秒
 * @returns Promise
 */
redis.ttl('key', 1000).then(res => {})
```
监听订阅数据
```js
/** 订阅数据
 * @param eventType 接受标记
 * @param listener 回调函数
 * @returns Promise
 */
redis.on('subscribe', (message) => {})
```
推送数据
```js
/** 推送数据
 * @param eventType 推送标记
 * @param data 数据
 * @returns Promise
 */
redis.pushMsg('subscribe', data).then(res => {})
```
取消监听
```js
/** 取消订阅
 * @param type 标记
 * @returns Promise
 */
redis.off('subscribe')
```
标记链上的任务异步执行
```js
redis.async().set('user1', 'test').set('user2', 'test').exec().then((res:any) => {
  console.log('exec', res)
})
```
标记链上的任务同步执行
```js
redis.await().set('user3', 'test').get('user3').exec().then(res => {
  console.log('exec', res)
})
```
执行链上任务
```js
redis.async().set('user1', 'test').set('user2', 'test').exec().then((res:any) => {
  console.log('exec', res)
})
redis.await().set('user3', 'test').get('user3').exec().then(res => {
  console.log('exec', res)
})
```