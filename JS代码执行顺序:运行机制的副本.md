# JS代码执行顺序（运行机制）

1、代码的检查装载阶段（**预编译阶段**），此阶段进行变量和函数的声明，但是不对变量进行赋值，变量的默认值为undefined。此处涉及到变量提升，函数声明提升

2、**代码的执行阶段**，此阶段对变量进行赋值和函数的声明。

代码块之间是相互独立的，一个报错不会影响另一个的执行，但代码块之间可以共享变量以及函数方法。

还会涉及到执行上下文/事件队列/任务栈，宏任务/微任务

# JS内存机制

内存管理机制是:

​	内存基元在变量（对象，字符串等等）创建时分配，然后在他们不再被使用时“自动”释放。后者被称为垃圾回收

## 1. 内存模型

JS内存空间分为**栈(stack)**、**堆(heap)**、**池(一般也会归类为栈中)**。
其中**栈**存放**变量**，**堆**存放复杂对象，**池**存放**常量**。

**基础数据类型**： `Number` `String` `Null` `Undefined` `Boolean`  (ES6中新增symbol类型)，值的大小固定

## 2. 引用数据类型与堆内存

**引用类型**：object，array，值的大小不固定

引用数据类型的值是保存在堆内存中的对象。JS不允许直接访问堆内存中的位置，因此我们不能直接操作对象的堆内存空间。在操作对象时，实际上是在操作对象的引用而不是实际的对象。因此，引用类型的值都是按引用访问的。这里的引用，我们可以粗浅地理解为保存在栈内存中的一个地址，该地址与堆内存的实际值相关联。

采取**先进后出**原则

## 3. 内存的生命周期

1. **内存分配**：当我们申明变量、函数、对象的时候，系统会自动为他 们分配内存
2. **内存使用：**即读写内存，也就是使用变量、函数等
3. **内存回收：**使用完毕，由垃圾回收机制自动回收不再使用的内存

为了便于理解，我们使用一个简单的例子来解释这个周期。

```javascript
var a = 20;  // 在内存中给数值变量分配空间
alert(a + 100);  // 使用内存
var a = null; // 使用完毕之后，释放内存空间
```

null和undefined的本质区别是：前者没有分配内存空间，没有赋值；而undefined是分配了内存空间，赋值为undefined

##### 内存回收：

通过标记清除算法来找到哪些对象是没有再引用的

标记清除算法将“不再使用的对象”定义为“无法达到的对象”。简单来说，就是从根部（在JS中就是全局对象）出发定时扫描内存中的对象。凡是能从根部到达的对象，都是还需要使用的。那些无法由根部出发触及到的对象被标记为不再使用，稍后进行回收。

## 4. 内存泄漏

不再用到的内存，没有及时释放，就叫做内存泄漏（memory leak）

内存泄漏累计多了就可能会导致**内存溢出**，内存泄漏不是错误，内存溢出就是一种错误

#### 内存泄漏的识别方法：

- 经验法则是，如果连续五次垃圾回收之后，内存占用一次比一次大，就有内存泄漏
- 浏览器的timeline面板查看
- 命令行：process.memoryUsage，以`heapUsed`字段为标准来判断

## 5. `WeakSet` 和 `WeakMap`

ES6的新结构，它们对于值的引用都是不计入垃圾回收机制的，对值的弱引用

**HTML5提出Web Worker标准，允许JavaScript脚本创建多个线程，但是子线程完全受主线程控制，且不得操作DOM。所以，这个新标准并没有改变JavaScript单线程的本质。**

## 6. 性能监控

性能指标如下：

![img](https://img-blog.csdn.net/20171031174036233?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaXRlc3RfMjAxNg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

性能监控工具：

![image-20200522155534040](/Users/yujiebao/Library/Application Support/typora-user-images/image-20200522155534040.png)

