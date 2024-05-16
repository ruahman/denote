-------------------------------------------------------------------------------
title:      "markdown"
date:       2023-07-11T21:39:13-04:00
tags:       ["emacs", "tech"]

identifier: "20230711T213913"
-----------------------------

# Links #
create a link :: C-c C-l

[Try Google](https://www.google.com)

# images #
create image link :: C-c C-i

![norman rockwell](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2F2.bp.blogspot.com%2F_c8Dl9oXWQuA%2FTTnaFWAel4I%2FAAAAAAAAAUQ%2FdtrhadJI_D8%2Fs1600%2Fart%2B009%2B%2525282%252529.jpg&f=1&nofb=1&ipt=af09d6c6fb1a25a675eac36998a2e49f88985e1272bbdb32a0ada9306e856119&ipo=images "rockwell image")

# headings #
C-c C-s h

## heading 2 ##
C-c C-s 2

### heading 3 ###
C-c C-s 3

#### heading 4 ####
C-c C-s 4


# mark down menu # #
C-c C-s

## more options ##
C-c C-s C-h


# Styling #

*Italic*
C-c C-s i
_Italic_

**bold**
__bold__
C-c C-s b

~~strik through~~
> blockquotes
`inline code`fLyMd-mAkEr


# horizontal rule #

-------------------------------------------------------------------------------
C-c C-s - 

# preview #
C-c C-c p  :: preview in browser
C-c C-c l  :: live previw modeh
C-c C-x C-m  :: markdown preview
    

# lists #

- lists
  - item1
    - item2
  - item3
  
`C-c <left>|<right>` :: demote or promote
- this applies to lists and headers


1. item1
    2. item2
3. item3

* item1
* item2
* item3

+ item1
+ item3
+ item2

`C-c <up>|<down>` :: move item up or down


C-c C-s - : horizontal rule
---------------------------

code section
``` js
let test = 123
```

<!-- comment -->aslfj


<!-- link -->
[test](http://www.linkadress.com "test")

`C-c C-l` :: insert link
`C-c C-o` :: open link


<!-- image -->
![name](link)

`C-c C-i` :: insert image


<!-- github markdown -->

```javascript
let x = 45
const y = 89

```

```python
print("asdjf")
```

<!-- tables -->
| fasf | asdfas |
|------|--------|
| th   | is     |
| cool |        |
|      |        |
|      |        |


<!-- reference -->
(my ref)[ref1]

[ref1]: this is a ref


`C-c C-s` :: styling commands

`C-c C-h` :: all help commands

`C-c C-s C-h` :: list all style commands

`C-c C-l` :: links

`C-c C-i` :: images

`C-c C-c` :: commands

`C-c C-x` :: toggle

GFM Code Blocks (GitHub Flavored Markdown)

``` python
print("hello world")
```

## Definition lists ##

Term 
: Definition


Mathematical Expressions
========================

\[ y = mx + b \]

or

$$ y = mx + b $$

`C-c C-x C-e` :: hightlight mathematica expressions
- toggle equations



###### Previewing ######

`C-c C-x C-m` :: toogle markdown preview

`C-c C-c p`  :: preview in a browser

`C-c C-x l`  :: preview in eww
