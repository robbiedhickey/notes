# $resource in Angular Training

$http is a low-level abstraction for communicating with web services. $resource is a higher-level abstraction that allows you to treat your endpoints in a more natural RESTful actions. 

$resource is simply a wrapper around $http. 

### What makes it different?

It returns a $resource object back to you. You can then make additional backend calls directly on that resource. It also gives you some default methods to start off with. 

In RESTful calls you often have two types of GETs -> a single record or a collection. So $query allows you to specifiy whether the resultset is an array with the isArray optional parameter. Use $get if it is a single object

```javascript
// default params collection lets you specify where angular should look for the values on the resource.
// this prevents you from having to pass in the $requestParams into your resource call
// instead it will automatically select that property and pass it in the query string
// also notice how we can create our own methods
var userResource = $resource('/rest/user/:_id', {_id:'@_id'}, {add:{method:"PUT"}});

//another example of extending resource
var companyResource = $resource('/rest/company/:_id', {_id:'@_id'},
	{
		departmentGet: {
			method:'GET',
			url:'/rest/company/:cid/:_id'
			params:{cid:'@cid', _id:'@_id'}
		}
	});
```

It is especially nice with grids because you don't have to jump through a lot of hoops as you already have the inline resource. 

You can view the source code for this example at https://github.com/jcdalton2201/resourceweb 

