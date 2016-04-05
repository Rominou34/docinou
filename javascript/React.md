# React JS

So I just started React 2 weeks ago and played with it for like one hour ( I didn't even finish the tutorial ), so I just create the file and note the only tips I wrote but it'll take some time to fill this one.

***** ROMINOU REACT TIPS *****
**Identifying DOM elements**

* We put an attribute `ref="example"` on the element
* We then access it using the function `React.findDOMNode()`:
  ```js
  React.findDOMNode(this.refs.example);
  ```
