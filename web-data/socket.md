http请求 [跳转连接](./HTTP.md)

跨浏览器标签页通信 [跳转连接](./channel.md)

indexedDB使用 [跳转连接](./indexedDB.md)

发布订阅 [跳转连接](./PS.md)

webRTC [跳转连接](./webRTC.md)

多线程 worker [跳转连接](./worker.md)

引入
```js
import {Socket} from '@stroll/data'
// or
import Socket from '@stroll/data/dist/socket.js'
```

调用
```js
/**
 * 长链接
 * @param socketUrl 必传 例如 ws://127.0.0.1:8080/ws
 * @param options 可选 onopen onclose onerror onmessage
 * - onopen 连接成功回调
 * - onclose 连接关闭回调
 * - onerror 连接错误回调
 * - onmessage 接收到消息回调
 * @returns WebSocket
 */
const socket = Socket('ws://127.0.0.1:8080/socket', {
  onopen(ev) {},
  onclose(ev) {},
  onerror(ev) {},
  onmessage(ev) {},
})



function callbackFn (res) {
  // 返回的信息
  console.log(res)
  // ......
}

/**
 * 订阅消息
 * - 第一个参数是监听的事件名称
 * - 第二个参数是回调函数
 */
socket.getMsg('add', callbackFn)

/**
 * 推送消息
 * - 第一个参数是发送的事件名称
 * - 第二个参数是发送的消息体
 * 发送格式是 {eventType: 'add', data: {}},接收也需要是这个格式，数据会被转换 string 类型
 */
socket.pushMsg('add', {})

// 取消订阅
socket.offMsg('add')
// 取消订阅回调
socket.offMsg('add', callbackFn)
```
