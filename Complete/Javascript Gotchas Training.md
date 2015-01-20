## Functions and Hoisting and Defining

There are two ways to define a function, but have different meanings:

```javascript
var myFunction = function(){};
```

Here you are creating an anonymous function object and assigning it to a variable myFunction. The myFunction var will be hoisted as undefined until it is defined

```javascript
function myFunction(){};
```

This will create a myFunction function object and can be used through out the scope of your scripts. Downside of this is that it goes into the global namespace. 

### Hoisting

JS interpreter passes over your code twice. The first step is to hoist all of the variables to the top of the function, and if not in a function, the global namespace. 

```javascript
var x = 5;

elem = document.getElementById("demo");
elem.innerHTML = x + " " + y;

var y = 7;
```

The result of this would be **"5 undefined"** because y is hoisted to the top of the scope. This is important because the first type of function declaration will be hoisted. So you cannot use it before you declare it. 

## Sameness: == vs ===

* == operator performs the comparison with type coercion 
* === operator performs type sensitive comparison

```javascript
function myController($scope){
	$scole.cal = function(){
		$scope.test1 = (1 == '1'); //true
		$scope.test2 = (1 === '1'); //false
		$scope.test3 = ([1,2] == "1,2"); //true
		$scope.test4 = ([1,2] === "1,2"); //false
		$scope.test5 = (true == 1); //true
		$scope.test6 = (true === 1); //false
		$scope.test7 = (null == undefined); //true
		$scope.test8 = (null === undefined); //false
	}
};
```

## String Replacement

String repalce only replaces the first match. You must use regular expressiosn to replace all matches

```javascript
function myController($scope) {
	$scope.quote = "All good things come to those who wait";
	$scope.replace = function(){
		$scope.quote1 = $scope.quote.replace('l','xx'); //Axxl good things come to those who wait
		$scope.quote1 = $scope.quote.replace(/l/,'xx'); // Axxl good things come to those who wait
		$scope.quote1 = $scope.quote.replace(/l/g,'xx'); // Axxxx good things come to those who wait
	}
}
```
the /g modifier represents a global search, and will continue to match until it reaches the end of the string. 

## Concatening a string

The '+' operator will concatenate a string if any of the operands is a string. Else it will attempt to do addition.

```javascript
function myController($scope){
	var a = 1;
	var b = '2';
	var c = 'my number is ';
	$scope.replace = function(){
		$scope.result1 = a + a; // 2
		$scope.result2 = a + b; // 12
		$scope.result3 = c + a; // my number is 1
		$scope.result4 = c + (a + b); // my number is 12
		$scope.result5 = c + (Number(a) + Number(b)); // my number is 3
		$scope.result6 = b - b; // 0
	}
}
```
Note the behavior of subtraction. When strings are involved, JS will attempt to convert them to numbers, and if they are do the arithmetic. 

With form data it is necessary to do conversions to ensure type safety.

## typeof and instanceof

The **instanceof** operator tests whether an object has in its prototype chain the prototype property of a constructor. 

The **typeof** operator returns the fundamental type. The types are:

* undefined
* null
* boolean
* number
* string
* function
* object

Notice the lack of 'Array'. DO NOT USE 'typeof Array'! It is just an object. 

```javascript
function myController($scope){
	var string = "string";
	var object = {};
	var array = [];
	var f1 = function(){return 'none'};

	$scope.cal = function(){
		$scope.result1 = (typeof string === 'string'); // true
		$scope.result2 = (typeof object === 'object'); // true 
		$scope.result3 = (typeof array === 'array'); // false
		$scope.result4 = (typeof array === 'object'); // true
		$scope.result5 = (typeof f1 === 'function'); // true
		$scope.result6 = (instanceof string === 'string');
		$scope.result7 = (typeof string === 'string');
	}
}
```

## There are no integers in JavaScript

Javascript only has the Number type and numbers are IEEE floating point double-precision. This means that Number will exhibit the floating point rounding errors. 

All languages have this problem that do floats, and handle it in different ways. 

Angular has a utility class to handle this. 

```javascript
0.1 + 0.2; // .3000000000000000004
0.01 + 0.06; // 0.0699999999999999
parseFloat((.01 + .06).toPrecision(12)); // .07
```

## Style does matter in Javascript

```javascript
return 
{ 
	ok: false
}

return {
	ok: false
};
```
These are different. The long return gets mangled by the semicolon insertion process and returns undefined! The rest will become a block statement and just be a label. The second will error because the semicolon is not used to define var in object creation. 

Also does not recommend using if/else without brackets. Hard to maintain if a developer comes in and adds a line, doesn't realize that it is not part of the if/else block. 

JSLint will complain about both of these.

## Null and undefined

**null** is a variable type that has no value

**undefined** is a variable that has not been typed

## Scope

There is no concept of block scoping, only functional scoping. The one way you can create a scope is by immediately-executing function. This is still just a function scope.

```javascript
for(var loop=0; loop<10;loop++){
	console.log(loop);
}
var loop;
console.log(loop); //10

// vs

// the var loop variable here gets hoisted to the top of the IEFE
(function(){
	for(var loop=0; loop<10;loop++){
		console.log(loop);
	}
}());

var loop;

console.log(loop); //undefined
```