# Mastering Markdown
To have a paragraph, it actually needs an extra enter. One hash = h1, two hashes = h2 and so on.

Paragraph
========
You can also use plenty of = for h1.
To bold a text you should use **two asterisk** before and after the text you want to bold. For italics, go ahead with _one set of underscore_. 

## Links
<http://dariamarczak.pl>

For an a [My portfolio website](http://dariamarczak.pl)

If a link is quite long, use this syntax: [Google][1]. Instead of numbers, you can also use a word.

[1]: http://google.com

##Images
Use ! [] () – ! says it's an image, in the square brackets you put alt text and in the parenthesis the link.

![Venice, Italy of Jakob Owens](https://images.unsplash.com/photo-1530251985675-fa6a8ceb0d63?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=1150efa56cbc04d298af7f28334ab205&auto=format&fit=crop&w=1050&q=80 "This is the tooltip")

It's also possible to use the same syntax as in the links, so: ![Cute pup][pup]

[pup]: https://images.unsplash.com/photo-1518717758536-85ae29035b6d?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=58321e04954daa3a0e2d3b1dc2a927da&auto=format&fit=crop&w=1050&q=80

A hyperlink using a photo: [![](address)](photo adress)

---

## Lists – ordered, unordered, bullets

### Unordered list
- Minus
* Asterisk
+ Or plus

### Ordered
1. It's better to use one for every item
1. As it will automatically increment
1. Regardless of the number inside

### Nested
1. Combine something
    * Use an asterisk for the indent*
        This is inline

        This is a paragraph
        *You can't control the bullet*

---

## Line breaks
For a full line break, use <br> tag.
For horizontal line use at least 3 - or * but you need to have an enter beforehand 

---

## Block quotes
For quotes, use chevron: >

> Vestibulum ante sapien, posuere in pellentesque at, placerat id tortor. Morbi accumsan velit orci, ac tincidunt lorem porttitor sit amet. Aliquam rutrum ante vitae laoreet maximus. 

> Praesent gravida placerat placerat. Maecenas gravida, turpis sit amet feugiat pharetra, metus nulla maximus neque, eleifend iaculis lectus nisl in ex. Curabitur et eros eget nulla ornare rutrum ac eu diam. Curabitur mattis neque eu purus posuere, sed placerat felis accumsan. Nullam aliquet at nisl at ultrices. Integer semper luctus lobortis. Morbi dictum dignissim eros eu sodales. Aliquam rutrum pretium tortor eget scelerisque. Nam id orci accumsan nunc egestas suscipit in a turpis. 
> **Donec ut auctor ante.**

---

## Code snippets
To have code snippets, go ahead and use three ticks. It actually has id's for further list of content creation.

```javascript
const name = "Daria";
let favColors = ["blue", "black", "grey"]
```

You can also indent it:

    const something = "Blah"
    const doggo = "snickers"

But it's probably better to use the code blocks as the editor actually takes into account which language it is (you don't have to specify this) and highlights it in the editor.

### Inline 
Hey did you try `const y = 100 ` ? For inline code.

### Diff
If you want to show that you should delete one line and add another, add - and + before the line:

```diff
const x = 100;
- const y = 2000;
+ const y = 300;
```

---

## Tables
Are not part of every standard.

You should put a pipe around every thing that you want. But you need |: ----| for every single column (of course without the space).

| Dog's name | Dog's age |
|:-----------|----------:|
| Snickers   | 3         |
| Tabby      | 2         |
| Grof       | 6         |

The :- actually aligns it to the left, :-: aligns it to the center and -: to the right!

---

## Checkboxes
* [ ] Should use asterisk, and square brackets
* [ ] For a done task, put an X inside
* [x] Like so 