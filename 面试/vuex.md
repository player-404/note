# 获取节点

### `1. childNodes 属性`

childNodes属性获取元素下的所有节点(包括文本节点)

```html
<body>
    <div id="app">
        <span> 1 </span>
        <h1> 2 </h1>
        <p> 3 </p>
        hello world!
    </div>
    <script>
        // childNodes属性 获取元素中的所有节点（包括文本节点）
        const app = document.getElementById("app");
        const nodes = app.childNodes;
        console.log(nodes)
    </script>
</body>
```



### `2. nodeType属性`

返回当前节点的类型

| 常量                               | 值   | 描述                                                         |
| :--------------------------------- | :--- | :----------------------------------------------------------- |
| `Node.ELEMENT_NODE`                | `1`  | 一个 [`元素`](https://developer.mozilla.org/zh-CN/docs/Web/API/Element) 节点，例如 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/p) 和 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/div)。 |
| `Node.TEXT_NODE`                   | `3`  | [`Element`](https://developer.mozilla.org/zh-CN/docs/Web/API/Element) 或者 [`Attr`](https://developer.mozilla.org/zh-CN/docs/Web/API/Attr) 中实际的 [`文字`](https://developer.mozilla.org/zh-CN/docs/Web/API/Text) |
| `Node.CDATA_SECTION_NODE`          | `4`  | 一个 [`CDATASection`](https://developer.mozilla.org/zh-CN/docs/Web/API/CDATASection)，例如 `<!CDATA[[ … ]]>`。 |
| `Node.PROCESSING_INSTRUCTION_NODE` | `7`  | 一个用于XML文档的 [`ProcessingInstruction` (en-US)](https://developer.mozilla.org/en-US/docs/Web/API/ProcessingInstruction) ，例如 `<?xml-stylesheet ... ?>` 声明。 |
| `Node.COMMENT_NODE`                | `8`  | 一个 [`Comment`](https://developer.mozilla.org/zh-CN/docs/Web/API/Comment) 节点。 |
| `Node.DOCUMENT_NODE`               | `9`  | 一个 [`Document`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document) 节点。 |
| `Node.DOCUMENT_TYPE_NODE`          | `10` | 描述文档类型的 [`DocumentType`](https://developer.mozilla.org/zh-CN/docs/Web/API/DocumentType) 节点。例如 `<!DOCTYPE html>` 就是用于 HTML5 的。 |
| `Node.DOCUMENT_FRAGMENT_NODE`      | `11` | 一个 [`DocumentFragment`](https://developer.mozilla.org/zh-CN/docs/Web/API/DocumentFragment) 节点 |

```html
<body>
    <div id="app">
        <span> 1 </span>
        <h1> 2 </h1>
        <p> 3 </p>
        hello world!
    </div>
    <script>
        //nodeType 返回当前节点的类型
        const type = nodes[1].nodeType;
        console.log('node type is', type); // node type is 1
    </script>
```

