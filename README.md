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

![image](https://github.com/snowBoby/DOM/blob/master/images/client.png)
