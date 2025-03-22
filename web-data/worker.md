http请求 [跳转连接](./HTTP.md)

跨浏览器标签页通信 [跳转连接](./channel.md)

indexedDB使用 [跳转连接](./indexedDB.md)

长链接 socket [跳转连接](./socket.md)

发布订阅 [跳转连接](./PS.md)

webRTC [跳转连接](./webRTC.md)

引入
```js
import {
  WebWorker
} from '@stroll/data'
// or
import WebWorker from '@stroll/data/dist/worker.js'
```

调用
```js
/**
* - 数据需要使用 self.postMessage(data) 返回，返回方式是 Promise
* - 如果有两个 self.postMessage ，靠前的会覆盖靠后的
* - 返回后会关闭这个worker, 如果不想关闭，第二个参数传false，则不会关闭
* @param code 需要执行的代码或者文件路径
* @param close 执行完后是否会关闭 默认 true
* @returns Promise<data>
*/
WebWorker(
  `console.log('hello world');self.postMessage({a: 1, b: 3})`,
  true
).then (res => {
  console.log(res);
})
```
