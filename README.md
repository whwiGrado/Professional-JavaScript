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
  ELEMENT_NODE (1)| null  |
  TEXT_NODE (3)| content of the text node  |
  COMMENT_NODE (8)| content of the comment  |
  DOCUMENT_NODE (9)| null  |
  DOCUMENT_TYPE_NODE (10)| null  |
  DOCUMENT_FRAGMENT_NODE (11)| null  |

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

#### Document.Methods
* document.getElementById()

```
  document.getElementById('id') 参数 id 区分大小写
  返回某个元素，例如[object HTMLInputElement]，如果 id 为空，则返回 null
  表单元素<input>、<textarea>、<button>、<select>的 name 属性在 <IE7 时会干扰其他元素 id 匹配，
  所以不要让表单元素的 name 属性名与其他元素的 id 属性名相同
```
* document.getElementsByTagName()

```
  document.getElementsByTagName('tagName') 参数 tagName 不区分大小写
  返回一个 [object HTMLCollection] 对象，也是动态集合，与 NodeList 很像
  HTMLCollection 对象的访问方法有三种：[index] && .item(index) && namedItem('name')
    var firstItem = getElementsByTagName('body')[0];
    var secondItem = getElementsByTagName('body').item(1);
    var img = getElementsByTagName('img').namedItem('myLogo');
  document.getElementsByTagName('*') 返回返回整个页面中的所有元素
```
* document.getElementsByName()

```
  document 特有的方法
  返回一个 [object NodeList] 对象
  最常用的场景就是用 document.getElementsByName() 方法取得单选按钮
    HTML:
      <fieldset>
        <legend>Which color do you prefer?</legend>
        <ul>
          <li>
            <input type="radio" value="red" name="color" id="colorRed">
            <label for="colorRed">Red</label>
          </li>
          <li>
            <input type="radio" value="green" name="color" id="colorGreen">
            <label for="colorGreen">Green</label>
          </li>
          <li>
            <input type="radio" value="blue" name="color" id="colorBlue">
            <label for="colorBlue">Blue</label>
          </li>
        </ul>
      </fieldset>
    JS:
      var radios = document.getElementsByName('color');
```
* document.getElementsByClassName()
* document.getElementsByTagNameNS()

###### 不常用的
* document.createElement()
* document.createComment()
* document.createTextNode()

### Element 类型
```
  Element 继承自 Node 类型的属性：
    Element.nodeType // 1
    Element.nodeName // 元素标签名（大写），与 Element.tagName 返回值相同
    Element.nodeValue // null
    Element.parentNode // Document 或 Element
    Element.ownerDocument // Document
    Element.childNodes // Element、Text、Comment
```
* 常用元素的类型：

    元素 | 类型
    ------------ | ------------- |
    HTML| HTMLHtmlElement  |
    HEAD| HTMLHeadElement  |
    TITLE| HTMLTitleElement  |
    META| HTMLMetaElement  |
    LINK| HTMLLinkElement  |
    STYLE| HTMLStyleElement  |
    BODY| HTMLBodyElement  |
    IFRAME| HTMLIFrameElement  |
    TABLE| HTMLTableElement  |
    DIV| HTMLDivElement  |
    H1-H6| HTMLHeadingElement  |
    P| HTMLParagraphElement  |
    SPAN| HTMLElement  |
    A | HTMLAnchorElement  |
    BUTTON| HTMLButtonElement  |
    IMG| HTMLImageElement  |
    OL| HTMLOListElement  |
    UL| HTMLUListElement  |
    LI| HTMLLIElement  |
    FORM| HTMLFormElement  |
    FIELDSET| HTMLFieldSetElement  |
    INPUT| HTMLInputElement  |
    TEXTAREA| HTMLTextAreaElement  |
    SCRIPT| HTMLScriptElement  |

Element.getAttribute("attrName")

```
  参数 attrName 不区分大小写
  Element.getAttribute("style") 返回的是 style 属性值中包含的 css 文本
  Element.style 返回的是一个对象
  Element.getAttribute("onClick") 返回的是 onClick 属性值中包含的 js 文本
  Element.onClick 返回的是一个函数
    HTML:
      <button style="background: lightgreen; color: #fff;" onclick="alert('run');">click me</button>
    JS:
      var btn = document.getElementsByTagName('button')[0];
      console.log(btn.style); // [object CSSStyleDeclaration] 对象
      console.log(btn.getAttribute('style')); // "background: lightgreen; color: #fff;"
      console.log(btn.onclick); // function onclick(event) {alert('run');}
      console.log(btn.getAttribute('onclick')); // "alert('run');"
```
Element.setAttribute("attrName", "value")

```
  如果属性 attrName 已经存在，则设置为新值
  如果属性 attrName 不存在，则创建属性并设值
  通过 setAttribute() 方法设置的属性，其属性名均会被转换为"小写形式"
```
Element.removeAttribute("attrName")


Element.closest()
Element.getAttributeNode()
Element.getAttributeNodeNS()
Element.getAttributeNS()
Element.getBoundingClientRect()
Element.getClientRects()
Element.getElementsByClassName()
Element.getElementsByTagName()
Element.getElementsByTagNameNS()
Element.hasAttribute()
Element.hasAttributeNS()
Element.hasAttributes()
Element.insertAdjacentHTML()
Element.matches()
Element.querySelector()
Element.querySelectorAll()
ChildNode.remove()
Element.removeAttributeNode()
Element.removeAttributeNS()
Element.requestFullscreen()
Element.requestPointerLock()
Element.scrollIntoView()
Element.setAttributeNode()
Element.setAttributeNodeNS()
Element.setAttributeNS()
Element.setCapture()
