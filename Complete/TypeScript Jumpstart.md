# TypeScript JumpStart

TypeScript is a set of extensions to Javascript, it has classes, lambda syntax, modules and a static type system. 

Classes are really just ECMAScript 6. 

Typescript is a structural type system. It compares types for compatibility based on their structure and not their name. In a structurally typed language you can assign object literals if the structures are the same, don't have to explicitly declare. Structural types break down when you have public and private fields, you need to declare the type. 

A class is basically two things: an interface and a constructor function of the same name that implements the interface. 

TypeScript also supports enums. They are implemented using numbers. 

The new lambda syntax has lexical scoping which is more natural for C# developers, recommends using the lambda syntax over the traditional function declaration. 

``` typescript
module syntax.variables{
	'use strict';

	// a basic var
	var explicitString: string = 'hello';

	// use type inference
	var inferredString = 'string';
	var inferredString2 = inferredString;

	// implicit any - don't know type yet
	var x: any;
	x.dynamicField = "foo";

	// ERROR: vannot convert 'string' to 'number'
	var x: number = inferredString;

	// ERROR (with noImplicitAny)
	// implicitAny implicitly has 'any' type
	var implicitAny;
}

module syntax.classes {
	'use strict'

	class BasicClass {
		stringField: string;
		numberField: number;
	}
}

module syntax.enums {
	enum EventType { KeyUp = 3, KeyDown = 4 };
}

module syntax.functions {
	
	// function definition
	function basicFunction(argument: string): void {
		console.log(argument);
	}

	// ecma6 lambda - lexically scoped
	var basicLambda = (argument: string: void => {
		console.log(argument);
	}

	class LexicalScopingExample {
		field = "hello";
		foo() {
			
			//E6 lambdas are lexically scoped so 'this' in the lambda is
			// the LexicalScopingExample instance
			runFunction(() => console.log(this.field)); //OUTPUT: 'hello'

			// the older function syntax is not lexically scope
			// so 'this' is undefined
			runFunction(function(){
				console.log(this.field);
			}); //OUTPUT: undefined
		}
	}
}
```

## Using Angular with TypeScript

Check out DefinitelyTyped project on Github. It is a typescript library that defines interfaces and conventions for working with Angularjs. 