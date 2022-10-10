---
title: 虚拟dom 
author: 小虎
date: '2022-09-15'
---
在Web开发中，需要将数据的变化实时反映到UI上，这时就需要对DOM进行操作，但是复杂或频繁的DOM操作通常是性能瓶颈> 产生的原因，为此，React引入了虚拟DOM（Virtual DOM）的机制。

## 什么是虚拟dom

在React中，render执行的结果得到的并不是真正的DOM节点，结果仅仅是轻量级的JavaScript对象，我们称之为virtual DOM。  
 
实际也是操作Dom树进行渲染更新,但是它只是针对修改部分进行局部渲染,将影响降到最低,虽然实现方式各有不同,但是大体步骤如下:  
1.用Javascript对象结构描述Dom树结构,然后用它来构建真正的Dom树插入文档  
2.当状态发生改变之后,重新构造新的Javascript对象结构和旧的作对比得出差异  
3.针对差异之处进行重新构建更新视图  
无非就是利用Js做一层映射比较,操作简单并且速度远远高于直接比较Dom树

## 虚拟DOM VS 直接操作原生DOM？
如果没有 Virtual DOM，简单来说就是直接重置 innerHTML。这样操作，在一个大型列表所有数据都变了的情况下，还算是合理，但是，当只有一行数据发生变化时，它也需要重置整个 innerHTML，这时候显然就造成了大量浪费。

比较innerHTML 和Virtual DOM 的重绘过程如下：

innerHTML: render html string + 重新创建所有 DOM 元素

Virtual DOM: render Virtual DOM + diff + 必要的 DOM 更新

和 DOM 操作比起来，js 计算是非常便宜的。Virtual DOM render + diff 显然比渲染 html 字符串要慢，但是，它依然是纯 js 层面的计算，比起后面的 DOM 操作来说，依然便宜了太多。当然，曾有人做过验证说React的性能不如直接操作真实DOM

## 虚拟DOM VS MVVM？
相比起 React，其他 MVVM 系框架比如 Angular, Knockout 以及 Vue、Avalon 采用的都是数据绑定：通过 Directive/Binding 对象，观察数据变化并保留对实际 DOM 元素的引用，当有数据变化时进行对应的操作。MVVM 的变化检查是数据层面的，而 React 的检查是 DOM 结构层面的。MVVM 的性能也根据变动检测的实现原理有所不同：Angular 的脏检查使得任何变动都有固定的 O(watcher count) 的代价；Knockout/Vue/Avalon 都采用了依赖收集，在 js 和 DOM 层面都是 O(change)：


- 脏检查：scope digest + 必要 DOM 更新
- 依赖收集：重新收集依赖 + 必要 DOM 更新

可以看到，Angular 最不效率的地方在于任何小变动都有的和 watcher 数量相关的性能代价。但是！当所有数据都变了的时候，Angular 其实并不吃亏。依赖收集在初始化和数据变化的时候都需要重新收集依赖，这个代价在小量更新的时候几乎可以忽略，但在数据量庞大的时候也会产生一定的消耗。
MVVM 渲染列表的时候，由于每一行都有自己的数据作用域，所以通常都是每一行有一个对应的 ViewModel 实例，或者是一个稍微轻量一些的利用原型继承的 "scope" 对象，但也有一定的代价。所以，MVVM 列表渲染的初始化几乎一定比 React 慢，因为创建 ViewModel / scope 实例比起 Virtual DOM 来说要昂贵很多。这里所有 MVVM 实现的一个共同问题就是在列表渲染的数据源变动时，尤其是当数据是全新的对象时，如何有效地复用已经创建的 ViewModel 实例和 DOM 元素。假如没有任何复用方面的优化，由于数据是 “全新” 的，MVVM 实际上需要销毁之前的所有实例，重新创建所有实例，最后再进行一次渲染！这就是为什么题目里链接的 angular/knockout 实现都相对比较慢。相比之下，React 的变动检查由于是 DOM 结构层面的，即使是全新的数据，只要最后渲染结果没变，那么就不需要做无用功。
Angular 和 Vue 都提供了列表重绘的优化机制，也就是 “提示” 框架如何有效地复用实例和 DOM 元素。比如数据库里的同一个对象，在两次前端 API 调用里面会成为不同的对象，但是它们依然有一样的 uid。这时候你就可以提示 track by uid 来让 Angular 知道，这两个对象其实是同一份数据。那么原来这份数据对应的实例和 DOM 元素都可以复用，只需要更新变动了的部分。或者，你也可以直接 track by $index 来进行 “原地复用”：直接根据在数组里的位置进行复用。在题目给出的例子里，如果 angular 实现加上 track by $index 的话，后续重绘是不会比 React 慢多少的。甚至在 dbmonster 测试中，Angular 和 Vue 用了 track by $index 以后都比 React 快: dbmon (注意 Angular 默认版本无优化，优化过的在下面）

在比较性能的时候，要分清楚初始渲染、小量数据更新、大量数据更新这些不同的场合。Virtual DOM、脏检查 MVVM、数据收集 MVVM 在不同场合各有不同的表现和不同的优化需求。Virtual DOM 为了提升小量数据更新时的性能，也需要针对性的优化，比如 shouldComponentUpdate 或是 immutable data。

初始渲染：Virtual DOM > 脏检查 >= 依赖收集
小量数据更新：依赖收集 >> Virtual DOM + 优化 > 脏检查（无法优化） > Virtual DOM 无优化
大量数据更新：脏检查 + 优化 >= 依赖收集 + 优化 > Virtual DOM（无法/无需优化）>> MVVM 无优化
（该段落借鉴了知乎的相关回答）




## 对React虚拟DOM的误解？
React 从来没有说过 “React 比原生操作 DOM 快”。React给我们的保证是，在不需要手动优化的情况下，它依然可以给我们提供过得去的性能。

React掩盖了底层的 DOM 操作，可以用更声明式的方式来描述我们目的，从而让代码更容易维护。下面还是借鉴了知乎上的回答：没有任何框架可以比纯手动的优化 DOM 操作更快，因为框架的 DOM 操作层需要应对任何上层 API 可能产生的操作，它的实现必须是普适的。针对任何一个 benchmark，我都可以写出比任何框架更快的手动优化，但是那有什么意义呢？在构建一个实际应用的时候，你难道为每一个地方都去做手动优化吗？出于可维护性的考虑，这显然不可能。

## react 合成事件
React 自己实现了这么一套事件机制，它在 DOM 事件体系基础上做了改进，减少了内存的消耗，并且最大程度上解决了 IE 等浏览器的不兼容问题

- React 上注册的事件最终会绑定在document这个 DOM 上，而不是 React 组件对应的 DOM(减少内存开销就是因为所有的事件都绑定在 document 上，其他节点没有绑定事件)

- React 自身实现了一套事件冒泡机制，所以这也就是为什么我们 event.stopPropagation() 无效的原因。

- React 通过队列的形式，从触发的组件向父组件回溯，然后调用他们 JSX 中定义的 callback

- React 有一套自己的合成事件 SyntheticEvent，不是原生的，这个可以自己去看官网

- React 通过对象池的形式管理合成事件对象的创建和销毁，减少了垃圾的生成和新对象内存的分配，提高了性能

## 谈谈你对react fiber 结构理解

fiber作为一种数据结构，用于代表某些worker，换句话说，就是一个work单元，通过Fiber的架构，提供了一种跟踪，调度，暂停和中止工作的便捷方式。  
- 为每个增加了优先级，优先级高的任务可以中断低优先级的任务。然后再重新，注意是重新执行优先级低的任务
- 增加了异步任务，调用requestIdleCallback api，浏览器空闲的时候执行
- dom diff树变成了链表，一个dom对应两个fiber（一个链表），对应两个队列，这都是为找到被中断的任务，重新执行  

从架构角度来看，Fiber 是对 React核心算法（即调和过程）的重写

从编码角度来看，Fiber是 React内部所定义的一种数据结构，它是 Fiber树结构的节点单位，也就是 React 16 新架构下的虚拟DOM

一个 fiber就是一个 JavaScript对象，包含了元素的信息、该元素的更新操作队列、类型

## react 中key是什么?有什么作用？
key是react用来追踪哪些列表的元素被修改，被添加或者是被删除的辅助标示。在开发过程中我们需要保证某个元素的key在其同级元素中具有唯一性。  

元素key属性的作用是用于判断元素是新创建的还是被移动的元素，从而减少不必要的元素渲染

也就是为了提高diff同级比较的效率，避免原地复用带来的副作用；key是react用来追踪列表的元素被修改，被添加或者是被删除的标识。  

## 组件通信有哪些方式？
- 父组件向子组件通信，可以通过 props 方式传递数据；也可以通过 ref 方式传递数据；
- 子组件向父组件通信，通过回调函数方式传递数据；
- 父组件向后代所有组件传递数据，如果组件层级过多，通过 props 的方式传递数据很繁琐，可以通过 Context.Provider 的方式；
- 一个数据源实现跨组件通信，通过指定 contextType 的方式来实现；
- 多个数据源实现跨组件通信，使用 Context.Consumer 方式实现；
- Redux


## react合成事件机制