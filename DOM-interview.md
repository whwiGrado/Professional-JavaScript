#DOM-interview

* 1.如何获取页面 DOM 树中的所有节点 ？

```
  document.getElementsByTagName('*');
  IE 浏览器在使用通配符的时候，会把文档最开始的 <!DOCTYPE html> 当作第一个元素节点
  其他浏览器则会从 <html> 元素开始算起
```

* 2.如何用 JS 遍历 DOM 树 ？

```

```

* 3.querySelector 与 querySelectorAll 是什么 ？

               | querySelector | querySelectorAll
  ------------ | :-----------: | :-----------: |
  接收的参数| css选择符 | css选择符 |
  如果参数不合法 | 抛出异常 | 抛出异常 |
  返回值| 匹配的第一个元素 | 匹配的所有元素 |
  没有匹配时返回| null | null |
  可以调用的对象| Document、Element | Document、Element |
  Document 调用时| 文档范围内检索 | 文档范围内检索 |
  Element 调用时 | 该元素后代里检索 | 该元素后代里检索 |

```
  querySelector
  如果要匹配的ID或选择器不符合 CSS语法（比如不恰当地使用了冒号或者空格）
  你必须用反斜杠将这些字符转义
  而且你必须将它转义两次（一次是为 JavaScript 字符串转义，另一次是为 querySelector 转义）

    例如：
      HTML:
        <div id="foo\bar"></div>
        <div id="foo:bar"></div>
      JS:
        console.log('#foo\bar')               // "#fooar"
        document.querySelector('#foo\bar')    // 不匹配任何元素

        console.log('#foo\\bar')              // "#foo\bar"
        console.log('#foo\\\\bar')            // "#foo\\bar"
        document.querySelector('#foo\\\\bar') // 匹配第一个div

        document.querySelector('#foo:bar')    // 不匹配任何元素
        document.querySelector('#foo\\:bar')  // 匹配第二个div

  querySelectorAll
  返回的是一个带有所有属性和方法的 NodeList 实例
  相当于 NodeList 的快照，而非不断对文档进行搜索的动态查询
  这样可以避免使用 NodeList 对象引起的大多数性能问题
```

* 4.列举几个常用的 nodeType 的值 ？

```
  常用的 nodeType（继承自 Node 类型）
    Node.ELEMENT_NODE (1)
    Node.TEXT_NODE (3)
    Node.COMMENT_NODE (8)
    Node.DOCUMENT_NODE (9)
    Node.DOCUMENT_TYPE_NODE (10)
    Node.DOCUMENT_FRAGMENT_NODE (11)
```

* 5.document.getElementsByTagName() 得到的是什么 ？querySelectorAll() 得到的是什么 ？ 有何异同 ？

```
  document.getElementsByTagName('tagName')，参数 tagName 不区分大小写
  返回一个 [object HTMLCollection] 对象，也是动态集合，与 NodeList 很像

  querySelectorAll('css选择符')
  返回的是一个带有所有属性和方法的 NodeList 实例
  相当于 NodeList 的快照，而非不断对文档进行搜索的动态查询
  这样可以避免使用 NodeList 对象引起的大多数性能问题

  区别：
  - HTMLCollection 对象的访问方法有三种：[index] && .item(index) && namedItem('name')
    而 NodeList 对象只有 [index] && .item(index) 两种访问方法
  - getElementsByTagName() 返回的 HTMLCollection 对象是动态集合
    而 querySelectorAll() 返回的 相当于 NodeList 的快照，而非不断对文档进行搜索的动态查询
```

* 6.如何交换两个 DOM 对象 ？

```
  HTML:
    <div id="container">
      <div>1</div>
      <div>2</div>
      <div>3</div>
    </div>
  JS:
    var div = firstElementChild(document.getElementById('container'));
    while (div !== null){
      div.onclick = swapWithPreviousElement;
      div= nextElementSibling(div);
    }

    function swapWithPreviousElement(){
      var previous = previousElementSibling(this);
      if(previous !== null){
        this.parentNode.insertBefore(this, previous);
      }
    }

    function previousElementSibling(element){
      if('previousElementSibling' in element){
        return element.previousElementSibling;
      }
      do {
        element = element.previousSibling;
      }
      while (element !== null && element.nodeType !== 1)

      return element;
    }

    function nextElementSibling(element){
      if('nextElementSibling' in element){
        return element.nextElementSibling;
      }
      do {
        element = element.nextElementSibling;
      }
      while (element !== null && element.nodeType !== 1)

      return element;
    }

    function firstElementChild(element){
      if('firstElementChild' in element){
        return element.firstElementChild;
      }

      var child = element.firstChild;

      while (child !== null && child.nodeType !==1 ){
        child= child.nextSibling;
      }

      return child;
    }
```

* 7.如何用JavaScript判断 DOM 元素节点是否有某个 class 类名 ？

```
  var hasClass = (function(){
    var div = document.createElement("div") ;
    if( "classList" in div && typeof div.classList.contains === "function" ){
      return function(elem, className){
        return elem.classList.contains(className);
      };
    } else {
      return function(elem, className){
        var classes = elem.className.split(/\s+/);
        for(var i= 0 ; i < classes.length ; i ++){
          if( classes[i] === className ){
            return true ;
          }
        }
      return false ;
      };
    }
  })();
```

* 8. offsetLeft 和 style.left 区别

```
  - style.left 返回的是字符串，比如 "10px" 。而 offsetLeft 返回的是数值，比如数值 10

  - style.left 是可读写的，offsetLeft 是只读的

  - style.left 的值需要事先定义(只包含内联式定义的 css 样式)，否则取到的值是空的
```

* 9.获取鼠标点击事件的位置

```
  document.onclick = mouseClick;

  function mouseClick(ev){
      ev = ev || window.event; // window.event用来兼容IE
      var x = 0; var y = 0;

      if(ev.pageX){
          x = ev.pageX;
          y = ev.pageY;
      }else if(ev.clientX){
          var offsetX = 0 , offsetY = 0;
          if(document.documentElement.scrollLeft){
              offsetX = document.documentElement.scrollLeft;
              offsetY = document.documentElement.scrollTop;
          }else if(document.body){
              offsetX = document.body.scrollLeft;
              offsetY = document.body.scrollTop;
          }
          x = ev.clientX + offsetX;
          y = ev.clientY + offsetY;
      }
      alert("你点击的位置是 x="+ x + " y=" + y);
  }
```

* 10. DOM 树的原型链

```
  文档节点的原型链：
    Document --> Node --> EventTarget --> Object
  元素节点的原型链，以 <a> 元素为例：
    HTMLAnchorElement --> HTMLElement --> Element --> Node --> EventTarget --> Object
  注释节点的原型链：
    Comment --> CharacterData --> Node --> EventTarget --> Object
  文本节点的原型链：
    Text --> CharacterData -- Node -- EventTarget --> Object
```

* 11.innerHTML、createElement() 和 createDocumentFragment()

```
  function appendMethod1(){
    var container = document.getElementById("test");
    var staDate = new Date();
    for(var i=0; i<50000; i++){
      var oDiv = document.createElement("div");
      var oText = document.createTextNode("text");
      oDiv.appendChild(oText);
      container.appendChild(oDiv);
    }
    console.log(new Date - staDate);
  }
  function appendMethod2(){
    var container = document.getElementById("test");
    var staDate = new Date();
    var arr = [];
    for(var i=0; i<50000; i++){
      var str="<div>test</div>";
      arr.push(str);
    }
    var str = arr.join("");
    container.innerHTML = str;
    console.log(new Date() - staDate);
  }
  function appendMethod3(){
    var container = document.getElementById("test");
    var staDate = new Date();
    var fragment = document.createDocumentFragment();
    for (i=0; i<50000; i++) {
      var oDiv = document.createElement('div');
      var oText = document.createTextNode("text");
      oDiv.appendChild(oText);
      fragment.appendChild(oDiv);
    }
    container.appendChild(fragment);
    console.log(new Date - staDate);
  }
  appendMethod1();
  appendMethod2();
  appendMethod3();
```
