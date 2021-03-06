# CSS Documentation

## CSS initial declarations

These are the CSS declarations I always put at the beginning of a CSS file because they will avoid you a lot of problem and you will have to declare them at one moment or another so it's better to do it at the beginning

```css
html, body {
  width: 100%;
  min-height: 100%;
  -webkit-font-smoothing: antialiased;
}

html, body, p, div, article, h1, h2, h3, h4, h5, h6, form {
  margin: 0px;
  padding: 0px;
}

* {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
```

With that, no more HTML overflowing horizontally because you set the width to 100% and didn't put the margin to 0px

## Tips and tricks

#### Gradient text color

We can use a gradient color on a text using a gradient background and the CSS properties `webkit-background-clip` and `webkit-text-fill-color`:

```css
.gradient-text {
  background: -webkit-linear-gradient(top,#fd0b58,#a32b68);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
```
