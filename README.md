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
  - 如果页面中有 frames ，则每个框架都拥有自己的 window 对象，并保存在 frames 集合中


#第十章 Dom
## 10.1 节点层次
### Node 类型
```
  除了IE，其他浏览器都可以访问到这个类型
```
#### Node.Properties
```
  除了 Node.textContent、Node.nodeValue，其他属性均为只读
```
* Node.childNodes

```
  Node.childNodes 返回 NodeList 对象，NodeList 是一个类数组对象，保存一组有序的节点
  当没有子节点时，返回空类数组，此时，someNode.childNodes.length === 0

  NodeList 的访问方法有两种：[index] 和 .item(index)
    var firstChild = someNode.childNodes[0];
    var secondChild = someNode.item(1);

  NodeList 转数组：
    function converToArray(nodes){
        var array = null;
        try {
          array = Array.prototype.slice.call(nodes, 0);
        } catch (ex) { // <=IE8
          array = [];
          for (var i=0, len=nodes.length; i<len; i++){
            array.push(nodes[i]);
          }
        }
    }
    converToArray(someNode.childNodes);

```
* Node.parentNode
* Node.parentElement

```
  当父节点的 nodeType 不是 1，即不是 element 节点的话，它的 parentElement 就会是 null
  一般情况 parentNode 可以取代 parentElement 的所有功能
```
* Node.firstChild
* Node.lastChild
* Node.previousSibling
* Node.nextSibling

```
  当访问 Node.parentNode、Node.firstChild、Node.lastChild、Node.previousSibling、
  Node.nextSibling 属性不存在时，返回 null
```
* Node.nodeName

```
  返回的字符串有时候是大写，有时候是小写，例如
    document.nodeName // "#document"
    document.doctype.nodeName // "html"
    document.documentElement.nodeName // "HTML"
    document.body.nodeName // "BODY"
    document.head.nodeName // "HEAD"

```
* Node.nodeType

```
  常用的 nodeType（继承自 Node 类型）
    Node.ELEMENT_NODE (1)
    Node.TEXT_NODE (3)
    Node.COMMENT_NODE (8)
    Node.DOCUMENT_NODE (9)
    Node.DOCUMENT_TYPE_NODE (10)
    Node.DOCUMENT_FRAGMENT_NODE (11)

```
* Node.nodeValue

  nodeType | value of nodeValue
  ------------ | :-----------: |
  ELEMENT_NODE | null  |
  TEXT_NODE | content of the text node  |
  COMMENT_NODE | content of the comment  |
  DOCUMENT_NODE | null  |
  DOCUMENT_TYPE_NODE | null  |
  DOCUMENT_FRAGMENT_NODE | null  |

* Node.ownerDocument

```
  总是返回根节点对象
  document.body.ownerDocument // document 对象
  document.head.ownerDocument // document 对象
```

* Node.baseURI
* Node.textContent

### Node.Methods
* Node.hasChildNodes()

```
  节点包含一个或多个子节点的时候返回 true
```
* Node.appendChild( newNode )

```
  在 Node 节点的末尾插入 newNode，返回 newNode
  如果 newNode 是已经是文档的一部分，则该节点移动到 Node 节点的末尾
    var returnedNode = someNode.appendChild(someNode.firstChild);
    console.log(returnedNode === someNode.firstChild); // false
    console.log(returnedNode === someNode.lastChild); // true
```
* Node.insertBefore( newNode, referenceNode )

```
  返回 newNode
  如果参照节点 referenceNode === null，则相当于 appendChild 操作
```
* Node.replaceChild( newNode，replacedNode )

```
  返回 replacedNode
```

* Node.removeChild()

```
  返回 removedNode
```


* Node.cloneNode()

```

```


* Node.normalize()

* Node.compareDocumentPosition()
* Node.contains()


* Node.isDefaultNamespace()
* Node.isEqualNode()

* Node.lookupNamespaceURI()
* Node.lookupPrefix()



