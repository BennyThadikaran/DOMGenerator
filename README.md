# DOMGenerator
A simple helper function for generating DOM elements and dynamically loading scripts and stylesheets

DOMGenerator makes it easy to dynamically generate HTML with very little code.

Written in vanilla javascript.

## Usage

Generate an HTML tag
```javascript
var h1 = Node.create("h1", {
    id: "title",
    class: "blog-title"
}, "DOMGenerator");
// outputs h1 node: <h1 id="title" class="blog-title">DOMGenerator</h1>
```

Append multiple elements into a parent node
```javascript
// create some empty nodes
var div = Node.create("div"),
    h1 = Node.create("h1"),
    h2 = Node.create("h2"),
    p = Node.create("p");

// Append all of them to the div node
Node.append([h1, h2, p], div);
// div node now has 3 child nodes attached
```

Remove an element from the DOM
```HTML
// HTML
<div>
  <h1 id="title" class="blog-title">DOMGenerator</h1>
</div>
```
```javascript
// JAVASCRIPT
var h1 = document.getElementById("title");

Node.remove(h1);
```

## Loading files dynamically

Load scripts dynamically

Appends a script tag to the document body with async attribute set.

Second argument is a call back function executed, when the onload event is fired.

```javascript
Node.loadScript("/js/comments.min.js", Comments.init);
```

Load CSS stylesheets dynamically

Appends a link tag to the document head for each stylesheet in the array.

Pass an array of stylesheets to load
```javascript
Node.loadCss([
    "/js/main.min.css",
    "/js/comment.min.css"
]);
```

## Examples
Generate a [Bootstrap List group](https://getbootstrap.com/docs/3.4/components/#list-group) dynamically in JS
```html
// HTML
<ul class="list-group">
  <li class="list-group-item">Cras justo odio</li>
  <li class="list-group-item">Dapibus ac facilisis in</li>
  <li class="list-group-item">Morbi leo risus</li>
  <li class="list-group-item">Porta ac consectetur ac</li>
  <li class="list-group-item">Vestibulum at eros</li>
</ul>
```

```javascript
// JAVASCRIPT
function generateList(listArray) {
    "use strict";
    var ul = Node.create("ul", {class: "list-group"});
    var li;

    listArray.forEach(function (listItem) {
        li = Node.create("li", {class: "list-group-item"}, listItem);
        ul.appendChild(li);
    });

    return ul;
}

var list = [
    "Cras justo odio",
    "Dapibus ac facilisis in",
    "Morbi leo risus",
    "Porta ac consectetur ac",
    "Vestibulum at eros"
];

// call the function
generateList(list);
```

### Little more complex example.

Lets generate the [Bootstrap basic form](https://getbootstrap.com/docs/3.4/css/#forms-example)
```HTML
// HTML
<form>
  <div class="form-group">
    <label for="exampleInputEmail1">Email address</label>
    <input type="email" class="form-control" id="exampleInputEmail1" placeholder="Email">
  </div>
  <div class="form-group">
    <label for="exampleInputPassword1">Password</label>
    <input type="password" class="form-control" id="exampleInputPassword1" placeholder="Password">
  </div>
  <div class="form-group">
    <label for="exampleInputFile">File input</label>
    <input type="file" id="exampleInputFile">
    <p class="help-block">Example block-level help text here.</p>
  </div>
  <div class="checkbox">
    <label>
      <input type="checkbox"> Check me out
    </label>
  </div>
  <button type="submit" class="btn btn-default">Submit</button>
</form>
```
```javascript
// JAVASCRIPT
function generateForm() {
    "use strict";
    var form = Node.create("form", {class: "form-group"}),

        group1 = Node.create("form", {class: "form-group"}),
        emailLabel = Node.create("label", {for: "InputEmail1"}, "Email address"),
        emailInput = Node.create("input", {
            type: "email",
            class: "form-control",
            id: "InputEmail1",
            placeholder: "Email"
        }),

        group2 = group1.cloneNode(),
        passwordLabel = Node.create("label", {for: "exampleInputPassword1"}, "Password"),
        passwordInput = Node.create("input", {
            type: "password",
            class: "form-control",
            id: "exampleInputPassword1",
            placeholder: "Password"
        }),

        group3 = group1.cloneNode(),
        fileLabel = Node.create("label", {for: "exampleInputFile"}, "File input"),
        fileInput = Node.create("input", {type: "file", id: "exampleInputFile"}),
        fileHelperBlock = Node.create("p", {
            class: "help-block"
        }, "Example block-level help text here."),

        checkbox = Node.create("div", {class: "checkbox"}),
        checkboxLabel = Node.create("label"),
        checkboxInput = Node.create("input", {type: "checkbox"}, "Check me out"),

        submitBtn = Node.create("button", {type: "submit", class: "btn btn-default"}, "Submit");

    Node.append([emailLabel, emailInput], group1);
    Node.append([passwordLabel, passwordInput], group2);
    Node.append([fileLabel, fileInput, fileHelperBlock], group3);

    checkboxLabel.appendChild(checkboxInput);
    checkbox.appendChild(checkboxLabel);

    Node.append([group1, group2, group3, checkbox, submitBtn], form);
    return form;
}

// call the function
var form = generateForm();
document.body.appendChild(form);
```
