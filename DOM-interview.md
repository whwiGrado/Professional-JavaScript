#DOM-interview

* 1.如何获取页面 DOM 树中的所有节点 ？

```
  document.getElementsByTagName('*');
```

* 2.如何用 JS 遍历 DOM 树 ？

```

```

* 3.querySelector 与 querySelectorAll 是什么 ？

```

```

* 4.列举几个常用的 nodeType 的值 ？

```

```

* 5.document.getElementsByTagName() 得到的是什么 ？querySelectorAll() 得到的是什么 ？ 有何异同 ？

```

```

* 6.如何交换两个 DOM 对象 ？

```
  <div id="container">
    <div>1</div>
    <div>2</div>
    <div>3</div>
  </div>
```

```
  var div = firstElementChild(document.getElementById('container'));
  while (div!==null) {
    div.onclick= swapWithPreviousElement;
    div= nextElementSibling(div);
  }

  function swapWithPreviousElement() {
    var previous= previousElementSibling(this);
    if (previous!==null) {
      this.parentNode.insertBefore(this, previous);
    }
  }

  function previousElementSibling(element) {
    if ('previousElementSibling' in element) {
      return element.previousElementSibling;
    }
    do {
      element = element.previousSibling;
    }
    while (element !== null && element.nodeType !== 1)

    return element;
  }

  function nextElementSibling(element) {
    if ('nextElementSibling' in element) {
      return element.nextElementSibling;
    }
    do {
      element = element.nextElementSibling;
    }
    while (element !== null && element.nodeType !== 1)

    return element;
  }
  function firstElementChild(element) {
     if ('firstElementChild' in element)
         return element.firstElementChild;
     var child= element.firstChild;
     while (child!==null && child.nodeType!==1)
         child= child.nextSibling;
     return child;
  }
```
