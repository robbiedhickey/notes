# ComponentOne AngularJS Presentation

> Chris Bannan from ComponentOne gives this talk: 

### Wijmo - C1 Core HTML5 Technology

Built on web standards HTML5/CSS/jQuery/jQueryUI/jQuery Mobile. Now is shifting away from relying on a specific framework. Has a focus on features, performance, support, run on all devices, support emerging standards/frameworks. 

#### Wijmo Architecture / Roadmap

* Extensible core
* Not tied to any frameworks
* Support emerging standards /frameworks
	* Past: KnockoutJS, BreezeJS
	* Present: AngularJS, Bootstrap, RequireJS (AMD)
	* Future: Web Components (Polymer, X-Tags, ??)
* What YOU need

C1 wants to know what TR roadmap is so they can anticipate their clients direction and provide. 

## AngularJS Overview

* Google's application framework
* Very complete platform (MVVM and more!)
* Emerging standard
* Great for new Web apps
* Ideal for developers coming from .NET

### What is MVVM?

* Model
	* Data for an application
	* Example: Web service
* ViewModel (aka Controller)
	* Logic
	* Example: JavaScript class or object
* View
	* Presentation
	* Example: HTML CSS & JavaScript UI

### MVVM Benefits

* Separation of logic (model) and presentation (view)
* ViewModels are easy to test (easier maintenance)
* ViewModels can "drive" multiple views (flexibility)
* Standardization (shorter cycles, easier maintenance)
* Not specific to AngularJS (help migration)

### Migrating from Silverlight to HTML5

* Choose an MVVM Framework (AngularJS)
* Convert C# ViewModel classes to JavaScript
	* focus on business logic, not lines of code
* Convert XAML views to HTML
	* focus on functionality, not appearance
	* best way to find important components is to look for data binding
* Bind the ViewModel to the Views
	* with AngularJS Markup
* Choose components carefully
	* to simplify Views and ViewModels

As you're porting views, you'll find that some are easier than others. Textboxes are easy, but charts and grids can be less straight-forward. This is where it is a good idea to leverage components, but choose them carefully. 

### Challenges in HTML5 Work

* Development Tools
	* TypeScript (type-checking, inheritance, intellisense)
	* Closure Compiler (smaller/faster code)
* Data Layer (RIA Services)
	* BreezeJS (LINQ-style queries)
	* OData (REST-based)
* Components
	* AngularJS directives (and Wijmo!)

Tooling is still lagging behind native C# tools, but it is improving. 

### AngularJS Concepts

AngularJS | .NET | Category |
----------|------| -------- |
app | Application | Application
module | Assembly | Application Building Block
view | UserControl | Presentation element
controller | ViewModel | Application Logic
scope | DataContext | Bundle of bindable data
filter | IValueConverter | Format data for display
factory/service | Utility classes | Re-usable code
directive | Control | Re-usable UI elements

### Quick Example: Logic

```javascript
// define an application module (app in .NET)
var myApp = angular.module("myApp", []);

// add a controller to the app module (ViewModel in .NET)
myApp.controller("myCtrl", function($scope) {
	$scope.msg = "hello world";
});

// add a filter to the app module (IValueConterter in .NET)
myApp.filter("myUpperFilter", function(){
	return function(input){
		return input.toUpperCase();
	};
});

```

### Quick Example: Directive

Add a hover affect to an element:
```javascript
// add a directive to the app module (control in .NET)
myApp.directive("myDctv", function() {
	return function(scope, element, attrs){
		element.bind("mouseenter", function() {
			element.css("background", "yellow");
		});
		element.bind("mouseleave", function() {
			element.css("background", "none");
		});
	};
});
```

### Quick Example: Presentation

```html
<body ng-app="myApp" ng-controller="myCtrl">
	<input ng-model="msg" />
	<p my-dctv>
		This is the message in uppercase:
		{{ msg | myUpperFilter }}
	</p>
</body>
```

Note the application of our directive, in code it was named "myDctv" and when referenced from the view it is referred to as "my-dctv". This is angular convention, case changes are followed by dashes. 

Also note that with filters, the name is used as we provided it. The dash convention only applies to directives (from what I can tell).

[JSFiddle of Quick Examples](http://jsfiddle.net/wijmo/JKBbV/)

## AngularJS Directives (Components/Controls)

* Define your own HTML tags/attributes (to modify the DOM and add custom logic)
* Create true components (finally! other playforms have always had them)
* Dramatically simplify your Views and ViewModels
* Provide consistent behavior and appearance
* Standard and/or Custom ( Wijmo includes AngularJS directives, but you can create your own)

### Samples

AngularJS Flexibility

Directive| Example |
--------|----------|
Bootstrap Accordion | http://jsfiddle.net/wijmo/MTKp7/
Google Maps | http://jsfiddle.net/wijmo/Rqcsj/
Wijmo Chart | http://jsfiddle.net/Wijmo/KRAzr/
Wijmo Grid | http://jsfiddle.net/wijmo/jmp47/

#### Bootstrap Accordion Directive

Real benefit to doing this is that you get a much cleaner, more declarative view. It makes intent clear, no longer just nested divs -- the html element name represents its purpose. 

##### JS:
```javascript
angular.module("btst", []).
directive("btstAccordion", function () {
    return {
        restrict: "E",
        transclude: true,
        replace: true,
        scope: {},
        template:
            "<div class='accordion' ng-transclude></div>",
        link: function (scope, element, attrs) {

            // give this element a unique id
            var id = element.attr("id");
            if (!id) {
                id = "btst-acc" + scope.$id;
                element.attr("id", id);
            }

            // set data-parent on accordion-toggle elements
            var arr = element.find(".accordion-toggle");
            for (var i = 0; i < arr.length; i++) {
                $(arr[i]).attr("data-parent", "#" + id);
                $(arr[i]).attr("href", "#" + id + "collapse" + i);
            }
            arr = element.find(".accordion-body");
            $(arr[0]).addClass("in"); // expand first pane
            for (var i = 0; i < arr.length; i++) {
                $(arr[i]).attr("id", id + "collapse" + i);
            }
        },
        controller: function () {}
    };
}).
directive('btstPane', function () {
    return {
        require: "^btstAccordion",
        restrict: "E",
        transclude: true,
        replace: true,
        scope: {
            title: "@",
            category: "=",
            order: "="
        },
        template:
            "<div class='accordion-group' >" +
            "  <div class='accordion-heading'>" +
            "    <a class='accordion-toggle' data-toggle='collapse'> {{category.name}} - </a>" +
       
            "  </div>" +
            "<div class='accordion-body collapse'>" +
            "  <div class='accordion-inner' ng-transclude></div>" +
            "  </div>" +
            "</div>",
        link: function (scope, element, attrs) {
            scope.$watch("title", function () {
                // NOTE: this requires jQuery (jQLite won't do html)
                var hdr = element.find(".accordion-toggle");
                hdr.html(scope.title);
            });
        }
    };
})
```
##### Markup:
```html
<body ng-app="btst">
     <h3>BootStrap Accordion Directives</h3>

    <btst-accordion>
        <btst-pane title="<b>Firstjm</b> Pane" category="{name:'test'}">
            <div>Anim pariatur cliche reprehenderit, enim eiusmod high life 
                accusamus terry richardson ad squid. 3 wolf moon officia aute,
                non cupidatat skateboard dolor brunch. Food truck quinoa nesciunt
                laborum eiusmod. Brunch 3 wolf moon tempor, sunt aliqua put a bird
                on it squid single-origin coffee nulla assumenda shoreditch et.
                Nihil anim keffiyeh helvetica, craft beer labore wes anderson 
                cred nesciunt sapiente ea proident. Ad vegan excepteur butcher
                vice lomo. Leggings occaecat craft beer farm-to-table, raw 
                denim aesthetic synth nesciunt you probably haven't heard of 
                them accusamus labore sustainable VHS.</div>
        </btst-pane>
        <btst-pane title="<b>Second</b> Pane">
            <div>Anim pariatur cliche reprehenderit, enim eiusmod high life 
                accusamus terry richardson ad squid. 3 wolf moon officia aute,
                non cupidatat skateboard dolor brunch. Food truck quinoa nesciunt
                laborum eiusmod. Brunch 3 wolf moon tempor, sunt aliqua put a bird
                on it squid single-origin coffee nulla assumenda shoreditch et.
                Nihil anim keffiyeh helvetica, craft beer labore wes anderson 
                cred nesciunt sapiente ea proident. Ad vegan excepteur butcher
                vice lomo. Leggings occaecat craft beer farm-to-table, raw 
                denim aesthetic synth nesciunt you probably haven't heard of 
                them accusamus labore sustainable VHS.</div>
        </btst-pane>
        <btst-pane title="<b>Third</b> Pane">
            <div>Anim pariatur cliche reprehenderit, enim eiusmod high life 
                accusamus terry richardson ad squid. 3 wolf moon officia aute,
                non cupidatat skateboard dolor brunch. Food truck quinoa nesciunt
                laborum eiusmod. Brunch 3 wolf moon tempor, sunt aliqua put a bird
                on it squid single-origin coffee nulla assumenda shoreditch et.
                Nihil anim keffiyeh helvetica, craft beer labore wes anderson 
                cred nesciunt sapiente ea proident. Ad vegan excepteur butcher
                vice lomo. Leggings occaecat craft beer farm-to-table, raw 
                denim aesthetic synth nesciunt you probably haven't heard of 
                them accusamus labore sustainable VHS.</div>
        </btst-pane>
    </btst-accordion>
</body>
```
Notice that we are still creating a namespace for the directive (btst). Angular Directive configuration options:

* **restrict** - tells angular whether this directive can be applied as an html element or an attribute ('e' for element, 'a' for attribute) 
* **transclude** - tells angular whether to include the original HTML when it is transforming the directive tag. In our example, 'true' allows the nested btst-pane tags to be copied. 
* **replace** - tells angular whether to leave the original element there, or replace it after rendering the template. 
* **scope** - tells angular the scope for the directive itself
* **template** - the HTML you are replacing the tag with. 
* **link** - the handler that allows you to modify the DOM and perform initialization. In our case, we need to initialize the accordion. It also allows you to set up observables
* **require** - tells angular if a directive depends on another directive. In our example, the pane has "^btstAccordion" which indicates it must be a child of the accordion. 

#### Google Maps Directive

##### JS

```javascript
var app = angular.module("app", []);

app.controller("appCtrl", function ($scope) {

    // current location
    $scope.loc = { lat: 40, lon: -73 };
    $scope.gotoCurrentLocation = function () {
        if ("geolocation" in navigator) {
            navigator.geolocation.getCurrentPosition(function (position) {
                var c = position.coords;
                $scope.gotoLocation(c.latitude, c.longitude);
            });
            return true;
        }
        return false;
    };
    $scope.gotoLocation = function (lat, lon) {
        if ($scope.lat != lat || $scope.lon != lon) {
            $scope.loc = { lat: lat, lon: lon };
            if (!$scope.$$phase) $scope.$apply("loc");
        }
    };

    // geo-coding
    $scope.search = "";
    $scope.geoCode = function () {
        if ($scope.search && $scope.search.length > 0) {
            if (!this.geocoder) this.geocoder = new google.maps.Geocoder();
            this.geocoder.geocode({ 'address': $scope.search }, function (results, status) {
                if (status == google.maps.GeocoderStatus.OK) {
                    var loc = results[0].geometry.location;
                    $scope.search = results[0].formatted_address;
                    $scope.gotoLocation(loc.lat(), loc.lng());
                } else {
                    alert("Sorry, this search produced no results.");
                }
            });
        }
    };

    // some points of interest to show on the map
    // to be user as markers, objects should have "lat", "lon", and "name" properties
    $scope.airports = [
        { "name": "Hartsfield Jackson Atlanta", "code": "ATL", "city": "Atlanta", "state": "GA", "lat": 33.64, "lon": -84.444, "vol2011": 44414121 },
        { "name": "O'Hare", "code": "ORD", "city": "Chicago", "state": "IL", "lat": 41.9794, "lon": -87.9044, "vol2011": 31892301 },
        { "name": "Los Angeles", "code": "LAX", "city": "Los Angeles", "state": "CA", "lat": 33.9425, "lon": -118.4081, "vol2011": 30528737 },
        { "name": "Dallas/Fort Worth", "code": "DFW", "city": "Dallas/Fort Worth", "state": "TX", "lat": 32.8974, "lon": -97.0407, "vol2011": 27518358 },
        { "name": "Denver", "code": "DEN", "city": "Denver", "state": "CO", "lat": 39.8631, "lon": -104.6736, "vol2011": 25667499 },
        { "name": "ATR @ 60180850", "code": "JFK", "city": "60180850", "state": "NY", "lat": 40.6438, "lon": -73.782, "vol2011": 23664830 },
        { "name": "San Francisco", "code": "SFO", "city": "San Francisco", "state": "CA", "lat": 37.6152, "lon": -122.39, "vol2011": 20038679 },
        { "name": "McCarran", "code": "LAS", "city": "Las Vegas", "state": "NV", "lat": 36.085, "lon": -115.1511, "vol2011": 19854759 },
        { "name": "Phoenix Sky Harbor", "code": "PHX", "city": "Phoenix", "state": "AZ", "lat": 33.4365, "lon": -112.0073, "vol2011": 19750306 },
        { "name": "George Bush", "code": "IAH", "city": "Houston", "state": "TX", "lat": 29.9867, "lon": -95.3381, "vol2011": 19306660 },
    ];
});

// formats a number as a latitude (e.g. 40.46... => "40°27'44"N")
app.filter('lat', function () {
    return function (input, decimals) {
        if (!decimals) decimals = 0;
        input = input * 1;
        var ns = input > 0 ? "N" : "S";
        input = Math.abs(input);
        var deg = Math.floor(input);
        var min = Math.floor((input - deg) * 60);
        var sec = ((input - deg - min / 60) * 3600).toFixed(decimals);
        return deg + "°" + min + "'" + sec + '"' + ns;
    }
});

// formats a number as a longitude (e.g. -80.02... => "80°1'24"W")
app.filter('lon', function () {
    return function (input, decimals) {
        if (!decimals) decimals = 0;
        input = input * 1;
        var ew = input > 0 ? "E" : "W";
        input = Math.abs(input);
        var deg = Math.floor(input);
        var min = Math.floor((input - deg) * 60);
        var sec = ((input - deg - min / 60) * 3600).toFixed(decimals);
        return deg + "°" + min + "'" + sec + '"' + ew;
    }
});

// - Documentation: https://developers.google.com/maps/documentation/
app.directive("appMap", function () {
    return {
        restrict: "E",
        replace: true,
        template: "<div></div>",
        scope: {
            center: "=",        // Center point on the map (e.g. <code>{ latitude: 10, longitude: 10 }</code>).
            markers: "=",       // Array of map markers (e.g. <code>[{ lat: 10, lon: 10, name: "hello" }]</code>).
            width: "@",         // Map width in pixels.
            height: "@",        // Map height in pixels.
            zoom: "@",          // Zoom level (one is totally zoomed out, 25 is very much zoomed in).
            mapTypeId: "@",     // Type of tile to show on the map (roadmap, satellite, hybrid, terrain).
            panControl: "@",    // Whether to show a pan control on the map.
            zoomControl: "@",   // Whether to show a zoom control on the map.
            scaleControl: "@"   // Whether to show scale control on the map.
        },
        link: function (scope, element, attrs) {
            var toResize, toCenter;
            var map;
            var currentMarkers;

            // listen to changes in scope variables and update the control
            var arr = ["width", "height", "markers", "mapTypeId", "panControl", "zoomControl", "scaleControl"];
            for (var i = 0, cnt = arr.length; i < arr.length; i++) {
                scope.$watch(arr[i], function () {
                    cnt--;
                    if (cnt <= 0) {
                        updateControl();
                    }
                });
            }

            // update zoom and center without re-creating the map
            scope.$watch("zoom", function () {
                if (map && scope.zoom)
                    map.setZoom(scope.zoom * 1);
            });
            scope.$watch("center", function () {
                if (map && scope.center)
                    map.setCenter(getLocation(scope.center));
            });

            // update the control
            function updateControl() {

                // update size
                if (scope.width) element.width(scope.width);
                if (scope.height) element.height(scope.height);

                // get map options
                var options =
                {
                    center: new google.maps.LatLng(40, -73),
                    zoom: 6,
                    mapTypeId: "roadmap"
                };
                if (scope.center) options.center = getLocation(scope.center);
                if (scope.zoom) options.zoom = scope.zoom * 1;
                if (scope.mapTypeId) options.mapTypeId = scope.mapTypeId;
                if (scope.panControl) options.panControl = scope.panControl;
                if (scope.zoomControl) options.zoomControl = scope.zoomControl;
                if (scope.scaleControl) options.scaleControl = scope.scaleControl;

                // create the map
                map = new google.maps.Map(element[0], options);

                // update markers
                updateMarkers();

                // listen to changes in the center property and update the scope
                google.maps.event.addListener(map, 'center_changed', function () {

                    // do not update while the user pans or zooms
                    if (toCenter) clearTimeout(toCenter);
                    toCenter = setTimeout(function () {
                        if (scope.center) {

                            // check if the center has really changed
                            if (map.center.lat() != scope.center.lat ||
                                map.center.lng() != scope.center.lon) {

                                // update the scope and apply the change
                                scope.center = { lat: map.center.lat(), lon: map.center.lng() };
                                if (!scope.$$phase) scope.$apply("center");
                            }
                        }
                    }, 500);
                });
            }

            // update map markers to match scope marker collection
            function updateMarkers() {
                if (map && scope.markers) {

                    // clear old markers
                    if (currentMarkers != null) {
                        for (var i = 0; i < currentMarkers.length; i++) {
                            currentMarkers[i] = m.setMap(null);
                        }
                    }

                    // create new markers
                    currentMarkers = [];
                    var markers = scope.markers;
                    if (angular.isString(markers)) markers = scope.$eval(scope.markers);
                    for (var i = 0; i < markers.length; i++) {
                        var m = markers[i];
                        var loc = new google.maps.LatLng(m.lat, m.lon);
                        var mm = new google.maps.Marker({ position: loc, map: map, title: m.name });
                        currentMarkers.push(mm);
                    }
                }
            }

            // convert current location to Google maps location
            function getLocation(loc) {
                if (loc == null) return new google.maps.LatLng(40, -73);
                if (angular.isString(loc)) loc = scope.$eval(loc);
                return new google.maps.LatLng(loc.lat, loc.lon);
            }
        }
    };
});

```

##### Markup

```html
<body ng-app="app" ng-controller="appCtrl">

<h3>Google Maps</h3>

    <!-- search/go to current location -->
    <div class="text-right">
        <div class="input-append text-right">
            <input type="text" ng-model="search"/>
            <button class="btn" type="button" ng-click="geoCode()" ng-disabled="search.length == 0" title="search" >
              &nbsp;<i class="icon-search"></i>
            </button>
            <button class="btn" type="button" ng-click="gotoCurrentLocation()" title="current location">
              &nbsp;<i class="icon-home"></i>
            </button>
        </div>
    </div>

    <!-- map -->
    <app-map style="height:400px;margin:12px;box-shadow:0 3px 25px black;"
        center="loc"
        markers="airports"
    > 
    </app-map>

    <!-- current location -->
    <div class="text-info text-right">
        {{loc.lat | lat:0}}, {{loc.lon | lon:0}}
    </div>

    <!-- list of airports -->
    <div class="container-fluid">
        <div class="span3 btn" 
            ng-repeat="a in airports"
            ng-click="gotoLocation(a.lat, a.lon)">
            <b>{{a.code}}</b>: {{a.name}}
        </div>
    </div>
</body>

```

Maps in general require a lot of javascript to create them, it is a perfect candidate for directive encapsulation. A great thing about directives is that you can limit the API to just your needs. Google maps exposes a ton of features and you can fine-grain that API to simplify interaction for other developers. In our example, we only expose the location and markers. 

In the controller (ViewModel) we set the current location and set up data, as well as define our functions that the view will need to call. 

We also define filters that turn numbers into a nice latitude/longitude format. 

"appMap" is the directive. He glosses over the scope, which has characters like '=' and '@'. Need to do some research on what these represent in directive scope. 

NOTE: the 'link' method is called everytime your directive is attached to an HTML element. So if it is in a repeat, it gets called each time.

#### Wijmo Line Chart Directive

I'm not including the JS/Markup for this, use the JSFiddle. 

The example uses a directive to wrap the wijmo chart and mock some data. Again, notice the simple API

#### Wijmo Data Grid Directive

This is a very opinionated API. If you have a proprietary control that you don't like the API of, you can wrap it with angular and design the API how you see fit. This was made by the maker of flex-grid.

Another good reason to make custom directives is to future proof your application. Internally your teams and dev staff might be using your own markup. You can change implementation within the directive and not impact anyone using the directive. This is a level of abstraction that we have not typically had for client-side applications. 

### Grid Benchmark Demo

In angular, it is more important to deliver the JS before the HTML, so common wisdom about loading your scripts at the bottom of the page does not always apply. May often be preferable to load them at the top. 

Wijmo and SpreadJS grid performance is not impacted by size of table. They both have virtualization built in. This means they have a virtual DOM that is rendered as needed. 

With the ng-grid, you have to define the options in the scope rather than inline on the directive element, which IMO is a step backwards. 

**REALLY COOL** TR sent C1 a screenshot of an existing active-x control that is a grid responsible for editing Tax Combination Codes. The C1 team mocked up a demo, and even changed/enhanced their grid to better support our scenario. Point: communicate to the C1 team, they want to hear our issues and are more than willing to accommodate us. 

## Conclusion

* AngularJS makes HTML5 a practical platform for app development, especially if you are coming from .NET and expect to have those framework features and guidelines
* AngularJS is a huge step forward, but there's more to come (Polymer)
* AngularJS is not the whole story: Bootstrap, Wijmo, and other libraries are useful
	* Globalize - localization and globalization
	* Underscore - utility functions for arrays/etc
* Not everything is new: MVVM, data tables, grids, charts, and other components are still relevant. 
* We want to help you move to HTML5

### Links and Resources

Topic | Resource
------|-----
AngularJS | http://angularjs.org/
Wijmo | http://wijmo.com/
Polymer | http://www.polymer-project.org/
AngularJS Directives | http://www.codeproject.com/Articles/607873/Extending-HTML-with-AngularJS-Directives
ComponentOne | chris.bannon@componentone.com (wijmo), eric.peng@componentone.com (SpreadJS), bernardo.castilho@componentone.com (CTO)

### FAQ

You have to make the decision whether you want to support a fixed size vs fluid. Many business-type applications have a hybrid with a static left nav-bar and a fluid right pane. You can use CSS media queries to have custom rules for different viewports. With most components - you want to react to resizing. C1 doesn't do this because of the performance impact. He recommends using some sort of timer to throttle the polling of the resize event. 

