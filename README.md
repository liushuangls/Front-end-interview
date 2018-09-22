# Front-end-interview

## 目录

- [JavaScript](#javascript)
- [CSS](#css)

## JavaScript

setTimeout中的this指向？

```js
let obj = {
	a: 10,
	fn: function () {
		console.log(this.a)
	}
}
setTimeout(obj.fn, 10) // undefined
// 上面的调用this指向全局，相当于把fn隐式赋值给了setTimeout的第一个参数，相当于下面
let fn = obj.fn
fn() // undefined，虽然引用的是obj的属性，但实际上它引用的是fn函数本身。
// 解决方案
setTimeout(obj.fn.bind(obj), 10)
```

undefined + 1?

- 隐式转换后两边不能使用 `+` 操作符进行运算的表达式结果为 `NaN`。



箭头函数与普通函数的区别？

- 箭头函数有作用域。

- 箭头函数中的this继承自它的外层作用域，并且不能被改变。

- 箭头函数会把 arguments 当成一个普通的变量，但是arguments可以用...rest取代。

  function foo() { setTimeout(() => { console.log(this.a) }, 10) } let obj = { a: 1 } foo.call(obj) // 1 // 类似于下面 function foo() { const self = this setTimeout(function () { console.log(self.a) }, 10) } foo.call(obj) // 1

什么是作用域？

- 用来存储、访问变量的一套规则，这套规则用来管理引擎如何在当前作用域及子作用域中根据标识符名进行变量查找。
- JavaScript 采用词法作用域(lexical scoping)，也就是静态作用域。



## CSS

使用css如何选择被选中的复选框/单选框？

- `:checked` CSS 伪类选择器表示任何处于选中状态的radio(``<input type="radio">``), checkbox (``<input type="checkbox">``) 或("select") 元素中的option HTML元素("option")。

