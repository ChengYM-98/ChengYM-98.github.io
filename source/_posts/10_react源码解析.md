---
title: ReactFiber
date: 2022-11-05 16:14:36
tags: React
categories: 前端进阶
cover: /img/05.jpg
---
# React核心
    React 是一个网页UI框架,通过组件化的方式解决视图层开发复用的问题，本质是一个组件化框架。

    他的核心设计思路有三点，分别是声明式、组件化与通用性

    声明式的优势在于直观与组合

    组件化的优势在于视图的拆分与模块复用，可以更容易做到高内聚低耦合

    通用性在于一次学习，随处编写，将DOM抽象为虚拟DOM，开发者并不会直接操作DOM使得React不再局限于Web开发，适用范围广

劣势：技术选型和学习使用上造成了一定的成本。
# React 特色
1. React 是一个纯粹的 ui 框架，ui=fn(x),通过 state 映射 ui 来屏蔽掉 dom 操作
2. React 的核心 API 是 setState，所有的内容都是围绕组件化进行，没有 direvtive 双向绑定和其他 API
3. 思想超前，React16 引入 fiber 概念，从根本上解决 js 单线程运行计算量大导致动画或 ui 卡顿

# ReactFiber
## currentFiber与workInProgressFiber
 当前被刷新用来渲染用户界面的树，被称为 current，它用来渲染当前用户界面。每当有更新时，Fiber 会建立一个 workInProgress 树，它是由 React 元素中已经更新数据创建的。React 在这个 workInProgress 树上执行工作，并在下次渲染时使用这个更新的树。一旦这个 workInProgress 树被渲染到用户界面上，它就成为 current 树。


Fiber 树的遍历是这样发生的。

    开始：Fiber 从最上面的 React 元素开始遍历，并为其创建一个 fiber 节点。
    子节点：然后，它转到子元素，为这个元素创建一个 fiber 节点。这样继续下去，直到到达叶子元素。
    兄弟节点： 现在，它检查是否有兄弟节点元素。如果有，它就遍历兄弟节点元素，然后再到兄弟姐妹的叶子元素。
    返回：如果没有兄弟节点，那么它就返回到父节点。
    每个 fiber 都有一个子属性（如果没有子属性，则为空值）、兄弟和父节点属性（正如你在前面的章节中看到的 fiber 的结构）。这些是 fiber 中的指针，可以作为一个链表来工作。


让我们举同样的例子，我们先给特定 React 元素的 fiber 命名。
```js
function App() {
  // App
  return (
    <div className="wrapper">
      {" "}
      // W<div className="list">
        {" "}
        // L<div className="list_item">List item A</div> // LA
        <div className="list_item">List item B</div> // LB
      </div>
      <div className="section">
        {" "}
        // S<button>Add</button> // SB
        <span>No. of items: 2</span> // SS
      </div>
    </div>
  );
}
```
ReactDOM.render(<App />, document.getElementById("root")); // HostRoot
首先，我们将快速介绍创建树时的挂载阶段，之后，我们将了解树更新后详细逻辑。

## 初始渲染
    App 组件被渲染在 root div 中，它的 id 是 root。

    在进一步遍历之前，React Fiber 创建一个根 fiber 。每个 fiber 树都有一个根节点。在我们这里，它是 HostRoot。如果我们在 DOM 中导入多个 React 应用，可以有多个根节点。

    在第一次渲染之前，不会有任何树。React Fiber 遍历每个组件的渲染函数的输出，并为每个 React 元素在树上创建一个 fiber 节点。 它用 createFiberFromTypeAndProps 来将 React 元素转换为 fiber 。React 元素可以是一个类组件，也可以是一个宿主组件，如 div 或 span。对于类组件，它创建一个实例，而对于宿主组件，它从 React 元素中获得数据和 props。

    因此，正如例子中所示，它创建了一个 fiber App。再往前走，它又创建了一个 fiber ，W，然后它转到子 div 并创建了一个 fiber L，如此下去，它为它的子代创建了一个 fiber ，LA 和 LB。 fiber ，LA，将有返回（在这种情况下也可以被称为父级） fiber 作为 L，和兄弟姐妹作为 LB。

![Fiber 节点链接图](/img/Fiber1.png)  


## 更新阶段
    现在，让我们来谈谈第二种情况，例如由于 setState 而导致的更新。

    所以，在这个时候，Fiber 已经有了 current tree。对于每次更新，它都会建立一个 workInProgress 树。它从根 fiber 开始，遍历该树，直到叶子节点。与初始渲染阶段不同，它不会为每个 React 元素创建一个新的 fiber 。它只是为该 React 元素使用预先存在的 fiber ，并在更新阶段合并来自更新元素的新数据和 props。

    早些时候，在 React 15 中，堆栈协调器是同步的。所以，一个更新会递归地遍历整个树，并制作一个树的副本。假设在这之间，如果有其他的更新比它的优先级更高，那么就没有机会中止或暂停第一个更新并执行第二个更新。

    React Fiber 将更新划分为工作单元。它可以为每个工作单元分配优先级，并有能力暂停、重用或在不需要时中止工作单元。 React Fiber 将工作分为多个工作单位，也就是 fiber 。它将工作安排在多个框架中，并使用来自 requestIdleCallback 的 deadline 。每个更新都有其优先级的定义，如动画，或用户输入的优先级高于从获取的数据中渲染项目的列表。Fiber 使用 requestAnimationFrame 来处理优先级较高的更新，使用 requestIdleCallback 处理优先级较低的更新。因此，在调度工作时，Fiber 检查当前更新的优先级和 deadline （帧结束后的自由时间）。

    如果优先级高于待处理的工作，或者没有 截止日期 或者截止日期尚未到达，Fiber 可以在一帧之后安排多个工作单元。而下一组工作单元会被带到更多的帧上。这就是使 Fiber 有可能暂停、重用和中止工作单元的原因。

    那么，让我们看看在预定的工作中实际发生了什么。有两个阶段来完成工作。render 和 commit。

## 渲染阶段
    实际的树形遍历和 deadline 的使用发生在这个阶段。这是 Fiber 的内部逻辑，所以在这个阶段对 Fiber 树所做的改变对用户来说是不可见的。因此，Fiber 可以暂停、中止或分担多个框架的工作。

    我们可以把这个阶段称为协调阶段。 fiber 从 fiber 树的根部开始遍历，处理每个 fiber 。每一个工作单位都会调用workLoop 函数来执行工作。我们可以把这个工作的处理分成两个步骤。begin 和 complete 。

## 开始阶段
    如果你从 React 代码库中找到 workLoop 函数，它就会调用 performUnitOfWork，它把 nextUnitOfWork 作为一个参数，它就只是个工作的单位，将被执行。 performUnitOfWork 函数内部调用 beginWork 函数。这是 fiber 上发生实际工作的地方，而 performUnitOfWork 只是发生迭代的地方。

    在 beginWork 函数中，如果 fiber 没有任何待处理的工作，它就会直接跳出（跳过） fiber 而不进入开始阶段。这就是在遍历大树时， fiber 跳过已经处理过的 fiber ，直接跳到有待处理工作的 fiber 。如果你看到大的 beginWork 函数代码块，我们会发现一个开关块，根据 fiber 标签，调用相应的 fiber 更新函数。就像 updateHostComponent 用于宿主组件。这些函数会更新 fiber 。

    如果有子 fiber ，beginWork函数返回子 fiber ，如果没有子 fiber 则返回空。函数 performUnitOfWork 持续迭代并调用子 fiber ，直到叶节点到达。在叶子节点的情况下，beginWork 返回 null，因为没有任何子节点，performUnitOfWork 函数调用 completeUnitOfWork 函数。现在让我们看看完善阶段。

## 完善阶段
    这个 completeUnitOfWork 函数通过调用一个 completeWork 函数来完成当前单位的工作。如果有的话，completeUnitOfWork 会返回一个同级的 fiber 来执行下一个工作单元，如果没有工作的话，则会完成 return(parent) fiber 。这将一直持续到返回值为空，也就是说，直到它到达根节点。和 beginWork 一样，completeWork 也是一个发生实际工作的函数，而 completeUnitOfWork 是用于迭代的。

    渲染阶段的结果会产生一个效果列表（副作用）。这些效果就像插入、更新或删除宿主组件的节点，或调用类组件节点的生命周期方法。这些 fiber 被标记为各自的效果标签。

    在渲染阶段之后，Fiber 将准备提交更新。

## 提交阶段
    这是一个阶段，完成的工作将被用来在用户界面上渲染它。由于这一阶段的结果对用户来说是可见的，所以不能被分成部分渲染。这个阶段是一个同步的阶段。

    在这个阶段的开始，Fiber 有已经在 UI 上渲染的 current 树，finishedWork，或者在渲染阶段建立的 workInProgress 树和效果列表。

    effect 列表是 fiber 的链表，它有副作用。所以，它是渲染阶段的 workInProgress 树的节点的一个子集，它有副作用（更新）。effect 列表的节点是用 nextEffect 指针链接的。

    在这个阶段调用的函数是 completeRoot。

    在这里，workInProgress 树成为 current 树，因为它被用来渲染 UI。实际的 DOM 更新，如插入、更新、删除，以及对生命周期方法的调用或者更新相对应的引用 —— 发生在 effect 列表中的节点上。

    这就是 fiber 协调器的工作方式。