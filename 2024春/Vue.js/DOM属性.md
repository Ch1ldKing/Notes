### 常用 DOM 属性

1. **`innerHTML`**:
    
    - 用于获取或设置元素的 HTML 内容。
    - 可以直接插入 HTML 代码，适合处理复杂的 HTML 结构。
    
    `element.innerHTML = '<p>New content</p>'; console.log(element.innerHTML);`
    
2. **`innerText`**:
    
    - 用于获取或设置元素的文本内容，只包括可见的文本。
    - 会触发重排和重绘，性能较低。
    
    
    `element.innerText = 'Visible text'; console.log(element.innerText);`
    
3. **`outerHTML`**:
    
    - 用于获取或设置元素及其所有后代节点的 HTML 内容。
    - 设置时会替换整个元素。

    
    `element.outerHTML = '<div>Replaced element</div>'; console.log(element.outerHTML);`
    
4. **`value`**:
    
    - 用于获取或设置表单元素（如 `<input>`, `<textarea>`）的值。
    
    javascript
    
    复制代码
    
    `const input = document.querySelector('input'); input.value = 'New value'; console.log(input.value);`
    
5. **`src`**:
    
    - 用于获取或设置 `<img>` 和 `<iframe>` 元素的源 URL。
    
    javascript
    
    复制代码
    
    `const img = document.querySelector('img'); img.src = 'new-image.png'; console.log(img.src);`
    
6. **`href`**:
    
    - 用于获取或设置 `<a>` 元素的链接 URL。
    
    javascript
    
    复制代码
    
    `const link = document.querySelector('a'); link.href = 'https://www.example.com'; console.log(link.href);`
    
7. **`style`**:
    
    - 用于获取或设置元素的行内样式。
    
    javascript
    
    复制代码
    
    `element.style.color = 'red'; console.log(element.style.color);`
    
8. **`className`**:
    
    - 用于获取或设置元素的 `class` 属性。
    - 可以通过 `classList` 进行更细粒度的操作。
    
    javascript
    
    复制代码
    
    `element.className = 'new-class'; console.log(element.className);`
    
    javascript
    
    复制代码
    
    `element.classList.add('another-class'); element.classList.remove('new-class'); console.log(element.classList.contains('another-class'));`
    
9. **`id`**:
    
    - 用于获取或设置元素的 `id` 属性。
    
    javascript
    
    复制代码
    
    `element.id = 'new-id'; console.log(element.id);`
    
10. **`dataset`**:
    
    - 用于获取或设置元素的自定义数据属性（data-* 属性）。
    
    javascript
    
    复制代码
    
    `element.dataset.customData = 'customValue'; console.log(element.dataset.customData);`
    

### 其他有用的属性

- **`tagName`**:
    
    - 返回元素的标签名（大写）。
    
    javascript
    
    复制代码
    
    `console.log(element.tagName); // e.g., "DIV"`
    
- **`children`**:
    
    - 返回元素的子元素集合（不包括文本节点）。
    
    javascript
    
    复制代码
    
    `const children = element.children; console.log(children.length);`
    
- **`parentNode`**:
    
    - 返回元素的父节点。
    
    javascript
    
    复制代码
    
    `const parent = element.parentNode; console.log(parent);`
    
- **`nextSibling` 和 `previousSibling`**:
    
    - 返回元素的下一个或上一个兄弟节点（包括文本节点）。
    
    javascript
    
    复制代码
    
    `const next = element.nextSibling; const previous = element.previousSibling; console.log(next, previous);`
    
- **`nextElementSibling` 和 `previousElementSibling`**:
    
    - 返回元素的下一个或上一个兄弟元素（不包括文本节点）。
    
    javascript
    
    复制代码
    
    `const nextElement = element.nextElementSibling; const previousElement = element.previousElementSibling; console.log(nextElement, previousElement);`