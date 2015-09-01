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
    - style.cssName: cssName 是驼峰式，例如：background-color > backgroundColor
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

```
  返回 StyleSheetList 对象，对象中存有文档中所有 CSSStyleSheet 对象（外联式+嵌入式）
    其中的 CSSStyleSheet 对象在 StyleSheetList 中的顺序
    按照 <sytle>、<link> 标签在 <head> 中出现的顺序排列

      var styleSheetList = []
      ,   firstCssStyleSheet
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
```


## 12.3 遍历
## 12.4 范围












Document.applets
Document.bgColor
Document.contentType
Document.currentScript
Document.designMode
Document.dir
Document.documentURI
Document.documentURIObject
Document.domConfig
Document.embeds
Document.fgColor
Document.height
Document.inputEncoding
Document.lastModified
Document.lastStyleSheetSet
Document.linkColor
Document.location
Document.mozFullScreen
Document.mozFullScreenElement
Document.mozFullScreenEnabled
Document.mozSyntheticDocument
GlobalEventHandlers.onabort
Document.onafterscriptexecute
Document.onbeforescriptexecute
GlobalEventHandlers.onblur
GlobalEventHandlers.onchange
GlobalEventHandlers.onclick
GlobalEventHandlers.onclose
GlobalEventHandlers.oncontextmenu
GlobalEventHandlers.ondblclick
GlobalEventHandlers.onerror
GlobalEventHandlers.onfocus
GlobalEventHandlers.oninput
GlobalEventHandlers.onkeydown
GlobalEventHandlers.onkeypress
GlobalEventHandlers.onkeyup
GlobalEventHandlers.onload
GlobalEventHandlers.onmousedown
GlobalEventHandlers.onmousemove
GlobalEventHandlers.onmouseout
GlobalEventHandlers.onmouseover
GlobalEventHandlers.onmouseup
Document.onoffline
Document.ononline
GlobalEventHandlers.onreset
GlobalEventHandlers.onresize
GlobalEventHandlers.onscroll
GlobalEventHandlers.onselect
GlobalEventHandlers.onsubmit
Document.origin
Document.plugins
Document.pointerLockElement
Document.popupNode
Document.preferredStyleSheetSet
Document.scrollingElement
Document.selectedStyleSheetSet
Document.styleSheetSets
Document.tooltipNode
Document.vlinkColor
Document.width
Document.xmlEncoding

Document.adoptNode()
Document.caretPositionFromPoint()
Document.caretRangeFromPoint()
Document.clear()
Document.createCDATASection()
Document.createDocumentFragment()
Document.createEntityReference()
Document.createEvent()
Document.createExpression()
Document.createNodeIterator()
Document.createNSResolver()
Document.createProcessingInstruction()
Document.createRange()
Document.createTouch()
Document.createTouchList()
Document.createTreeWalker()
Document.elementFromPoint()
Document.enableStyleSheetsForSet()
Document.evaluate()
Document.execCommand()
Document.exitPointerLock()
Document.getBoxObjectFor()
Document.getSelection()
Document.loadOverlay()
Document.mozCancelFullScreen()
Document.mozSetImageElement()
Document.queryCommandSupported()
Document.registerElement()
Document.releaseCapture()



Element.accessKey
Element.className
Element.clientHeight
Element.clientLeft
Element.clientTop
Element.clientWidth
Element.id
Element.name
Element.onwheel
Element.scrollHeight
Element.scrollLeft
Element.scrollLeftMax
Element.scrollTop
Element.scrollTopMax
Element.scrollWidth
Element.tabStop
Element.tagName

Element.closest()
Element.getBoundingClientRect()
Element.getClientRects()
Element.matches()
ChildNode.remove()
Element.requestFullscreen()
Element.requestPointerLock()
Element.setCapture()
