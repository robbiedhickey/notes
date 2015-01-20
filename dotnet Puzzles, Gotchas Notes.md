# .NET Puzzles, Gotchas & Cautionary Tales

Puzzles are a lot more fun than bugs!

Puzzle Format: Context -> Puzzle -> Puzzle Answer -> Further Explanation -> Review

## Floating Point Puzzle

What is the real value of a floating point number?

### Floating Point Context

```c#
public void CalculateCompliance()
{
	Single[] monthlyEF {1,1,1};
	Single[] monthlyOut = {3,3,3};
	Double quarterlyCompliance = 0;
	for(int i = 0; i < 3; i++)
	{
		quarterlyCompliance += monthlyEF[i] / monthlyOut[i];
	}
	if(quarterlyCompliance < 1) {} // out of compliance!
	else if(quarterlyCompliance > 1 {} // Wasting money!
	else {} //everyone gets a bonsu!
}
```

* Floating point refers to numbers where the decimal moves
	* Precision - how many digits are stored
	* Range - how big those digits are
	* Example - 4200 == 4.2E3
	* Example - 4.2E200
* .NET Types
	* Single, also called float, is a single-precision floating point type
	* Double is a double precision floating point type. 
	* Decimal is a base 10 fractional type

Fractional numbers frequently used in business calculations.

### Puzzle

```c#
[TestMethod]
//[ExpectedException(typeof(Exception)]
public void ReconstructPie()
{
	Single one = 1;
	Single three = 3;
	Single x = one / three;
	Double result = 3 * x;
	//Assert.IsTrue(result == 1.0);
	//Assert.IsTrue(result < 1.0);
	//Assert.IsTrue(result > 1.0);
	//Assert.IsTrue(Math.Round(result, 7) == 1);
}
```

Core question: How to compare floating point values.

### Answer

```c#
[TestMethod]
//[ExpectedException(typeof(Exception)]
public void ReconstructPie()
{
	Single one = 1;
	Single three = 3;
	Single x = one / three;
	Double result = 3 * x;
	//Assert.IsTrue(result == 1.0);
	//Assert.IsTrue(result < 1.0);
	Assert.IsTrue(result > 1.0);
	Assert.IsTrue(Math.Round(result, 7) == 1);
}
```

* Floating point values can't hold certain values with repeating digits, so fuzzy rounding is needed.
* Single and double types are base 2 representations
	* You cannot predict values based on decimal math!

You might expect the result to be less than one, but that would be based on your assumptions of base 10 arithmetic, where .333 * 3 = .999. In base 2 arithmetic, the value of 1/3 is slightly larger than .333, and so the result would actually be 

Another gotcha, if you change the platform target to x64 then the result will be equal to 1.0!

### Take away

* Use 'fuzzy' comparisons
* Most of the time the answers are spot on
	* Extra 'hidden' digits help accuracy
	* If "most" of the time is OK for your calculations
* When they are off, you can't predict direction
	* Base 10 thinking will fail, base 2 rounds differently
	* Calculation result can differ by optimizations and platform
* Avoid data conversions between fractional types
	* Conversion from double to single loses precision
	* Conversion from single to double increases inaccuracy (this puzzle)
	* Conversion to/from Decimal introduces rounding errors between bases

## Division Puzzle

What is the result of division?

### Division Context

Business gives a discount, based on customer's history of being polite, and the current markup. 

```c#
...
int discount = CalculateDiscount(30,20,30);
...

public int CalculateDiscount(int maxDiscountPercent, int markupPercent, int niceFactor)
{
	int discount = maxDiscountPercent * (markupPercent / niceFactor);
	return discount;
}
```
### Puzzle

```c#
[TestMethod]
//[ExpectedException(typeof(Exception),AllowDerivedTypes=true)]
public void DivisionTest()
{
	int maxDiscountPercent = 30;
	int markupPercent = 20;
	int niceFactor = 30;
	double discount = maxDiscountPercent * (markupPercent / niceFactor);
	//Assert.IsTrue( 0 == discount);
	//Assert.IsTrue( 10 == discount);
	//Assert.IsTrue( 20 == discount);
	//Assert.IsTrue( 30 == discount);
}
```

### Answer

Integer division truncates to zero!

```c#
[TestMethod]
//[ExpectedException(typeof(Exception),AllowDerivedTypes=true)]
public void DivisionTest()
{
	int maxDiscountPercent = 30;
	int markupPercent = 20;
	int niceFactor = 30;
	double discount = maxDiscountPercent * (markupPercent / niceFactor);
	Assert.IsTrue( 0 == discount);
	//Assert.IsTrue( 10 == discount);
	//Assert.IsTrue( 20 == discount);
	//Assert.IsTrue( 30 == discount);
}
```

### Take away

* Integer Division in C#
	* Integer and fractional division use the slash operator
	* Integer division when both operands are integers
* Integer divison in Visual Basic
	* Explicit backslash operator
* Integer division in both languages
	* Result is truncated to the nearest integer
	* Particularly tricky when specific values cause divide by zero exceptions:

```c#
[TestMethod]
[ExpectedException(typeof(DivideByZeroException)]
public void DivisionTest3()
{
	int x = 9; int y = 10; int z = 100;
	if (x == 0 || y == 0) { throw new InvalidOperationException();}
	decimal interim = x / y; // uh oh!
	decimal result = z / interim;
}
```

## Divide By Zero Puzzle

### Puzzle

```c#
[TestMethod]
//[ExpectedException(typeof(DivideByZeroException]
public void DivideByZeroInteger()
{
	int three=3;
	int zero=0;
	int result = three / zero;
	//Assert.AreEqual(0, result);
}

[TestMethod]
//[ExpectedException(typeof(DivideByZeroException]
public void DivideByZeroDecimal()
{
	Decimal three=3;
	Decimal zero=0;
	Decimal result = three / zero;
	//Assert.AreEqual(0, result);
}

[TestMethod]
//[ExpectedException(typeof(DivideByZeroException]
public void DivideByZeroDouble()
{
	Double three=3;
	Double zero=0;
	Double result = three / zero;
	//Assert.AreEqual(0, result);
}
```

### Answer

```c#
[TestMethod]
[ExpectedException(typeof(DivideByZeroException]
public void DivideByZeroInteger()
{
	int three=3;
	int zero=0;
	int result = three / zero;
	//Assert.AreEqual(0, result);
}

[TestMethod]
[ExpectedException(typeof(DivideByZeroException]
public void DivideByZeroDecimal()
{
	Decimal three=3;
	Decimal zero=0;
	Decimal result = three / zero;
	//Assert.AreEqual(0, result);
}

[TestMethod]
//[ExpectedException(typeof(DivideByZeroException]
public void DivideByZeroDouble()
{
	Double three=3;
	Double zero=0;
	Double result = three / zero;
	//Assert.AreEqual(0, result);
	//Assert.AreEqual(Double.PositiveInfinity, result);
}
```

Because of the special concepts that Double is aware of, it will never throw an exception due to arithmetic calculations. Decimal throws an exception just like the integer because it is base 10. 

### Bonus: Floating Point Types

* Digital Computers use a series of on/off switches
* Integers
	* can be held exactly in memory
	* integers have a fixed max and min value
	* each integer uses a block: 16,32,64 bit
* Considerations for fractional numbers
	* Precision
	* Range
	* Exception Behavior
	* Performance
	* Equality
* Large, small and fractional numbers
	* Can be displayed with floating point notation
	* One part of the digital representation states the value (mantissa)
	* One part of the digital representation states the size.

#### Base or Radix for floating point

* Base 2 (traditional 'floating point')
	* Base 2 - precision lost converting from base 10
	* Supported by hardware so very fast
	* Immense range
	* Exceptions never thrown
		* Minus zero for scientific and complex calculations
		* Positive and negative infinity supported
		* NaN (undefined) supported for 0/0
	* Complies with IEEE 754 standard
* Decimal
	* Base 10 - no precision los with base 10 data
	* 20-30 times slower in calculations
	* Currency support based on standard fractional units (ie, pennies)
	* Complies with human logic.

#### Conversion from base 10 to base 2

```c#
[TestMethod]
public void DecimalConversions()
{
	Single pointOne = 0.1F;
	Single result = 0;
	for (int i = 0; i < 10; i++)
	{
		result += pointOne;
	}

	Assert.AreEqual("1.000000012", string.Format("{0:R}", result));
}
```

* Generally data is expressed in base 10
	* So this code requires conversion to and back from base 2
	* .1 can't be held exactly
	* The precision error can be exacerbated by addition
	* The "R" format gives extra digits, without this, the output would be 1

#### Use-cases

Computer graphics are a good candidate for Double/Single calculations because they are not susceptible to rounding errors. Economic forecasting or financial models might be a good fit as well, for the performance tradeoff. 

Accounting/Currency calculations should use the Decimal type, and performance is rarely a bottleneck for these types of calculations. 

#### Fractional Type Comparison

|  | Single | Double | Decimal |
| ------|--------|--------|---------|
Radix | Base 2 | Base 2 | Base 10
Size | 32 bit | 64 bit | 128 bit
Hardware | Yes | Yes | No
Min/Max | +/- 3.402823E+38 | +/- 1.79769313486232E+308 | +/- 79,228,162,514,264,337,593,543,950,335
Zero Approach | 1.401298E-45 | 4.94065645841247E-324 | Lose accuracy approaching 5E-29
Currency Support | No | No | Yes
42/0 | Single.Infinity | Double.Infinity | Exception thrown
-42/0 | Single.NegativeInfinity | Double.NegativeInfinity | Exception thrown
0/0 | Single.NaN | Double.NaN | Exception thrown
Best for | | Fast calcs, infinity/NaN/-0 support, large/small numbers| base 10 accuracy | 

## Rounding Puzzle

### Puzzle
```c#
[TestMethod]
//[ExpectedException(typeof(Exception))]
public void RoundingFrom5()
{
	var x = Math.Round(3.5);
	//Assert.AreEqual(3,x);
	//Assert.AreEqual(4,y);
	var y = Math.Round(4.5);
	//Assert.AreEqual(4,y);
	//Assert.AreEqual(5,y);
	var z = Math.Round(3.25,1);
	//Assert.AreEqual(3.2, z);
	//Assert.AreEqual(3.3, z);
}
```
### Answer

```c#
[TestMethod]
//[ExpectedException(typeof(Exception))]
public void RoundingFrom5()
{
	var x = Math.Round(3.5);
	//Assert.AreEqual(3,x);
	Assert.AreEqual(4,y);
	var y = Math.Round(4.5);
	Assert.AreEqual(4,y);
	//Assert.AreEqual(5,y);
	var z = Math.Round(3.25,1);
	Assert.AreEqual(3.2, z);
	//Assert.AreEqual(3.3, z);
}
```

* By default, .NET rounds midpoint numbers to the nearest even number!

You can specify what type of midpoint rounding to use by passing the MidPointRounding enum to the Round method. 

### Bonus: Floating Point Equality

* Solution 1: Round before comparison
	* Introduces midpoint issues (4.999 compared to 5.001)
	* Makes sense at decision points where roudning is logical and the desired absolute difference, not relative precision is known
* Solution 2: Use hard-coded difference (epsilon value). Specifiy precision!
	* Avoids midpoint issue
	* Makes sense when desired absolute (not relative) precision is known
* SOlution 3: Relative precision
	* Ideally, compare specific number of digits, regardless of scale
	* Works, except impossible near zero
		* Can't tell a small offset number from rounding error

## Top of the Type Puzzle

### Puzzle

```c#
[TestMethod]
//[ExpectedException(typeof(Exception), AllowDerivedTypes=true)]
public void TopOfInteger()
{
	int top = int.MaxValue;
	int next = top + 1;
	//Assert.IsTrue(next < top);
	//Assert.IsTrue(next == top);
	//Assert.IsTrue(next > top);
	//Assert.IsTrue(next < 0);
	//Assert.IsTrue(next == 0);
	//Assert.IsTrue(next > 0);
}

[TestMethod]
//[ExpectedException(typeof(OverflowException), AllowDerivedTypes=true)]
public void TopOfDecimal()
{
	decimal top = decimal.MaxValue;
	decimal next = top + 1;
	//Assert.IsTrue(next < top);
	//Assert.IsTrue(next == top);
	//Assert.IsTrue(next > top);
	//Assert.IsTrue(next < 0);
	//Assert.IsTrue(next == 0);
	//Assert.IsTrue(next > 0);
}

[TestMethod]
//[ExpectedException(typeof(Exception), AllowDerivedTypes=true)]
public void TopOfDouble()
{
	double top = double.MaxValue;
	double next = top + 1;
	//Assert.IsTrue(next < top);
	//Assert.IsTrue(next == top);
	//Assert.IsTrue(next > top);
	//Assert.IsTrue(next < 0);
	//Assert.IsTrue(next == 0);
	//Assert.IsTrue(next > 0);
}
```

### Answer

```c#
[TestMethod]
[ExpectedException(typeof(Exception), AllowDerivedTypes=true)]
public void TopOfInteger()
{
	int top = int.MaxValue;
	int next = top + 1;
	Assert.IsTrue(next < top);
	//Assert.IsTrue(next == top);
	//Assert.IsTrue(next > top);
	Assert.IsTrue(next < 0);
	//Assert.IsTrue(next == 0);
	//Assert.IsTrue(next > 0);
}

[TestMethod]
[ExpectedException(typeof(OverflowException), AllowDerivedTypes=true)]
public void TopOfDecimal()
{
	decimal top = decimal.MaxValue;
	decimal next = top + 1;
	//Assert.IsTrue(next < top);
	//Assert.IsTrue(next == top);
	//Assert.IsTrue(next > top);
	//Assert.IsTrue(next < 0);
	//Assert.IsTrue(next == 0);
	//Assert.IsTrue(next > 0);
}

[TestMethod]
//[ExpectedException(typeof(Exception), AllowDerivedTypes=true)]
public void TopOfDouble()
{
	double top = double.MaxValue;
	double next = top + 1;
	//Assert.IsTrue(next < top);
	Assert.IsTrue(next == top);
	//Assert.IsTrue(next > top);
	//Assert.IsTrue(next < 0);
	//Assert.IsTrue(next == 0);
	Assert.IsTrue(next > 0);
}
```
* Double or Single
	* Adding 1 is effectively adding zero at the range of 1E308
	* Adding sufficiently large value results in infinity
* Decimal
	* throws OverflowException
	* Ignores "Check for arithmetic overflow" setting.
* Integer 
	* In C#, check for arithmetic overflow is turned off by default
	* Value 'rolls over' to the minimum value
	* For C#, turn it on in Build/Advanced for project
	* Use "Checked" blocks to do overflow checks on specified code. 

# Methods and Overload Puzzles

Benefits of Method Overloading
* Reduce the surface area of your API
* Simplifies Intellisense and help
* Allow methods to have the best possible name
* Allows default parameters
* Improves performance when type specific overloads offered
	* Particularly for value-types that would be boxed as object parameters

## Date Arithmetic Puzzle

### Context

```c#
public DateTime NextClosingDate(int dayOffset)
{
	if(dayOffset > 28) throw new ArgumentOutofRangeException();
	DateTime today = DateTime.nNow;
	if(today.Day >= dayOffset)
	{
		var nextMonth = today.AddMonths(1);
		return new DateTime(nextMonth.Year, nextMonth.Month, dayOffset);
	}
	return new DateTime(today.Year, today.Month, dayOffset);
}
```
* Date Arithmetic is common
	* in this case, determining the closing date of a billing cycle

### Puzzle

```c#
[TestMethod]
//[ExpectedException(typeof(Exception)]
public void DateArithmeticTest()
{
	var newYears = new DateTime(2012, 1, 1);
	for (int i = 0; i < 365; i++)
	{
		var date = newYears.AddDays(i);
		var newDate = IncrementByYear(date);
		//Assert.AreEqual(newDate.Year, initialDate + 1);
		//Assert.AreNotEqual(newDate.Year, initialDate.Year + 1);
	}
}
static DateTime IncrementByYear(DateTime d)
{
	return new DateTime(d.Year + 1, d.Month, d.Day);
}
```
* Cycling through the days in the year
	* Will it work?
	* Does this code refer to next year?

### Answer

This example is special, because code like this cost a company over a million dollars. 

```c#
[TestMethod]
[ExpectedException(typeof(ArgumentOutOfRangeException)]
public void DateArithmeticTest()
{
	var newYears = new DateTime(2012, 1, 1);
	for (int i = 0; i < 365; i++)
	{
		var date = newYears.AddDays(i);
		var newDate = IncrementByYear(date);
		//Assert.AreEqual(newDate.Year, initialDate + 1);
		//Assert.AreNotEqual(newDate.Year, initialDate.Year + 1);
	}
}
static DateTime IncrementByYear(DateTime d)
{
	return new DateTime(d.Year + 1, d.Month, d.Day);
}
```
* This code throws an exception
	* Because adding a year to February 29, 2012 is not a valid date
* This issue caused much of Azure to crash in February 2012.

#### Avoid Date Arithmetic Problems

* Don't do any date arithmetic you don't absolutely have to do
	* Awlays use tested and frequently used code if its available (framework methods)

```c#
[TestMethod]
[ExpectedException(typeof(ArgumentOutOfRangeException)]
public void DateArithmeticTest()
{
	var newYears = new DateTime(2012, 1, 1);
	for (int i = 0; i < 365; i++)
	{
		var date = newYears.AddDays(i);
		var newDate = date.AddYears(1);
		Assert.AreEqual(newDate.Year, date.Year + 1);
	}
}
```

#### Fixing our context problem

* The original code works, but the staff is complaining that sometimes they have no work for 3 days. 



