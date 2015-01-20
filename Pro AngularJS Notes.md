# Pro Angularjs Notes

I have excluded most of the first 8 chapters as they focused on basics and building a demo application. The demo application is thoroughly commented and documented as a replacement for this written documentation. 

## Chapter 9 - Anatomy of an AngularJS App

Problem | Solution | Listing
-------|----------|--------
Create an AngularJS Module | Use the angular.module method | 1,2 
Set the scope of a module | Use the ng-app attribute | 3
Define a controller | use the Module.controller method | 4,8
Apply a controller to a view | use the ng-controller attribute | 5.7
Pass data from a controller to a view | Use the $scope service | 6
Define a directive | Use the Module.directive method | 9
Define a filter | use the Module.filter method | 10
Use a filter programatically | use the $filter service | 11
Define a service | Use the Module.service, Module.factory or Module.provider method | 12
Define a service from an existing object or value | Use the Module.value method | 13
Add stricture to the code in an application | Create multiple modules and declare dependencies from the module referenced by the ng-app attribute | 14-16
Register functions that are called when modules are loaded | use Module.config and Module.run methods | 17


### Working with modules

Three main roles:

* To associate an angular application with a region of an HTML document
* To act as a gateway to key angular framework features
* To help organize the code and components in an angular application

### Members of the Module object

Name | Description
-----|-----------
animation(name, factory) | Supports the animation feature
config(callback) | Registers a function that can be used to configure a module when it is loaded.
constant(key,value) | Defines a service that returns a constant value
controller(name, constructor) | Creates a controller
directive(name,factory) | Creates a directive which extends the standard HTML vocabulary
factory(name,provider) | Creates a service
filter(name, factory) | Creates a filter that formats data for display to  the user.
provider(name,type) | Creates a service
name | returns the name of the module
run(callback) | Registers a function that is invoked after AngularJS has loaded and configured all of the modules.
service(name, constructor) | Creates a service
value(name,value) | Defines a services that returns a constant value

### Working with the Module Life Cycle

The Module.config and Module.run methods register functions that are invoked at key moments in the life cycle of an Angular app. A function passed to the config method is invoked when the current module has been loaded, and a function passed to the run method is invoked when all modules have been loaded.

## Chapter 10 - Using Binding and Template Directives

Problem | Solution | Listing
--------|--------|-----------
Create a one-way binding | Define properties on the controller $scope and use the ng-bind or ng-bind-template directive or inline expressions (denoted by the {{ }}) | 1-2
Prevent angular from processing inline binding expressions | Use the ng-non-bindable directive | 2
Create two-way data bindings | Use the ng-model directive | 3
Generate repeated elements | Use the ng-repeat directive | 4-6
Get context information about the current object in an ng-repeat directive | Use the built-in variables provided by the ng-repeat directive, such as $first, $last, $index | 7-9
Repeat multiple top-level attributes | Use the ng-repeat-start and ng-repeat-end directives | 10
Load a partial view | use the ng-include directive | 11-16
Conditionally display elements | Use the ng-switch directive | 17
Hide inline template expressions while Angular is processing content | Use the ng-cloak directive | 18

Example Skeleton:
```html
<!DOCTYPE html>
<html ng-app="exampleApp">

<head>
    <title>Directives</title>
    <script src="angular.js"></script>
    <link href="bootstrap.css" rel="stylesheet" />
    <link href="bootstrap-theme.css" rel="stylesheet" />
    <script>
    angular.module("exampleApp", [])
        .controller("defaultCtrl", function($scope) {
            $scope.todos = [{
                action: "Get groceries",
                complete: false
            }, {
                action: "Call plumber",
                complete: false
            }, {
                action: "Buy running shoes",
                complete: true
            }, {
                action: "Buy flowers",
                complete: false
            }, {
                action: "Call family",
                complete: false
            }];
        });
    </script>
</head>

<body>
    <div id="todoPanel" class="panel" ng-controller="defaultCtrl">
        <h3 class="panel-header">To Do List</h3>
        Data items will go here...
    </div>
</body>

</html>

```

### The Data Binding Directives

Directive | Applied As | Description
----------|-----------|----------
ng-bind | Attribute, class | Binds the innerText property of an HTML element
ng-bind-html | Attribute, class | Creates data bindings using the innerHtml property of an HTML element. This is potentially dangerous because it means the browser will interpet content as HTML, rather than content. 
ng-bind-template | Attribute, class | Similar to the ng-bind directive but allows for multiple template expressions to be specified in the attribute value
ng-model | Attribute, class | Creates a two-way binding
ng-non-bindable | Attribute, class | Declares a region of content for which data binding will not be perfomed. 

#### Creating a one-way data binding

```html
...
<div id="todoPanel" class="panel" ng-controller="defaultCtrl">
	<h3 class="panel-header">To Do List</h3>
	<div>There are {{todos.length}} items</div>
	<div>
		There are <span ng-bind="todos.length"></span> items
	</div>
	<div ng-bind-template=
		"First: {{todos[0].action}}. Second: {{todos[1].action}}">
	</div>
	<div ng-non-bindable>
		AngularJS uses {{ and }} characters for templates
	</div>
</div>
...
```

#### Creating Two-Way Data Bindings

```html
...
<div id="todoPanel" class="panel" ng-controller="defaultCtrl">
    <h3 class="panel-header">To Do List</h3>
    <div class="well">
		<!-- inline one-way binding -->
        <div>The first item is: {{todos[0].action}}</div>
    </div>
    <div class="form-group well">
        <label for="firstItem">Set First Item:</label>
        <input name="firstItem" class="form-control" ng-model="todos[0].action" />
    </div>
</div>
...
```

### Using the Template Directives

Directive | Applied As | Description
----------|-----------|----------
ng-cloak | Attribute, class | Applies a CSS style that hides inline binding expressions, which can be briefly visible when the document first loads
ng-include | Element, attribute, class | Loads, processes, and inserts a fragment of HTML into the DOM
ng-repeat | Attribute, class | Generates new copies of a single element and its contents for each object in an array or property on an object
ng-repeat-start | Attribute, class | Denotes the start of a repeating section with multiple top-level elements
ng-repeat-end | Attribute, class | Denotes the end of a repeating section with multiple top-level elements
ng-switch | Element, attribute | Changes the elements in the DOM based on the value of data bindings

#### Repeating multiple top-level elements

```html
...
<table class="table">
    <tbody>
        <tr ng-repeat-start="item in todos">
            <td>This is item {{$index}}</td>
        </tr>
        <tr>
            <td>The action is: {{item.action}}</td>
        </tr>
        <tr ng-repeat-end>
            <td>Item {{$index}} is {{$item.complete? '' : "not "}} complete</td>
        </tr>
    </tbody>
</table>
...
```
#### Dynamically loading a view

The ng-include attribute can take a function to dynamically load an html fragment based on some runtime value. This makes it really nice for toggling display formats. 

```html
<!DOCTYPE html>
<html ng-app="exampleApp">

<head>
    <title>Directives</title>
    <script src="angular.js"></script>
    <link href="bootstrap.css" rel="stylesheet" />
    <link href="bootstrap-theme.css" rel="stylesheet" />
    <script>
    angular.module("exampleApp", [])
        .controller("defaultCtrl", function($scope) {
            $scope.todos = [{
                action: "Get groceries",
                complete: false
            }, {
                action: "Call plumber",
                complete: false
            }, {
                action: "Buy running shoes",
                complete: true
            }, {
                action: "Buy flowers",
                complete: false
            }, {
                action: "Call family",
                complete: false
            }];
            $scope.viewFile = function() {
                return $scope.showList ? "list.html" : "table.html";
            };
        });
    </script>
</head>

<body>
    <div id="todoPanel" class="panel" ng-controller="defaultCtrl">
        <h3 class="panel-header">To Do List</h3>
        <div class="well">
            <div class="checkbox">
                <label>
                    <input type="checkbox" ng-model="showList">Use the list view
                </label>
            </div>
        </div>
        <ng-include src="viewFile()"></ng-include>
    </div>
</body>

</html>

```

#### Conditionally Swapping Elements

Can also use the ng-switch statement to dynamically render specific views. 

```html
...
<div id="todoPanel" class="panel" ng-controller="defaultCtrl">
    <h3 class="panel-header">To Do List</h3>
    <div class="well">
        <div class="radio" ng-repeat="button in ['None', 'Table', 'List']">
            <label>
                <input type="radio" ng-model="data.mode" value="{{button}}" ng-checked="$first" />{{button}}
            </label>
        </div>
    </div>
    <div ng-switch on="data.mode">
        <div ng-switch-when="Table">
            <table class="table">
                <thead>
                    <tr>
                        <th>#</th>
                        <th>Action</th>
                        <th>Done</th>
                    </tr>
                </thead>
                <tr ng-repeat="item in todos" ng-class="$odd ? 'odd' : 'even'">
                    <td>{{$index + 1}}</td>
                    <td ng-repeat="prop in item">{{prop}}</td>
                </tr>
            </table>
        </div>
        <div ng-switch-when="List">
            <ol>
                <li ng-repeat="item in todos">
                    {{item.action}}
                    <span ng-if="item.complete">(Done)</span>
                </li>
            </ol>
        </div>
        <div ng-switch-default>
            Select another option to display a layout
        </div>
    </div>
</div>
...
``` 

#### When to use ng-switch vs ng-include

ng-include is better suited for complex content because it allws you to encapsulate full-blown partial views in an external file. Use ng-switch if the blocks of content are simpler and if they are always visible (because they will always be part of the html document). 

### Hiding Unprocessed Inline Template Binding Expressions

Place the ng-cloak attribute on the content that you want angular to hide until the data is bound.

## Chapter 11 - Using Element and Event Directives

Problem | Solution
-------|--------
Show or hide elements | Use the ng-show and ng-hide directives
Remove elements from the DOM | Use the ng-if directive
Avoid the transclusion problem when generating elements that can't have an immediate parent element | Use the ng-repeat directive with a filter
Assign elements to classes or set individual CSS style properties | use ng-class or ng-style directive
Assign different classes to odd and even elements generated by ng-repeat directive | use the ng-class-odd and ng-class-even directives
Define behavior to be performed when an event is triggered | Use an event directive such as ng-click
Handle an event for which angular does not provide a directive | create a custom event directive
Apply boolean attribtues to elements | Use one of the boolean attribute directive, such as ng-checked

Example baseline:

```html
<!DOCTYPE html>
<html ng-app="exampleApp">

<head>
    <title>Directives</title>
    <script src="angular.js"></script>
    <link href="bootstrap.css" rel="stylesheet" />
    <link href="bootstrap-theme.css" rel="stylesheet" />
    <script>
    angular.module("exampleApp", [])
        .controller("defaultCtrl", function($scope) {
            $scope.todos = [{
                action: "Get groceries",
                complete: false
            }, {
                action: "Call plumber",
                complete: false
            }, {
                action: "Buy running shoes",
                complete: true
            }, {
                action: "Buy flowers",
                complete: false
            }, {
                action: "Call family",
                complete: false
            }];
        });
    </script>
</head>

<body>
    <div id="todoPanel" class="panel" ng-controller="defaultCtrl">
        <h3 class="panel-header">To Do List</h3>
        <table class="table">
            <thead>
                <tr>
                    <th>#</th>
                    <th>Action</th>
                    <th>Done</th>
                </tr>
            </thead>
            <tr ng-repeat="item in todos">
                <td>{{$index + 1}}</td>
                <td ng-repeat="prop in item">{{prop}}</td>
            </tr>
        </table>
    </div>
</body>

</html>
```

### Handling Events

The Event Directives

Directive | Applied As | Description
-------|----------|----------
ng-blur | Attribute, class | Specifies a custom behavior for the blur event, which is triggered when an element loses the focus
ng-change | Attribute, class | Specifies a custom behavior for the change vent, which is tirggered by form elements when their state of content is changed
ng-click | Attribute, class | Specifies a custom behavior for the click event, which is triggered when the user clicks the mouse
ng-copy / ng-cut / ng-past | Attribute, class | Specifies a custom behavior for the copy, cut and paste events
ng-dblclick | Attribute, class | Specifies a custom behavior for the dblclick event
ng-focus | Attribute, class | Specifies a custom behavior for the focus event, which is triggered when an element gains focus
ng-keydown / ng-keypress / ng-keyup | Attribute, class | Specifies a custom behavior for when the user presses/release a key
ng-mousedown / ng-mouseenter / ng-mouseleave / ng-mousemove / ng-mouseover / ng-mouseup | Attribute, class | Specifies custom behavior for the six standard mouse events
ng-submit | Attribute, class | Specifies a custom behavior for the submit event, which is triggered when the form is submitted. 

There is also an angular touch module that specifies event bindings for touch events and gestures.

### Creating a custom event directive

```html
<!DOCTYPE html>
<html ng-app="exampleApp">

<head>
    <title>Directives</title>
    <script src="angular.js"></script>
    <link href="bootstrap.css" rel="stylesheet" />
    <link href="bootstrap-theme.css" rel="stylesheet" />
    <script>
    angular.module("exampleApp", [])
        .controller("defaultCtrl", function($scope, $location) {
            $scope.message = "Tap Me!";
        }).directive("tap", function() {
            return function(scope, elem, attrs) {
                elem.on("touchstart touchend", function() {
                    scope.$apply(attrs["tap"]);
                });
            }
        });
    </script>
</head>

<body>
    <div id="todoPanel" class="panel" ng-controller="defaultCtrl">
        <div class="well" tap="message = 'Tapped!'">
            {{message}}
        </div>
    </div>
</body>

</html>
```

### Managing Boolean Attributes

Some attributes, like 'disabled', do not derive meaning from their attribute value, but are meaningful just by the presence of the attribute. Angular provides built-in directives to handle this:

Directive | Applied As | Description
---------|----------|------------
ng-checked | Attribute | Manages the checked attribute (used on input elements)
ng-disabled | Attribute | Manages the disabled attribute (used on input and button elements)
ng-open | Attribute | Manages the open attribute (used on details elements)
ng-readonly | Attribute | Manages the readonly attribute (used on input elements)
ng-selected | Attribute | Manages the selected attribute (used on option elements)

### Managing Other Attributes

 Directive | Applied As | Description
---------|----------|------------
ng-href | Attribute | Sets the href attribute on a elements
ng-src | Attribute | Sets the src attribute on img elements
ng-srcset | Attribute | Sets the srcset attribute on img elements. The srcset attribute is a draft standard to extend HTML5, allowing for multiple images to be specified for different display sizes and pixel densities. Browser support is limited as I write this.

These directives allow AngularJS data bindings to be used to set the value of the attribute they correspond to and, in the case of the  ng-href directive, prevent the user from being able to navigate to the wrong destination by clicking the link before AngularJS has processed the element.

## Chapter 12 - Working with Forms

