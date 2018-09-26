## 目录

- [JavaScript](#javascript)
- [CSS](#css)
- [HTML](#html)

## JavaScript

### `setTimeout`中的`this`指向？

```js
const obj = {
  a: 10,
  fn: function() {
    console.log(this.a)
  }
}
setTimeout(obj.fn, 10) // undefined
// 上面的调用中 this 指向全局，因为obj.fn会隐式赋值给setTimeout的第一个参数，相当于下面
let fn = obj.fn
fn() // undefined，虽然引用的是obj的属性，但实际上它引用的是fn函数本身。
// 解决方案
setTimeout(obj.fn.bind(obj), 10)
```

### `undefined + 1`?

- 隐式转换后，两边不能使用 `+` 操作符进行运算的表达式的结果为 `NaN`。

### 箭头函数与普通函数的区别？

- 箭头函数中的`this`继承自它的外层作用域，并且不能被改变。

- 箭头函数会把 `arguments` 当成一个普通的变量，但是`arguments`可以用`...rest`取代。

  ```js
  function foo() { 
    setTimeout(() => { console.log(this.a) }, 10) 
  } 
  let obj = { a: 1 } 
  foo.call(obj) // 1 
  // 类似于下面 
  function foo() { 
    const self = this 
    setTimeout(function () { console.log(self.a) }, 10) 
  } 
  foo.call(obj) // 1
  ```

### 什么是作用域？

- 用来存储、访问变量的一套规则，这套规则用来管理引擎如何在当前作用域及子作用域中根据标识符名进行变量查找。
- JavaScript 采用词法作用域(lexical scoping)，也就是静态作用域。

### `null`的类型？

- `null`本身为基本类型，但是`typeof null`返回值为`object`这是语言本身的bug。 
- 原理：不同的对象在底层表示为二进制，js中前三位为0的话被判断为`object`，而`null`的二进制都为0，因此`typeof null`为`object`。

### `JSON.stringify`

```js
JSON.stringify('') // """"
function fn() {}
JSON.stringify(fn) // undefined
```

### 请解释事件委托

事件委托是将事件监听器添加到父元素，而不是每个子元素。当触发子元素时，事件会冒泡到父元素，监听器就会触发。这种技术的好处是：

- 内存占用减少，因为只需要一个父元素的事件处理程序，而不必为每个后代都添加事件处理程序。
- 无需从已删除的元素中解绑处理程序，也无需将处理程序绑定到新元素上。

### 使用 Ajax 的优缺点分别是什么？

优点

- 交互性更好。来自服务器的新内容可以动态更改，无需重新加载整个页面。
- 减少与服务器的连接，因为脚本和样式只需要被请求一次。
- 状态可以维护在一个页面上。JavaScript 变量和 DOM 状态将得到保持，因为主容器页面未被重新加载。

缺点

- 动态网页很难收藏。
- 如果 JavaScript 已在浏览器中被禁用，则不起作用。
- 有些网络爬虫不执行 JavaScript，也不会看到 JavaScript 加载的内容。

### 请解释单页应用是什么，如何使其对 SEO 友好。

在过去，浏览器从服务器接收 HTML 并渲染。当用户导航到其它 URL 时，服务器会为新页面发送新的 HTML。这被称为服务器端渲染。

然而，在现代的 SPA 中，客户端渲染取而代之。浏览器从服务器加载初始页面、整个应用程序所需的脚本（框架、库、应用代码）和样式表。当用户导航到其他页面时，不会触发页面刷新。该页面的 URL 通过 [HTML5 History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API) 进行更新。浏览器通过 [AJAX](https://developer.mozilla.org/en-US/docs/AJAX/Getting_Started) 请求向服务器检索新页面所需的数据（通常采用 JSON 格式）。然后，SPA 通过 JavaScript 来动态更新页面，这些 JavaScript 在初始页面加载时已经下载。这种模式类似于原生移动应用的工作方式。

优点：

- 用户感知响应更快，用户切换页面时，不再看到因页面刷新而导致的白屏。
- 对服务器进行的 HTTP 请求减少，因为对于每个页面加载，不必再次下载相同的资源。
- 客户端和服务器之间的关注点分离。可以为不同平台（例如手机、聊天机器人、智能手表）建立新的客户端，而无需修改服务器代码。只要 API 没有修改，可以单独修改客户端和服务器上的代码。

缺点：

- 由于加载了多个页面所需的框架、应用代码和资源，导致初始页面加载时间较长。
- SPA 依赖于 JavaScript 来呈现内容，但并非所有搜索引擎都在抓取过程中执行 JavaScript。这无意中损害了应用的搜索引擎优化（SEO）。为了解决这个问题，可以在服务器端渲染你的应用，或者使用诸如 [Prerender](https://prerender.io/) 的服务来“在浏览器中呈现你的 javascript，保存静态 HTML，并将其返回给爬虫”。

### 使用`let`、`var`和`const`创建变量有什么区别？

用`var`声明的变量的作用域是它当前的执行上下文，它可以是嵌套的函数，也可以是声明在任何函数外的变量。

`let`和`const`是块级作用域，意味着它们只能在最近的一组花括号（function、if-else 代码块或 for 循环中）中访问。

`var`会使变量提升，这意味着变量可以在声明之前使用。`let`和`const`不会使变量提升，提前使用会报错。

用`var`重复声明不会报错，但`let`和`const`会。

`let`和`const`的区别在于：`let`允许多次赋值，而`const`只允许一次。

### 你能举出一个柯里化函数（curry function）的例子吗？它有哪些好处？

柯里化（currying）是一种模式，其中具有多个参数的函数被分解为多个函数，当被串联调用时，将一次一个地累积所有需要的参数。这种技术帮助编写函数式风格的代码，使代码更易读、紧凑。值得注意的是，对于需要被 curry 的函数，它需要从一个函数开始，然后分解成一系列函数，每个函数都需要一个参数。

```js
function curry(fn) {
  if (fn.length === 0) {
    return fn;
  }

  function _curried(depth, args) {
    // 封装后的fn，再次调用封装后的fn时，如果不是最后一个参数，返回封装后的fn，如果是返回fn运行的结果
    return function(newArgument) {
      if (depth - 1 === 0) {
        return fn(...args, newArgument);
      }
      return _curried(depth - 1, [...args, newArgument]);
    };
  }

  return _curried(fn.length, []);
}

function add(a, b) {
  return a + b;
}

var curriedAdd = curry(add);
var addFive = curriedAdd(5);

var result = [0, 1, 2, 3, 4, 5].map(addFive); // [5, 6, 7, 8, 9, 10]
```

### Function.length?

`length` 是函数对象的一个属性值，指该函数有多少个必须要传入的参数，即形参的个数。

### 使用扩展运算符（spread）的好处是什么，它与使用剩余参数语句（rest）有什么区别？

在函数泛型编码时，ES6 的扩展运算符非常有用，因为我们可以轻松创建数组和对象的拷贝，而无需使用`Object.assign`、`slice`或其他函数库。这个语言特性在 Redux 和 rx.js 的项目中经常用到。

```js
function putDookieInAnyArray(arr) {
  return [...arr, 'dookie'];
}

const result = putDookieInAnyArray(['I', 'really', "don't", 'like']); // ["I", "really", "don't", "like", "dookie"]

const person = {
  name: 'Todd',
  age: 29,
};

const copyOfTodd = { ...person };
```

ES6 的剩余参数语句提供了一个简写，允许我们将不定数量的参数表示为一个数组。它就像是扩展运算符语法的反面，将数据收集到数组中，而不是解构数组。剩余参数语句在函数参数、数组和对象的解构赋值中有很大作用。

```js
function addFiveToABunchOfNumbers(...numbers) {
  return numbers.map(x => x + 5);
}

const result = addFiveToABunchOfNumbers(4, 5, 6, 7, 8, 9, 10); // [9, 10, 11, 12, 13, 14, 15]

const [a, b, ...rest] = [1, 2, 3, 4]; // a: 1, b: 2, rest: [3, 4]
```

### 数组去重？

```js
let arr = [1,2,3,3]
[...new Set(arr)] // [1,2,3]
```



## CSS

### 使用css如何选择被选中的复选框/单选框？

- `:checked` CSS 伪类选择器表示任何处于选中状态的radio(``<input type="radio">``), checkbox (``<input type="checkbox">``) 或 select 元素中的option元素。

### CSS 选择器的优先级？

- 第一级：内联样式（inline style）。
- 第二级：ID 选择器。
- 第三级：类选择器、属性选择器和伪类选择器。
- 第四级：标签（类型）选择器和伪元素选择器。

### 重置（resetting）CSS 和 标准化（normalizing）CSS 的区别是什么？

- **重置（Resetting）**： 重置意味着除去所有的浏览器默认样式。对于页面所有的元素，像`margin`、`padding`、`font-size`这些样式全部置成一样。你将必须重新定义各种元素的样式。
- **标准化（Normalizing）**： 标准化没有去掉所有的默认样式，而是保留了有用的一部分，同时还纠正了一些常见错误。

### 如何解决不同浏览器的样式兼容性问题？

- 使用已经处理好此类问题的库，比如 Bootstrap。
- 使用 `autoprefixer` 自动生成 CSS 属性前缀。
- 使用 Reset CSS 或 Normalize.css。

### 使用 CSS 预处理的优缺点分别是什么？

- 优点
  - 提高 CSS 可维护性。
  - 易于编写嵌套选择器。
  - 引入变量，增添主题功能。可以在不同的项目中共享主题文件。
  - 通过混合（Mixins）生成重复的 CSS。
  - 将代码分割成多个文件。不进行预处理的 CSS，虽然也可以分割成多个文件，但需要建立多个 HTTP 请求加载这些文件。
- 缺点
  - 需要预处理工具。
  - 重新编译的时间可能会很慢。

### 什么情况下，用`translate()`而不用绝对定位？什么时候，情况相反。

- `translate()`是`transform`的一个值。改变`transform`或`opacity`不会触发浏览器重新布局（reflow）或重绘（repaint），只会触发复合（compositions）。而改变绝对定位会触发重新布局，进而触发重绘和复合。 因此`translate()`更高效，可以缩短平滑动画的绘制时间。

- 当使用`translate()`时，元素仍然占据其原始空间（有点像`position：relative`），这与改变绝对定位不同。

## HTML

### `DOCTYPE`有什么用？

- `DOCTYPE`是“document type”的缩写。它是 HTML 中用来区分标准模式和[怪异模式](https://quirks.spec.whatwg.org/#history)的声明，用来告知浏览器使用标准模式渲染页面。

- html5中，在页面开始处添加`<!DOCTYPE html>`即可。

### HTML5有哪些更新？

- 语义：提供更准确的描述内容。
- 连接：新的方式与服务器通信（WebSocket、Server-sent events、WebRTC）。
- 离线 & 存储：能够让网页在客户端本地存储数据以及更高效地离线运行。
- 多媒体：使 video 和 audio 成为了在所有 Web 中的一等公民。
- 2D / 3D 绘图 & 效果：提供了一个更加分化范围的呈现选择（Canvas、WebGL、SVG）。

- 性能 & 集成：提供了非常显著的性能优化和更有效的计算机硬件使用（Web Workers）。
- 设备访问 Device Access：能够处理各种输入和输出设备（Camera API、地理位置、）。
- 样式设计：CSS3。

### 为什么最好把 CSS 的`<link>`标签放在`<head></head>`之间？为什么最好把 JS 的`<script>`标签恰好放在`</body>`之前，有例外情况吗？

- 把`<link>`标签放在`<head></head>`之间是规范要求的内容。这种做法可以让页面逐步呈现，提高了用户体验。将样式表放在文档底部附近，会使许多浏览器，重新绘制页面中的元素。
- 脚本在下载和执行期间会阻止 HTML 解析。把`<script>`标签放在底部，保证 HTML 首先完成解析，将页面尽早呈现给用户。

### 什么是渐进式渲染（progressive rendering）？

- 渐进式渲染是用于提高网页性能（尤其是提高用户感知的加载速度），以尽快呈现页面的技术。
- 一些举例：图片懒加载、确定显示内容的优先级（分层次渲染）。

