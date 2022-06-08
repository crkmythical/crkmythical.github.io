---
title: org-syntax
encrypt: false
enc_pwd: askding
categories:
  - Evolution

date:  2021-05-08 16:59:18
top:
tags:
layout:
---

# Table of Contents

1.  [Org Mode - Your life in plain text](#org1f2e3d0)
    1.  [Metadata](#org22e2b21)
    2.  [Headlines](#orgf520299)
    3.  [Markup](#orgca6af80)
    4.  [Lists](#org08cbd90)
    5.  [Links](#org29a3a9a)
    6.  [Images](#org4616742)
    7.  [Blocks](#org51f208e)
    8.  [Table](#org617375d)
    9.  [Commnet](#orga1ab3ca)
    10. [Macros](#org3561d83)
    11. [footnotes](#org6c0fec9)



<a id="org1f2e3d0"></a>

# Org Mode - Your life in plain text

Org is a highly flexible structured plain text file format, composed of a few simple, yet versatile, structures &#x2013; constructed to be both **simple enough for the novice** and **powerful enough for the expert**.

Org mode is Emacs major mode for convenient plain text markup,is for 

-   keeping notes,
-   maintaining to-do lists,
-   planning projects,
-   authoring documents,
-   computational notebooks,
-   literate programming
-   and more

in a fast and effective plain text system.


<a id="org22e2b21"></a>

## Metadata

At the start of a file(before the first headline), it's common to set the title, author and other **export options**

    #+title: Example Org File
    #+author: Mr.Frame
    #+date: Fri May  7 10:07:35 CST 2021
    #+startup: overview content showall showeverything nohideblocks  hideblocks


<a id="orgf520299"></a>

## Headlines

Lines that start with an asterisk `*` are **headlines** 
A single `*` denotes a 1st-level headline

    * Revamp orgmode.org website
    ** Make screenshots


<a id="orgca6af80"></a>

## Markup

Text markup follows th pattern,  PRE, MARKER, CONTENTS, MARKER and POST are not separated by whitespace characters. 
`<Pre><Marker><Contents><Marker><Post>`

-   <Pre>: - ( { ' " and whitespace<sup><a id="fnr.1" class="footref" href="#fn.1">1</a></sup>,
-   <Marker>: a character among \*(bold), =(verbatim), /(italic), +(strikethrough), \_(underline), ~(code)
-   <Post>: <whitespace character>,- . , ; : ! ? ' " ) } [
-   <Contents>: `<non-whitespace-character><any character but may not span over more than 3 lines><non-whitespace-character>`

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Font Style</th>
<th scope="col" class="org-left">Singal</th>
<th scope="col" class="org-left">Effect</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">Bold</td>
<td class="org-left"><code>*Bold*</code></td>
<td class="org-left"><b>Bold</b></td>
</tr>


<tr>
<td class="org-left">Italic</td>
<td class="org-left"><code>/Italic/</code></td>
<td class="org-left"><i>Italic</i></td>
</tr>


<tr>
<td class="org-left">Underline</td>
<td class="org-left"><code>_underline_</code></td>
<td class="org-left"><span class="underline">underline</span></td>
</tr>


<tr>
<td class="org-left">Strikethrough</td>
<td class="org-left"><code>+strikethrough+</code></td>
<td class="org-left"><del>strikethrough</del></td>
</tr>


<tr>
<td class="org-left">code</td>
<td class="org-left"><code>~code~</code></td>
<td class="org-left"><code>code</code></td>
</tr>


<tr>
<td class="org-left">verbatim</td>
<td class="org-left"><code>=verbatim=</code></td>
<td class="org-left"><code>berbatim</code></td>
</tr>


<tr>
<td class="org-left">Superscripts</td>
<td class="org-left"><code>R_sun=6.96 x 10^8m</code></td>
<td class="org-left">R<sub>sun</sub>=6.96 x 10<sup>8m</sup></td>
</tr>


<tr>
<td class="org-left">Subscripts</td>
<td class="org-left"><code>R_{AlphaCentauri} = 1.28 x R_{sum}</code></td>
<td class="org-left">R<sub>AlphaCentauri</sub> = 1.28 x R<sub>sum</sub></td>
</tr>
</tbody>
</table>


<a id="org08cbd90"></a>

## Lists

Unordered lists start with + -
Ordered lists start with 1. 1)  A.  A)
Unordered and odered bullets can be nested in any order.

    To buy:
    1. Milk
    2. Eggs
      - Organic
    3. Cheese
      + Parmesan
      + Mozzarella
    
    Lists can contain checkboxes [ ] [-] [X]
    - [ ] not started
    - [-] in progress
    - [X] complete 
    
    Lists can contain tags (and checkboxes at the same time)
    - [ ] fruits :: get apples
    - [X] vegiies :: get carrots


<a id="org29a3a9a"></a>

## Links

put the target between two square brackets,like so: `[[type:target]]` or `[[type:target][desc]]` 
same as an html `<a>` tag `<a href="target">desc</a>`

    [[https://orgmode.org][a nice website]]
    [[file:~/Pictures/dank-emem.png]]
    [[earlier heading][an earlier heading in the document]]


<a id="org4616742"></a>

## Images

Org mode automatically recognizes and renders image links during export. 
Just link to an image **(don't include a description)**.   

`[[https://upload.wikimedia.org/wikipedia/commons/5/5d/Konigsberg_bridges.png]]`  

Images located on your computer can also be rendered in the Emacs buffer with **`C-c C-x C-v`**


<a id="org51f208e"></a>

## Blocks

Org mode uses **#+BEGIN** &#x2026; **#+END** blocks for many purposes. 

    (defun org-xor (a b)
       "Exclusive or."
       (if a (not b) b))


<a id="org617375d"></a>

## Table

    | Tool         | Literate programming? | Reproducible Research? | Languages |
    |--------------+-----------------------+------------------------+-----------|
    | Javadoc      | partial               | no                     | Java      |
    | Haskell .lhs | partial               | no                     | Haskell   |
    | noweb        | yes                   | no                     | any       |
    | Sweave       | partial               | yes                    | R         |
    | Org-mode     | yes                   | yes                    | any       |


<a id="orga1ab3ca"></a>

## Commnet

-   Line comments start with **#**
-   Inline comments wrap `@@comment: like so@@`
-   Block comments are wrapped with **`#+BEGIN_COMMENT`** and **`#+END_COMMENT`**
-   Section comments can be created by adding the **COMMENT** keyword to a headline
    
          # A line commment
        
          Example of an @@comment: inline@@ comment.
        
          
          #+begin_comment
          This is a block comment.
          It can span multiple line.
          As well as other markup.
          
          #+begin_src emacs-lisp
          (+ 1 2)
          #+end_src
          
          #+end_comment
          * A top level headline
          ** COMMENT This section and subsections are commented out
          *** This headline inherits the =COMMENT= keyword
        This text is commented out
          ** This headline is not commented


<a id="org3561d83"></a>

## Macros

    #+macro: attn _*/$1/*_
    {{{attn(This text gets all the markup!)}}}

<span class="underline">***Attention! This text gets all the markup!***</span> \_ this will underline\_


<a id="org6c0fec9"></a>

## footnotes

-   `[fn:name:description]`
-   `[fn::description]`
-   `[fn:name2]`
    
    `[fn:name2] description`

    ** Footnotes
    The Org homepage[fn:1] now looks a lot better than it used to.
    
    Tom is a boy[fn:name]. 
    
    Jim is a boy[fn:: This is the inline definition of this footnote] too.
    
    Lily is a girl[fn:lily: a definition].
    
    
    [fn:1] The link is: http://orgmode.org
    [fn:name] Tom is a boy.


# Footnotes

<sup><a id="fn.1" href="#fnr.1">1</a></sup> \space \tab \enter
