# Professional-JavaScript

```
  《JavaScript高级程序设计》第3版 读书笔记
```

#第十章 DOM

## 10.1 节点层次
* Node.childNodes
* Node.parentNode
* Node.parentElement
* Node.firstChild
* Node.lastChild
* Node.previousSibling
* Node.nextSibling
* Node.nodeName
* Node.nodeType
* Node.nodeValue
* Node.ownerDocument
* Node.baseURI
* Node.textContent
* Document.doctype
* Document.documentElement
* Document.body
* Document.title
* Document.URL
* Document.domain
* Document.referrer
* Document.links
* Document.anchors
* Document.forms
* Document.images
* Document.scripts
* Document.implementation
* Element.attributes

## 10.2 DOM 操作
* Node.hasChildNodes()
* Node.appendChild( newNode )
* Node.insertBefore( newNode, referenceNode )
* Node.replaceChild( newNode，replacedNode )
* Node.removeChild( removedNode )
* Node.cloneNode( deep )
* Node.normalize()
* Document.getElementById()
* Document.getElementsByName()
* Document.getElementsByTagName()
* Document.createElement()
* Document.createTextNode()
* Document.createComment()
* Document.open()
* Document.close()
* Document.write()
* Document.writeln()
* Element.getElementsByTagName()
* Element.getAttribute()
* Element.setAttribute()
* Element.removeAttribute()
* Element.hasAttribute()
* Element.hasAttributes()


#第十一章 DOM 扩展

## 选择符API
* Element.querySelector()
* Element.querySelectorAll()

## 元素遍历
* Element.childElementCount
* Element.firstElementChild
* Element.lastElementChild
* Element.nextElementSibling
* Element.previousElementSibling

## HTML5
* Document.activeElement
* Document.readyState
* Document.compatMode
* Document.head
* Document.characterSet
* Document.hasFocus()
* Element.classList
* Element.innerHTML
* Element.outerHTML
* Element.getElementsByClassName()
* Element.insertAdjacentHTML()
* Element.scrollIntoView()

## 专有扩展

#第十二章 DOM2 和 DOM3
## 12.1 DOM变化

### 针对 XML 命名空间的变化

#### Node 类型的变化
##### DOM2
* Node.localName
* Node.namespaceURI
* Node.prefix
##### DOM3
* Node.isDefaultNamespace()
* Node.lookupNamespaceURI()
* Node.lookupPrefix()

#### Document 类型的变化
##### DOM2
* Document.createElementNS()
* Document.createAttributeNS()
* Document.getElementsByTagNameNS()

#### Element 类型的变化
##### DOM2
* Element.getElementsByTagNameNS()
* Element.getAttributeNS()
* Element.getAttributeNodeNS()
* Element.setAttributeNS()
* Element.setAttributeNodeNS()
* Element.hasAttributeNS()
* Element.removeAttributeNS()

### 其他方面变化
* Node.isSupported() 功能与 document.implementation.hasFeature() 相同
* Node.isSameNode()
* Node.isEqualNode()
* Document.defaultView
* Document.doctype.publicId
* Document.doctype.systemId
* Document.implementation.createDocumentType()
* Document.implementation.createDocument()
* Document.implementation.createHTMLDocument(title)
* Document.importNode(oldNode, boolean)

## 12.2 样式

### 访问元素的样式
* Element.style

### 操作元素的样式
* Document.styleSheets
* Document.getElementsByTagName('link')[0].sheet
* Document.getElementsByTagName('style')[0].sheet

### 元素的大小
#### 偏移量
* Element.offsetHeight
* Element.offsetWidth
* Element.offsetTop
* Element.offsetLeft
* Element.offsetParent
#### 客户区大小
* Element.clientHeight
* Element.clientLeft
* Element.clientTop
* Element.clientWidth
#### 滚动大小
* Element.scrollHeight
* Element.scrollWidth
* Element.scrollTop
* Element.scrollLeft
#### 元素位置
* Element.getClientRect()
* Element.getBoundingClientRect()

## 12.3 遍历
* Document.createNodeIterator() (IE9+)
* Document.createTreeWalker() (IE9+)

## 12.4 范围

#第十三章 事件
## 事件流
### 事件冒泡
### 事件捕获
### DOM 事件流

## 事件处理程序
### HTML 事件处理程序
### DOM0 级事件处理程序
### DOM2 级事件处理程序
### IE 事件处理程序
### 跨浏览器的事件处理程序

## 事件对象
### DOM 中的事件对象
### IE 中的事件对象
### 跨浏览器的事件对象

## 事件类型
### UI 事件
* load 事件
* unload 事件
* resize 事件
* scroll 事件

### 焦点事件
* blur 事件
* focus 事件
* focusin 事件
* focusout 事件

### 鼠标与滚轮事件
* click 事件
* mousedown 事件
* mouseup 事件
* mouseover 事件
* mouseout 事件
* mousemove 事件
* dblclick 事件
* mouseenter 事件
* mouseleave 事件

### 键盘与文本事件

