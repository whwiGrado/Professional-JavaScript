# Professional-JavaScript

## 第八章 BOM
* BOM 的核心对象是 window，是 ECMAScript 规定的 Global 对象
* 全局作用域：
  - 所有在全局作用域中声明的变量、函数都会变成 window 对象的属性和方法
  - 但是在全局作用域中定义变量和直接在 window 对象上定义属性是有区别的：
    e.g.
    ```
    var age = 25;
    window.color = red;
    
    delete window.age; // IE<9 下抛出错误，其他浏览器返回 false
    delete window.color; // IE<9 下抛出错误，其他浏览器返回 true
    
    alert(window.age); // 25 ,即不可以通过 delete 操作符删除
    alert(window.color); // undefined ,即可以通过 delete 操作符删除
    ```
  - 尝试访问未声明的变量会抛出错误，但是通过查询 window 对象的属性的方法访问，则不会抛出错误
    e.g.
    ```
    var newValue = oldValue; // 抛出错误，因为 oldValue 未定义
    var newValue = window.oldValue; // 如果 oldValue 未定义，则 newValue 的值为 undefined，不会抛出错误
    ```
* 窗口关系及 frames
