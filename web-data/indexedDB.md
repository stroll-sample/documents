跨浏览器标签页通信 [跳转连接](./channel.md)

http请求 [跳转连接](./HTTP.md)

长链接 socket [跳转连接](./socket.md)

发布订阅 [跳转连接](./SP.md)

多线程 worker [跳转连接](./worker.md)

webRTC [跳转连接](./webRTC.md)

引入

```js
import { DB } from "@stroll/data";
// or
import DB from "@stroll/data/dist/indexedDB";
```

调用

```js
// 实例化数据库
const webDB = new DB({
  name: string; // 数据库名称
  schema?: { // 数据库结构
    [table: string]: { // 表结构
      [key: string]: { // 字段结构
        multiEntry?: boolean; // 是否支持多字段索引
        unique?: boolean; // 是否唯一
        default?: any; // 默认值
        ref?: string; // 关联字段
      };
    };
  };
  config?: {
    version?: number; // 数据库版本 默认为1
    onerror?: (ev: IDBVersionChangeEvent) => {}; // 监听错误回调
    onsuccess?: (ev: IDBVersionChangeEvent) => {}; // 监听成功回调
    onupgradeneeded?: (ev: IDBVersionChangeEvent) => {}; // 监听升级回调
  };
})

// 关闭数据库
webDB.close();

// 删除数据库
webDB.delete();

// 获取数据库状态
// 返回值：closed | opened | connecting | init
webDB.getDBState()

// 获取表实例
const table1 = webDB.table('tableName', tableSchema?: {
  [key: string]: { // 字段结构
    multiEntry?: boolean; // 是否支持多字段索引
    unique?: boolean; // 是否唯一
    default?: any; // 默认值
    ref?: string; // 关联字段
  };
})

/**
 * 获取数据
 * - criteria 值为数字时必须为主键值 默认主键是 _id
 */
table1.get(criteria: { [key: string]: number | string; } | number)

/**
 * 获取所有数据
 * @param limit 获取条数 非必传
 */
table1.gets(limit?: number)

/**
 * 添加数据
 * @param data 数据
 */
table1.add(data: { [key: string]: any; })

/**
 * 批量添加数据
 * @param data 数据
 */
table1.adds(data: { [key: string]: any; }[])

/**
 * 修改数据
 * @param data 数据 数据必须带有 _id
 */
table1.put(data: { _id: number; [key: string]: any; })

/**
 * 删除数据
 * @param criteria 删除条件 值为数字时必须为主键值 默认主键是 _id
 */
table1.delete(criteria: { [key: string]: number | string; } | number)

/**
 * 查找数据 返回一条
 * @param fn 函数 非必传 默认返回第一条 如果传入则返回满足条件的数据 函数需要返回 true/false
 */
table1.find((item) => true)

 /**
 * 查找所有数据 返回多条
 * @param fn 函数 非必传 默认返回所有数据 如果传入则返回满足条件的数据 函数需要返回 true/false
 */
table1.findAll((item) => true)

/**
 * 控制台输出
 * @param limit 输出限制 默认 100
 */
table1.console(limit)
```
