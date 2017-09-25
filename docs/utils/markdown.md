# Github Flavored Markdown Syntax Simple Guide

---

## Comments
> You can add comment to markdown by three ways like below:

**Syntax One:**
``` markdown
[comment]: <> (This is a comment, it will not be included)
[comment]: <> (in the output file unless you use it in)
[comment]: <> (a reference style link.)
```

**Syntax Two:**
``` markdown
[//]: <> (This is also a comment.)
```

**Syntax Three: _(Recommend)_**
``` markdown
[//]: # (This may be the most platform independent comment)
```
neither
``` markdown
[//]: # "This may be the most platform independent comment"
```

---

## Horizontal lines
> A line consisting of 0-3 spaces of indentation, followed by a sequence of three or more matching -, _, or * characters, each followed optionally by any number of spaces.

**Syntax:**
``` markdown
***
---
___
```

---

## Headings
> There are six headings just like html tag h1-h6.

- # Heading 1
  `# this is heading 1`

- ## Heading 2
  `## this is heading 2`

- ### Heading 3
  `### this is heading 3`

- #### Heading 4
  `#### this is heading 4`

- ##### Heading 5
  `##### this is heading 5`

- ###### Heading 6
  `###### this is heading 6`

---

## Code highlight
> Print code and highlight code syntax.

- single line code:  `console.log('hi there').`

- multi lines code:

  - javascript code:

    ``` javascript
    // class of dog
    export default class Dog {
      constructor(name) {
        this.name = name;
      }
      bark() {
        console.log('Wun, wun, wun...');
      }
    }
    ```

  - Ruby code:

    ``` ruby
    def foo(x)
      return 3
    end
    ```

---

## Link
> Square brackets shows link text and parentheses define the link url.

**Syntax:**
``` markdown
[link_text](link_url)
[link_text](link_url "link_title")

[link_text][link_id]
[link_id]: link_url "link_title"
```

**Usage:**
- This is the link to [Google](http://www.google.com).
- This is the link to [Baidu](https://www.baidu.com "Baidu").
- This is the link to [Github][github].
- This is the link to [Twitter][twitter].

---

## Image

**Syntax:**
``` markdown
![Alt text](/path/to/img.jpg)
![Alt text](/path/to/img.jpg "Optional title")

![Alt text][id]
[id]: url/to/image "Optional title attribute"
```

**Usage:**

![Dinotocat Image](https://octodex.github.com/images/dinotocat.png)

![Inflatocat Image](https://octodex.github.com/images/inflatocat.png "this is the inflatocat image")

![Privateinvestocat Image][privateinverstocat]

---

## Image Link

**Syntax:**
``` markdown
[![Alt text](/path/to/img.jpg "Optional image title")](link_url "Optional link title")
```

**Usage:**
Click the page and redirct to my Github. [![Github Image](https://octodex.github.com/images/saritocat.png "Saritocat")](https://github.com/armdong)

---

## Bold and Italic
> We can use '*' and '_' character to bold or italic text.

### Bold

**Syntax:**

``` markdown
**text to bold**
__text to bold also__
```

**Usage:**

**This is the bolded text.**

__This is the bolded text again.__

### Italic

**Syntax:**
``` markdown
*text to italic*
_text to italic also_
```

**Usage:**

*This is the italiced text.*

_This is the italiced text again._

---

## List
> It is simple to make ordered list or unordered list with markdown.

### Unordered List
> We can use '+', '-' or '*' characters to make unordered list, just like the word below:

**Syntax:**

``` markdown
+ item 1
+ item 2
+ item 3
```

``` markdown
- item 1
- item 2
- item 3
```

``` markdown
* item 1
* item 2
* item 3
```

**Usage:**

> Here are some programming languages

- JavaScript
- HTML
- CSS
- C#
- Java

> Here is the content part of a book

+ Chapter 1
  - Section 1
  - Section 2
  - Section 3
+ Chapter 2
  - Section 1
  - Section 2
    * Content 1
    * Content 2
    * Content 3
    * Content 4
  - Section 3
+ Chapter 3

### Ordered List
> You just need to use a number and a dot character to build the Ordered List, whatever the number is.

**Syntax:**

``` markdown
1. Level One
2. Level Two
3. Level Three
```

**Usage:**

> Here is the example of a student's score.

1. Math: 99
2. English: 90
3. Art: 88

---

## TODO List

**Syntax:**

``` markdown
- [ ] unchecked item
- [x] checked item
```

**Usage:**
> Here are some todo list you have to do or already done.

- [x] Wash your face.
- [x] Clean your tooth.
- [ ] Eat your breakfast.
- [ ] Ride you bicycle then go to school.

---

## Table
> In markdown, you can build table like this:

First Header | Second Header | Third Header
:----------- | :-----------: | -----------:
Left         | Center        | Right
Left         | Center        | Right
Left         | Center        | Right

**Syntax:**

``` markdown
First Header | Second Header | Third Header
:----------- | :-----------: | -----------:
Left         | Center        | Right
Left         | Center        | Right
Left         | Center        | Right
```



[//]: # "Some pre defined links here"
[homepage]: https://armdong.github.io/markdown-101/
[github]: https://github.com
[twitter]: https://twitter.com "Twritter Homapage"

[//]: # "Some pre defined images here"
[privateinverstocat]: https://octodex.github.com/images/privateinvestocat.jpg "This is the private investocat image"
