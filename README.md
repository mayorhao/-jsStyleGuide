# 搜狐媒体平台前端组代码规范（ES6）

*说明：本规范遵循[aribnb style guide](https://github.com/airbnb/javascript),ES6语法可参考[ECMAscript6入门](http://es6.ruanyifeng.com/)*
<!-- TOC -->

- [命名](#命名)
- [变量声明](#变量声明)
- [对象](#对象)
- [数组](#数组)
- [字符串](#字符串)
- [函数](#函数****)
- [类和构造函数](#类和构造函数)
- [变量与属性](#变量与属性)
- [比较操作](#比较操作)
- [空格](#空格)
- [块状作用域](#块状作用域)
- [条件语句](#条件语句)
- [分号与逗号](#分号与逗号)
- [写在最后](#写在最后)
- [Todolist](#Todolist)

<!-- /TOC -->
## 命名
- 不要使用单个字母、拼音或者无意义的单词作为变量名。
  1. 函数命名
    - 普通函数采用动词+名词的方式，如`getList，getVersion`等
    - 返回值为bool类型，采用`is,has,can`开头，如：`isAdmin，hasChild`；
  2. 变量命名，应采用类型前缀+有意义的单词(具体规则待定)，比如：
    - 字符串：sXXX，如：`sName，sHtml`；
    - 数字：nXXX，如：`nPage，nTotal`；
    - 逻辑：bXXX，如：`bChecked，·bHasLogin`；
    - 数组：aXXX，如：`aList，aGroup`；
    - 正则：rXXX，如：`rDomain，rEmail`
    - JQurey实例：$XXX,如`$panel,$navBar`
- 使用驼峰命名法来命名对象、函数以及变量.

```javascript
// bad
let OBject = {};
// not bad ,but not suit our style
let this_is_my_object = {};
```
- 使用大写来命名常量，单词之间以'_'隔开.
```javascript
//good
const PI=3.1415;
//good
const DELETE_ITEM_ACTION='delete_item';
```
- 构造函数、类名首字母大写
```javascript
// bad
function user(options) {
  this.name = options.name;
}

const bad = new user({
  name: 'nope',
});

// good
class User {
  constructor(options) {
    this.name = options.name;
  }
}

const good = new User({
  name: 'yup',
});
```
- 私有变量,私有方法前加'_'作为前缀标明
```javascript
class Lady(){
    constructor(name,age){
        this.name = name;
        // 不对外部暴露的属性
        this._trueAge = age;
    }
    getName(){
        return this.name;
    }
    // 不对外暴露该方法，只限内部调用
    _getTrueAge(){
        return this._trueAge;
    }
    getAge(){
        return this._getTrueAge()-20;
    }   
}
```
- 有了箭头函数，不要再用变量保存对`this`的引用
```javascript
// 应尽量避免
let that = this;
let self = this
```

**[⬆ back to top](#目录)**

## 变量声明
- 变量声明采用`const`和`let`代替`var`,用`const`声明常量，用`let`声明变量

>why？因为const和let可以保证变量不被重复声明。`let`和`const`提供了块状作用域。这是以前版本JS所缺失的功能。
```javascript
// const and let only exist in the blocks they are defined in.
{
  let a = 1;
  const b = 1;
}
console.log(a); // ReferenceError
console.log(b); // ReferenceError
```
- 声明变量一定要使用`const`或者`let`关键字。否则会声明一个全局变量,污染全局命名空间。
```javascript
// bad
superPower = new SuperPower();

// good
const superPower = new SuperPower();
```
- 多`let`或者`const`来声明多条变量,并将`let`和`const`声明的变量分好组。
>why?想象你以后想再加一个变量，或者给其中一个变量赋上初始值。
```javascript
// bad
const items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';

// good
const items = getItems();
const goSportsTeam = true;
const dragonball = 'z';

// bad 
let i;
const items = getItems();
let dragonball;
const goSportsTeam = true;
let len;
// good
const goSportsTeam = true;
const items = getItems();
let dragonball;
let i;
let length;
```
- 尽量避免变量的连等
>why?这样会隐式的声明全局变量
```javascript
// bad
(function example() {
  let a = b = c = 1;
}());

console.log(a); // throws ReferenceError
console.log(b); // 1
console.log(c); // 1

// good
(function example() {
  let a = 1;
  let b = a;
  let c = a;
}());

console.log(a); // throws ReferenceError
console.log(b); // throws ReferenceError
console.log(c); // throws ReferenceError  
```
- 注意`const`与`let`没有变量提升，在代码块内，注意暂时性锁区
>使用`let`命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）
```javascript
if (true) {
// TDZ开始
tmp = 'abc'; // ReferenceError
console.log(tmp); // ReferenceError

let tmp; // TDZ结束
console.log(tmp); // undefined

tmp = 123;
console.log(tmp); // 123
}
```
**[⬆ back to top](#目录)**

## 对象
- 使用字面量来声明对象
```javascript
// bad
const item = new Object();

// good
const item = {};
```
- 对象方法定义，统一使用method缩写
```javascript
// bad
const atom = {
  value: 1,

  addValue: function (value) {
    return atom.value + value;
  },
};

// good
const atom = {
  value: 1,

  addValue(value) {
    return atom.value + value;
  },
};
```
- 使用属性缩写，并且把属性缩写放在对象声明最开头
```javascript
const anakinSkywalker = 'Anakin Skywalker';
const lukeSkywalker = 'Luke Skywalker';

// bad
const obj = {
  episodeOne: 1,
  twoJediWalkIntoACantina: 2,
  lukeSkywalker,
  episodeThree: 3,
  mayTheFourth: 4,
  anakinSkywalker,
};

// good
const obj = {
  lukeSkywalker,
  anakinSkywalker,
  episodeOne: 1,
  twoJediWalkIntoACantina: 2,
  episodeThree: 3,
  mayTheFourth: 4,
};
```

- 对象的key不要用引号，key中含有非法字符的情况除外
>why 这样提高了代码的可能性，js引擎也更容易优化该段代码

```javascript
// bad
const bad = {
  'foo': 3,
  'bar': 4,
  'data-blah': 5,
};

// good
const good = {
  foo: 3,
  bar: 4,
  'data-blah': 5,
};
```
- 多使用ES6的对象展开操作符`...`,来实现对象的浅复制，解构赋值等功能。
```javascript
// very bad
const original = { a: 1, b: 2 };
const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
delete copy.a; // so does this

// bad
const original = { a: 1, b: 2 };
const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

// good
const original = { a: 1, b: 2 };
const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
```
**[⬆ back to top](#目录)**

## 数组
- 在数组末尾插入新元素时，用push方法。
```javascript
const someStack = [];

// bad
someStack[someStack.length] = 'abracadabra';

// good
someStack.push('abracadabra');
```
- 和对象一样，使用数组的展开操作符"..."来拷贝数组
```javascript
// bad
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i += 1) {
  itemsCopy[i] = items[i];
}

// good
const itemsCopy = [...items];
```
- 将类数组对象，例如nodeList HTMLollection等转化为数组时，使用[Array.from](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from)

>是时候该丢弃繁琐的`Array.prototype.slice.call`了！

```javascript
const foo = document.querySelectorAll('.foo');
const nodes = Array.from(foo);
```
- 使用数组方法时，传入的函数callback一定要有返回值。例如Array的reduce，map，filter方法
```javascript
  // bad
const flat = {};
[[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
  const flatten = memo.concat(item);
  flat[index] = flatten;
});

// good
const flat = {};
[[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
  const flatten = memo.concat(item);
  flat[index] = flatten;
  return flatten;
});

// bad
inbox.filter((msg) => {
  const { subject, author } = msg;
  if (subject === 'Mockingbird') {
    return author === 'Harper Lee';
  } else {
    return false;
  }
});

// good
inbox.filter((msg) => {
  const { subject, author } = msg;
  if (subject === 'Mockingbird') {
    return author === 'Harper Lee';
  }

  return false;
});
```
**[⬆ back to top](#目录)**

## 字符串
- 用单引号`''`来括起字符串，而不是双引号`""`,如果字符串中包含变量或者折行，则应使用模板字符串

```javascript
// bad
const name = "Capt. Janeway";

// bad - template literals should contain interpolation or newlines
const name = `Capt. Janeway`;

// good
const name = 'Capt. Janeway';
```
- 当字符串是动态构建时，使用模板字符串来代替`+`号连接
```javascript
// bad
function sayHi(name) {
  return 'How are you, ' + name + '?';
}

// bad es5推荐此方法，但是兄弟，现在是es6
function sayHi(name) {
  return ['How are you, ', name, '?'].join();
}

// bad 空格问题
function sayHi(name) {
  return `How are you, ${ name }?`;
}

// good
function sayHi(name) {
  return `How are you, ${name}?`;
}
```
**[⬆ back to top](#目录)**

## 函数
- 使用命名的函数表达式来定义函数。不要使用函数声明，因为变量提升会影响可读性。记得给函数命名，方便在报错的时候定位错误

```javascript
// bad
function foo() {
  // ...
}

// bad
const foo = function () {
  // ...
};

// good
const foo = function bar() {
  // ...
};
```
- 立即执行函数表达式(IIFE)，统一采用括号包起来的格式
>虽然IIFE有多种写法，但是括号包起来更能够体现它的模块性质。但是，现在module这么方便，IIFE基本用不着了。

```javascript
// immediately-invoked function expression (IIFE)
(function () {
  console.log('Welcome to the Internet. Please follow me.');
}());
```
- 不要在non-function的块级作用域（`if`,`while`,etc)使用函数声明。请使用变量来定义一个函数。
>一方面是变量提升，一方面是因为各浏览器对该写法的解析有差异。
```javascript
// bad stupid
if (currentUser) {
  function test() {
    console.log('Nope.');
  }
}

// good
let test;
if (currentUser) {
  test = () => {
    console.log('Yup.');
  };
}
```
- 不要使用`arguments`来定义一个形参，这会覆盖函数内部的`arguments`。当然，ES6以后，`arguments`也不推荐使用了，请用`...`剩余参数来替换。
```javascript
// bad 
function foo(name, options, arguments) {
  // ...
}

// good
function foo(name, options, args) {
  // ...
}
// bad
function concatenateAll() {
  const args = Array.prototype.slice.call(arguments);
  return args.join('');
}

// good
function concatenateAll(...args) {
  return args.join('');
}
```
- 使用默认参数，并且总是把默认参数放在最后面
```javascript
// really bad 没有必要这样了
function handleThings(opts) {
  opts = opts || {};
  // ...
}

// good
function handleThings(opts = {}) {
  // ...
}
// bad
function handleThings(opts = {}, name) {
  // ...
}
// good
function handleThings(name, opts = {}) {
  // ...
}
```
- 不要使用`Function`构造函数来创建一个新的`function`
>类似于eval(),这是特别脆弱的语法。
```javascript
// bad
var add = new Function('a', 'b', 'return a + b');

// still bad
var subtract = Function('a', 'b', 'return a - b');
```
- 不要重新给函数参数赋值。
> 重新赋值可能会产生意料之外的错误，尤其是当你访问`arguments`对象时，并且这样容易产生性能问题。
```javascript
// bad
function f1(a) {
  a = 1;
  // ...
}
// good 
function f3(a) {
  const b = a || 1;
  // ...
}
```
- 使用箭头函数自动继承外层作用域

```javascript	
function test() {
// this.name is jh

// bad
setTimeout(function() {
  console.log(this.name); // is window.name
}, 100);

// good
setTimeout(() => {
  console.log(this.name); // jh
}, 100);
}
```
- 当箭头函数内存在`>`或`<`时，需要注意可读性：

```javascript	

// bad
const itemHeight = item => item.height > 256 ? item.largeSize : item.smallSize;

// good
const itemHeight = item => (item.height > 256 ? item.largeSize : item.smallSize);
// good
const itemHeight = (item) => {
  const { height, largeSize, smallSize } = item;
  return height > 256 ? largeSize : smallSize;
};
```

- 总是命名函数

```javascript		
// bad
var named = function() {
  console.log('named');
};

// good
var named = function named() {
  console.log('named');
};
```
**[⬆ back to top](#目录)**

## 类和构造函数

- 优先使用`class`，避免使用原型`prototype`


```javascript		
// bad
function Queue(contents = []) {
  this.queue = [...contents];
}
Queue.prototype.pop = function () {
  const value = this.queue[0];
  this.queue.splice(0, 1);
  return value;
};

// good
class Queue {
  constructor(contents = []) {
    this.queue = [...contents];
  }
  pop() {
    const value = this.queue[0];
    this.queue.splice(0, 1);
    return value;
  }
}
```

- 继承优先使用`extends`

```javascript	

// bad
const inherits = require('inherits');
function PeekableQueue(contents) {
  Queue.apply(this, contents);
}
inherits(PeekableQueue, Queue);
PeekableQueue.prototype.peek = function () {
  return this.queue[0];
};

// good
class PeekableQueue extends Queue {
    peek() {
      return this.queue[0];
    }
}
```
**[⬆ back to top](#目录)**
## 变量与属性

- 优先使用`.`号访问对象属性

```javascript	

// bad
const isJedi = luke['jedi'];

// good
const isJedi = luke.jedi;
```
- 使用`const`和`let`定义常量和变量

```javascript	

// bad
superPower = new SuperPower();
// bad
var superPower = new SuperPower();

// good
const superPower = new SuperPower();
```
- 一个语句定义一个变量或常量

```javascript	
// bad
const items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';

// good
const items = getItems();
const goSportsTeam = true;
const dragonball = 'z';
```

- 常量总是定义在变量前面

```javascript	

// bad
let i;
const items = getItems();
let dragonball;
const goSportsTeam = true;
let len;

// good
const goSportsTeam = true;
const items = getItems();
let dragonball;
let i;
let length;
```

## 比较操作

- 优先使用`===`和`!==`，而不是`==`和`!=`
- 简写判断`boolean`类型值，而明确比较字符串和数字值

```javascript	

// bad
if (isValid === true) {}

// good
if (isValid) {}

// bad
if (name) {
    // ...
}

// good
if (name !== '') {
    // ...
}

// bad
if (collection.length) {
    // ...
}

// good
if (collection.length > 0) {
    // ...
}
```
- 避免使用三元运算符
```javascript	

// bad
const foo = a ? a : b;
const bar = c ? true : false;
const baz = c ? false : true;

// good
const foo = a || b;
const bar = !!c;
const baz = !c;
```
**[⬆ back to top](#目录)**
## 空格

- 使用4个空格缩进
```javascript	

// bad
function bar() {
∙let name;
}

// good
function baz() {
∙∙∙∙let name;
}
```
- 在花括号前添加一个空格

```javascript	

// bad
function test(){
    console.log('test');
}

// good
function test() {
    console.log('test');
}

// bad
dog.set('attr',{
    age: '1 year',
    breed: 'Bernese Mountain Dog',
});

// good
dog.set('attr', {
    age: '1 year',
    breed: 'Bernese Mountain Dog',
});
```
- 在控制语句括号前后各添加一个空格

```javascript	
// bad
if(isJedi) {
    fight ();
}

// good
if (isJedi) {
    fight();
}

// bad
function fight () {
    console.log ('Swooosh!');
}

// good
function fight() {
    console.log('Swooosh!');
}
```
- 在操作符前后各添加一个空格

```javascript	****

// bad
const x=y+5;

// good
const x = y + 5;
```
- 链式调用,在行首使用`.`符号(leading dot)，来明显的强调这一行是一个方法调用。
```javascript
// bad
$('#items').find('.selected').highlight().end().find('.open').updateCount();

// bad
$('#items').
  find('.selected').
    highlight().
    end().
  find('.open').
    updateCount();

// good
$('#items')
  .find('.selected')
    .highlight()
    .end()
  .find('.open')
    .updateCount();

// bad
const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
    .attr('width', (radius + margin) * 2).append('svg:g')
    .attr('transform', `translate(${radius + margin},${radius + margin})`)
    .call(tron.led);

// good
const leds = stage.selectAll('.led')
    .data(data)
  .enter().append('svg:svg')
    .classed('led', true)
    .attr('width', (radius + margin) * 2)
  .append('svg:g')
    .attr('transform', `translate(${radius + margin},${radius + margin})`)
    .call(tron.led);

// good
const leds = stage.selectAll('.led').data(data);
```
- 在块状语句后,加一个空行
```javascript
// bad
if (foo) {
  return bar;
}
return baz;

// good
if (foo) {
  return bar;
}

return baz;

// bad
const obj = {
  foo() {
  },
  bar() {
  },
};
return obj;

// good
const obj = {
  foo() {
  },

  bar() {
  },
};

return obj;

// bad
const arr = [
  function foo() {
  },
  function bar() {
  },
];
return arr;

// good
const arr = [
  function foo() {
  },

  function bar() {
  },
];

return arr;
```
- 在*括号*以及*方括号*内都不要加空格，在花括号内加入空格。
```javascript
// bad
if ( foo ) {
  console.log(foo);
}

// good
if (foo) {
  console.log(foo);
}
  // good
const foo = [1, 2, 3];
console.log(foo[0]);
// bad
const foo = {clark: 'kent'};
// good
const foo = { clark: 'kent' };
```
**[⬆ back to top](#目录)**


## 块状作用域
- 多行的块代码，用括号包起来

```javascript
// bad
if (test)
  return false;

// good
if (test) return false;

// good
if (test) {
  return false;
}

// bad
function foo() { return false; }

// good
function bar() {
  return false;
}
```
- 如果是`if-else`语句。则将`else`和`if`放在同一行
```javascript
// bad
if (test) {
  thing1();
  thing2();
}
else {
  thing3();
}

// good
if (test) {
  thing1();
  thing2();
} else {
  thing3();
}
```
**[⬆ back to top](#目录)**

## 条件语句
- 如果`if`或者`while`中条件语句太长的话，那么你应该新起一行，分多行来写，逻辑操作符`&&`统一写在行首或者行尾都可以。

```javascript
// bad
if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
  thing1();
}

// bad
if (foo === 123 &&
  bar === 'abc') {
  thing1();
}

// bad
if (foo === 123
  && bar === 'abc') {
  thing1();
}

// good
if (
  (foo === 123 || bar === "abc") &&
  doesItLookGoodWhenItBecomesThatLong() &&
  isThisReallyHappening()
) {
  thing1();
}

// good
if (foo === 123 && bar === 'abc') {
  thing1();
}

// good
if (
  foo === 123 &&
  bar === 'abc'
) {
  thing1();
}

// good
if (
  foo === 123
  && bar === 'abc'
) {
  thing1();
}
```
## 分号与逗号
- 在尾部增加额外的逗号。但是如果最后是带`...`展开操作符的元素，就不要加尾部的逗号了。
> why?这样会在git diff时显示的更为清楚。babel会在编译时移除这个额外的逗号，所以，不用担心这个有关[尾部逗号bug](https://github.com/airbnb/javascript/blob/es5-deprecated/es5/README.md#commas)的问题

```javascript
// bad
const hero = {
  firstName: 'Dana',
  lastName: 'Scully'
};

const heroes = [
  'Batman',
  'Superman'
];
// good
const hero = {
  firstName: 'Dana',
  lastName: 'Scully',
};

const heroes = [
  'Batman',
  'Superman',
];
//bad 
createHero(
  firstName,
  lastName,
  inventorOf,
  ...heroArgs,
);
//good
createHero(
  firstName,
  lastName,
  inventorOf,
  ...heroArgs
);
```
- 我们这里分号是要写的。
```javascript
// bad
(function () {
  const name = 'Skywalker'
  return name
})()

// good
(function () {
  const name = 'Skywalker';
  return name;
}());
``` 
**[⬆ back to top](#table-of-contents)**

## 写在最后
本规范大部分基于[aribnb style guide](https://github.com/airbnb/javascript)。有些地方加入了自己的想法。此为初稿，欢迎大家积极提出意见，也请在搬砖时遵循此代码的风格约定。
## Todolist
1. 代码注释规范。涉及注释的时机，注释的规范化等
1. 高性能代码tips，涉及dom操作、浏览器重排等知识点。可参考《高性能javascript》