# var、let、const之间的区别

## var

ES5中`var`声明的变量即是全局变量，也是顶层变量

注意：顶层对象，在浏览器环境指的是`window`对象，在 `Node` 指的是`global`对象

1、使用`var`声明的变量存在变量提升的情况

```js
console.log(a);  //undefined
var a = 20;
//编译阶段，编译器会将其变成以下执行
var a;
console.log(a);  //undefined
a = 20;
```

2、使用`var`对一个变量进行多次声明，后面声明的变量会覆盖前面的变量声明

```js
var a = 20;
var a = 30;
console.log(a);  //20
```

3、在函数使用`var`声明变量时，该变量时局部的；在函数内不使用`var`，该变量是全局的

```js
var a = 20
function change() {
    var a = 30
}
change()
console.log(a) // 20 
```

```js
var a = 20
function change() {
    a = 30
}
change()
console.log(a) // 30 
```

## let

ES6 新增了`let`命令，用来声明变量

1、声明的变量，只在`let`命令所在的代码块内有效

```js
{
  let a = 10;
  var b = 1;
}
console.log(a); // ReferenceError: a is not defined.
console.log(b); // 1
```

2、不存在变量提升

```js
console.log(bar); // 报错ReferenceError
let bar = 2;
//这表示在声明它之前，变量a是不存在的，这时如果用到它，就会抛出一个错误
```

3、`let`不允许在相同作用域内，重复声明同一个变量

```js
 // 报错
function func() {
    let a = 10;
    var a = 1;
}
// 报错
function func() {
    let a = 10;
    let a = 1;
}
```

不允许在函数内部重新声明变量

```js
function func(arg) {
  let arg;  //同一作用域再次声明arg
}
func() // 报错

function func(arg) {
  {
    let arg;
  }
}
func() // 不报错
```

4、暂时性死区，只要块级作用域内存在`let`命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响

```js
var tmp = 123;
if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
```

上面代码中，存在全局变量`tmp`，但是块级作用域内`let`又声明了一个局部变量`tmp`，导致后者绑定这个块级作用域，所以在`let`声明变量前，对`tmp`赋值会报错

## const

1、`const`声明一个只读的常量。一旦声明，常量的值就不能改变。

```js
const a = 1
a = 3
// TypeError: Assignment to constant variable.
```

这意味着，`const`一旦声明变量，就必须立即初始化，不能留到以后赋值。

```js
const foo;
// SyntaxError: Missing initializer in const declaration
```

`const`实际上保证的并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动

对于简单类型的数据，值就保存在变量指向的那个内存地址，因此等同于常量

对于复杂类型的数据，变量指向的内存地址，保存的只是一个指向实际数据的指针，`const`只能保证这个指针是固定的，并不能确保改变量的结构不变

```js
const foo = {};
// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123
// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only
```

2、`const`的作用域与`let`命令相同：只在声明所在的块级作用域内有效。

```js
if (true) {
  const MAX = 5;
}
MAX // Uncaught ReferenceError: MAX is not defined
```

3、`const`命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用。

```js
if (true) {
  console.log(MAX); // ReferenceError
  const MAX = 5;
}
```

4、`const`声明的常量，也与`let`一样不可重复声明。

```js
var message = "Hello!";
let age = 25;
// 以下两行都会报错
const message = "Goodbye!";
const age = 30;
```

## 区别

`var`、`let`、`const`三者区别可以围绕下面五点展开：

- 变量提升：`var`存在变量提升，`let`和`const`不存在变量提升。
- 暂时性死区：`var`不存在暂时性死区，`let`和`const`存在暂时性死区。
- 块级作用域：`var`不存在块级作用域，`let`和`const`存在块级作用域。
- 重复声明：`var`允许重复声明变量，`let`和`const`在同一作用域不允许重复声明变量。
- 修改声明的变量：`var`和`let`可以，`const`声明一个只读的常量。一旦声明，常量的值就不能改变。

## 使用

能用`const`的情况尽量使用`const`，其他情况下大多数使用`let`，避免使用`var`

## 参考

>https://es6.ruanyifeng.com/#docs/let
