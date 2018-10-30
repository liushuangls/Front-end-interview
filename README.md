

## 目录

- [JavaScript](#javascript)
  - [setTimeout中的this指向？](#setTimeout中的this指向？)
- [CSS](#css)
- [HTML](#html)
- [前端框架](#前端框架)

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

- 箭头函数不能作为构造函数调用。

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
// ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。
let arr = [1,2,3,3]
[...new Set(arr)] // [1,2,3]
```

### offsetWidth/offsetHeight,clientWidth/clientHeight与scrollWidth/scrollHeight的区别

- offsetWidth/offsetHeight返回值包含**content + padding + border**
- clientWidth/clientHeight返回值只包含**content + padding**
- scrollWidth/scrollHeight返回值包含**content + padding + 溢出内容的尺寸**

### sessionStorage,localStorage,cookie区别

- cookie会在请求时发送到服务器，作为会话标识，服务器可修改cookie；web storage不会发送到服务器
- 有效期：cookie在设置的有效期内有效，默认为浏览器关闭；sessionStorage在窗口关闭前有效，localStorage长期有效，直到用户删除
- 共享：sessionStorage不能共享，localStorage在同源文档之间共享，cookie在同源且符合path规则的文档之间共享
- 浏览器不能保存超过300个cookie，单个服务器不能超过20个，每个cookie不能超过4k。web storage大小支持能达到5M

### 应用程序存储和离线web应用

HTML5新增应用程序缓存，允许web应用将应用程序自身保存到用户浏览器中，用户离线状态也能访问。

- 为html元素设置manifest属性:`<html manifest="myapp.appcache">`，其中后缀名只是一个约定，真正识别方式是通过`text/cache-manifest`作为MIME类型。所以需要配置服务器保证设置正确 
- manifest文件首行为`CACHE MANIFEST`，其余就是要缓存的URL列表，每个一行，相对路径都相对于manifest文件的url。注释以#开头
- url分为三种类型：`CACHE`:为默认类型。`NETWORK`：表示资源从不缓存。 `FALLBACK`:每行包含两个url，第二个URL是指需要加载和存储在缓存中的资源， 第一个URL是一个前缀。任何匹配该前缀的URL都不会缓存，如果从网络中载入这样的URL失败的话，就会用第二个URL指定的缓存资源来替代。
- 以下是一个文件例子：

```js
CACHE MANIFEST

CACHE:
myapp.html
myapp.css
myapp.js

FALLBACK:
videos/ offline_help.html

NETWORK:
cgi/
```

### 字符串比较？

- 按照字母顺序比较unicode值大小。

### 原型继承

```js
function Shape() {}

function Rect() {}

// 方法1
// 正确设置原型链实现继承
// Rect的实例将不能继承Shape的属性，而只能通过原型访问
// 继承应该是继承方法而不是属性，为子类设置父类实例属性应该是通过在子类构造函数中调用父类构造函数进行初始化
Rect.prototype = new Shape();

// 方法2
// 父类构造函数原型与子类相同。修改子类原型添加方法会修改父类
Rect.prototype = Shape.prototype;

// 方法3
// 正确设置原型链且避免方法1.2中的缺点
// 但是没有继承Shape的属性
Rect.prototype = Object.create(Shape.prototype);

// 改进：
// 所有三种方法应该在子类构造函数中调用父类构造函数实现实例属性初始化
// 用新创建的对象替代子类默认原型，设置Rect.prototype.constructor = Rect;保证一致性
function Rect() {
    Shape.call(this);
}
```

### 现有一个Page类,其原型对象上有许多以post开头的方法(如postMsg);另有一拦截函数chekc,只返回ture或false.请设计一个函数,该函数应批量改造原Page的postXXX方法,在保留其原有功能的同时,为每个postXXX方法增加拦截验证功能,当chekc返回true时继续执行原postXXX方法,返回false时不再执行原postXXX方法

```js
function Page() {}

Page.prototype = {
  postA: function (a) {
    console.log('a:' + a);
  },
  postB: function (b) {
    console.log('b:' + b);
  },
  postC: function (c) {
    console.log('c:' + c);
  },
  check: function () {
    return Math.random() > 0.5;
  }
}

function checkfy(obj) {
  for (var key in obj) {
    if (key.indexOf('post') === 0 && typeof obj[key] === 'function') {
      var fn = obj[key];
      obj[key] = function () {
        if (obj.check()) {
          fn.apply(obj, arguments);
        }
      };
    }
  }
} // end checkfy()

checkfy(Page.prototype);

```

### 编写javascript深度克隆函数deepClone

```js
function deepClone(obj) {
    var _toString = Object.prototype.toString;

    // null, undefined, non-object, function
    if (!obj || typeof obj !== 'object') {
        return obj;
    }

    // DOM Node
    if (obj.nodeType && 'cloneNode' in obj) {
        return obj.cloneNode(true);
    }

    // Date
    if (_toString.call(obj) === '[object Date]') {
        return new Date(obj.getTime());
    }

    // RegExp
    if (_toString.call(obj) === '[object RegExp]') {
        var flags = [];
        if (obj.global) { flags.push('g'); }
        if (obj.multiline) { flags.push('m'); }
        if (obj.ignoreCase) { flags.push('i'); }

        return new RegExp(obj.source, flags.join(''));
    }

    var result = Array.isArray(obj) ? [] :
        obj.constructor ? new obj.constructor() : {};

    for (var key in obj ) {
        result[key] = deepClone(obj[key]);
    }

    return result;
}
```

### 完成一个函数,接受数组作为参数,数组元素为整数或者数组,数组元素包含整数或数组,函数返回扁平化后的数组

```js
function flat(arr, newArr = []) {
  for (let v of arr) {
    if (Array.isArray(v)) {
      flat(v, newArr)
    } else {
      newArr.push(v)
    }
  }
  return newArr
}


console.log(flat([1, [2, [[3, 4], 5], 6]]))
```

### 函数参数传递问题

```js
function setName(obj) {
  obj.name = 'ted'
  obj = new Object()
  obj.name = 'marry'
}
let o = new Object()
o.name = 'tom'
setName(o)
console.log(o.name) // ted
// 函数的参数传递原理：在函数内部，隐式声明形参，将传入的 值/地址 复制给形参
// 在obj被重新赋值后，它的地址不再是o的地址，所以修改它不再影响o。因此结果不是marry
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

### `link`与`@import`的区别

- `link`最大限度支持并行下载，`@import`过多嵌套导致串行下载，出现FOUC
- `@import`必须在样式规则之前，可以在css文件中引用其他文件

### 如何水平居中一个元素

- inline：`text-align: center;`、`line-height`。
- block:`margin:auto`、 position 加 translate、绝对定位、flex



## HTML

### `DOCTYPE`有什么用？

- `<!doctype>`声明不是一个HTML标签，是一个用于告诉浏览器当前HTMl版本的指令。
- 现代浏览器的html布局引擎通过检查doctype决定使用兼容模式还是标准模式对文档进行渲染，一些浏览器有一个接近标准模型。
- 在HTML4.01中`<!doctype>`声明指向一个DTD，由于HTML4.01基于SGML，所以DTD指定了标记规则以保证浏览器正确渲染内容。
- HTML5不基于SGML，所以不用指定DTD。

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

### SEO优化？

- 合理的title、description、keywords：搜索对着三项的权重逐个减小，title值强调重点即可，重要关键词出现不要超过2次，而且要靠前，不同页面title要有所不同；
- 语义化的HTML代码，符合W3C规范：语义化代码让搜索引擎容易理解网页。
- 重要内容HTML代码放在最前：搜索引擎抓取HTML顺序是从上到下，有的搜索引擎对抓取长度有限制，保证重要内容一定会被抓取。
- 重要内容不要用js输出：爬虫不会执行js获取内容。
- 少用iframe：搜索引擎不会抓取iframe中的内容。
- 非装饰性图片必须加alt。
- 提高网站速度：网站速度是搜索引擎排序的一个重要指标。

### HTTP request报文结构是怎样的

- 首行是**请求行**包括：**请求方法**，**请求URI**，**协议版本**，**CRLF**
- 首行之后是若干行**请求头**，包括**general-header**，**request-header**或者**entity-header**，每个一行以CRLF结束
- 请求头和消息实体之间有一个**CRLF分隔**
- 根据实际请求需要可能包含一个**消息实体** 一个请求报文例子如下：

```http
GET /Protocols/rfc2616/rfc2616-sec5.html HTTP/1.1
Host: www.w3.org
Connection: keep-alive
Cache-Control: max-age=0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/35.0.1916.153 Safari/537.36
Referer: https://www.google.com.hk/
Accept-Encoding: gzip,deflate,sdch
Accept-Language: zh-CN,zh;q=0.8,en;q=0.6
Cookie: authorstyle=yes
If-None-Match: "2cc8-3e3073913b100"
If-Modified-Since: Wed, 01 Sep 2004 13:24:52 GMT

name=qiu&age=25
```

### HTTP response报文结构是怎样的

- 首行是响应头包括：**HTTP版本，状态码，状态描述**，后面跟一个CRLF
- 首行之后是**若干行响应头**，包括：**通用头部，响应头部，实体头部**
- 响应头部和响应实体之间用**一个CRLF空行**分隔
- 最后是一个可能的**消息实体** 响应报文例子如下：

```http
HTTP/1.1 200 OK
Date: Tue, 08 Jul 2014 05:28:43 GMT
Server: Apache/2
Last-Modified: Wed, 01 Sep 2004 13:24:52 GMT
ETag: "40d7-3e3073913b100"
Accept-Ranges: bytes
Content-Length: 16599
Cache-Control: max-age=21600
Expires: Tue, 08 Jul 2014 11:28:43 GMT
P3P: policyref="http://www.w3.org/2001/05/P3P/p3p.xml"
Content-Type: text/html; charset=iso-8859-1

{"name": "qiu", "age": 25}
```

### 如何进行网站性能优化

- content方面：
  - 减少HTTP请求：合并文件、CSS精灵、inline Image
  - 减少DNS查询：DNS查询完成之前浏览器不能从这个主机下载任何任何文件。方法：DNS缓存、将资源分布到恰当数量的主机名，平衡并行下载和DNS查询
  - 避免重定向：多余的中间访问
  - 使Ajax可缓存
  - 非必须组件延迟加载
  - 未来所需组件预加载
  - 减少DOM元素数量
  - 将资源放到不同的域下：浏览器同时从一个域下载资源的数目有限，增加域可以提高并行下载量
  - 减少iframe数量
- server方面：
  - 使用CDN
  - 添加Expires或者Cache-Control响应头
  - 对组件使用Gzip压缩
  - Ajax使用GET进行请求
  - 避免空src的img标签
- Cookie:
  - 减少cookie大小。
  - 引入资源的域名不要加入cookie。
- css：
  - 将样式表放到页面顶部
  - 不使用CSS表达式
  - 不使用@import
- JavaScript：
  - 将脚本放到页面底部
  - 将javascript和css从外部引入
  - 压缩javascript和css
  - 减少DOM访问
  - 合理设计事件监听器
- 图片方面：
  - 优化图片：根据实际颜色需要选择色深、压缩
  - 不要在HTML中拉伸图片
  - 保证favicon.ico小并且可缓存

### HTTP状态码及其含义

1XX:信息状态码。

- **100 Continue**：客户端应当继续发送请求。
- **101 Switching Protocols**：服务器已经理解了客户端的请求，并将通过Upgrade消息头通知客户端采用不同的协议来完成这个请求。

2XX：成功状态码。

- **200 OK**：请求成功。
- 201：表示请求已经被成功处理，并且创建了新的资源。
- 202：服务器端已经收到请求消息，但是尚未进行处理。
- 204：目前请求成功，但客户端不需要更新其现有页面。
- 205：通知客户端重置文档视图，比如清空表单内容、重置 canvas 状态或者刷新用户界面。
- 206：请求已成功，并且主体包含所请求的数据区间，该数据区间是在请求的 [`Range`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Range) 首部指定的。

3XX：重定向。

- 300：表示重定向的响应状态码，表示该请求拥有多种可能的响应。用户代理或者用户自身应该从中选择一个。
- 301：说明请求的资源已经被移动到了由 [`Location`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Location) 头部指定的url上，是固定的不会再改变。
- 304：可以使用缓存的内容。

4XX：客户端错误。

- 400：由于语法无效，服务器无法理解该请求。 
- 401：代表客户端错误，指的是由于缺乏目标资源要求的身份验证凭证，发送的请求未得到满足。
- 403：客户端没有权利访问所请求内容,服务器拒绝本次请求。
- 404：服务器找不到所请求的资源.

5XX: 服务器错误

- 500: 服务器遇到未知的无法解决的问题.
- 501:服务器不支持该请求中使用的方法.
- 502:服务器作为网关且从上游服务器获取到了一个无效的HTTP响应.
- 504:服务器作为网关且不能从上游服务器及时的得到响应返回给客户端.
- 505:服务器不支持客户端发送的HTTP请求中所使用的HTTP协议版本.



## 前端框架

### react生命周期

```js
Mounting:
componentWillMount => render => componentDidMount

Updation:
props: componentWillReceiveProps => shouldComponentUpdate => componentWillUpdate => render => componentDidUpdate
states: shouldComponentUpdate => componentWillUpdate => render => componentDidUpdate

Unmunting:
componentWillUnmount
```

### vue生命周期

```js
beforeCreate => created => beforeMount => mounted => beforeUpdate => updated => beforeDestroy => destroyed
```

