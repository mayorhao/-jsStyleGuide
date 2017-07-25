# 搜狐媒体平台前端组代码规范

*说明：本规范遵循[aribnb style guide](https://github.com/airbnb/javascript),组内同学务必遵守*

## 目录

1. [命名规范](#命名)

·## 命名
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
    





