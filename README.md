# 搜狐媒体平台前端组代码规范（ES6）

*说明：本规范遵循[aribnb style guide](https://github.com/airbnb/javascript),组内同学务必遵守,ES6语法可参考[ECMAscript6入门](http://es6.ruanyifeng.com/)*

## 目录

1. [命名规范](#命名)
1. [变量声明]（#变量声明）
1. [对象](#对象)
1. [数组](#数组)

## 命名
 - 不要使用单个字母、拼音或者无意义的单词作为变量名。
    1. 函数命名
        - 普通函数采用动词+名词的方式，如getList，getVersion等
        - 返回值为bool类型，采用is、has、can开头，如：isAdmin，hasChild；
    1. 变量命名，应采用类型前缀+有意义的单词(具体规则待定)，比如：
        - 字符串：sXXX，如：sName，sHtml；
        - 数字：nXXX，如：nPage，nTotal；
        - 逻辑：bXXX，如：bChecked，·bHasLogin；
        - 数组：aXXX，如：aList，aGroup；
        - 正则：rXXX，如：rDomain，rEmail
        - JQurey实例：$XXX,如$panel,$navBar
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
- 对象方法定义，统一使用mehtod缩写
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



    





