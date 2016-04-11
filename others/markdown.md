# Markdown

Markdown is a formatting language used on various websites ( like Reddit, GitHub... )

In fact, this text uses Markdown :D

Mastering Markdown is not only cool, it's useful as fuck as it allows you to write good comments and good documentation on GitHub using a nice formatting. Stop writing ugly READMEs that makes everyone go away at the first look on your doc :)

## Basics

Markdown is a formatting language that compiles into HTML, so you just have to write Markdown and you get HTML in the output.

Because of this, pretty much every Markdown element will be explained using his HTML cousin

### Line breaks

In Markdown, doing a single line break won't do shit ( it will just create a space character like when you line break in your HTML code ).

**Single line**

To do a single line break, skip a line:
```
Hi there

Hello
```

will render to that:

>Hi there
>
>Hello

**Multiple lines**

To skip multiple lines, use the `&nbsp;` separator with a skipped line before and after it:

```
Hi, this will ...

&nbsp;

.. skip a line
```

will render to that:

>Hi, this will ...
>
>&nbsp;
>
>... skip a line

### Titles

In markdown you can use different titles, corresponding to HTML `<h1>`, `<h2>`, `<h3>`, `<h4>`, `<h5>`, `<h6>`

To do that, just write your title like that:
```
# Title
## Smaller title
```

The more `#` you use, the smaller your title will get:
# # h1
## ## h2
### ### h3
#### #### h4
##### ##### h5
###### ###### h6

### Text formatting

So in Markdown you can format your text to have better visuals and outline some elements:

\**Italic text*\* ( `<i>` )

\*\***Bold text**\*\* ( `<b>` )

\~\~~~Deleted text~~\~\~ ( `<del>` )

### Lists

Lists are useful when writing documentation, so you better learn how to use them:

**Unordered lists**

By using \* at the star of a line, you can create unordered lists:

```
* Item 1
* Item 2
  * Nested item 1
  * Nested item 2
* Item 3
```

will render to that:

* Item 1
* Item 2
  * Nested item 1
  * Nested item 2
* Item 3

**Ordered lists**

To use ordered lists, just write 'n.' with `n` being a number at the beginning of your line:

```
1. Item 1
2. Item 2
  1. Nested item 1
  2. Nested item 2
3. Item 3
```

will render to that:

1. Item 1
2. Item 2
  1. Nested item 1
  2. Nested item 2
3. Item 3

## Going deeper ( no jokerino plz )

### Quotes

You can quote people ( either Confucius or some idiot on Reddit ) using the `>` symbol at the start of a line ( and you can even nest them ):

```
>mfw no gf
>>who needs a 3d girl anyway ?

>blaah :p
```

will render to that:

>mfw no gf
>>who needs a 3d girl anyway ?

>blaah :p

### Code

A nice thing when writing Documentation or even a README is that you can easily insert snippets of code using the character \`

**Span-style code**

You can insert code inside a text without skipping a line or anything by writing \``Your code`\`

To do an alert in Javascript, use the `alert("Your message");` function

**Multiple lines and syntax highlighting**

You can also do multiple lines code blocks, and the good thing is that they have syntax highlighting, so your code will look way nicer :D

To do a multiple line block, write your code like this:

\`\`\`

Multiple lines

coding

\`\`\`

This will render like this:
```
Multiple lines
coding
```

To use syntax highlighting, just declare the language after the first brackets ( I don't know how you call that shit ):

\`\`\`javascript

var msg = "Hello world!";

alert(msg);

\`\`\`

will render like that:

```javascript
var msg = "Hello world!";
alert(msg);
```
