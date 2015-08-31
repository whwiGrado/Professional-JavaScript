#第十一章 DOM 扩展
## 选择符API（IE8+）

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
querySelectorAll 返回的是一个带有所有属性和方法的 NodeList 实例
相当于 NodeList 的快照，而非不断对文档进行搜索的动态查询
这样可以避免使用 NodeList 对象引起的大多数性能问题
```

## 元素遍历（IE9+）

  方法| 返回值 |
  ------------ | :-----------: |
  childElementCount| 返回子元素的个数（不包含文本节点和注释节点） |
  firstElementChild| firstChild 的元素版 |
  lastElementChild| laseChild 的元素版 |
  previousElementSibling| previousSibling 的元素版 |
  nextElementSibling| nextSibling 的元素版 |

## HTML5

#### 元素获取
* getElementsByClassName('className1 className2 ...') (IE9+)

```
  接收一个参数：包含一个或多个类名的字符串，多个类名时，顺序无所谓
  返回同时含有参数中所有类名的所有元素的 [object HTMLCollection] 对象（动态）
  document.getElementsByClassName() 在文档范围内检索
  Element.getElementsByClassName() 在该元素的后代里检索
```

* classList (IE10+)

```
  Element.classList 返回一个 DOMTokenList 的实例
  这个实例有一个 length 属性，和四个方法:
  classList.add('className1') or classList.add('className1', 'className2', ...)
  classList.remove('className1') or classList.remove('className1', 'className2', ...)
  classList.toggle('classToBeRemoved', false)
  classList.toggle('classToBeadded', true)
  classList.contains('className')

```

#### 焦点管理
* document.activeElement 属性 (IE4+)
* document.hasFocus() 方法 (IE4+)

```
  document.activeElement 动态保存着 DOM 中当前获得焦点的元素，如果没有元素获得焦点，则返回 null
  元素获得焦点的方式有三种:
    - 页面加载完成
    - 用户输入（通常是按 Tab 键）
    - 调用 focus() 方法
  文档刚刚加载完成时，document.activeElement 保存的值是 document.body
    if (document.readyState === "complete") {
      // 文档加载完毕
      console.log(document.activeElement === document.body); // true
    }

  document.hasFocus() 可以检测到文档是否获得了焦点，从而判断用户是否正在与页面交互
```

#### Document 类型扩展
* document.readyState (IE4+)
* document.compatMode (IE6+)
* document.head (Chrome、Safari 5+)

```
  document.readyState 属性有两个取值:
    loading: 正在加载文档
    complete: 已经加载完文档
    替代以前的 window.onload() 事件
      if (document.readyState === "complete") {
        // 文档加载完毕
      }

  document.compatMode 属性有两个取值:
    CSS1Compat: 标准模式下渲染
    BackCompat: 混杂模式下渲染
      if (document.compatMode === 'CSS1Compat') {
        // Standards mode
      } else {
        // Quirks mode
      }

  document.head 返回 document "HEAD" 元素，如果不支持，则使用 getElementsByTagName() 方法
    var head = document.head || document.getElementsByTagName('head')[0];
```

#### 字符集属性

```
  document.characterSet 仅支持读取
  document.charset 支持读写
    var charset = document.characterSet ? document.characterSet : document.charset;
  但是字符集属性一般
    在 meta 元素中设置
      <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    在响应头部设置
      <form method="post" accept-charset="utf-8"></form>
    不建议用 document.charset 设置
```

#### 自定义数据属性

```
  为元素添加非标准的属性，需要加前缀 data- ，目的是为元素提供与渲染无关的信息，或者提供语义信息
  添加了自定义属性后，可以通过元素的 dataset 属性来访问自定义属性的值
  自定义属性名的形式是 data-*-*-* （例如:data-some-info-added）
  而通过 dataset 访问时需要用驼峰式: dataset.someInfoAdded
    HTML:
      <div id="user" data-id="1234567890" data-user="whw" data-date-of-birth="2015-08-29">John Doe</div>
    JS:
      var el = document.querySelector('#user');
      console.log(
        el.id, // "user"
        el.dataset.id, // "1234567890"
        el.dataset.user, // "whw"
        el.dataset.dateOfBirth // "2015-08-29"
      );
```

#### 插入标记

* Element.innerHTML
* Element.outerHTML
* Element.innerText
* Element.textContent

```
  总结: innerHTML、innerText、textContent

    取值操作:
      Element.innerHTML 属性: 获取 innerHTML 属性值，直接返回

      Element.innerText 属性: 首先获取 innerHTML 属性值，
        然后对 innerHTML 的属性值进行一系列处理，然后返回，具体步骤如下
          - 对 HTML 标签进行解析
          - 对 CSS 样式进行带限制的解析和渲染
          - 将 ASCII 实体转换为对应的字符
          - 剔除格式信息（如\t、\r和\n等），将多个连续的空格合并为一个

      Element.textContent 属性: 首先获取 innerHTML 属性值，
        然后对 innerHTML 的属性值进行一系列处理，然后返回，具体步骤如下
          - 对 HTML 标签进行剔除（也包括 <style> <script> <br />）
          - 将 ASCII 实体转换为对应的字符
          - 不会剔除格式信息和合并连续的空格，因此\t、\r、\n和连续的空格将生效

    赋值操作:
      Element.innerHTML = "string"
        - 并不是所有元素都支持 innerHTML 属性，
          例如: <html> <head> <title> <style> <table> <frameset> 均不支持
        - "string" 中的 < > & ' " 这五个符号会被转换为实体名
          对应为: &lt; &gt; &amp; &quot;
        - 转换之后，对 HTML CSS 进行解析（JS 脚本不会被执行），然后将结果返回给 Element.innerHTML 属性

      Element.innerText = "string"
        - "string" 中的 < > & ' " 这五个符号会被转换为实体名
          对应为: &lt; &gt; &amp; &quot;
        - 转换之后直接将结果返回给 Element.innerText 属性

      Element.textContent = "string"
        - "string" 中的 < > & ' " 这五个符号会被转换为实体名
          对应为: &lt; &gt; &amp; &quot;
        - 转换之后直接将结果返回给 Element.innerText 属性
```

* Element.insertAdjacentHTML(position, text);

```
  支持四个位置插入（即 position 的取值）
    'beforebegin': 在该元素之前插入一个同辈元素
    'afterbegin' : 在该元素的 firstChild 之前插入一个元素
    'beforeend'  : 在该元素的 lastChild 之后插入一个元素
    'afterend'   : 在该元素之后插入一个同辈元素
```

## 专有扩展




