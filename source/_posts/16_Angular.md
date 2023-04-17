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

