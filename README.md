# Professional-JavaScript

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

* Node.removeChild( removedNode )

```
  返回 removedNode
```


* Node.cloneNode( deep )

```
  接受一个(boolean)参数
  true: 深复制
  false: 浅复制
    HTML:
      <ul id="list">
          <li>a</li>
          <li>b</li>
          <li>c</li>
        </ul>
      JS:
        var myList = document.getElementById('list');
        var deepList = myList.cloneNode(true);
        console.log(deepList.childNodes.length); // 3 (IE<9) 或 7 (其他浏览器)
      var shallowList = myList.cloneNode(false);
        console.log(shallowList.childNodes.length); // 0
```

* Node.normalize()

```
  如果在一个包含两个以上子节点的节点上调用 Node.normalize() 则会将所有文本子节点合并为一个节点
    var element = document.createElement('div');
    var textNode = document.createTextNode('hello world!');
    var anotherTextNode = document.createTextNode('forever young!');
    element.appendChild(textNode);
    element.appendChild(anotherTextNode);
    document.body.appendChild(element);

    console.log(element.childNodes.length); // 2

    element.normalize();

    console.log(element.childNodes.length); // 1
    console.log(element.firstChild.nodeValue); // "hello world!forever young!"
```

###### 不常用的
* Node.compareDocumentPosition()
* Node.contains()
* Node.isDefaultNamespace()
* Node.isEqualNode()
* Node.lookupNamespaceURI()
* Node.lookupPrefix()

### Document 类型
```
  document 对象是 HTMLDocument 类型的一个实例，HTMLDocument 类型继承自 Document 类型
  document 对象也是 window 对象的一个属性
  document 继承自 Node 类型的属性：
    document.nodeType // 9
    document.nodeName // "#document"
    document.nodeValue // null
    document.parentNode // null
    document.ownerDocument // null
    document.childNodes // DocumentType(最多一个)、Element(最多一个) 或 Comment
```
#### Document.Properties
* document.doctype
* document.documentElement
* document.body
* document.head

```
  document.doctype.nodeName 是 "html"
  document.doctype.nodeType 是 10
  document.documentElement 返回 document 的根元素 "HTML" 元素
  document.body 返回 document "BODY" 元素
  document.head 返回 document "HEAD" 元素
  后三者的 nodeType 都是 1
```
* document.title
* document.URL
* document.domain
* document.referrer

```
  document.URL 返回页面完整的 URL (地址栏中显示的 URL)
  document.referrer 返回链接到当前页面的那个页面的 URL，可能为空字符串
  document.domain 返回当前页面所在的域名
  document.URL、document.referrer 是只读的
  document.title、document.domain 是可以设置的
    当页面中包含来自其他子域的框架或内嵌框架时，通过将每个页面的 domain 设为相同的值
    则这些页面就可以相互访问对方包含的 js 对象了
    但是 domain 的设置只允许 紧绷型 > 松散型
      console.log(document.domain); // "map.baidu.com" (紧绷型)
      document.domain = "baidu.com"; // 可以改为 (松散型)
```
* document.links
* document.anchors
* document.forms
* document.images
* document.scripts


```
  document.links 返回带有 href 属性的 <a> 元素的 HTMLCollection
  document.anchors 返回带有 name 属性的 <a> 元素的 HTMLCollection
  document.forms 相当于 document.getElementByTagName('form')
  document.images 相当于 document.getElementByTagName('img')
  document.scripts 相当于 document.getElementByTagName('script')
```
* document.implementation

```
  DOM 一致性检测
    var hasDom1 = document.implementation.hasFeature('core', 1.0);
    var hasDom2 = document.implementation.hasFeature('core', 2.0);
    var hasDom3 = document.implementation.hasFeature('core', 3.0);
  尽管 implementation.hasFeature() 很方便，但是在使用 DOM 的某些特殊功能之前
  除了进行 implementation.hasFeature() 的检测，还要进行能力检测
```

###### 不常用的
* document.activeElement
* document.cookie
* document.defaultView
* document.designMode
* document.dir
* document.documentURI
* document.embeds
* document.lastModified
* document.lastStyleSheetSet
* document.location
* document.onafterscriptexecute
* document.onbeforescriptexecute
* document.onoffline
* document.ononline
* document.origin
* document.plugins
* document.preferredStyleSheetSet
* document.readyState
* document.selectedStyleSheetSet
* document.styleSheets
* document.styleSheetSets

#### Document.Methods
* document.getElementById()
* document.getElementsByClassName()
* document.getElementsByName()
* document.getElementsByTagName()
* document.getElementsByTagNameNS()

```
  document.getElementById('id') 参数 id 区分大小写，如果 id 为空，则返回 null
  表单元素<input>、<textarea>、<button>、<select>的 name 属性在 <IE7 时会干扰其他元素 id   匹配
  所以不要让表单元素的 name 属性名与其他元素的 id 属性名相同

  document.getElementsByTagName('tagName') 参数 tagName 区分大小写，返回一个 HTMLCollection 对象，也是动态集合，与 NodeList 很像
  HTMLCollection 对象的访问方法有三种：[index] && .item(index) && namedItem('name')
    var firstItem = getElementsByTagName('body')[0];
    var secondItem = getElementsByTagName('body').item(1);
    var img = getElementsByTagName('img').namedItem('myLogo');
  document.getElementsByTagName('*') 返回

```
* document.adoptNode()
* document.close()
* document.createAttribute()
* document.createCDATASection()
* document.createComment()
* document.createDocumentFragment()
* document.createElement()
* document.createElementNS()
* document.createEvent()
* document.createExpression()
* document.createNSResolver()
* document.createNodeIterator()
* document.createProcessingInstruction()
* document.createRange()
* document.createTextNode()
* document.createTouch()
* document.createTouchList()
* document.createTreeWalker()
* document.enableStyleSheetsForSet
* document.evaluate()
* document.execCommand()

* document.getSelection()
* document.hasFocus
* document.importNode
* document.open
* document.queryCommandSupported
* document.querySelector()
* document.querySelectorAll()
* document.registerElement()
* document.write
* document.writeln