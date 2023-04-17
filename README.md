# ChengYM-98.github.io
个人博客

---
title: Angular
date: 2022-01-05 20:01:02
tags: JS
categories: 前端进阶
cover: /img/01.jpg
---
## **组件**

### @Component装饰器的TS类

- 指定如下信息

  - 一个CSS选择器，用于定义如何在模板中使用组件
  - HTML模板，用于指示Angular如何渲染组件
  - 可选的CSS样式

1.2 Angular创建组件

要手动创建一个新组件：

1. 导航到你的 Angular 项目目录。

2. 创建一个新文件 `<component-name>.component.ts`。

3. 在文件的顶部，添加下面的 import 语句。

   `import { Component } from '@angular/core';`

4. 在 `import` 语句之后，添加一个 `@Component` 装饰器。

   `@Component({ })`

5. 为组件选择一个 CSS 选择器。

   `@Component({  selector: 'app-component-overview', })`

6. 定义组件用以显示信息的 HTML 模板。在大多数情况下，这个模板是一个单独的 HTML 文件。

   `@Component({  selector: 'app-component-overview',  templateUrl: './component-overview.component.html', })`

   关于定义组件模板的更多信息，参阅[定义组件的模板](https://angular.cn/guide/component-overview#defining-a-components-template)。

7. 为组件的模板选择样式。在大多数情况下，你可以在单独的文件中定义组件模板的样式。

   `@Component({  selector: 'app-component-overview',  templateUrl: './component-overview.component.html',  styleUrls: ['./component-overview.component.css'] })`

8. 添加一个包含该组件代码 `class` 语句。

   `export class ComponentOverviewComponent { }`

   

   ### 1.2 模板

   每个组件都有一个HTML模板，用于声明该组件的渲染方式。你可以内联它或用文件路径定义此模板。

   模板中可使用插值表达式{{}}、属性绑定、事件监听器监听并响应用户的操作

   ```javascript
   <button
     type="button"
     [disabled]="canClick"
     (click)="sayMessage()">
     Trigger alert message
   </button>
   ```

   初步理解为标签中定义的属性就是调用的类中的属性

   ### 1.3 依赖注入

   声明TS类的依赖项，无需操行如何实例化它们。

   logger.service.ts

   ```javascript
   import { Injectable } from '@angular/core';
   
   @Injectable({providedIn: 'root'})
   export class Logger {
     writeCount(count: number) {
       console.warn(count);
     }
   }
   ```

   hello-world-di.component.ts

   ```javascript
   import { Component } from '@angular/core';
   import { Logger } from '../logger.service';
   
   @Component({
     selector: 'hello-world-di',
     templateUrl: './hello-world-di.component.html'
   })
   export class HelloWorldDependencyInjectionComponent  {
     count = 0;
   
     constructor(private logger: Logger) { }
   
     onLogMe() {
       this.logger.writeCount(this.count);
       this.count++;
     }
   }
   ```

   

   ### Angular CLI

   Angular CLI是开发Angular应用程序的最快、直接和推荐的方式。

   * ng build 编译
   * ng serve 启动服务
   * ng generate 生成文件
   * ng test 运行单元测试
   * ng e2e 构建一个Angular应用并启动开发服务器，然后运行端到端测试

   

   ### 将数据传递给子组件

   在父组件中引入**Input**

   import { [Component](https://so.csdn.net/so/search?q=Component&spm=1001.2101.3001.7020), Input } from ‘@angular/core’；

   在父组件的@Componentl里面写要传的数据（可以不写public）；

   ```javascript
   @Component({
   
   	public parentsData='父组件传值到子组件'
   
   })
   ```

   在父组件的页面引用的组件里面写要传输的数据

   <app-home [msg]='parentsData'>

   在子组件中同样引入**Input**

   

   # 手机真机调试方法

   * 确保手机和电脑处于同一Wifi
   * ng serve --host 0.0.0.0
   * 手机浏览器打开http：//<电脑IP>:4200

   

   

   ## Angular与React、Vue对比  （!性能问题）

   ### 1 与React对比

   Angular是一个完整框架，而React是一个类库，其对应Angular的各种特性，需要寻找各种开源社区类库。

   Angular使用HTML+CSS+组件库，而React是所有都是JS

   ### 2 与Vue对比

   Vue很多思想脱胎于Angular.js，和React类似是一个轻量级的，面向View层的类库

   Vue适合快速开发较小的工程，而Angular自带编码范式，使得它成为与多个开发人员合作时的好选择。

   

   ## 重要指令

   ### ngFor

   ​	变量应用范围

   ​	索引的获取

   ```html
   *ngFor="let menu of menus; let i = index"
   ```

   ​	第一个和最后一个元素

   ```html
   *ngFor="let menu of menus; let a = first; let b =last"
   ```

   ​	奇和偶

   ```html
   *ngFor="let menu of menus; let odd = odd;let even = even"
   ```

   ​	性能优化

   ```html
   *ngFor="let menu of menus; trackBy：trackElement"
   唯一表达元素的值，根据这个渲染元素
   ```

   ### ngif

   ```html
   <div *ngif="条件表达式"else elseContent>
       在条件为真的时候需要显示的内容
   </div>
   <ng-template #elseContent>
   	在条件为假的时候需要显示的内荣
   </ng-template>
   ```

   

   ## 组件间数据通信

   ### 1 父传子

   父组件定义数据，在父组件的Html中通过[menus]="topMenus"这样的格式传递数据给子组件，子组件通过@Input接受数据

   ### 2 子传父

   子组件通过@Output传递数据，父组件HTML中通(tabSelected)="handleTopSelection($event)"绑定事件

   

   ### 事件的处理和样式绑定

   ```html
   <a
      href="#"
      [class.active]="selectedIndex === i"
      (click)="selectedIndex = i">
       {{menu.title}}
   </a>
   ```

   * [class.样式类名]=“判断表达式”是应用单个class时常用技巧
   * 使用方括号[]是**数据绑定**，如果带方括号，等号后面就是一个对象或表达式
   * 不适用方括号，等号后面Angular认为是一个字符串，但如果我们此时在等号后使用{{}}就是和方括号等效
   * 圆括号（）用于**事件绑定**，等号后可以接表达式也可以是一个定义在类中的函数

   ### 样式绑定的几种方式

   [class.className] = "条件表达式"

   [ngClass] = "{'One':true,'Two':true}"

   [ngStyle] = "{'color':someColor,'font-size':fontSize}"

   

   ## 组件的生命周期

   ### constructor

   ### ngOnChanges

   输入属性变化时调用

   ### ngOnInit

   组件初始化时被调用

   ### ngDoCheck

   脏值检测时调用

   ### ngAfterContentInit

   当内容投影ng-content完成时调用

   ### ngAfterContentChecked

   Angular检测投影内容时调用（多次）

   ### ngAfterViewInit

   当组件视图（子视图）初始化完成时

   ### ngAfterViewChecked

   当检测视图变化时（多次）

   ### ngOnDestroy

   当组件销毁时

   

   ## 模板在组件类中的引用

   ```html
   <div #helloDiv>
       hello
   </div>
   ```

   #后面是给模板或者DOM元素起一个引用名字，以便可以在组件类或模板中进行引用

   ```javascript
   @ViewChild('helloDiv') helloDivRef：ElementRef
   ```

   @ViewChild 是一个选择器，用来查找要引用的DOM元素或者组件

   ```js
   @ViewChild('ImageSliderComponent') imageSlider:ImageSliderComponent;
   ```

   如果想引用模板中的Angular组件，@ViewChild中可以使用引用名，也可以使用组件类型

   ```js
   @ViewChildren('Img') imgs:QueryList<ElementRef>;
   ```

   引用多个模板

   总结：

   * @ViewChild用来在类中引用模板中的视图节点
   * 可以是Angular组件，也可以是HTML元素
   * 在AfterViewInit中可以安全的使用@ViewChild引用的元素
   * 推荐使用Renderer2操作DOM元素



## 双向绑定

```html
<input [value]="username" (input)="username = $event.target.value"></input>
```



## 什么是模块

@NgModule注解

* declarations 数组：模块拥有的组件、指令或管道。注意每个组件、指令、管道只能在一个模块中声明
* providers数组：模块中需要使用的服务
* exports数组：暴露给其它模块使用的组件、指令或管道等
* imports数组：导入本模块需要的依赖模块，注意是模块。



## 什么是装饰器(注解)

装饰器/注解就是一个函数

但它是一个返回函数的函数

它是TS的一个特性，而非Angular的特性





---
title: VUE
date: 2022-01-05 20:01:02
tags: JS
categories: 前端进阶
cover: /img/02.jpg
---
## **MVVM**
  - 设计模式
    - m:model数据
    - v:view视图
    - vm:viewModel:视图数据连接层
    
  ```javascript
  const app = Vue.createApp({
    // M层
    data(){
      return {
        message:'hello world'
      }
    },
    // V层
    template:'<div>{{message}}</div>'
  });
  // VM层
  const vm = app.mount('#root')
   
  ```

## **Vue生命周期**
  在某一时刻会自动执行的函数

  1. beforeCreate：实例刚刚被创建，数据观测，事件，watch/computed都还没有初始化

  2. created：实例已经被创建完成，数据观测，事件，watch/computed都被完成初始化，但DOM还没有渲染

  3. beforeMount：在挂载开始之前被调用，即在调用mount方法之前调用

  4. mounted：el被新创建的vm.el替换，并挂载到实例上去之后调用，也就是dom渲染完成

  5. beforeUpdate：状态更新之前执行，发生在虚拟dom重新渲染和打补丁之前

  6. updated：状态更新完成后调用，发生在虚拟dom重新渲染和打补丁之后

  7. beforeUnmount：实例卸载之前调用，在这一步，实例仍然完全可用

  8. unmounted：Vue实例卸载之后调用，调用后，Vue实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁


## **常用模板语法**
  - v-html
    - 通过html的方式解析标签内容

  - v-bind(:)
    - 数据与标签属性绑定

  - v-once
    - 使用初始数据，不做变更

  - v-if
    - 

  - v-on(@)
    - 事件绑定指令



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
| 方法 | 说明 |
| ---- | ---- |
| readFile | 异步读取 |
| readFileSync | 同步读取 |
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
