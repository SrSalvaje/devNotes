## Table of contents

|[Shortcuts](#shortcuts)|[Extensions](#extensions)|
|:----|:------|
| [Debugging](#debugging)|

## Shortcuts
|[navigating](#navigating-and-manipulating-text) |[search and replace](#search-and-replace)| [Toggle Sidebar](#toggle) | [split screen](#split-screen-files) |
|:----|:------|:-----|:---
|**[Terminal](#terminal)**|**[Emmet](#emmet)**|**[Better Comments](#better-comments)**|**[Bookmarks](#bookmarks)**

## Better Comments

|Action|Shortcut|
|:----|:------|
|in Block| /** **/
|in line|//
|normal| *
|highlighted| * *
|Alert | !
|question| ?
|todo| TODO

[home][home]

## BookMarks
|Action|Shortcut|
|:----|:------|
|Toggle| ctrl+alt+k
|Labeled|ctrl+shift+p and choose toggle labeled
|Next| ctrl+alt+L
|Prev|ctrl+alt+J
|List|ctrl+shift+p and choose from drop down
|Clear all|ctrl+shift+p and choose from drop down

[home][home]

**Ctrl+Shift+P** = open the command palette

**ctrl+alt+r**= es7 snippet search

**ctrl+shift+j** = convert css-in-js

### Settings sync

**shift+alt+d** = download settings
**shift+alt+u** =  to sync vscode settings

shift+alt+. autofix

[home][home]


### Toggle

|Action|Shortcut|
|:----|:------|
|Toggle Sidebar|**Ctrl+b**
|Toggle Git| **Ctrl+Shift+g**
|Toggle Debug|**Ctrl+Shift+d**
|Toggle Explorer|**Ctrl+Shift+E**
|Toggle Terminal | **Ctrl+Ö**
Toggle Search|**Ctrl+Shift+F**
Zen Mode|**Ctrl+K Z**

[home][home]

### Terminal

|Action|Shortcut|
|:----|:------|
|New|**Ctrl+Shift+Ö**
|split| **Ctrl+§**
|Debug|**Ctrl+Shift+Y**
Output|**Ctrl+Shift+U**
Problems|**Ctrl+Shift+M**

[home][home]

### Split Screen  / Files

|Action|Shortcut|Action|Shortcut|
|:----|:------|:----|:------|
toggle vertical/horizontal |**Shift+Alt+0**|create directory and file| **dir/file.ext**
change editor group |**Ctrl+1-9**|open file to the side|**Ctrl+enter**
tab trough files in editor group|**Ctrl+tab**|tab in reverse|**Ctrl+Shift+tab**|
search and open file|**Ctrl+p**|new file|**Ctrl+n**
close file|**Ctrl+w**|Open new window for same workspace|**Ctrl+Shift+n**

[home][home]

### Navigating and Manipulating Text

|Action|Shortcut|Action|Shortcut|
|:----|:------|:----|:------|
Go to end of word|**Ctrl+lft/rght**|select |**Shift+arrow keys** for letter or  **Ctrl+Shift+arrow keys** for word
end of line|**home+end**|start of line|**end+home**
end of file|**Ctrl+end**|start of file| **Ctrl+home**
select current word|**Ctrl+d**|expand/shrink selection|**Shift+Alt+lft/rght**
select all document content|**Ctrl+a**|move entire line up/down|**Alt+up/down**
copy line up/down|**Shift+Alt+up/down**|delete line |**Ctrl+Shift+k**
add cursor|**Ctrl+Alt+up/down**|go to matching bracket| **Ctrl+Shift+§**

[home][home]

### Search and Replace

Tips:
* Make the selection before launching the search box to auto populate it
* Use workspace search and replace (`Ctrl+Shift+h`) to refactor a variable across multiple files.

|Action|Shortcut|Action|Shortcut|
|:----|:------|:----|:------|
|launch find|**Ctrl+f**|find and replace|**Ctrl+h**
|next match|**f3**|prev match|**Shift+f3**
replace 1 by 1|**Ctrl+Shift+1**|replace all| **Ctrl+Alt+enter**

[home][home]

### Emmet
|[Base Elements](#base-elements)|[id/class](#id-and-class-attributes)|[spawn several](#multiplications)|[children](#child)|
|:----|:----|:----|:----
|**[siblings](#sibling)**|**[Groupings (complex children)](#groupings)**|**[custom attributes](#custom-attribute)**|**[element plus text](#text)**|
|**[other](#good-to-know)**|||

[home][home]

### Base Elements

> * `div`
> *   `h1`
> *   `p`
> *   `nav`

[home][home]

### Id and Class Attributes

***Add '.' followed by classname***

> .container -> div with a class of container

> div.container -> div with a class of container

> h3.text-center -> h3 with class of text-center

> div.container.flex-container -> div with class of container AND flex-container

***Add '#' followed by id***

> #mainContainer -> div with id of mainContainer

> div#mainContainer -> div with id of mainContainer

> h1#header -> h1 with id of header

***Combine class and id***

> div.container#mainContainer -> div with class of container and id of mainContainer

> h1.text-center#header -> h1 with class of text-center and id of header

[home][home]

### Multiplications

> h1\*3 -> 3 h1's

> li\*3 -> 3 li's

[home][home]

### Numbering

***use the '$' operator combined with a multiplication to apply an index to an element***

> ul>li.item$\*5 -> ul with 5 elements that have classes of item1, item2, etc.

[home][home]

### Child

***Use '>' to add children***

> div>h1 -> div with a child of H1

> ul>li -> ul with child of li

> ul>li\*3 -> ul with 3 li children

[home][home]

### Sibling

***Use '+' to add siblings***

> div>h1+p -> div with two children, h1 and p

> ul>li+li -> ul with two li children

> form>input:text+input:text+input:submit -> form with two text input children and a child submitt button

[home][home]

### Groupings

***Use parenthesis to group things together...think complex children***

> div>(form>input:text+input:text+input:submit)+p -> div with two children a form (with two inputs and a submit button) and a p tag

[home][home]

### Custom Attribute

***Set custom attributes with brackets '[]'***

> p[title="Hello world"] -> p with title attribute set to 'Hello world'

[home][home]

### Text

***Set inner text for text elements with curly braces '{}'***

> p{hello} -> p with inner text of hello

> a{Click here} -> anchor tag with inner text of Click here

[home][home]

### Good to know

***Here's some extra ones to know!***

> !

> link:css

> script:src

> input:text|email|password|date|number|submit|button

> btn

[home][home]

## Extensions

Extensions that I am either using or that I find interesting.

[vsnotes](https://marketplace.visualstudio.com/items?itemName=patricklee.vsnotes) Might be useful to organize my dev notes.

[home][home]

## Debugging

[General](https://scotch.io/tutorials/debugging-javascript-in-google-chrome-and-visual-studio-code)

[For React](https://scotch.io/tutorials/debugging-create-react-app-applications-in-visual-studio-code)

[home]:#table-of-contents