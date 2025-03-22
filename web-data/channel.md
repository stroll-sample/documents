indexedDB使用 [跳转连接](./indexedDB.md)

长链接 socket [跳转连接](./socket.md)

多线程 worker [跳转连接](./worker.md)

发布订阅 [跳转连接](./PS.md)

webRTC [跳转连接](./webRTC.md)

引入
```js
import {Channel} from '@stroll/data'
// or
import Channel from '@stroll/data/dist/channel.js'
```

调用
```js
/** 数据监听 */
Channel.onmessage = (message: unknown) => {
  console.log(message);
};
/** 数据错误监听 */
Channel.onmessageerror = (message: unknown) => {
  console.log(message);
};
/** 关闭 */
Channel.close();
/** 发送数据 */
Channel.postMessage(message);

/**
 * 跨浏览器标签页通信-发送
 * @param type 发送标记 与接收标记对应
 * @param data 数据
 */
Channel.pushMsg('add', (res) => {
  // 返回的信息
  console.log(res)
  // ......
})

/**
 * 跨浏览器标签页通信-接收
 * @param type 接收标记 与发送标记对应
 * @param callback 回调
 */
Channel.getMsg('add', （data）=>{})
```
