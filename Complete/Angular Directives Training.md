# Angular Directive Training 

> given by Jason Dalton

Directives enable the ability to create custom DOM elements and interactions. In Angular, any time you do DOM manipulations, you should do it from within directives, not from within services or controllers. This is because inside directives, the elements and attributes are injected for you and all you to test them. 

He uses the Yeoman generator to maintain the structure of his application. 

### IE Gotchas

Because we are supporting IE7-8, they do not support custom DOM elements. There are a couple ways to get around this:

* change the restrict property from 'E' to 'A', and make the directive an attribute of a div.
* use replace option and set to true, it will replace your custom element with a div. This helps with IE7/8.
	* They say this is being deprecated, but will not remove it any time soon, probably not until version 2 which is expected to drop support for IE7/8.

In IE7, only certain attributes are recognized. You can get around this by including data-* or x-* in front of your directive. data-*/x-* will be stripped from the beginning of the directive in the normalization process, which also includes coverting :,-,_ to delimited names to camelCase. 


### How directives work

Angular goes through a page at the beginning of the event cycle and grab all directives it can find, put them in an array, order them by priority, and then compile the directives. Compilation involves doing any DOM manipulation it needs to do. Once compilation finishes, it will attach scopes to the directives, and link data binders and watchers to the directives. 

Angular exposes a 'compile' method, that takes a function:

```javascript
angular.module("jdApp")
	.directive('gridBox', function(){
		return {
			templateUrl: 'views/gridbox.html',
			replace: true,
			restrict: "EACM",
			compile: function(element, attrs, transclude){
				console.log('in the compile');
				return function(scope, element, attrs){
					console.log('in the compile linking');
				};
			},
			link: function postLink(scope, element, attrs){
				console.log('in the link function');
			}
		}
	});
```

If you execute this you will notice that the link function does not get called. This is because when you specify a compile function, the function that it returns is your override for the link function, and angular will use that instead. 

Note: if you have two grid-box elements on a page, the compile function will be called for each instance of the element. HOWEVER, if you are rendering directives within in ng-repeat, it will clone the compile function (thus only calling it once) and only do the linking (which is required no matter what) for each of the elements. So when using ng-repeat, Angular effectively considers it one DOM element. But because each element could have different data, the link function is still called for each.

#### Why use the compile function? 

99.9% of the time you will not need to use it. If you need to do DOM manipulation BEFORE you link your data to your element. But for most of the time, you do not need compile and he does not recommend using it. 


### Creating a custom directive

Restrict can take any of the following values:

* A - matches attribute name
* E - matches element name
* C - matches class name
* These can be chained together, AEC, to match any/all.
* M - He also mentions a 'message' where it will match on a comment but doesn't recommend it. 

Templates can be specified in-line or you can give it a template URL.

```javascript
angular.module("jdApp")
	.directive('gridBox', function(){
		return {
			templateUrl: 'views/gridbox.html',
			replace: true,
			restrict: "EACM",
			link: function postLink(scope, element, attrs){
				scope.myText = attrs.text;
			}
		}
	});
```

```html
<!-- grid-box view -->
<div>
	<input type='text' ng-model="myText" />
	<span>{{myText}}</span>
</div>
```
```html
<!-- layout view -->
<div>
	<grid-box text='Queen'></grid-box>
</div>
```

Note how we can use the attribute on the element when it is injected into the link function.

A problem with this implementation is that if you want to include multiple grid-box's on a page, the scope is shared. So both would show the binded value for 'text'. 

#### Scope Isolation

You can create a scope object to ensure that each directive has it's own copy of the scope:

```javascript
angular.module("jdApp")
	.directive('gridBox', function(){
		return {
			templateUrl: 'views/gridbox.html',
			replace: true,
			scope: {},
			restrict: "EACM",
			link: function postLink(scope, element, attrs){
				scope.myText = attrs.text;
			}
		}
	});
```

You can also tell the scope how to populate itself. For example, instead of binding myText in our link function we can do it by:

```javascript
angular.module("jdApp")
	.directive('gridBox', function(){
		return {
			templateUrl: 'views/gridbox.html',
			replace: true,
			scope: {
				myText: '@text'
			},
			restrict: "EACM",
			link: function postLink(scope, element, attrs){
			}
		}
	});
```
@ tells angular to look for an attribute named text, and if it finds it, bind it to myText. Note that this is a one-way binding. If you want the binding to be two-way then use the '=' symbol:

```javascript
angular.module("jdApp")
	.directive('gridBox', function(){
		return {
			templateUrl: 'views/gridbox.html',
			replace: true,
			scope: {
				myText: '=text'
			},
			restrict: "EACM",
			link: function postLink(scope, element, attrs){
			}
		}
	});
```

#### Scope Functions

You can create functions off of the scope that can then be bound on the view. 

```html
<!-- grid-box view -->
<div>
	<input type='text' ng-model="myText" />
	<span ng-click="clickMe">{{myText}}</span>
</div>
```

```javascript
angular.module("jdApp")
	.directive('gridBox', function(){
		return {
			templateUrl: 'views/gridbox.html',
			replace: true,
			scope: {
				myText: '=text'
			},
			restrict: "EACM",
			link: function postLink(scope, element, attrs){
				scope.clickMe = function(){
					console.log('click');
				};
			}
		}
	});
```

We can also bind a listener to the element itself:

```javascript
angular.module("jdApp")
	.directive('gridBox', function(){
		return {
			templateUrl: 'views/gridbox.html',
			replace: true,
			scope: {
				myText: '=text'
			},
			restrict: "EACM",
			link: function postLink(scope, element, attrs){
				scope.clickMe = function(){
					element.on('click'), function(){
						console.log('click');
					});
				};
			}
		}
	});
```
There may be cases where you want to observe data. With this, anytime the specified attribute changes your function will be called:

```javascript
angular.module("jdApp")
	.directive('gridBox', function(){
		return {
			templateUrl: 'views/gridbox.html',
			replace: true,
			scope: {
				myText: '=text'
			},
			restrict: "EACM",
			link: function postLink(scope, element, attrs){
				attrs.observe('text', function(){
					console.log('text changed'); 
				});
			}
		}
	});
```

This is different than a watch, because a watch is on a scope item. Observe is for anything that has been passed into the directive that is not attached to the scope. 

[Stack Overflow Explanation](http://stackoverflow.com/questions/14876112/difference-between-the-observe-and-watch-methods). $observe can watch for changes on a DOM attribute and is only used/called inside directives. Use $watch for everything else. $watch is more complicated because it can observe an 'expression', where the expression is a function or a string. It is generally used inside controllers or inside a linking function in a directive -- basically anything that gets a scope object. 

### Transclude

This will allow you to compile content within the directive and include it within the template. The content will be placed inside of the element that has the ng-transclude attribute. Note that we also have access to the transclusion in the link function. 

```html
<!-- grid-box view -->
<div>
	<input type='text' ng-model="myText" />
	<span ng-click="clickMe">{{myText}}</span>
	<div ng-transclude></div>
</div>
```
```html
<!-- layout view -->
<div>
	<grid-box text='Queen'>
		<h1>HELLO!</h1>
	</grid-box>
</div>
```
```javascript
angular.module("jdApp")
	.directive('gridBox', function(){
		return {
			templateUrl: 'views/gridbox.html',
			replace: true,
			scope: {
				myText: '=text'
			},
			transclude: true,
			restrict: "EACM",
			link: function postLink(scope, element, attrs, trans){
				attrs.observe('text', function(){
					console.log('text changed'); 
				});
			}
		}
	});
```

### Directive Type

You can specify a type, by default it is HTML. But you can also specify SVG and math. Template enforces that there is only one parent (opening and closing tag). 

* HTML - must be valid html with one parent
* SVG - must be valid SVG
* Math - must be math expression

### Directive - require

You can enforce that some directives depend on one another. 

### Directive - controller function

Allows you to communicate between directives, it is shared between nested elements. 

## TRTA Custom Directives

### Split Pane

Binders/Charts both are using the split pane. It is a slider that you can move back and forth to re-size panes. The template itself consists of a left, right and middle pane. They are driven by Bento.  

This code leverages the transclude function. We loop through the parent elements, looking for classes for 'leftPane', etc. If we find it, then we append that content to our template. We do the same for the content pane and right pane. This gives us the ability to have html tags inside of our element that are not transcluded into one spot. We can pick out specific pieces and place them where we need, it is essentially manual transclusion. 

We do not isolate the scope for this directive because there should only be one split pane -- it is a layout directive. We should not need to worry about two or more of them. 

It also exposes the ability to assign expand and collapse callback functions. 

### Tree Navigation

This works in tandem with ng-repeat. We load 200 node items and the ng-repeat will stop. When this directive is watching the scope item called 'last', when 'last' gets called, it will call the loadMore function. It will change the limit variable and add another 200 to it. This highlights how we can use observables to dynamically load content for performance reasons. There are a ton of directives that don't display anything (ng-click, ng-drag, etc) and are just there for interaction/watching purposes. 

We do not have a template property for this. This is because this is a recursive element. We have a treeNavigationData variable that contains the data and we use this to generate the HTML dynamically. parentElementString is the recursive function responsible for generating the html. We call the $compile function manually on this to compile the html string into the angular equivalent, then we append it to our html. This gives us the ability to manipulate our DOM objects or HTML strings before we actually compile it. 

We also handle the on destroy to clean up the tree. We also have callback functions defined. 

The treeSelection factory lets us know which elements we have and have not selected. 

### OIT Menu

We do create a unique scope for this directive. The @ signs indicate we are only doing one-way binding (the parents will not get notified of our changes). 

Whenever we pass in a current item as an attribute, we will observe it. 





