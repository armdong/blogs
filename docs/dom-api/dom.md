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

  - ### CSS
    - #### Get Style

    ```javascript
    const win = el.ownerDocument.defaultView;
    // null 的意思是不返回伪类元素
    win.getComputedStyle(el, null).color;
    ```
    !> 注意：`el.ownerDocument.defaultView` 此处为了解决当 `style` 值为 `auto` 时，返回 `auto` 的问题。

    - #### Set Style

    ```javascript
    el.style.backgroundColor = '#ff0011';
    ```

    - #### Get/Set Styles
    > 一次设置多个 style。

    ```javascript
    const reUnit = /width|height|top|left|right|bottom|margin|padding/i;

    // func of set single style
    function setStyle(node, att, val, style) {
      style = style || node.style;

      if (style) {

        if (val === null || val === '') { // normalize unsetting
          val = '';
        } else if (!isNaN(Number(val)) && reUnit.test(att)) { // number values may need a unit
          val += 'px';
        }

        if (att === '') {
          att = 'cssText';
          val = '';
        }

        style[att] = val;
      }
    }

    // funs of set multi styles
    function setStyles(el, hash) {
      const HAS_CSSTEXT_FEATURE = typeof(el.style.cssText) !== 'undefined';
      function trim(str) {
        return str.replace(/^\s+|\s+$/g, '');
      }

      let originStyleText;
      const originStyleObj = {};

      if (!!HAS_CSSTEXT_FEATURE) {
        originStyleText = el.style.cssText;
      } else {
        originStyleText = el.getAttribute('style');
      }

      originStyleText.split(';').forEach(item => {
        if (item.indexOf(':') !== -1) {
          const obj = item.split(':');
          originStyleObj[trim(obj[0])] = trim(obj[1]);
        }
      });

      const styleObj = {};
      Object.keys(hash).forEach(item => {
        setStyle(el, item, hash[item], styleObj);
      });

      const mergedStyleObj = Object.assign({}, originStyleObj, styleObj);
      const styleText = Object.keys(mergedStyleObj)
        .map(item => item + ': ' + mergedStyleObj[item] + ';')
        .join(' ');

      if (!!HAS_CSSTEXT_FEATURE) {
        el.style.cssText = styleText;
      } else {
        el.setAttribute('style', styleText);
      }
    }

    // func of get style
    function getStyle(el, att, style) {
      style = style || el.style;
      let val = '';

      if (style) {
        val = style[att];
        if (val === '') {
          val = getComputedStyle(el, att);
        }
      }

      return val;
    }

    // func of getComputedStyle
    function getComputedStyle(el, att) {
      const win = el.ownerDocument.defaultView;

      // null means not return presudo styles
      const computed = win.getComputedStyle(el, null);
      return att ? computed[att] : computed;
    }
    ```

    - #### Add/Remove/Has/Toggle class

    ```javascript
    // Native
    el.classList.add(className); // add class
    el.classList.remove(className); //remove class
    el.classList.contains(className); // has class
    el.classList.toggle(className); // toggle class

    // or

    // func of addClass
    // el can be an Element, NodeList or selector
    function addClass(el, className) {
      if (typeof el === 'string') {
        el = document.querySelectorAll(el);
      }

      const els = (el instanceof NodeList) ? [].slice.call(el) : [el];
      els.forEach(e => {
        if (hasClass(e, className)) {
          return;
        }

        if (e.classList) {
          e.classList.add(className);
        } else {
          e.className += ' ' + className;
        }
      });
    }

    // func of removeClass
    // el can be an Element, NodeList or selector
    function removeClass(el, className) {
      if (typeof el === 'string') {
        el = document.querySelectorAll(el);
      }

      const els = (el instanceof NodeList) ? [].slice.call(el) : [el];
      els.forEach(e => {
        if (hasClass(e, className)) {
          if (e.classList) {
            e.classList.remove(className);
          } else {
            e.className = e.className.replace(new RegExp('(^|\\b)' + className.split(' ').join('|') + '(\\b|$)', 'gi'), ' ');
          }
        }
      });
    }

    // func of hasClass
    // el can be an Element or selector
    function hasClass(el, className) {
      if (typeof el === 'string') {
        el = document.querySelector(el);
      }

      if (el.classList) {
        return el.classList.contains(className);
      }

      return new RegExp('(^| )' + className + '( |$)', 'gi').test(el.className);
    }

    // func of toggleClass
    // el can be an Element or selector
    function toggleClass(el, className) {
      if (typeof el === 'string') {
        el = document.querySelector(el);
      }

      const flag = hasClass(el, className);
      if (flag) {
        removeClass(el, className);
      } else {
        addClass(el, className);
      }

      return flag;
    }
    ```

  - ### Width & Height

    - #### Window width/height

    ```javascript
    // 含 scrollbar
    const clientWidth = window.document.documentElement.clientWidth;
    const clientHeight = window.document.documentElement.clientHeight;

    // 不含 scrollbar
    const innerWidth = window.innerWidth;
    const innerHeight = window.innerHeight;
    ```

    - #### Document width/height

    ```javascript
    const body = document.body;
    const html = document.documentElement;
    const width = Math.max(
      body.offsetWidth,
      body.scrollWidth,
      html.clientWidth,
      html.offsetWidth,
      html.scrollWidth
    );
    const height = Math.max(
      body.offsetHeight,
      body.scrollHeight,
      html.clientHeight,
      html.offsetHeight,
      html.scrollHeight
    );
    ```

    - #### Element width/height

    ```javascript
    /**
     * Native 精确到整数
     */

    // 当 box-sizing 为 border-box 时为 width - border 值
    // 当 box-sizing 为 content-box 时为 width + padding 值
    const width = el.clientWidth;

    // 当 box-sizing 为 border-box 时为 height - border 值
    // 当 box-sizing 为 content-box 时为 height + padding 值
    const height = el.clientHeight;

    /**
     * Native 精确到小数
     */

    // 当 box-sizing 为 border-box 时为 width 值
    // 当 box-sizing 为 content-box 时为 width + padding + border 值
    const width = el.getBoundingClientRect().width;

    // 当 box-sizing 为 border-box 时为 height 值
    // 当 box-sizing 为 content-box 时为 height + padding + border 值
    const height = el.getBoundingClientRect().height;

    /**
     * funcs of get width/height
     */

    // outer height
    function outerHeight(el) {
      return el.offsetHeight;
    }

    // outer height with margin
    function outerHeightWithMargin(el) {
      let height = el.offsetHeight;
      const style = getComputedStyle(el);

      height += (parseFloat(style.marginTop) || 0) + (parseFloat(style.marginBottom) || 0);
      return height;
    }

    // outer width
    function outerWidth(el) {
      return el.offsetWidth;
    }

    // outer width width margin
    function outerWidthWithMargin(el) {
      let width = el.offsetWidth;
      const style = getComputedStyle(el);

      width += (parseFloat(style.marginLeft) || 0) + (parseFloat(style.marginRight) || 0);
      return width;
    }

    // get comptuted styles
    function getComputedStyles(el) {
      return el.ownerDocument.defaultView.getComputedStyle(el, null);
    }
    ```

  - ### Position & Offset

    - #### Position

    > 获得匹配元素相对父元素的偏移

    ```javascript
    function getPosition(el) {
      if (!el) {
        return {
          left: 0,
          top: 0
        };
      }

      return {
        left: el.offsetLeft,
        top: el.offsetTop
      };
    }
    ```

    - #### Offset

    > 获得匹配元素相对文档的偏移

    ```javascript
    function getOffset(el) {
      const html = el.ownerDocument.documentElement;
      let box = { top: 0, left: 0 };

      // If we don't have gBCR, just use 0,0 rather than error
      // BlackBerry 5, iOS 3 (original iPhone)
      if ( typeof el.getBoundingClientRect !== 'undefined' ) {
        box = el.getBoundingClientRect();
      }

      return {
        top: box.top + window.pageYOffset - html.clientTop,
        left: box.left + window.pageXOffset - html.clientLeft
      };
    }
    ```

  - ### Scroll Top

    - #### 获取文档滚动条垂直位置

    ```javascript
    function getDocumentScrollTop() {
      // IE8 used `document.documentElement`
      return (document.documentElement && document.documentElement.scrollTop) ||
        document.body.scrollTop;
    }
    ```

    - #### 设置文档滚动条垂直位置

    ```javascript
    function setDocumentScrollTop(value) {
      window.scrollTo(0, value);
      return value;
    }
    ```

- ## DOM Manipulation

  - ### Remove Element

  > 从 DOM 中移出元素

  ```javascript
  el.parentNode.removeChild(el);
  ```

  - ### Get/Set Element's Text

    - #### Get Element's Text

    > 返回指定元素的文本内容。

    ```javascript
    el.textContent;
    ```

    - #### Set Element's Text

    > 设置指定元素的文本内容。

    ```javascript
    el.textContent = string;
    ```

  - ### Get/Set Element's HTML

    - #### Get Element's HTML

    > 获取指定元素的 HTML 内容。

    ```javascript
    el.innerHTML;
    ```

    - #### Set Element's HTML

    > 设置指定元素的 HTML 内容。

    ```javascript
    el.innerHTML = htmlString;
    ```

  - ### Append Element

  > Append 插入到子节点的末尾

  ```javascript
  // Native (HTML string)
  el.insertAdjacentHTML('beforeend', '<div id="container">Hello world</div>');

  // Native (Element)
  el.appendChild(newEl);
  ```

  - ### Prepend Element

  ```javascript
  // Native (HTML string)
  el.insertAdjacentHTML('afterbegin', '<div id="container">Hello world</div>');

  // Native (Element)
  el.insertBefore(newEl, el.firstChild);
  ```

  - ### insertBefore

  > 在指定元素前插入新节点

  ```javascript
  // Native (HTML string)
  el.insertAdjacentHTML('beforebegin', '<div id="container">Hello world</div>');

  // Native (Element)
  const el = document.querySelector(selector);
  if (el.parentNode) {
    el.parentNode.insertBefore(newEl, el);
  }
  ```

  - ### insertAfter

  > 在指定元素后插入新节点

  ```javascript
  // Native (HTML string)
  el.insertAdjacentHTML('afterend', '<div id="container">Hello world</div>');

  // Native (Element)
  const el = document.querySelector(selector);
  if (el.parentNode) {
    el.parentNode.insertBefore(newEl, el.nextSibling);
  }
  ```

  - ### is

  > 如果匹配给定的选择器，返回 true

  ```javascript
  el.matches(selector);
  ```

  - ### clone

  > 拷贝被选元素。（生成被选元素的副本，包含子节点、文本和属性。）

  ```javascript
  el.cloneNode();

  // 深拷贝添加参数 true
  el.cloneNode(true);
  ```

  - ### empty

  > 移除所有子节点

  ```javascript
  el.innerHTML = '';
  ```

  - ### wrap/unwrap

    - #### wrap

    > 把每个被选元素放置在指定的 HTML 结构中。

    ```javascript
    Array.prototype.forEach.call(document.querySelectorAll('.inner'), el => {
      const wrapper = document.createElement('div');
      wrapper.classList.add('wrapper');
      el.parentNode.insertBefore(wrapper, el);
      el.parentNode.removeChild(el);
      wrapper.appendChild(el);
    });
    ```

    - #### unwrap

    > 把被选元素的父元素移除 DOM 结构。

    ```javascript
    Array.prototype.forEach.call(document.querySelectorAll('.inner'), el => {
      Array.prototype.forEach.call(el.childNodes, child => {
        el.parentNode.insertBefore(child, el);
      });
      el.parentNode.removeChild(el);
    });
    ```

  - ### replaceWith

  > 用指定的元素替换被选的元素

  ```javascript
  Array.prototype.forEach.call(document.querySelectorAll('.inner'), el => {
    const outer = document.createElement('div');
    outer.classList.add('outer');
    el.parentNode.insertBefore(outer, el);
    el.parentNode.removeChild(el);
  });
  ```

  - ### simple parse

  > 解析 HTML/SVG/XML 字符串

  ```javascript
  const range = document.createRange();
  const parse = range.createContextualFragment.bind(range);

  parse(`
    <ol>
      <li>a</li>
      <li>b</li>
    </ol>
    <ol>
      <li>c</li>
      <li>d</li>
    </ol>
  `);
  ```

- ## Polyfills
  - [Underscore.js](http://underscorejs.org/)
  - [Lodash](https://github.com/lodash/lodash)
  - [oui-dom-utils](https://github.com/oneuijs/oui-dom-utils)
