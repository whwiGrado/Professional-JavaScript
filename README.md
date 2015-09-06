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

## 12.3 遍历

## 12.4 范围

