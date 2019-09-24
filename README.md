# DOM
## Node类型
## 节点信息
每种node类型所对应下面的三种信息都不相同:
* nodeType：节点类型
* nodeName：取决于元素类型
* nodeValue
## 节点关系
以下几种方式都是只读的：
* **parentNode**：父节点。
* **ownerDocument**：所在整个文档的文档节点document。
* **childNodes**：所有子节点。补充：他是个 NodeList 对象。NodeList 是一种类数组对象，它实际上是基于 DOM 结构动态执行查询的结果，因此 DOM 结构的变化能够自动反映在 NodeList 对象中。——可以通过方括号，也可以使用 item()方法来访问。
* **hasChildNodes()**：是否有子节点。比查询 childNodes.length 属性更简单的方法。
* **firstChild**：第一个子节点。firstChild=== someNode.childNodes[0] 
* **lastchild**：最后一个子节点。lastChild===someNode.
childNodes [someNode.childNodes.length-1]
* **previousSibling**：前一个兄弟节点。
* **nextSibling**：后一个兄弟节点。
* **compareDocumentPosition**(给定节点)：
    1 无关（给定的节点不在当前文档中）
    2 居前(给定节点在DOM树中位于参考节点之前)
    4 居后
    8 包含（给定的节点是参考节点的祖先）
    16 被包含（给定的节点是参考节点的后代）


对于元素间的空格，IE9 及之前版本不会返回文本节点，而其他所有浏览器都会返回文本节点。这样，就导致了在使用 childNodes 和 firstChild 等属性时的行为不一致。以下是DOM新规范解决此问题。

* **children**：与childNodes类似，但是他是 HTMLCollection 的实例，只返回元素节点
* **contains**(后代节点)：是否有哪个后代节点。
* **childElementCount**：返回子元素（不包括文本节点和注释）的个数
* **firstElementChild**：指向第一个子元素；firstChild 的元素版。
* **lastElementChild**：指向最后一个子元素；lastChild 的元素版。
* **previousElementSibling**：指向前一个同辈元素；previousSibling 元素版
* **nextElementSibling**：指向后一个同辈元素；nextSibling 的元素版。
![image](https://github.com/snowBoby/DOM/blob/master/images/nodeRelation.png)
## 节点操作
下面前四个都是基于父节点的操作：
* **appendChild**()：向 childNodes 列表的末尾添加一个节点，返回新增的节点。
* **insertBefore**(newNode,someNode)：要插入的节点和作为参照的节点，第二个参数为null，则 insertBefore()与 appendChild()执行相同的操作。
* **removeChild**()：移除节点，返回被移除的节点
* **replaceChild**(newNode,someNode)：要插入的节点和要替换的节点
* **cloneNode(Boolean)**：参数为true，深复制，也就是复制节点及其整个子节点树；为false浅复制，即只复制节点本身，ie>9和其他浏览器 会为空白符也创建节点。这个方法只复制节点，其他一切都不会复制。IE 在此存在一个 bug，即它会复制事件处理程序，所以我们建议在复制之前最好先移除事件处理程序。
* **normalize**()：多个文本节点合并成一个
## 节点插入
性能：在使用 innerHTML、outerHTML 属性和 insertAdjacentHTML()方法时，最好先手工删除要被替换的元素的所有事件处理程序和 JavaScript 对象属性。
* **innerHTML**
* **outerHTNL**
* **innerText/textContent**：兼容ie两种写法。
* **outerText**
* **insertAdjacentHTML**(action,newNode字符串)：action有以下四种：
    "beforebegin"：在当前元素之前插入一个紧邻的同辈元素；
    "afterbegin"：在第一个子元素之前再插入新的子元素；
    "beforeEnd"：在最后一个子元素之后再插入新的子元素；
    "afterend"：在当前元素之后插入一个紧邻的同辈元素
![image](https://github.com/snowBoby/DOM/blob/master/images/client.png)
