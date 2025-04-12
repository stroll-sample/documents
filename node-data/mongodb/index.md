cockroachdb [跳转连接](./cockroachdb/index.md)

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
import { Mongodb } from "@stroll/db";
// or
import Mongodb from "@stroll/db/dist/mongodb";
```

调用

```js
// 连接 mongodb
const mongodb = new Mongodb('mongodb://localhost:27017')

// 创建/连接 数据库
const userDb = mongodb.db('userDb')

// 创建/连接 表
const userInfo =  userDb.collection('userInfo')
// or
const userInfo = userDb.table('userInfo')

// 获取数据库下所有表
const userInfo = userDb.tables().then((tables) => {
  console.log(tables);
})

// 增
const addData = { name: "zhang san", age: 18 }
// or
const addData = [
  { name: "zhang san", age: 18 },
  { name: "li si", age: 19 },
  { name: "wang wu", age: 20 }
]
userInfo.add(addData).then((data) => {
  console.log('add-data', data)
})

// 删 软删除，_id 必须携带且只能携带 _id
const _id = '_id'
// or
const _id = ['_id_1', '_id_2']
userInfo.del({ // 单个传 _id, 多个传 _id 的集合数组
  _id
}).then((data) => {
  console.log('del-data', data)
})

// 改，_id 必须携带,其它为需要修改的数据
const setData = {_id: '67d3e99f10ca22845171407b', name: 'zhao liu'}
// or
const setData = [
  {_id: '67d3e99f10ca22845171407b', name: 'zhao liu'},
  {_id: '67d3fe550a07b6aebd5647b3', name: 'zhao liu'},
  {_id: '67d3fe550a07b6aebd5647b5', name: 'zhao liu'},
]
userInfo.set(setData).then((data) => {
  console.log('del-data', data)
})

// 查
const filter = { // 查询条件，不带分页信息返回全部（不包括软删除数据）
  pages: 7, // 分页
  count: 10, // 每页条数
  .... // 其他条件
}
userInfo.get(filter).then((res: any) => {
  console.log('get-data', res)
})
```

#### 以上为简单增删改查，复杂操作请使用 aggregate 或 bulkWrite

```js
userInfo.aggregate([
  {
    $lookup: { // 链表操作
      from: "customers", // 要连接的集合名
      localField: "customer_id", // 本集合中的字段名
      foreignField: "customer_id", // 目标集合中的字段名
      as: "customer_details", // 结果数组的字段名
    },
  },
  { $unwind: "$customer_details" }, // 如果返回的是数组可使用此操作符展开文档，以便下面对每个文档作处理
  {
    $project: { // 文档处理
      order_id: 1, // 选择order_id字段
      customer_details: 1, // 选择customer_details字段（包含客户信息）
      "customer_details.name": 1, // 选择返回客户名字段
      customer_name: "$customer_details.name", // 重命名字段
    },
  },
]);
```

```js
userInfo.bulkWrite([
  {
    insertOne: {
      document: { name: 'Alice', age: 30 }
    }
  },
  {
    updateOne: {
      filter: { name: 'Bob' },
      update: { $set: { age: 25 } }
    }
  },
  {
    insertOne: {
      document: { name: 'Charlie', age: 35 }
    }
  },
  {
    deleteOne: {
      filter: { _id: '67d3e99f10ca22845171407b' }
    }
  }
])
```
#### 索引操作
```js
// 创建索引
userInfo.addIndex({ name: 1 })
// or
userInfo.addIndex([{ name: 1 }, { age: -1 }])

// 删除索引
userInfo.delIndex('name')
// or
userInfo.delIndex(['name', 'age'])

// 修改索引
userInfo.setIndex({ name: 1 })
// or
userInfo.setIndex([{ name: 1 }, { age: -1 }])

// 获取索引
userInfo.getIndex().then((data) => {
  console.log(data)
})
```
索引类型

1 (升序):
 - 这是默认的索引方向，表示索引按照字段值的升序排列。
 - 例如，如果你有一个age字段并希望按照年龄的增加顺序进行查询，可以使用{ age: 1 }作为索引。
 - 
-1 (降序):
 - 这是与默认的升序相反的索引方向，表示索引按照字段值的降序排列。
 - 例如，如果你想要按照年龄的减少顺序进行查询，可以使用{ age: -1 }作为索引。

'2d' (二维索引):
 - 用于空间数据的索引，适用于包含经纬度坐标的字段。
 - 例如，如果你有一个存储地理位置的字段（如location: [longitude, latitude]），你可以使用{ location: "2d" }来创建一个二维索引。

'2dsphere' (地理空间索引):
 - 类似于2d索引，但是支持更复杂的空间查询，包括球面几何。
 - 这对于需要处理全球或大规模地理数据的场景特别有用。
 - 例如，使用{ location: "2dsphere" }可以为包含经纬度坐标的字段创建地理空间索引。

'text' (文本索引):
 - 用于全文搜索的索引。
 - 当你需要对文档中的文本内容进行搜索时（例如，在多个字段中搜索特定的单词或短语），可以使用文本索引。
 - 例如，使用{ description: "text" }可以为description字段创建文本索引。

'geoHaystack' (地理Haystack索引):
 - 用于在特定区域内进行高效的点查询。
 - 它适用于已知查询将集中在地理空间中的小区域的情况。
 - 例如，使用{ location: "geoHaystack", haystack: { "2d": [x1, y1, x2, y2] } }可以创建一个地理Haystack索引，其中[x1, y1, x2, y2]定义了查询区域的范围。

'hashed' (哈希索引):
 - 用于基于哈希值的快速等值查询。
 - 哈希索引可以加快基于哈希值的等值查询的速度，特别是在处理大量数据时。
 - 例如，使用{ userId: "hashed" }可以为userId字段创建一个哈希索引。
