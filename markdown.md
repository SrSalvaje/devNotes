# Mastering Markdown


## Table of contents
[VS documentation](https://code.visualstudio.com/docs/languages/markdown)
* [Shortcuts](#shortcuts)                         
* [Headings](#headings)
* [Paragraph and text decoration](#paragraph-and-text-decoration)
* [Links](#links)
* [images](#images)
* [lists](lists)
* [Line Breaks, horizontal rules and block quotes](#line-breaks-horizontal-rules-and-block-quotes)
* [Code Blocks](#code-blocks)
* [Tables](#tables)
* [Github flavored markdown](#github-flavored-markdown)
----

## Shortcuts
**Open Live view**: `ctrl+shift+v`

**Live view side by side**: `ctrl+k v`

## Headings
Use `#` to make headings `#`= h1, `##`=h2 etc...

## Paragraph and text decoration
There needs to be a full space between lines in order to make a new paragraph.

To decorate text we can use eithjer `*` or `_` , one set makes it *italic*, two **bold** and three ***bold italics***. To ~~strike text~~ we use a double set of `~`.

[home][home]

## Links

Use angled brackets to make a link clickable `<>`, to turn text into a link use `[your text](yourLink)` to add a tooltip to the link: `[your text](httpp/yourLink "your tooltip text")`.
You can also reference the link as a footnote to make it easier to read:
```
Click [here][1] to go to the website.
/*more text*/
[1]:http://aVeryLongLink 

```

[home][home]
## Images

Mostly the same as links but with `!` before `![alt text](linkToImage)`. You can also add a tool tip `![alt text](linkToImage "tool tip text")`.

If I want to make the link reference a larger version of the photo im displaying I can do nested markdown: `[![](linkToSmallerImage)](linkToLargerImage)`.

You can´t size an image on markdown but every time you find something you cant do with md just use HTML/CSS:
```html
<img src="image.jpg" alt="alt description" width="250px" height="250px">

/*or use css*/

<style>
    img{
        width:250px;
        height:250px;
    }
</style>
```

[home][home]
## Lists

Just like HTML there are `ul` and `ol`

### ul

Use `*`, `-` or `+` c before the item, no double space needed to start a new line:
* item1
+ item2
- item3

### ol
Just use `1.`before the item and the editor will sort out the order:
1. item 1
1. item 2
1. item 3

You can also indent lists:

1. Item 1
    * do something else

[home][home]

## Line Breaks, horizontal rules and block quotes

**Line break**<br>
If you dont want to start a new paragraph just use `<br>`.

[home][home]

**Horizontal rules**<br>

---
Use at leas 3 `-` or `=` to create a horizontal rule, there **needs** to be double space in between the rule and the previous line, otherwise it will turn it into an heading.

---

[home][home]

**Block Quotes**<br>
 Use `>` to start a quote block and for each line of the quote:

 ```
 >for example, this would be parsed into
 >
 >a two line block quote
 ```
>for example, this would be parsed into
>  
>a two line block quote

[home][home]

## Code Blocks

You can either just indent the text you want to turn into a code block, or use three back ticks `` ` ``   followed by the language to get extra styling  

```js
var jon = 100;
const dog= "perro";
```

you can also use `diff` and `-`, `+` to emphasize changes:
````
```diff
let x= 100;
-let y = 200;
+let y = 300;
``` 
````

gets parsed into:

```diff
let x= 100;
-let y = 200;
+let y = 300;
```

[home][home]
## Tables

Common but **not** standardized, you use pipe `|` to wrap the column items and `|:----`to separate the headers from the table body:

```

|I am header 1|I am header 2|
|:------------|:------------|
|i go under header 1|I go under header 2|
```
Is parsed into:

|I am header 1|I am header 2|
|:----------|:-----------|
|i go under header 1|I go under header 2|

You can use the `:`position to justify the text:<br>
`|:-----|`justify it to the left<br>
`|:-------:|` centered <br>
`|--------:|`right

[home][home]

## Github flavored markdown

### check boxes
Use `* [ ] Step 1`  for an empty checkbox and `*[x] step 2` for a ticked one.

[home][home]

### Reference issues / pull request

Use # plus the issue number (pick it from the dropdown).

[home][home]

[home]:#table-of-contents
























