# 搜狐媒体平台前端组代码规范

*说明：本规范遵循[aribnb style guide](https://github.com/airbnb/javascript),组内同学务必遵守*

## 目录

1. [命名规范](#nameming-conventions)

## 命名
 - 不要使用单个字母、拼音或者无意义的单词作为变量名。
 ```javascript
    // bad
    let a = {};
    // too bad
    let duixiang = {};
    // good
    let map = {};

 ```
 - 使用驼峰命名法来命名对象、函数以及变量.
```javascript
    // bad
    let OBject = {};
    // not bad ,but not suit our style
    let this_is_my_object = {};
```
 - 使用大写来命名常量，单词之间以'_'隔开.
```javascript
    const PI=3.1415;
    const DELETE_ITEM_ACTION='delete_item';
```
 - 构造函数、类名首字母大写





