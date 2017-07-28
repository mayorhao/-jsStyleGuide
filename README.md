# 搜狐媒体平台前端组代码规范

*说明：本规范遵循[aribnb style guide](https://github.com/airbnb/javascript),组内同学务必遵守*

## 目录

1. [命名规范](#nameming-conventions)
1. [函数](#functions)<a href="#functions">sss</a>

## 命名


## [函数](id:functions)

1. 使用箭头函数自动继承外层作用域

	```
	
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

1. 当箭头函数内存在`>`或`<`时，需要注意可读性：

	```

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

1. 总是命名函数

	```
		
		// bad
		var named = function() {
    		console.log('named');
  		};

		// good
		var named = function named() {
    		console.log('named');
  		};
	```

## [类和构造函数](id:class)

1. 优先使用`class`，避免使用原型`prototype`


	```
		
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

1. 继承优先使用`extends`

	```

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


## [变量与属性](id:variable-property)

1. 优先使用`.`号访问对象属性

	```

		// bad
		const isJedi = luke['jedi'];

		// good
		const isJedi = luke.jedi;
	```
1. 使用`const`和`let`定义常量和变量

	```

		// bad
		superPower = new SuperPower();
		// bad
		var superPower = new SuperPower();

		// good
		const superPower = new SuperPower();
	```
1. 一个语句定义一个变量或常量

	```

		// bad
		const items = getItems(),
    		goSportsTeam = true,
    		dragonball = 'z';

		// good
		const items = getItems();
		const goSportsTeam = true;
		const dragonball = 'z';
	```

1. 常量总是定义在变量前面

	```

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

## [比较操作](id:comparison)

1. 优先使用`===`和`!==`，而不是`==`和`!=`
1. 简写判断`boolean`类型值，而明确比较字符串和数字值

	```

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
1. 避免使用三元运算符

	```

		// bad
		const foo = a ? a : b;
		const bar = c ? true : false;
		const baz = c ? false : true;

		// good
		const foo = a || b;
		const bar = !!c;
		const baz = !c;
	```

## [空格](id:space)

1. 使用4个空格缩进

	```

		// bad
		function bar() {
		∙let name;
		}

		// good
		function baz() {
		∙∙∙∙let name;
		}
	```
1. 在花括号前添加一个空格

	```

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
1. 在控制语句括号前后各添加一个空格

	```

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
1. 在操作符前后各添加一个空格

	```
		
		// bad
		const x=y+5;

		// good
		const x = y + 5;
	```
