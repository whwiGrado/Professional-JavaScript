#第十二章 DOM2 和 DOM3

## 12.1 DOM变化
### 针对 XML 命名空间的变化
#### Node 类型的变化

* DOM2

```
  Node.localName
  Node.namespaceURI
  Node.prefix
```
* DOM3

```
  Node.isDefaultNamespace()
  Node.lookupNamespaceURI()
  Node.lookupPrefix()
```

#### Document 类型的变化
* DOM2

```
  Document.createElementNS()
  Document.createAttributeNS()
  Document.getElementsByTagNameNS()
```

#### Element 类型的变化
* DOM2

```
  Element.getElementsByTagNameNS()
  Element.getAttributeNS()
  Element.getAttributeNodeNS()
  Element.setAttributeNS()
  Element.setAttributeNodeNS()
  Element.hasAttributeNS()
  Element.removeAttributeNS()
```

### 其他方面变化
* Node.isSupported() 功能与 document.implementation.hasFeature() 相同
* Node.isSameNode()
* Node.isEqualNode()

* Document.defaultView

```
  返回一个指针，指向拥有当前文档的窗口或框架
  IE 不支持这个属性，但是 IE 下有一个等价的属性 parentWindow
  因此要确定当前文档归属哪个窗口，可以使用如下代码
    var parentWindow = document.defaultView || document.parentWindow;
```
* Document.doctype.publicId
* Document.doctype.systemId
* Document.implementation.createDocumentType()
* Document.implementation.createDocument()
* Document.implementation.createHTMLDocument(title)

```
  var dtype = document.implementation.createDocumentType(
                'svg:svg',
                '-//W3C//DTD SVG 1.1//EN',
                'http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd');
  console.log(dtype.publicId); // '-//W3C//DTD SVG 1.1//EN'
  console.log(dtype.systemId); // 'http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd'

  var doc = document.implementation.createDocument(
            'http://www.w3.org/2000/svg',
            'svg:svg',
            dtype);

  var htmldoc = document.implementation.createHTMLDocument("New Document");
```

* Document.importNode(oldNode, boolean)

```
  当调用 Node.appendChild(newNode) 时，如果 newNode 不属于当前文档，则会报错
  当调用 Document.importNode(oldNode, boolean) 时，oldNode 可以不属于当前文档
  Document.importNode() 方法会返回一个新节点（是一个原节点的副本），而节点归当前文档所有
```


## 12.2 样式
### 访问元素的样式

```
  HTML 中定义样式有三种方法：
    - 外联式：<link rel="stylesheet" href="">
    — 嵌入式：<style type="text/css"></style>
    - 内联式：<div style=""></div>
```

* Element.style

```
  Element.style 属性返回一个 CSSStyleDeclaration 的实例，即 style 对象
  这个对象拥有的属性和方法如下（IE9+）
    - Element.style.cssName: cssName 是驼峰式，例如：background-color > backgroundColor
      > 返回的 sytle 对象只包含内联式定义的 css 样式
      > 一般 css 属性直接转驼峰就可以，但 float 属性名是 JS 中的保留字，所以(IE 与 FF 不同)
          var cssFloat = Element.style.cssFloat || Element.style.styleFloat;
      > 在给 style.cssName 属性赋值的时候，要指定度量单位，例如：style.width = "100px";

    - document.defaultView.getComputedStyle(ele, pseudoElt)
      > 返回的 computedStyle 对象包含所有 css 样式（外联+嵌入+内联 的叠加样式）
      > IE 下不支持这个方法，但是有个功能相同的属性
          var computedStyle = document.defaultView.getComputedStyle(ele, null) || ele.currentStyle;
      > 获取到的 computedStyle.cssName 的值都是只读的
```

### 操作元素的样式

* Document.styleSheets
* Document.getElementsByTagName('link')[0].sheet
* Document.getElementsByTagName('style')[0].sheet

```
  Document.styleSheets
    > 返回 StyleSheetList 对象
    > 对象中存有文档中所有 CSSStyleSheet 对象（外联式+嵌入式）
    > CSSStyleSheet 对象在 StyleSheetList 中的顺序按照 <sytle>、<link> 标签在 <head> 中出现的顺序排列

  Document.getElementsByTagName('link')[0].sheet
    > 返回文档中的第一个外联式样式的 CSSStyleSheet 对象

  Document.getElementsByTagName('style')[0].sheet
    > 返回文档中的第一个嵌入式样式的 CSSStyleSheet 对象

  CSSStyleSheet 对象的属性和方法：
    属性：
      > 只读属性
        - href: 外联式 <link rel="stylesheet" href=""> 样式表的 url / null
        - media: 当前样式表支持的所有媒体类型的集合
        - ownerNode: 指向拥有当前样式表的节点，IE 不支持这个属性
        - ownerRule: null / 通过 @import 导入时不为 null
        - parentStyleSheet: null / 通过 @import 导入时不为 null
        - cssRules: CSSStyleRule 对象的集合，IE 下为 rules 属性（只适用于嵌入式）
        - rules: 同 cssRules（只适用于嵌入式）
        - title: ownerNode 指向的节点的 title 属性的值
        - type: "text/css"
      > 读写属性
        - disabled: true 禁用当前样式表 / false 不禁用
    方法：（只适用于嵌入式）
      - deleteRule(index) 删除 rules 集合中指定位置的规则，IE 下为 removeRule() 方法
      - insertRule(rule, index) 在 rules 集合中的指定位置插入 rule 规则，IE 下为 addRule() 方法

  CSSStyleRule 对象的属性：
    - cssText(只读): 与 Element.style.cssText 有点不同，因为其包含选择符文本和花括号
    - sytle(读写): 返回一个 CSSStyleDeclaration 的实例，同 Element.style

  例子：获取文档中的第一个 CSSStyleSheet 对象（外联式 或 嵌入式）
    var styleSheetList = []
    ,   firstCssStyleSheet
    ,   firstCssStyleRule
    ,   childName
    ,   headChildren = document.head.children;

    for (var i=0; i<headChildren.length; i++) {
      childName = headChildren[i].nodeName.toLowerCase();
      if (childName === "style" || childName === "link") {
        styleSheetList.push(headChildren[i].sheet);
      }
    }
    firstCssStyleSheet = styleSheetList[0];
    console.log(document.styleSheets[0] === firstCssStyleSheet); // true
    firstCssStyleRule = firstCssStyleSheet.cssRules[0];
    console.log(firstCssStyleRule);
```
### 元素的大小

#### 偏移量
* Element.offsetHeight
* Element.offsetWidth
* Element.offsetTop
* Element.offsetLeft
* Element.offsetParent

```
  Element.offsetHeight 与 Element.offsetWidth 是由元素的 4 部分相加得到的
    - content
    - padding
    - border
    - 滚动条

  Element.offsetTop 与 Element.offsetLeft 是由 3 个部分相加得到的
    - Element 的 margin—top / margin—left
    - Element.offsetParent 的 padding—top / padding—left
    - Element.offsetParent 的 border—top-width / border—left-width

  Element.offsetParent 与 Element.parentNode 很多时候不相等，所以会影响 offsetTop 和 offsetLeft 的计算值
    例如 div 的 Element.offsetParent 永远是 body
      HTML:
        <div id="container">
          <div id="child"></div>
        </div>
      JS:
        var container = document.getElementById('container');
        var child = document.getElementById('child');
        console.log(container.offsetParent.nodeName); // "BODY"
        console.log(child.offsetParent.nodeName); // "BODY"
```

#### 客户区大小
* Element.clientHeight
* Element.clientWidth

```
  确定浏览器视口的大小，可以使用 document.documentElement 或 document.body（< IE7）的 clientHeight 和 clientWidth 属性
    function getViewport(){
      if(document.compatMode === "BackCompat"){
        return {
          width: document.body.clientWidth,
          height: document.body.clientHeight
        };
      } else {
        return {
          width: document.documentElement.clientWidth,
          height: document.documentElement.clientHeight
        };
      }
    }
```
#### 滚动大小
* Element.scrollHeight
* Element.scrollWidth
* Element.scrollTop
* Element.scrollLeft

```
  上述四个属性的返回值均为 Number 类型
  scrollHeight（只读）返回元素内容的总高度
  scrollWidth （只读）返回元素内容的总宽度
  scrollTop   （读/写）返回因为纵向滚动而被隐藏在内容区上方的像素数
  scrollLeft  （读/写）返回因为横向滚动而被隐藏在内容区左侧的像素数
  可以通过给 scrollTop、scrollLeft 两个属性赋值，使得元素滚动的设定的位置
```
#### 元素位置
* Element.getClientRect()
* Element.getBoundingClientRect()

```
  Element.getClientRects 返回一个 ClientRectList 对象，其中包含一个或多个 ClientRect 对象
  Element.getBoundingClientRect() 返回一个 ClientRect 对象
    ClientRect 对象有四个属性：left、top、right、bottom，分别代表元素相对与浏览器视口的位置
    注意：<=IE8 时，浏览器视口的左上角的坐标是(2,2)，IE9 之后修正为(0,0)

  对于块级元素来说：两者并没有区别
    Element.getClientRect()[0] === Element.getBoundingClientRect() // true
  对于内联元素来说，
```
## 12.3 遍历
## 12.4 范围

