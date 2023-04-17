---
title: NodeJS
date: 2022-01-05 20:01:02
tags: JS
categories: 前端进阶
cover: /img/03.jpg
---

## **NodeJS 概念**

Node.js 是一个开源的，跨平台的 JS 运行环境

- NodeJs 中并没有 BOM、DOM，可以使用 console 和定时器 API
- NodeJs 中的顶级对象未 global，也可以用 globalThis 访问顶级对象

## **Buffer(缓冲器)**

**概念**

- Buffer 是一个类似于数组的对象，用于表示固定长度的字节序列。
- Buffer 本质是一段内存空间，专门用来处理二进制数据。

**特点**

- Buffer 大小固定且无法调整
- Buffer 性能较好，也可以直接对计算机内存进行操作
- 每个元素的大小为 1 字节(byte)

**使用**

```javascript
// 1.alloc 会进行归零操作
let buf = Buffer.alloc(10);
// 2.allocUnsafe 不会归零，可能引入脏数据
let buf_2 = Buffer.allocUnsafe(10);
// 3.from
let buf_3 = Buffer.from("hello");
let buf_4 = Buffer.from([105, 104]);

// Buffer与字符串的转换，我们可以借助toString方法将Buffer转换为字符串
console.log(buf_3.toString()); // utf-8
// Buffer的读写
let buf_5 = Buffer.from("hello");
console.log(buf_5[0]);
buf_5[0] = 95;
// 溢出，舍弃高于8位的二进制数字
buf_5[0] = 361;
console.log(buf_5);
// 中文
let buf_6 = Buffer.from("你好");
console.log(buf_6);
```

## **进程与线程**

- 进程是程序的一次执行过程，一个进程至少包含一个线程
- 线程是一个进程中执行的一个执行流，一个线程是属于某个进程的

## **fs 模块**

fs 模块可以实现与硬盘的交互，例如文件的创建、删除、重命名、移动，还有文件内容的写入、读取，以及文件夹的相关操作。

**writeFile**
`fs.writeFile(file,data[,options],callback)`
参数说明：

- file 文件名
- data 待写入数据
- options 选项设置
- callback 写入回调
  返回值：undefined

代码示例：

```javascript
/**
 * 需求：
 * 新建一个文件，座右铭.txt，写入内容，三人行，则必有我师焉
 */

// 1.导入 fs 模块
const fs = require("fs");

// 2.写入文件
fs.writeFile("./座右铭.txt", "三人行，则必有我师焉", (err) => {
  // err 写入失败：错误对象 写入成功：null
  if (err) {
    console.log("写入失败");
  }
  console.log("写入成功");
});

// 3.同步写入
fs.writeFileSync("./data.txt", "test");
```

**appendFile/appendFileSync 追加写入**
appendFile 作用是在文件尾部追加内容。appendFile 语法与 writeFile 语法完全相同
语法：
`fs.appendFile(file,data[,options],callback)`
`fs.appendFileSync(file,data[,options])`
返回值：二者都为 undefined
代码示例：

```javascript
fs.appendFile("./座右铭.txt", "择其善者而从之，其不善者而改之。", (err) => {
  if (err) throw status;
  console.log("追加成功");
});
fs.appendFileSync("./座右铭.txt", "\r\n温故而知新，可以为师矣");

fs.writeFile(
  "./座右铭.txt",
  "择其善者而从之，其不善者而改之。",
  { flag: "a" },
  (err) => {
    if (err) throw status;
    console.log("追加成功");
  }
);
```

**流式写入**
语法：`fs.createWriteStream(path[,options]);`
参数说明：

- path 文件路径
- options 选项配置(可选)
  返回值：Object
  代码示例：

```javascript
/**
 * 观书有感.txt
 */
// 1.导入 fs
const fs = require("fs");

// 2.创建写入流对象
const ws = fs.createWriteStream("./观书有感.txt");

// 3.write
ws.write("半亩方塘一鉴开\r\n");
ws.write("天光云影共徘徊\r\n");
ws.write("问渠那得清如许\r\n");
ws.write("为有源头活水来\r\n");

// 4.关闭通道
ws.close();
```

**读取文件应用场景**

- 电脑开机
- 程序运行
- 编辑器打开文件
- 查看图片
- 播放视频
- 播放音乐
- Git 查看日志
- 上传文件
- 查看聊天记录

**文件读取**
| 方法             | 说明     |
| ---------------- | -------- |
| readFile         | 异步读取 |
| readFileSync     | 同步读取 |
| createReadStream | 流式读取 |

```javascript
// 1.导入 fs
const fs = require("fs");

// 2.创建读取流对象
const rs = fs.createReadStream("./观书有感.txt");

// 3.绑定 data 事件 chunk 块儿 大块儿
rs.on("data", (chunk) => {
  console.log(chunk); // 65536 字节 => 64KB
});
rs.on("end", () => {
  console.log("读取完成");
});

ws.write("半亩方塘一鉴开\r\n");
ws.write("天光云影共徘徊\r\n");
ws.write("问渠那得清如许\r\n");
ws.write("为有源头活水来\r\n");

// 4.关闭通道
ws.close();
```

**文件移动与重命名**
在 Node.js 中，我们可以使用 rename 或 renameSync 来移动或重命名 文件或文件夹
语法：
`fs.rename(oldPath,newPath,callback)`
`fs.renameSync(oldPath,newPath,callback)`
参数说明：

- oldPath 文件当前路径
- newPath 文件新的路径
- callback 操作后的回调
  代码示例

```javascript
// 1.导入 fs
const fs = require("fs");

// 2.调用 rename 方法
fs.rename("./观书有感.txt", "./论语.txt", (err) => {
  if (err) {
    console.log("操作失败");
    return;
  }
  console.log("操作成功");
});

// 3.文件的移动
fs.rename("./data.txt", "../资料/data.txt", (err) => {
  if (err) {
    console.log("操作失败");
    return;
  }
  console.log("操作成功");
});
```

**文件删除**

```javascript
// 1.导入 fs
const fs = require("fs");

// 2.调用 unlink 方法  unlinkSync
fs.unlink("./观书有感.txt", (err) => {
  if (err) {
    console.log("操作失败");
    return;
  }
  console.log("操作成功");
});

// 3.调用 rm 方法 14.4  rmSync
fs.rm("./观书有感.txt", (err) => {
  if (err) {
    console.log("操作失败");
    return;
  }
  console.log("操作成功");
});
```

**文件夹操作**

```javascript
// 1.导入 fs
const fs = require("fs");

// 2-1.创建文件夹 mkdir
fs.mkdir("./html", (err) => {
  if (err) {
    console.log("创建失败");
    return;
  }
  console.log("创建成功");
});

// 2-2.递归创建
fs.mkdir("./a/b/c", { recursive: true }, (err) => {
  if (err) {
    console.log("创建失败");
    return;
  }
  console.log("创建成功");
});

// 2-3 读取文件夹
fs.readdir("../about", (err, data) => {
  if (err) {
    console.log("读取失败");
    return;
  }
  console.log(data);
});

// 2-4 删除文件夹
fs.rmdir("./html", (err) => {
  if (err) {
    console.log("删除失败", err);
    return;
  }
  console.log("删除成功");
});

// 2-5 递归文件夹
fs.rmdir("./html", { recursive: true }, (err) => {
  if (err) {
    console.log("删除失败", err);
    return;
  }
  console.log("删除成功");
});

// 3.查看资料信息
fs.stat('../资料/笑看风云.MP4',(err,data)=>{
  if(err){
    console.log('操作失败')；
    return
  }
  console.log(data);
})
```

## **路径**

- 相对路径 相对路径参照物：命令行的工作目录
- 绝对路径 \_\_dirname:全局变量 保存的是所在文件夹的所在目录的绝对路径

**path 模块**

- resolve 拼接规范的绝对路径
- sep 获取操作系统的路径分隔符
- parse 解析路径并返回对象
- basename 获取路径的基础名称
- dirname 获取路径的目录名
- extname 获取路径的扩展名

## **HTTP**

协议：双方共同遵守的约定
**请求行**

- 请求方法
  - GET 主要作用于获取数据
  - POST 主要作用于新增数据
  - PUT/PATCH 主要作用于更新数据
  - DELETE 主要作用于删除数据
- URL
  - 协议名
  - 主机名
  - 端口号
  - 路径
  - 查询字符串
- HTTP 版本号
  - 1.0
  - 1.1
  - 2
  - 3
  <!-- P41 -->
