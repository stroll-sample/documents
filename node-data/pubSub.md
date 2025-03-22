cockroachdb [跳转连接](./cockroachdb/index.md)

mongodb [跳转连接](./mongodb/index.md)

postgreSQL [跳转连接](./postgreSQL/index.md)

redis [跳转连接](./redis/index.md)

graph使用 [跳转连接](./graph.md)

http请求 [跳转连接](./HTTP.md)

webRTC [跳转连接](./webRTC.md)

socket [跳转连接](./socket.md)

token [跳转连接](./token.md)

worker [跳转连接](./worker.md)

引入

```js
import { SP, PubSub } from "@stroll/node-data";
// or
import SP, { SP , PubSub} from "@stroll/node-data/dist/SP";
```
 - SP 是单例模式
 - PubSub 是封装的 class，可以实例化，也可以直接使用，也可以生成单例。

调用
```js
function callbackFn (res) {
  // 返回的信息
  console.log(res)
  // ......
}

// SP 单例模式 可以直接使用

/** 订阅
 * @param eventType 事件类型
 * @param listener 回调函数
 */
SP.on("test", callbackFn)
/** 推送消息
 * @param {string} eventType 事件类型
 * @param {any} data 事件数据
 */
SP.pushMsg("test", "hello");
/** 取消订阅事件
 * @param eventType 事件类型
 * @param listener 订阅的事件
 */
SP.off("test", callbackFn)

// PubSub 是 class
// 可以静态调用
PubSub.on("test", callbackFn);
PubSub.pushMsg("test", "hello");
PubSub.off("test", callbackFn)
//也可以实例化
const pubsub = new PubSub();
pubsub.on("test", callbackFn);
pubsub.pushMsg("test", "hello");
pubsub.off("test", callbackFn);
// 或者生成单例，单例子 不管多少次实例化，都是同一个实例
const pubsub1 = PubSub.init();
const pubsub2 = PubSub.init();
// pubsub1 === pubsub2
pubsub1.on("test", callbackFn);
pubsub2.pushMsg("test", "hello");
pubsub1.off("test", callbackFn);
```