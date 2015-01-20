# Intro To Web Components Training

Web components are a collection of standards to help bundle markup and styles into custom HTML elements. This makes your components fully encapsulated, hiding your html from external Javascript and keep your HTML safe.

## 4 Elements of Web Components

* Templates (Support by all but IE)
* Custom Elements (Firefox 31 and Chrome)
* Shadow DOM (Chrome and Opera)
* HTML Import (Chrome)

All can be used with polyfills.

### Templates

Template is an element that contains markup intended to be used later. The element is parsed by the parser but images and links are not downloaded and the element is not rendered.

```html
<template id="myTemplate">
	<input>
	<img src="../image.jpg">
</template>
```
Performance implications because you can defer 

### Custom Elements

Custom elements are new types of DOM elements that can be defined by authors. Custom elements can encapsulate state and provide script interfaces

```html
<element extends="button" name="fancy-button">
	<script>
	({
		tick: function() {}
	});
	</script>
</element>
```

### Shadow DOM

Shadow DOM provides us an elegant way to overlay the normal DOM subtree with a special document fragment that contains another subtree of nodes, which are impregnable to scripts and styles. This means that any styles that you declare in the shadow root cannot be overridden. 

Shadow DOM is already in all of the browsers, but it is a private API that developers do not have access to. 

### HTML Import

HTML Imports, or just imports from here on, are HTML documents that are linked as external resources from another HTML document. You're not limited to markup either. An import can also include CSS, Javascript, or anything else an html file can contain. In other words, this makes imports a fantastic tool for loading related HTML/CSS/JS

```html
<link rel="import" href="bootstrap.html">
```

## Why should we care?

* This is the future of web development. As they mature you will find more and more shops supporting and using web components. 
* Angular 2.0 is working to integrate these parts -- http://ng-learn.org  
	* This means that writing directives moving forward will probably be web components compatible
* Google is using Polymer for its new material look and feel. 
* Angular and Polymer teams are working together to collaborate on custom elements (Polymer is an API on top of web components that simplify their development)
