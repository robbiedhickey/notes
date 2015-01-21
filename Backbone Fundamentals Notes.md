# Backbone.js Fundamentals Notes

## Introduction

Backbone is not MVC and not a framework, it has its own pattern. It is designed to give structure to modern web applications and prioritizes flexibility. 

### SPA Challenges

* Lack of tooling and experience
* Search engine optimization
* Working with different browsers

### Why Backbone?

Backbone provides set of tools for introducing structure into client-side applications. 

### Required Dependencies

* Underscore.js
	* functional programming support for javascript
* jQuery/ zepto
	* DOM manipulation and ajax
	* zepto is only for modern browsers

### A Minimal Backbone Environment

> JSFiddle: http://jsfiddle.net/ynkJE/

```html
<!DOCTYPE html>
<html>
	<head>
		<title>title</title>
		<script src="//cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
		<script src="//cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
		<script src="//cdnjs.cloudflare.com/ajax/libs/backbone.js/1.1.2/backbone-min.js"></script>
	</head>
	<body></body>
</html>
```
Note that backbone models do not store their properties directly on the object, have to use get and set methods.

### A Backbone Example

#### Specs

* Render rectangles on a canvas
* Rectangles will be represented as Backbone models
	* A rectangle has a width and height
	* A rectangle has a position
	* A rectangle has a color
* A Backbone view will have responsibility for rendering a rectangle model

#### The code

```html
<!DOCTYPE html>
<html>
	<head>
		<title>Rectangles</title>
		<script src="//cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
		<script src="//cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
		<script src="//cdnjs.cloudflare.com/ajax/libs/backbone.js/1.1.2/backbone-min.js"></script>
		<style type="text/css">
			.rectangle { 
				position: absolute;
				border: 4px solid black;
			}
		</style>
	</head>
	<body>
		<h1>Rectangles</h1>
		<div id="canvas"></div>
	</body>

	<script type="text/javascript" src="rectangles.js"></script>
</html>
```

```javascript
(function(){

	var Rectangle = Backbone.Model.extend({});

	var RectangleView = Backbone.View.extend({
		tagName: 'div',
		className: 'rectangle',

		events: {
			'click': 'move'
		},
		render: function(){
			this.setDimensions();
			this.setPosition();
			this.setColor();
			return this;
		},
		setDimensions: function(){
			this.$el.css({
				width: this.model.get('width') + 'px',
				height: this.model.get('height') + 'px'
			});
		},
		setPosition: function(){
			var position = this.model.get('position');
			this.$el.css({
				left: position.x,
				top: position.y
			});
		},
		setColor: function(){
			this.$el.css('background', this.model.get('color'));
		},
		move: function(){
			this.$el.css('left', this.$el.position().left + 10);
		}
	});

	var models = [
		new Rectangle({
			width: 100,
			height: 60,
			position: {
				x: 300, 
				y: 150
			},
			color: 'red'
		}),
		new Rectangle({
			width: 26,
			height: 300,
			position: {
				x: 500, 
				y: 75
			},
			color: 'yellow'
		}),
		new Rectangle({
			width: 300,
			height: 70,
			position: {
				x: 310, 
				y: 200
			},
			color: 'blue'
		}),

	];

	_(models).each(function(model){
		$('div#canvas').append(new RectangleView({ model: myRectangle }).render().el);
	});
})();
```

## Models

### The purpose of models

Models are the foundation of your user interface

* Models form the core of your application
* They contain your application's state as well as logic and behavior
* Models are the single point of truth for data
* Models provide a lifecycle
* Models communicate changes to the rest of the application via events

### Defining new model types

* Create new Model 'types' by extending Backbone.Model

```javascript
var Vehicle = Backbone.Model.extend({
	make: 'Ford'
});

var v1 = new Vehicle();
var v2 = new Vehicle({ make: 'Chevy' });
```
We use uppercase for type names (constructor functions). extend() is a function shared by Model, Collection, Router and View. It establishes an ineritance relationship between two objects. 

* Model types can have 'class properties' as well (available to all instances)

```javascript
var Vehicle = Backbone.Model.extend(
	{},
	{
		summary: function(){
			return 'Vehicles are for traveling';
		}
	}
);

Vehicle.summary();
```

### Instantiating models

* To create a new model object, call its constructor function with the 'new' operator
* The simplest case is to create an instance of Backbone.Model

```javascript
var model = new Backbone.Model();
```

* Or use custom types

```javascript
var Vehicle = Backbone.Model.extend({});
var ford = new Vehicle();
```

* Instantiate with property values

```javascript
var model = new Backbone.Model({
	name: 'Peter',
	age: 52
});
```

#### Initialize

* If a model has an 'initialize' function defined it will be called when the model is instantiated

```javascript
var Vehicle = Backbone.Model.extend({
	initialize: function(){
		console.log('vehicle created');
	}
});

var car = new Vehicle();
```

### Model inheritance

* Models can inherit from other models

```javascript
var Vehicle = Backbone.Model.extend({});
var Car = Vehicle.extend({});
```

### Model attributes

* Attributes can be set by passing an object to a model type's constructor, or by using the 'set' method.

```javascript
var ford = new Vehicle();
ford.set('type', 'car');
```

* Or set many properties at once

```javascript
ford.set({
	'maxSpeed': '99',
	'color': 'blue'
});
```

* Read attributes with the 'get' method

```javascript
ford.get('type')
// car
```

* 'Escape' is like 'get' except that the output is html escaped

```javascript
ford.set('description', '<script>alert("script injection")</script>');
ford.escape('description'); 
$('body').append(ford.escape('description')); // alert won't fire
```

* Use 'has' to test if an attribute has been defined

```javascript
var ford = new Vehicle();
ford.set('type', 'car');
ford.has('type'); //true
ford.has('year'); //false
```

### Model events

One of the most valuable features of models. Events are why we need to use 'set' and 'get' functions. 

* Models raise events when their state changes
* To detect a change to a model, listen for the 'change' event. 

```javascript
ford.on('change', function(){});
```

* Or, listen to a change to a property using Event Namespacing

```javascript
ford.on('change:color', function(){});
```

#### Custom model events

* It is possible to define, trigger and observe custom model events
* Events are identified by string identifiers
* Use the 'on' method to bind to an event

```javascript
ford.on('retired', function(){});
```

* Use the 'trigger' method to trigger an event

```javascript
ford.trigger('retired');
```

#### Custom model event example

```javascript
var volcano = _.extend({}, Backbone.Events);

//register event listener
volcano.on('disaster:eruption', function(options){
	console.log('duck and cover - ' + options.plan);
});

//trigger event
volcano.trigger('disaster:eruption', {plan: 'run'});

//unsubscribe
volcano.off('disaster:eruption');
```

### Model identity

* The 'id' property represents the model's persistent identity. It is undefined until the model has been saved. 

```javascript
var ford = new Vehicle();
ford.id; //undefined
```

* the 'cid' property is a temporary identifier used until a model is assigned its id. 

```javascript
ford.cid; // c0
```

* Models also have an 'isNew()' property that can test whether a model has been saved to the server. 

```javascript
var ford = new Vehicle();
ford.isNew(); // true
```

### Defaults

* The 'defaults' property specifies default values for attributes that are not set in the constructor
* Also has the additional benefit of documenting a model type's properties

```javascript
var Vehicle = Backbone.Model.extend({
	defaults: {
		'color': 'white',
		'type': 'car'
	}
});
var car = new Vehicle();
car.get('color'); // white
car.get('type'); // car
```

### Validation

* Backbone exposes model validity through two methods:
	* validate - returns collection of model errors
	* isValid - returns boolean
* Validate is called by backbone prior to performing 'save' operations
	* **Backbone requires the {validate: true} option to be passed with 'set' to trigger validation**

```javascript
var Vehicle = Backbone.Model.extend({
	validate: function(attrs){
		var validColors = ['white', 'red', 'blue', 'yellow'];
		var colorIsValid = function(attrs){
			if(!attrs.color) return true;
			return _(validColors).include(attrs.color);
		}

		if(!colorIsValid(attrs)){
			return "color must be one of: " + validColors.join(",");
		}
	}
});

var car = new Vehicle();
car.on('invalid', function(model, error){
	console.log(error);
});

car.set('color', 'turqoise', {validate: true}); // will trigger invalid event
color.get('color'); // undefined
```

### toJSON

* Converts a model's attributes to a javascript object (NOT A STRING!)

```javascript
var ford = new Vehicle();
ford.set('type', 'car');
ford.toJSON(); // {type:'car'}
```

### Save, Fetch, Destroy

Details of server integration covered later in course

* Models have save, fetch and destroy methods for synchronizing with the server. 
* Save performs insert and update operations, depending upon the state of the model.
* Fetch updates the model with the server-side state.
* Destroy deletes the model from the server

## Views

Interface in both directions between html document and Backbone models/collections.

* Views provide the 'glue' between models and the document.
* They handle events. Events from the DOM and the Model propogate to the View
* Can define new View types by extending Backbone.View

```javascript
var VehicleListView = Backbone.View.extend({
	// properties
});
```

* All views have an associated DOM element at all times (.el)
* Some views create new elements. The new element is defined by the id, tagName, className and attributes. 

```javascript
var V = Backbond.View.extend({
	tagName: 'li', 
	id: 'thing',
	className: 'active',
	attributes: {
		'data-value': 12345
	}
});
var v = new V();
$('body').prepend(v.el);
```

* Some views attach to existing elements. Pass a 'el' property to the view's constructor

```javascript
var V = Backbond.View.extend({});
var v = new V({el: '#test'});
v.$el.css('background-color', 'CornflowerBlue');
```

### Instantiating Views

* To create a new view object call its constructor function with the 'new' operator
* The simplest case is to create an instance of Backbone.View

```javascript
var view = new Backbone.View();
```

* Usually you will want to instantiate instances of your own type

```javascript
var VehicleListView = Backbone.View.extend({});
var myView = new VehicleListView();
```

* Often you will pass a odel to the view constructor

```javascript
var myView = new VehicleListView({
	model: myModelObject
});
```

* Any of the following properties will be attached directly to the view object if passed to the constructor:
	* model, collection, el, id, className, tagName, attributes

### The el and $el property

* All views have an 'el' property that references the view's DOM element

```javascript
var v = new Backbone.View({el:'body'});
v.el; // <body></body>
```

* $el is a cached jQuery (or zepto) wrapper around el
	* avoids repeated $(this.el)
* this.$ is the jQuery function scoped to the current view
	* this.$('selector') is equivalent to this.$el.find('selector')

### The render function

* Render is the function that render's the views element (.el)
* The default implementation is a no-op. Provide an implementation with your view definitions. 
* By convention, render functions return 'this', making it easy to chain method calls in a fluent style. 

```javascript
var V = Backbone.View.extend({
	render: function(){
		this.$el.html('some content');
		return this; //convention
	}
});
```

#### Combinig Views and Models

* Pass the model to the view's constructor

```javascript
var v = new View({
	model: myModel
});
```

* Bind the view's render method

```javascript
myModel.on('change', function(){
	$('body').append(v.render().el);
});
```

#### Example

```javascript
var RefreshingView = Backbone.View.extend({
	initialize: function(){
		this.model.on('change', function(){
			this.render();
		}, this);
	}, 
	render: function(){
		this.$el.html(this.model.get('text'));
	}
});

var m = new Backbone.Model({text: new Date().toString()});
var v = new RefreshingView({model: m, el: 'body'});
v.render();

setInterval(function(){
	m.set({text: new Date().toString() });
}, 1000)
```

### The make function

* Sometimes you need a lightweight technique for generating DOM elements without templates, the make function allows this.

```javascript
var el = new Backbone.View().make(
	'h3',
	{class: 'not-very-important'},
	'Preliminary Version'
);

// <h3 class="not-very-important">Preliminary Version</h3>
```
### The remove function

* Remove is a shortcut method to remove the view from the DOM
* Equivalent to jQuery remove method: $el.remove()

```javascript
var h = new Backbone.Model({
	content: 'Historical context'
});

var HeadingView = Backbone.View.extend({
	tagName: 'p', 
	render: function(){
		this.$el.html(this.model.get('content'));
		return this;
	}
});

var v = new HeadingView({model: h});
$('body').append(v.render().el);

v.remove();
```

### Events

* Declarative syntax to register handlers for DOM events
* Syntax is { events: { 'eventName cssSelector': 'handler' } }

```javascript
var FormView = Backbone.View.extend({
	events:{
		'click .clickable': 'handleClick',
	},
	handleClick: function(){}
});
```

* Equivalent to: this.$('.clickable').click(handleClick);

### Guidelines and Summary

* Views should render self-contained DOM elements
	* Do not attach to existing elements
	* Do not access DOM elements the view does not own
* Pass el to the constructor of self-updating view

## Templating

Backbone doesn't natively provide a template engine. You have the flexibility to choose your own. 

### Client-side templating

* Dynamically build markup from a template and some data
* All but the simplest UI elements will require templating
* Templating happens in the view's render method

### Underscore.js templates

* Although backbone doesn't natively provide templates, it does require underscore, and underscore can render templates.
* There are three valid types of code blocks:
	* <% ... %> - execute arbitary code
	* <%= ... %> - evaluate an expression and render the result inline
	* <%- ... %> - evaluate an expression and render the html escaped result inline

```javascript
var V = Backbone.View.extend({
	render: function(){
		var data = { lat: -27, lon: 153 };
		this.$el.html(
			_.template("<%= lat %> <%= lon %>")(data);
		);
		return this;
	}
});

var v = new V();
v.render(); // -27 153
```

* You can also remove the template from the render function and place it in the DOM:

```html
<script id="latlon-template" type="text/html">
	<p><%= lat %></p>
	<p><%= lon %></p>
</script>

<script>
var V = Backbone.View.extend({
	render: function(){
		var data = { lat: -27, lon: 153 };
		var template = $('#latlon-template').html()
		this.$el.html(_.template(template)(data));
		return this;
	}
});
var v = new V({el: 'body'});
v.render();
</script>
```

* Underscore template blocks can contain any javascript expression

```html
<script id="latlon-template" type="text/html">
	<p><%= lat %></p>
	<p><%= lon %></p>
	<% _([1,2,3]).each(function(number) { %>
		<p> <%= number %> </p>
	<% }); %>
</script>
```

### Handlebars templates

* Templating engine based on another templating library called mustache
* Philosophically opposed to code in templates
	* because of this, they only support basic constructs like conditionals and loops
	* anything more complicated is achieved through helper methods
* Code blocks are delimitted by {{ .. }}

```html
<p>{{ lat }}</p>
<p>{{ lon }}</p>
{{ #each numbers }}
	<p> {{ this }} </p>
{{ /each }}
```

* Rendering is a two stage process:
	* Compile
	* Execute

* Compile

```javascript
var source = '<p>Latitude: {{lat}}</p>';
var compiled = Handlebars.compile(source); // outputs template function
```

* Execute

```javascript
compiled({lat: -27}); // <p>Latitude: -27</p>
```

### Pre-compiling templates

* Compiling a template == converting to a function
* Underscore compilation

```javascript
var source = '<p>Latitude: <%= lat %></p>';
var compiled = _.template(source); // pass template source but not data for compilation
```

* Pre-compile templates for performance
* Script compilation as a build step
* Handlebars includes support for pre-compiling file-based templates
	* This requires node and npm to be installed, as well as handlebars module

```bash
handlebars <input> -f <output>
```

#### Pre-compilation example with Handlebars

In the real-world you would set this up on a file system watcher, post build script, etc

```bash
handlebars templates/ -f templates.js
```

```html
<!DOCTYPE html>
<html>
	<head>
		<title>Pre-compilation</title>
		<script src="http://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
		<script src="http://cdnjs.cloudflare.com/ajax/libs/underscore.js/1.7.0/underscore-min.js"></script>
		<script src="http://cdnjs.cloudflare.com/ajax/libs/backbone.js/1.1.2/backbone-min.js"></script>

		<!-- generated by build process-->
		<script src="templates.js"></script>
	</head>
	<body>
		<script>
			$(function(){
				var data = {
					people: ['Mark Twain', 'Eric Blair', 'Salman Rushdie']
				};

				var rendered = Handlebars.templates.list(data); // would map to templates/list.handlebars file
				$('body').html(rendered);
			});
		</script>
	</body>
</html>
```

## Routing

* Client-side routes are a way to trigger a function when the browser url changes
* Backbone routing includes parsing of the url and matching the url to the correct route handler
* **DON'T** use routes like MVC actions
* Each route results in two different scenarios:
	* Routing a browser intiated request
	* Routing a client request 
* Rule of thumb: don't use routes unless you have a significant state transition or bookmarkable scenario

### A document router demo

In this demo we will implemented our first backbone router so you better understand what it does and does not do. 

``` 
Given a set of documents, each having a title and some content
When a user selects a document
Then that document is displayed
```

```html
<!DOCTYPE html>
<html>
	<head>
		<title>Document Router</title>
		<script src="http://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
		<script src="http://cdnjs.cloudflare.com/ajax/libs/underscore.js/1.7.0/underscore-min.js"></script>
		<script src="http://cdnjs.cloudflare.com/ajax/libs/backbone.js/1.1.2/backbone-min.js"></script>
	</head>
	<body>
		<script>
			var documents = [
				new Backbone.Model({
					title:"Javascript Modules",
					content: "why modules? blah blah"
				}),
				new Backbone.Model({
					title:"Module Systems",
					content: "why module systems? blah blah"
				}),
			];

			var eventAggregator = _.extend({}, Backbone.Events);

			var ContentsView = Backbone.View.extend({
				tagName: 'ul',
				render: function(){
					_(this.collection).each(function(document){
						this.$el.append(new DocumentListView({model: document}).render().el);
					}, this);
					return this;
				}
			});

			var DocumentListView = Backbone.View.extend({
				tagName: 'li', 
				events: {
					'click': function(){
						eventAggregator.trigger('document:selected', this.model);
					}
				},
				render: function(){
					this.$el.html(this.model.get('title'));
					return this;
				}
			});

			var DocumentView = Backbone.View.extend({
				render: function(){
					this.$el.append(this.make('h1', null, this.model.get('title')));
					this.$el.append(this.make('div', null, this.model.get('content')));
					return this;
				}
			});

			var DocumentRouter = Backbone.Router.extend({
				routes:{
					'contents': 'contents',
					'view/:title': 'viewDocument'
				},
				contents: function(){
					$('body').html(new ContentsView({collection: documents}).render().el);
				},
				viewDocument: function(title){
					var selectedDocument = _(documents).find(function(document){
						return document.get("title") === title;
					});

					$('body').empty().append(new DocumentView({model: selectedDocument}).render().el)
				}

			});

			eventAggregator.on('document:selected', function(document){
				var urlPath = 'view/' + document.get('title');
				router.navigate(urlPath, {trigger: true});
			});
			var router = new DocumentRouter();
			Backbone.history.start();

			router.navigate('contents', {trigger: true});

		</script>
	</body>
</html>
```

### How to define routes

* Define routes by defining a type that extends Backbone.router

```javascript
var Workspace = Backbone.Router.extend({
	routes: {
		"search/:query": "search"
	}, 
	search: function(query){
		console.log('searched for ' + query);
	}
});
```

### Triggering routes (navigate)

* Navigate is the backbone function for updating the browser's address and triggering routing
* The first parameter is the new url path (after # fragment)
* The second parameter tells Backbone if it should trigger routing

```javascript
var router = new Workspace();
Backbone.history.start();

router.navigate('search/cats', {trigger: true});
```

### HTML5 History pushstate

* The HTML5 history api introduced a way to change the browser url without reloading the page

```javascript
window.history.pushState(...);
```

* Browser support is patchy
	* Currently not supported by any versio of IE
	* http://caniuse.com/ 

### Hash fragments

* If the browser does not support the HTML5 history api Backbone will use hash fragments
* Browsers have always allowed javascript to modify the page url by appending a hash followed by a string

> http://somedomain.com/#search/cats

* The big difference between push state and hash fragments is that has fragments are not sent to the server
	* ex. Given http://localhost/#search/cats -- only http://localhost/ is sent to server

### Search engine indexability

* Markup that is rendered on the client will not be indexed by search engines.
* Option 1: render content on the server
* Option 2: #! URLs ( need to disable pushState )

```javascript
Backbone.history.start({ pushState: false });
router.navigate('!search/cats', {trigger: true});

// generated url -> /#!search/cats
// google converts to -> /_escaped_fragment_=search/cats
```

* Hashbang (#!) is largely obsolete due to html5 history api. 

## Collections

Group models together, and provides a set of functions to manipulate them.

* Container for multiple models of the same type
* Retrieve models from the server
* Create models and save them to the server
* Group models by some attribute
* Collection is an array-like object (cannot use [], must use at() method)

```javascript
var c = new Backbone.Collection({
	{name: 'thing'},
	{name: 'other'},
});

console.log(c.at(0));
```

### Defining new collection types

* Define a new type of collection by extending Backbone.Collection
* Common to specify the type of model that the collection holds

```javascript
var Vehicle = Backbone.Model.extend({});
var Vehicles = Backbone.Collection.extend({
	model: Vehicle
});
var vehicles = new Vehicles([
	{color: 'blue', {color: 'red'}
]);
console.log(JSON.stringify(vehicles.at(0)))
```

* Collection can have 'class properties' too

```javascript
var Vehicles = Backbone.Collection.extend(
	{
		model: Vehicle
	},
	{
		oneVehicle: function(){
			return new Vehicle({color: 'green'});
		}
	}
);

var v = Vehicles.oneVehicle();
```

### Sorting

* Collections are sorted - either by insertion order or by a comparator function
	* if you define a comparator, collections will always be ordered by said function

```javascript
var Vehicles = Backbone.Collection.extend({
	model: Vehicle,
	comparator: function(vehicle){
		return vehicle.get('sequence');
	}
});
```

* There is another type of comparator function that takes two arguments
	* function should return:
		* -1 if args are already in correct order
		* 0 if args are the same
		* 1 if args should be reversed 

```javascript
var Vehicles = Backbone.Collection.extend({
	model: Vehicle,
	comparator: function(vehicle1, vehicle2){
		return vehicle1.get('sequence') < vehicle2.get('sequence') ? -1 : 1
	}
});
```
### Instantiating a collection

* To create a new collection object call its constructor function with the 'new' operator
* The simplest case is to create an instance of Backbone.Collection

```javascript
var collection = new Backbone.Collection();
```

* Or use custom types

```javascript
var Vehicles = Backbone.Collection.extend({});
var fords = new Vehicles();
```

* You can pass the collections data to the constructor

```javascript
var collection = new Backbone.Collection([
	model1, model2, model3
]);
```

* If your collection has an initialize function it will be invoked after the constructor is called

### Adding and removing elements

* add() and remove() work exactly as you would expect

```javascript
var model = new Backbone.Model();
collection.add(model);
```

* Use the 'at' option to insert a model at a specific index and the 'silent' option to suppress the 'add' event

```javascript
var model = new Backbone.Model();
collection.add(model, {at: 2});
collection.at(2); //model
```

* add() and remove() both work on a single model or an array of models

```javascript
collection.remove(model);

collection.remove([model2, model3]);
```

* Handling the add event

```javascript
collection.on('add', function(model, col, options){
	console.log('added ' + model.get('name') + ' at index ' + options.index);
});
```

* Suppressing the add event

```javascript
collection.add({name: 'Eric'}, {silent: true});
```

### Getting elements

* at() retrieves a model from a collection by the index of the model in the collection

```javascript
collection.at(0); //first model
collection.at(collection.length - 1); //last model
```

* get() retrieves a model from a collection by its id

```javascript
collection.get(1);
```

* if your model has not been saved it will not have an id, so use getByCid() 

```javascript
collection.getByCid('c1');
```

### Collection iterators

* Backbone proxies a set of underscore collection functions
* forEach

```javascript
collection.forEach(function(item){
	console.log(item);
});
//alternative
collection.forEach(console.log);
```

* map

```javascript
collection.map(function(item){
	return transform(item);
});
```

* find

```javascript
collection.find(function(model){
	return model.get('name') === 'Dave';
});
```

### Events

* Collections raise events when models are added or removed
	* 'add' event when a model is added
	* 'remove' event when a model is removed

```javascript
collection.on('add', function(model, collection){

});
collection.on('remove', function(model, collection){

});
```

* Collections forward model change events
	* Bind to 'change' or 'change:[attribute]' events

```javascript
collection.on('change', function(model, options){

});
collection.on('change:name', function(model, options){

});
```

## Connecting to a Server

* Backbone uses RESTful web request to synchronize data to and from a server
* Backbone's data server does not have to be the server that served the page
	* But the same origin policy applies (same domain)

### Same origin policy

* The same origin policy prevents scripts from accessing resources belonging to another site
* Origin = application layer protocol + domain name + port number
	* ex. http://localhost:3000

#### Cross-origin Resource Sharing (CORS)

* Technology that allows cross-origin requests
* Uses special http headers to specify the set of valid origins
* Alternative to jsonp
* Supported by most modern browsers (Not <IE10)

### The server

* Backbone defines a HTTP persistence protocol, but does not include a server.
* Use any http server that can implement Backbone's restful protocol.

#### Example of Model Synchronization to Server

```javascript
var Book = Backbone.Model.extend({
	url: 'http://withouttheloop.com:3002/books'
});

var midnight = new Book({
	title: "Midnight in the garden of good and evil",
	author: "John Berendt"
});

midnight.save({}, {
	// determined by HTTP 200 status
	success: function(){
		console.log('success');
	},
	// anything other than 200 is considered an error
	error: function(){
		console.log('error');
	}
});
```
Course author implemented a simple backbone server: https://github.com/liammclennan/backbone-server

### Collection requests

* people.create({name:"Tom", age:50});
	* Saves to the server and adds to the collection

**Request**
```
POST /people HTTP/1.1
Host: localhost:3002

{'name': 'Tom', 'age': 50 }
```

**Response**
```
HTTP/1.1 200 OK

{"id": 3}
```

Note that backbone expects the server to always return on "id" property when models are saved.

* people.fetch()
	* Fetch the collection from the server

**Request**

```
GET /people HTTP/1.1
Host: localhost:3002
```

**Response**

```
HTTP/1.1 200 OK

[{'name': 'Sarah': 'age': 64, 'id': 0},
{'name': 'Robbie': 'age': 19, 'id': 1}]
```

### Model requests

* person.fetch()
	* Reset the model's state from the server

**Request**

```
GET /people/1 HTTP/1.1
Host: localhost:3002
```

**Response**

```
HTTP/1.1 200 OK

{'name': 'Robbie': 'age': 19, 'id': 1}
```

* person.save()
	* Create or update depending upon person.isNew()
	* Create is the same as collection.create()

**Request (update)**

```
PUT /people/1 HTTP/1.1
Host: localhost:3002

{"name": "Tom", "age": 50}
```

**Response (update)**

```
HTTP/1.1 200 OK
```

* person.destroy()
	* Deletes the model on the server and removes it from its client-side collection

**Request**

```
DELETE /people/1 HTTP/1.1
Host: localhost:3002
```

**Response**

```
HTTP/1.1 200 OK
```

### Backbone.sync

* A function that interfaces between backbone and the server
* By replacing Backbone.sync, you can completely or partially redefine Backbone's model persistence. 
* Implements create, read, update and delete behavior
	* Can be overridden globally, per collection or per model

#### Backbone.localStorage

* This plugin is an example of replacing Backbone.sync
* It uses local storage for persistence

## Testing

**Reasons to Test**

* To catch bugs
	* As a dynamic language, Javascript will not report problems at compile time
* To enable change
	* Without comprehensive tests change is extremely difficult
* To account for browser differences
	* Automated tests will help you to keep your application running across all supported browsers. 

### Testing tools

* Test runner - they collect and execute tests. they also collect and report results. most also include an assertion library
	* Jasmine
	* Mocha
	* QUnit

#### Jasmine

> http://pivotal.github.com/jasmine

```javascript
describe('some context', function(){
	describe('nested context', function(){
		it('should show some behavior', function(){
			// assert expectations here
		});
		it('should show some other behavior', function(){
			// assert expectations here
		});
	});
});
```

### Testing models

Testing models is easy!

**Test Pattern**

1. Initialize a model with a specific state
2. Test that the model's behavior matches expectations
3. Goto 1

#### Testing a Rectangle Model

* A Rectangle has a length and a width

**Rectangle Specification**

```
Rectangle
	with length 7 and width 4
		should have an area of 28
		should have a perimeter of 22
```

**Jasmine Specification & Example**

```javascript
var app = {};

(function(shapes){
	shapes.Rectangle = Backbone.Model.extend({
		initialize: function(){
			this.on('change', function(){
				if(this.get('length') <== 0 || this.get('width') <== 0){
					throw new Error('Invalid dimensions');
				}
			}, this);
		},
		area: function(){
			return this.get('length') * this.get('width');
		},
		perimeter: function(){
			return 2 * this.get('length') + 2 * this.get('width');
		},
		isSquare: function(){
			return this.get('length') === this.get('width');
		}
	});
})(app);


describe('Rectangle', function(){
	var rectangle;

	beforeEach(function(){
		recangle = new app.Rectangle();
	});

	describe('with length 7 and width 4', function() {
		
		beforeEach(function(){
			rectangle.set({
				length: 7, 
				width: 4
			});
		});

		it('should have an area of 28', function(){
			expect(rectangle.area()).toBe(28);
		});
		it('should have perimeter of 22', function(){
			expect(rectangle.perimeter()).toBe(22);
		});
	});

	describe('with equal length and width', function(){
		
		beforeEach(function(){
			rectangle.set({
				length: 5,
				width: 5
			});
		});

		it('should be a square', function(){
			expecte(rectangle.isSquare()).toBe(true);
		});
	});

	describe('with unqueal length and width', function(){

		beforeEach(function(){
			rectangle.set({
				length: 5, 
				width: 3
			});
		});

		it('should not be a square', function(){
			expect(rectangle.isSquare()).toBe(false);
		});
	});

	describe('setting invalid values', function(){
		describe('negative length or width', function(){
			it('should throw an error if the length or width is set to a negative number', function(){
				function setDimensions() {
					rectangle.set({length: 1, width: -9});
				}

				expect(setDimensions).toThrow();
			});
		});

		describe('zero length or width', function(){
			it('should throw an error if the length or width is set to zero', function(){
				function setDimensions() {
					rectangle.set({length: 1, width: 0});
				}

				expect(setDimensions).toThrow();
			});
		});
	});
});
```

### Testing views

* Write testable views!
	* Do not depend on specific DOM elements
	* Render a completely new DOM element for the view or render into an element passed to the view's constructor
	* Never access DOM elements outside of the view
* Test
	* Rendered elements
	* Raised events

#### The Rectangle View Example

**Rectangle View Specification**

```
Rectangle View
	with length 70 and width 40
		should render a div with class rectangle
		should have dimensions 70x40
		should raise rectangle:selected when clicked
```

**Jasmine Rectangle View Specification and Example**

```javascript
var app = app || {};

(function(shapes){
	shapes.views = {
		RectangleView: Backbone.View.extend({
			className: 'rectangle',
			events: {
				'click': function(){
					app.eventAggregator.trigger('rectangle:selected');
				}
			},
			render: function(){
				this.$el.width(this.model.get('width'));
				this.$el.height(this.model.get('height'));
				return this;
			}
		})
	};

	shapes.eventAggregator = _.extend({}, Backbone.Events);	
})(app);
describe('Rectangle View', function(){
	var rectangleView;

	describe('with length 70 and width 40', function(){

		beforeEach(function(){
			var rectangle = new app.Rectangle({ width: 70, length: 40});
			rectangleView = new app.views.RectangleView({ model: rectangle });
			rectangleView.render();
		});

		it('should render a div', function(){
			expect(rectangleView.el.tagName).to.be('DIV');
		});

		it('should render with class rectangle', function(){
			expect(rectangleView.$el.hasClass('rectangle')).toBe(true);

		});

		it('should have dimensions 70x40', function(){
			expect(rectangleView.$el.width()).toBe(70);
			expect(rectangleView.$el.height()).toBe(40);
		});

		it('should raise rectangle:selected when clicked', function(){
			var rectangleSelectedRaised = false;
			app.eventAggregator.on('rectangle:selected', function(){
				rectangleSelectedRaised = true;
			});
			rectangleView.$el.click();

			expect(rectangleSelectedRaised).toBe(true);
		});
	});
});
```

### Testing Routes

* Don't
* Keep all logic out of route handlers so that route handlers aren't required for testing

### Testing without a browser

* Jasmine-node or Mocha
	* For tests that do not require a DOM
* Use a headless browser (phantom.js)
	* Tests in a full browser environment

#### Phantom.js

* Headless webkit with js api
* Useful for running tests in a browser from the command line
* http://phantomjs.org/

