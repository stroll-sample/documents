cockroachdb [跳转连接](./cockroachdb/index.md)

mongodb [跳转连接](./mongodb/index.md)

postgreSQL [跳转连接](./postgreSQL/index.md)

redis [跳转连接](./redis/index.md)

graph使用 [跳转连接](./graph.md)

http请求 [跳转连接](./HTTP.md)

webRTC [跳转连接](./webRTC.md)

发布订阅 [跳转连接](./pubSub.md)

token [跳转连接](./token.md)

worker [跳转连接](./worker.md)

引入
```js
import {Socket} from '@stroll/node-data'
// or
import Socket from '@stroll/node-data/dist/socket'
```

调用
```js
import Koa from "koa";
import cors from "koa2-cors";
import Router from "koa-router";
import {Socket} from '@stroll/node-data'

const app = koaSocket(new Koa(), {
  url: '/ws',
  onopen: ((event: Event) => void); // 监听连接事件
  onerror: ((event: ErrorEvent) => void); // 监听错误事件
  onclose: ((event: CloseEvent) => void); // 监听关闭事件
  onmessage: ((event: MessageEvent) => void); // 监听消息事件
  use(ctx, socket){ // 拦截器
    const { websocket, url } = ctx;
    if (websocket && url === '/ws') {
      socket.getMsg('add', (data: any) => {
        console.log('data', data)
        socket.pushMsg('add', {a: '123'})
      })
    }
  }
});

const router = new Router();

app.use(
  cors({
    origin: function(ctx: { url: string; }) { //设置允许来自指定域名请求
      if (ctx.url === '/test') {
        return '*'; // 允许来自所有域名请求
      }
      return 'http://localhost:8080'; //只允许http://localhost:8080这个域名的请求
    },
    maxAge: 5, //指定本次预检请求的有效期，单位为秒。
    credentials: true, //是否允许发送Cookie
    allowMethods: ['GET', 'POST', 'PUT', 'DELETE', 'OPTIONS'], //设置所允许的HTTP请求方法
    allowHeaders: ['Content-Type', 'Authorization', 'Accept'], //设置服务器支持的所有头信息字段
    exposeHeaders: ['WWW-Authenticate', 'Server-Authorization'] //设置获取其他自定义字段
  })
);

// 使用路由器中间件
app.use(router.routes()).use(router.allowedMethods());

router.get("/test", async (ctx) => {
  let { text } = ctx.query;
  text = text ?? decodeURI(ctx.querystring);
  if (text) {
    ctx.status = 200;
    ctx.body = { data: text };
  } else {
    ctx.status = 201;
    ctx.body = [
      {user: 'a', age: 1},
      {user: 'b', age: 2},
      {user: 'c', age: 3},
    ];
  }
});

const hostname = "127.0.0.1";
const port = 6060;
app.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});


```
