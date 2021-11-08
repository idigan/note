## 浏览器渲染机制

#### 网页生成过程：

1. `html` 被`html`解析器解析成`dom`树
2. `css` 被 `css` 解析器解析成 `CSSOM` 树
3. 结合`DOM`树和`CSSOM`树，生成一棵渲染树(`Render Tree`)
4. 生成布局（`flow`），即将所有渲染树的所有节点进行平面合成
5. 将布局绘制（`paint`）在屏幕上

#### 重绘

当一个元素的外观发生改变，但没有改变布局,重新把元素外观绘制出来的过程，叫做重绘。

触发：

1. 改变元素的`color、background、box-shadow`等属性

#### 重排(回流)

当`DOM`的变化影响了元素的几何信息(`DOM`对象的位置和尺寸大小)，浏览器需要重新计算元素的几何属性，将其安放在界面中的正确位置，这个过程叫做重排。

触发：

1. 添加或者删除可见的DOM元素
2. 元素尺寸改变——边距、填充、边框、宽度和高度

重排优化建议：

1. 分离读写操作
2. 样式集中修改
3. 存需要修改的`DOM`元素
4. 尽量只修改`position：absolute`或`fixed`元素，对其他元素影响不大
5. 动画开始`GPU`加速，`translate`使用`3D`变化

#### 输入 URL 到展示页面过程

1. 进行 `URL `解析，检查输入的内容是否符合规则

2. 浏览器查找本地是否有缓存

3. 进行 `DNS` 解析，获取请求域名服务器的 `IP`地址

4. 三次握手，建立`TCP`连接

5. 浏览器端会构建请求行、 请求头等信息，并把和该域名相关的 Cookie 等数据附加到请求头中，然后向服务器发送构建的请求信息

6. 四次挥手，断开`TCP`连接

7. 客户端解析资源

   ```js
   三个方面：
   网络篇:
   	     构建请求
                查找强缓存
                DNS解析
                建立TCP连接(三次握手)
   			发送HTTP请求(网络请求后网络响应)
   浏览器解析篇:
   	    解析html构建DOM树
               解析css构建CSS树、样式计算
               生成布局树(Layout Tree)
   浏览器渲染篇:
               建立图层树(Layer Tree)
               生成绘制列表
               生成图块并栅格化
               显示器显示内容
               最后断开连接：TCP 四次挥手
               (浏览器会将各层的信息发送给GPU,GPU会将各层合成,显示在屏幕上)
   ```

## css盒子模型

#### 盒子

所有`HTML`元素可以看作盒子，在CSS中，`"box model"`这一术语是用来设计和布局时使用。 `CSS`盒模型本质上是一个盒子，封装周围的`HTML`元素，它包括：边距，边框，填充，和实际内容。 盒模型允许我们在其它元素和周围元素边框之间的空间放置元素

#### h5新特性，语义化

从标签上可以直观的知道这个标签的作用，而不是滥用 div

优点：

1. 代码结构清晰，易于阅读，方便后续维护和开发
2. 方便其他设备解析（屏幕阅读器）更具语义渲染页面
3. 有利于搜索引擎优化（`SEO`），搜索引擎会根据不同的标签来赋予不同的权重

#### css样式优先级

!important>style>id>class

#### BFC

`BFC` 是 `Block Formatting Context `的缩写，即块格式化上下文。`BFC`是CSS布局的一个概念，是一个环境，里面的元素不会影响外面的元素。 

布局规则：Box是CSS布局的对象和基本单位，页面是由若干个Box组成的。元素的类型和display属性，决定了这个Box的类型。不同类型的Box会参与不同的`Formatting Context`。

创建：浮动元素 `display：inline-block position:absolute` 

应用: 

1. 分属于不同的`BFC`时,可以防止`margin`重叠
2. 清除内部浮动 3.自适应多栏布局

## DOM、BOM对象

`BOM（Browser Object Model）`是指浏览器对象模型，可以对浏览器窗口进行访问和操作。使用 BOM，开发者可以移动窗口、改变状态栏中的文本以及执行其他与页面内容不直接相关的动作。 使 `JavaScript` 有能力与浏览器"对话"。 

`DOM （Document Object Model）`是指文档对象模型，通过它，可以访问`HTML`文档的所有元素。 `DOM `是 `W3C`（万维网联盟）的标准。`DOM` 定义了访问 `HTML` 和` XML` 文档的标准： "W3C 文档对象模型（DOM）是中立于平台和语言的接口，它允许程序和脚本动态地访问和更新文档的内容、结构和样式。

`W3C DOM` 标准被分为 3 个不同的部分：

- 核心 `DOM` - 针对任何结构化文档的标准模型
- `XML DOM` - 针对 XML 文档的标准模型
- `HTML DOM` - 针对 HTML 文档的标准模型

什么是` XML DOM`？ `XML DOM` 定义了所有 XML 元素的对象和属性，以及访问它们的方法。

 什么是 HTML DOM？ HTML DOM 定义了所有 HTML 元素的对象和属性，以及访问它们的方法。

## js数据类型

- `string、number、boolean、null、undefined、object(function、array)、symbol(ES10 BigInt)`
- `typeof` 主要用来判断数据类型 返回值有`string、boolean、number、function、object、undefined。`
- `instanceof` 判断该对象是谁的实例。
- `Object.prototype.toString.call()` 判断数据类型
- `null`表示空对象 `undefined`表示已在作用域中声明但未赋值的变量

## 闭包

闭包是指有权访问另一个函数作用域中的变量的函数

内部函数没有执行完成，外部函数变量不会被销毁

用途：

- 能够访问函数定义时所在的词法作用域(阻止其被回收)
- 私有化变量
- 模拟块级作用域
- 创建模块

缺点：

- 会导致函数的变量一直保存在内存中，过多的闭包可能会导致内存泄漏

## 原型、原型链

**原型**：对象中固有的`__proto__`属性，该属性指向对象的`prototype`原型属性

**原型链**：当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么它就会去它的原型对象里找这个属性，这个原型对象又会有自己的原型，于是就这样一直找下去，也就是原型链的概念。原型链的尽头一般来说都是`Object.prototype`所以这就是我们新建的对象为什么能够使用`toString()`等方法的原因。

**特点**：`javaScript`对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变。

## this指向

`this`对象是是执行上下文中的一个属性，它指向最后一次调用这个方法的对象，在全局函数中，`this`等于`window`，而当函数被作为某个对象调用时，this等于那个对象。 在实际开发中，`this `的指向可以通过四种调用模式来判断。

- 函数调用，当一个函数不是一个对象的属性时，直接作为函数来调用时，`this`指向全局对象。
- 方法调用，如果一个函数作为一个对象的方法来调用时，`this`指向这个对象。
- 构造函数调用，`this`指向这个用`new`新创建的对象。
- 第四种是 `apply 、 call 和 bind `调用模式，这三个方法都可以显示的指定调用函数的 this 指向。`apply`接收参数的是数组，`call`接受参数列表，`` bind`方法通过传入一个对象，返回一个` this `绑定了传入对象的新函数。这个函数的 `this`指向除了使用`new `时会被改变，其他情况下都不会改变。

## new关键字

1. 首先创建了一个新的空对象
2. 设置原型，将对象的原型设置为函数的`prototype`对象。
3. 让函数的`this`指向这个对象，执行构造函数的代码（为这个新对象添加属性）
4. 判断函数的返回值类型，如果是值类型，返回创建的对象。如果是引用类型，就返回这个引用类型的对象。

## 作用域、作用域链

`作用域`负责收集和维护由所有声明的标识符（变量）组成的一系列查询，并实施一套非常严格的规则，确定当前执行的代码对这些标识符的访问权限。(全局作用域、函数作用域、块级作用域)。 作用域链就是从当前作用域开始一层一层向上寻找某个变量，直到找到全局作用域还是没找到，就宣布放弃。这种一层一层的关系，就是`作用域链`。
