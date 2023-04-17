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

