# DOM
DOM 操作往往是 JavaScript 程序中开销最大的部分，而因访问 NodeList 导致的问题为最多。NodeList 对象都是“动态的”，这就意味着每次访问NodeList 对象，都会运行一次查询。有鉴于此，最好的办法就是尽量减少 DOM 操作。
## Node类型
提供了所有节点类型，除了ie之外，其他浏览器都有提供此接口。所以用的时候直接用数值someNode.nodeType == 1
![image](https://github.com/snowBoby/DOM/blob/master/images/nodeType1.png)
![image](https://github.com/snowBoby/DOM/blob/master/images/nodeType2.png)
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
* **appendChild**()：向 childNodes 列表的末尾添加一个节点，返回新增的节点。如果为 appendChild()方法传入一个文档中已有的元素，那么就会先从该元素的父节点中移除它，再把它添加到指定的位置。
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
    "afterend"：在当前元素之后插入一个紧邻的同辈元素；
    
## 节点特性
推荐：自定义属性用getAttribute、setAttribute、removeAttribute方式，非自定义用属性。
* getAttribute()：传递给 getAttribute()的特性名与实际的特性名相同。因此要想得到 class 特性值，应该传入"class"而不是"className"。也可以取得自定义特性。补充：有两类特殊的特性,style和onclick，getAttribute()这俩返回的都是字符串，通过属性访问会得到style对象和js函数，由于存在这些差别，在通过 JS以编程方式操作 DOM 时，开发人员经常不使用 getAttribute()，而是只使用对象的属性。只有在取得自定义特性值的情况下，才会使用 getAttribute()方法。
* **setAttribute**()：只有通过该属性，自定义特性才会变成元素的特性
* **removeAttribute**()：移除某个特性。
* **data-**：自定义数据属性，可以通过dataset获取数据
* **attributes**：包含一个NamedNodeMap，与 NodeList 类似。

    1. ***getNamedItem***(name)：也可以用方括号形式或属性名，返回 nodeName 属性等于 name 的节点，element.attributes.getNamedItem("id").nodeValue;
    2. ***removeNamedItem***(name)：从列表中移除 nodeName 属性等于 name 的节点；removeAttribute()方法的效果相同，区别：即 removeNamedItem()返回表示被删除特性的 Attr 节点
    3. ***setNamedItem***(newAttrNode)：通过属性节点createAttribute，设置新属性。
    4. ***item***(pos)：返回位于数字 pos 位置处的节点。
    5. ***attributes[i].specified***：每个特性节点都有该属性，为 true，则意味着要么是在 HTML 中指定了相应特性，要么是通过 setAttribute()方法设置了该特性。在 IE 中，所有未设置过的特性的该属性值都为 false，而在其他浏览器中根本不会为这类特性生成对应的特性节点（因此，在这些浏览器中，任何特性节点的 specified 值始终为 true）。针对IE7 及更早的版本会返回 HTML 元素中所有可能的特性，包括没有指定的特性。
## 节点样式操作

* **classList**：classList 属性是新集合类型 DOMTokenList 的实例。与其他 DOM 集合类似，有length 属性，而要取得每个元素可以使用 ***item***()方法，也可以使用方括号语法。
    1. ***add***()：将给定的字符串值添加到列表中。如果值已经存在，就不添加了。
    2. ***remove***()：从列表中删除给定的字符串。
    3. ***toggle***()：若存在删除，不存在添加他。
    4. ***contains***()：返回Boolean值
* **style**：获取的是行内样式，有length 属性，而要取得每个元素可以使用 ***item***()方法，也可以使用方括号语法。注意：float是保留字，要用cssFloat/styleFloat（ie）来获取
    1. ***cssText***：行内样式字符串
    2. ***item***(index)：返回给定位置的 CSS 属性的名称
    3. ***getPropertyValue***(propertyName)：返回给定属性的字符串值
    4. ***getPropertyPriority***(propertyName)：如果给定的属性使用了!important 设置，则返回"important"；否则，返回空字符串
    5. ***removeProperty***(propertyName)：从样式中删除给定属性
    6. ***setProperty***(propertyName,value,priority)：将给定属性设置为相应的值，并加上优先权标志（"important"或者一个空字符串）

以下获取层叠样式：
* **document.defaultView.getComputedStyle**()：要取得计算样式的元素和一个伪元素字符串（例如":after"）。如果不需要伪元素信息，第二个参数可以是 null。
* **style.currentStyle**：ie写法
* **document.styleSheets**：获取样式表

## 节点滚动

* **scrollIntoView**()：true 或者不传入任何参数，那么窗口滚动
之后会让调用元素的顶部与视口顶部尽可能平齐。如果传入 false 作为参数，调用元素会尽可能全部出现在视口中,由于 scrollIntoView()是唯一一个所有浏览器都支持的方法，因此还是这个方法最常用。
* **scrollIntoViewIfNeeded**(alignCenter)：只在当前元素在视口中不可见的情况下，才滚动浏览器窗口或容器元素，最终让它可见。如果当前元素在视口中可见，这个方法什么也不做。如果将可选的 alignCenter 参数设置为 true，则表示尽量将元素显示在视口中部（垂直方向）。
Safari 和 Chrome 实现了这个方法。
*  **scrollByLines**(lineCount)：将元素的内容滚动指定的行高，lineCount 值可以是正值，也可以是负值。Safari 和 Chrome 实现了这个方法
*  **scrollByPages**(pageCount)：将元素的内容滚动指定的页面高度，具体高度由元素的高度决定。Safari 和 Chrome 实现了这个方法。-1：往回滚动一页
## 节点大小
偏移量大小：

* **offsetParent**：定位父级，【1】元素自身有fixed定位，offsetParent的结果为null，【2】父级有定位，为离自身元素最近的父级元素，否则为body
* **offsetWidth**：同下
* **offsetHeight**：包括元素的高度、（可见的）
水平滚动条的高度、上边框高度和下边框高度。
* **offsetTop**:同下
* **offsetLeft**：元素的左外边框至offsetParent包含元素的左内边框之间的像素距离
![image](https://github.com/snowBoby/DOM/blob/master/images/offset.png)

客户区大小：
* **clientHeight**
* **clientWidth**：元素内容区宽度加上左右内边距宽度，window.innerWidth === document.documentElement.clientWidth === document.body.clientWidth
![image](https://github.com/snowBoby/DOM/blob/master/images/client.png)

滚动大小：
* **scrollWidth**：元素内容的总宽度
* **scrollHeight**：元素内容的总高度
* **scrollLeft**：被隐藏在内容区域左侧的像素数。通过设置这个属性可以改变元素的滚动位置
* **scrollTop**：被隐藏在内容区域上方的像素数。通过设置这个属性可以改变元素的滚动位置
![image](https://github.com/snowBoby/DOM/blob/master/images/scroll.png)
![image](https://github.com/snowBoby/DOM/blob/master/images/composite.png)

确定元素大小：
* **getBoundingClientRect**()：包含 4 个属性：left、top、right 和 bottom。这些属性给出了元素在页面中相对于视口的位置。有的浏览器还包括width、height、x、y。

`*注意*：对于不包含滚动条的页面而言， scrollWidth 和 scrollHeight 与 clientWidth 和clientHeight 之间的关系并不十分清晰。【1】Firefox 中这两组属性始终都是相等的，但大小代表的是文档内容区域的实际尺寸，而非视口的尺寸【2】Opera、Safari 3.1 及更高版本、Chrome 中的这两组属性是有差别的，其中 scrollWidth 和scrollHeight 等于视口大小，而 clientWidth 和 clientHeight 等于文档内容区域的大小【3】IE（在标准模式）中的这两组属性不相等，其中 scrollWidth 和 scrollHeight 等于文档内容区域的大小，而 clientWidth 和 clientHeight 等于视口大小。因此，在确定文档的总高度时（包括基于视口的最小高度时），必须取得 scrollWidth/clientWidth 和scrollHeight/clientHeight 中的最大值，才能保证在跨浏览器的环境下得到精确的结果`

## document
创建节点：

* **createElement**()：只接受一个参数，即要创建元素的标签名
* **createAttribute**()：传入特性的名称可以创建新的特性节点。
* **createTextNode**()：创建文本节点
* **createCDataSection**()：只在XML 文档中使用
* **createComment**()：创建注释节点
* **createDocumentFragment**()：创建文档片段，不是文档树的一部分。

文档获取节点：

* **getElementById**()：ie<7如果有哪个表单元素的 name 特性等于指
定的 ID，而且该元素在文档中位于带有给定 ID 的元素前面，那么 IE 就会返回那个表单元素。
* **getElementsByTagName**()：返回HTMLCollection对象，类似nodeList对象，方括号、item()、namedItem()、还支持按名称访问项来访问，namedItem使用这个方法可以通过元素的 name特性取得集合中的项 images.namedItem("myImage")或images["myImage"]
* **getElementsByName**()：与 getElementsByTagName()类似，namedItem()方法则只会取得第一项
* **getElementsByClassName**()
* **querySelector**()：通过 Document 类型调用 querySelector()方法时，会在文档元素的范围内查找匹配的元素。而
通过 Element 类型调用 querySelector()方法时，只会在该元素后代元素的范围内查找匹配的元素。
* **querySelectorAll**()：返回的是一个 NodeList 的实例

文档获取节点快捷方式：

* **doctype**：<!DOCTYPE>标签，支持差别很大
* **documentElement**：html标签
* **head**：HTML5 新增了 document.head 属性,document.head || document.getElementsByTagName("head")[0]
* **body**：body标签
* **title**：<title>元素中的文本
* **images**：包含文档中所有的<img>元素
* **forms**：包含文档中所有的<form>元素
* **anchors**：包含文档中所有带 name 特性的<a>元素
* **links**：包含文档中所有带 href 特性的<a>元素。
* **charset**：表示文档中实际使用的字符集，
也可以用来指定新字符集。
* **defaultCharset**：表示根据默认浏览器及操作系统的设置，当前文档默认的字符集应该是什么

网页请求相关：

* **URL**：页面完整的 URL
* **domain**：页面的域名
* **referrer**：取得来源页面的 URL

焦点管理：

* **activeElement**：会引用 DOM 中当前获得了焦点的元素
* **hasFocus**()：确定文档是否获得了焦点

HTMLDocument新功能：

* **readyState**：loading（正在加载文档）/complete（已经加载完文档）
* **compatMode**：CSS1Compat（标准模式）/BackCompat（混杂模式）
* **documentMode**：文档模式

## table

* caption
* tHead
* tBodies
* tFoot
* rows：保存着<tbody>元素中行的 HTMLCollection。
* cells：保存着<tr>元素中单元格的 HTMLCollection。
* createCaption()/deleteCaption()：创建<caption>元素，将其放到表格中，返回引用。
* createTHead()/deleteTHead()
* createTFoot()/deleteTFoot()
* insertRow(pos)/deleteRow(pos)：向 rows 集合中的指定位置插入一行。返回对新插入行的引用
* insertCell(pos)/deleteCell(pos)：向 cells 集合中的指定位置插入一个单元格，返回对新插入单元格的引用。
