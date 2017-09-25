# Basics for DOM APIs

- ## Query Selector
常用的 class、id、属性 选择器都可以使用 `document.querySelector` 或 `document.querySelectorAll` 替代。区别是：
  - `document.querySelector` 返回的是第一个匹配的 Element；
  - `document.querySelectorAll` 返回的是所有匹配的 Element 组成的 `NodeList`。`NodeList` 可以通过 `Array.prototype.slice.call()` 或 `Array.from()` 把它转成 Array；
  - 如果匹配不到任何 Element，`document.querySelector` 返回 `null`，`document.querySelectorAll` 返回长度为 0 的 `NodeList`。

  !> 注意：`document.querySelector` 和 `document.querySelectorAll` 性能很差，如果想要提高性能，尽量使用 `document.getElementById`、`document.getElementsByClassName` 或 `document.getElementsByTagName`。

  - ### 选择器查询
  ```javascript
  document.querySelectorAll('selector');
  ```

  - ### class 查询
  ```javascript
  document.querySelectorAll('.class');
  // or
  document.getElementsByClassName('class');
  ```
  !> `document.getElementsByClassName` 返回的结果是 `HTMLCollection` 类型。

  - ### id 查询
  ```javascript
  document.querySelector('#id');
  // or
  document.getElementById('id');
  ```
  !> `document.getElementById` 如果未匹配到 Element 则返回 `null`。

  - ### 属性查询
  ```javascript
  document.querySelectorAll('a[target=_blank]');
  ```

  - ### 后代查询
  ```javascript
  element.querySelectorAll('li');
  // or
  element.getElementsByTagName('li');
  // or
  element.getElementsByClassName('item');
  // ...
  ```

  - ### 兄弟及上下元素

    - #### 兄弟元素
    ```javascript
    // latest, Edge13+
    [...el.parentNode.children].filter(child => child !== el);
    // alternative - latest, Edge13+
    Array.from(el.parentNode.children).filter(child => child !== el);
    // or IE10+
    Array.prototype.filter.call(el.parentNode.children, child => child !== el);
    ```

    - #### 上一个元素
    ```javascript
    el.previousElementSibling;
    ```

    - #### 下一个元素
    ```javascript
    el.nextElementSibling;
    ```

  - ### Closest
  > Closest 获取匹配选择器的第一个祖先元素，从当前元素开始沿 DOM 树向上查找。

  ```javascript
  // Only latest, No IE
  el.closest(selector);
  // IE10+
  function closest(el, selector) {
    const matchesSelector = el.matches ||
      el.webkitMatchesSelector ||
      el.mozMatchesSelector ||
      el.msMatchesSelector;

    while (el) {
      if (matchesSelector.call(el, selector)) {
        return el;
      } else {
        el = el.parentElement;
      }
    }

    return null;
  }
  ```

  - ### Parents Until
  > 获取当前每一个匹配元素集的祖先，不包括匹配元素本身。

  ```javascript
  function parentsUntil(el, selector, filter) {
    const result = [];
    const matchesSelector = el.matches ||
      el.webkitMatchesSelector ||
      el.mozMatchesSelector ||
      el.msMatchesSelector;

    // match start from parent
    el = el.parentElement;
    while (el && !matchesSelector.call(el, selector)) {
      if (!filter) {
        result.push(el);
      } else {
        if (matchesSelector.call(el, filter)) {
          result.push(el);
        }
      }

      el = el.parentElement;
    }

    return result;
  }
  ```

  - ### Form

    - #### input/textarea
    ```javascript
    document.querySelector('#my-input').value;
    ```

    - #### 获取 e.currentTarget 在 `.radio` 中的数组索引
    ```javascript
    Array.prototype.indexOf.call(document.querySelectorAll('.radio'), e.currentTarget);
    ```

  - ### Iframe

    - #### Iframe contents
    ```javascript
    iframe.contentDocument;
    ```

    - #### Iframe Query
    ```javascript
    iframe.contentDocument.querySelectorAll('.css');
    ```

  - ### 获取 body
  ```javascript
  document.body;
  ```

  - ### 获取或设置属性

    - #### 获取属性
    ```javascript
    el.getAttribute('foo');
    ```

    - #### 设置属性
    ```javascript
    el.setAttribute('foo', 'bar');
    ```

    - #### 获取 `data-` 属性
    ```javascript
    el.getAttribute('data-foo');
    // or (use `dataset` if only need to support IE 11+)
    el.dataset['foo'];
    ```

- ## CSS & Style

- ## DOM Manipulation

- ## Ajax

- ## Events

- ## Utilities

- ## Promises

- ## Animation

- ## Alternatives

- ## Browser Support

- ## Polyfills
