## dom

## DOM的事件模型是什么
一个事件发生后，会在子元素及父元素之间进行传播（propagation），这种传播分为三个阶段。
（这种三阶段的传播模型，使得同一个事件会在多个节点上触发。）

由外向内找监听函数就是事件捕获
在目标节点触发事件  
由内而外找监听函数就是事件冒泡

事件传播的最上层对象是window，上例的事件传播顺序，在捕获阶段依次为window、document、html、body、父节点、目标节点，在冒泡阶段依次为目标节点、父节点、body、html、document、window。

DOM事件传播的三个阶段：捕获阶段，目标阶段，冒泡阶段
## DOM的事件流是什么
在js中，事件流就是事件在目标元素和祖先元素间的触发顺序。按照事件传播的顺序，事件流可分两种：
1、冒泡型事件流，事件的传播是从最特定的事件目标到最不特定的事件目标；  
2、捕获型事件流，事件的传播是从最不特定的事件目标到最特定的事件目标。
## 什么是事件委托
事件委托就是把原本需要绑定在子元素上的事件（onclick、onkeydown 等）委托给它的父元素，让父元素来监听子元素的冒泡事件，并在子元素发生事件冒泡时找到这个子元素。