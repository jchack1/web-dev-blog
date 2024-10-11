---
layout: post
title: "JavaScript modules - CommonJS vs ES modules"
date: 2024-10-10 16:30
tags: javascript
navigation: true
class: post-template
current: post
---

### Why do we need modules in JavaScript?

There are a lot of good reasons to separate our code into modules.

Each module should have a specific use. This makes the code reusable across your whole application, and easier to maintain and debug. This can also help you to avoid repetition in your code.

Another reason is we don't want to add all of our variables to the global namespace. You could end up having variables of the same name conflicting with one another. Each module has its own scope, with variables and functions that are not visible to outside code unless specifically exported.

### Different types of module solutions

Modules in JavaScript evolved over time, so developers came up with different solutions independently. We'll talk about a couple popular solutions here.

#### CommonJS

Whenever you see `module.exports` and `require()` used together it is from CommonJS. You will see this in Node.js, but not in the browser.

`require()` is used to import code from another file, which was exported using `module.exports`. Multiple things can be exported from a module using an object.

With CommonJS, modules are loaded <em>synchronously</em>, meaning the whole module needs to be loaded before the code can execute.

#### ES Modules

This is being used when you see `export default` and `import` in an app.

Instead of `require` we use `import`, and instead of `module.exports` we use `export default`.

ES modules are used mainly in the browser, but can be used server-side as well. It seems as though the goal is to make ESModules the standard for both server and browser as time goes on. In your HTML, you can import modules in your script tag, as long as you specify the `type` attribute to `module`.

```html
<script src="myModule.js" type="module"></script>
```

Sometimes you can get CORS errors if you try to open your HTML page as a file in your browser. You can use the npm package "serve" to host your page locally. Also, if you are using a framework with ES6 modules, this shouldn't be an issue you need to fix yourself.

ES6 Modules also offer the ability to load modules asynchronously through dynamic imports. They can be used like this:

```js
if (someCondition) {
  const {myFunction} = await import("/my-module.js");

  myFunction();
}
```

### Helpful links

- [https://blog.logrocket.com/commonjs-vs-es-modules-node-js/](https://blog.logrocket.com/commonjs-vs-es-modules-node-js/)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
- [https://www.youtube.com/watch?v=d-0uCi61rtg](https://www.youtube.com/watch?v=d-0uCi61rtg)
