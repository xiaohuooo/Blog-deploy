# js内存

## 讲讲JavaScript垃圾回收是怎么做的
JS中内存的分配和回收都是自动完成的，内存在不使用的时候会被垃圾回收器自动回收。

常见的浏览器垃圾回收算法
- 标记清除(mark and sweep)
 > 这是JavaScript最常见的垃圾回收方式，当变量进入执行环境的时候，比如函数中声明一个变量，垃圾回收器将其标记为“进入环境”，当变量离开环境的时候（函数执行结束）将其标记为“离开环境”。垃圾回收器会在运行的时候给存储在内存中的所有变量加上标记，然后去掉环境中的变量以及被环境中变量所引用的变量（闭包），在这些完成之后仍存在标记的就是要删除的变量了
- 引用计数(reference counting)
> 在低版本IE中经常会出现内存泄露，很多时候就是因为其采用引用计数方式进行垃圾回收。引用计数的
> 策略是跟踪记录每个值被使用的次数，当声明了一个变量并将一个引用类型赋值给该变量的时候这个值
> 的引用次数就加1，如果该变量的值变成了另外一个，则这个值得引用次数减1，当这个值的引用次数变为0的时 候，说明没有变量在使用，这个值没法被访问了，因此可以将其占用的空间回收，这样垃圾回收器会在运行的时候清理掉引用次数为0的值占用的空间。在IE中虽然JavaScript对象通过标记清除的方式进行垃圾回收，但BOM与DOM对象却是通过引用计数回收垃圾的，

## js的基本类型和复杂类型是储存在哪⾥的
>基本类型 储存在栈里面 先进先出  但是一旦被闭包引用则成为常驻内存，会存在内存中。
>复杂类型 储存在堆里面 先进后出

![](https://img-blog.csdnimg.cn/20201121195959565.jpg)


