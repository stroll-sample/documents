cockroachdb [跳转连接](./cockroachdb/index.md)

mongodb [跳转连接](./mongodb/index.md)

postgreSQL [跳转连接](./postgreSQL/index.md)

redis [跳转连接](./redis/index.md)

graph使用 [跳转连接](./graph.md)

webRTC [跳转连接](./webRTC.md)

发布订阅 [跳转连接](./pubSub.md)

socket [跳转连接](./socket.md)

token [跳转连接](./token.md)

worker [跳转连接](./worker.md)

引入

```js
import { http } from "@stroll/node-data";
// or
import http from "@stroll/node-data/dist/SP";
```

调用
```js
/** config 配置
 * @param {string} baseURL 基础URL
 * @param {string} method 请求方法
 * @param {string} url 请求地址
 * @param {string} headers 请求头
 * @param {string} body 请求体
 * @param {string} params 请求参数
 * @param {string} signal 关闭标识
 * @param {string} timeout 超时时间
 * @param {string} requestInterceptors 请求拦截器
 * @param {string} responseInterceptors 响应拦截器
 * @param {string} onError 错误处理
 * @param {string} keepalive 是否允许关闭页面后继续发送请求
 * @param {string} credentials 确定浏览器是否应该发送cookie， omit 不发送， same-origin 发送同域的cookie， cors 发送跨域的cookie， navigate 发送所有cookie
 * @param {string} mode 设置请求的跨域行为
 * @param {string} referrer 指定用于请求的 Referer 标头的值字符串
 * @param {string} referrerPolicy 一个设置 Referer 标头策略的字符串。此选项的语法和语义与 Referrer-Policy 标头完全相同。
 * @param {string} browsingTopics 布尔值，指定应将当前用户的选定主题发送到与请求关联的 Sec-Browsing-Topics 标题中。
 * @param {string} cache 请求要使用的缓存模式
 * @param {string} priority 指定了 fetch 请求相对于同一类型的其他请求的优先级
 * @param {string} responseType null
 * @param {string} window null
 * @param {string} dispatcher
 * @param {string} duplex half
 */
export const HTTP = http.create({
  baseURL: 'http://127.0.0.1:6060',
  timeout: 3000,
  // 请求拦截器
  requestInterceptors: (config) => {
    // ...
    return config
  },
  // 响应拦截器
  responseInterceptors: (response) => {
    // ...
    return response
  },
  // 错误处理
  onError: (error) => {
    // ...
  }
})
//or 
export const HTTP = new http({
  baseURL: 'http://127.0.0.1:6060',
  timeout: 3000,
})

HTTP.interceptRequest((config)) => {
  console.log('请求拦截器')
  return config;
})
HTTP.interceptResponse((response)) => {
  console.log('响应拦截器')
}
HTTP.onError((err)) => {
  console.log('错误处理')
}
```
取消请求
```js
HTTP.cancel('请求时定义的 signal')
```
get 请求
```js

HTTP.get({
  url:'/test',
  params: {
    a: 1
  },
  config: {
    signal: 'biao_ji'
  }
})

//  下载文件
HTTP.getDL({
  url:'/test',
  params: {
    a: 1
  },
}, (res) => {
  console.log('进度', res)
})
```
post 请求
```js
HTTP.post({
  url:'/test',
  params: {b:1},
  body: {a:2}
})

// 表单
HTTP.postForm({
  url:'/test',
  body: {a:2}
})

// 下载文件
HTTP.postDL({
  url:'/test',
  params: {b:1},
  body: {a:2}
})

// 上传文件
HTTP.postFileForm({
  url:'/test',
  params: {b:1},
  body: {a:2， file：File||File[]},
}, (res) => {
  console.log('进度', res)
})
```
path 请求
```js
HTTP.path({
  url:'/test',
  params: {b:1},
  body: {a:2}
})

// 表单
HTTP.pathForm({
  url:'/test',
  body: {a:2}
})

// 上传文件
HTTP.pathFileForm({
  url:'/test',
  params: {b:1},
  body: {a:2， file：File||File[]},
}, (res) => {
  console.log('进度', res)
})
```
put 请求
```js
HTTP.put({
  url:'/test',
  params: {b:1},
  body: {a:2}
})

// 表单
HTTP.putForm({
  url:'/test',
  body: {a:2}
})

// 上传文件
HTTP.putFileForm({
  url:'/test',
  params: {b:1},
  body: {a:2， file：File||File[]},
}, (res) => {
  console.log('进度', res)
})
```
connect 请求 建立一个允许后端不断推送信息的连接
```js
const ES = HTTP.connect({
  url:'/test'
}, (res) => {
  console.log('后端推送的信息', res)
})

// 关闭
ES.close()
```
delete 请求
```js
HTTP.delete({
  url:'/test',
  params: {b:1},
  body: {a:2}
})
```
head 请求
```js
HTTP.head({
  url:'/test',
  params: {b:1},
})
```
options 请求
```js
HTTP.options({
  url:'/test',
  params: {b:1},
  body: {a:2}
})
```
