# Learning a new language

## techniques

* translate main concepts - for example
  - variables
  - methods
  - loop
  - arguments/parameters
  - operators
  - arrays
  - conditionals

  VERY IMPORTANT - printing and logging and error messages

* how to - for example
  - TDD
  - which frameworks
  - gems - NPM in js
  - patterns/idioms

* research/answering questions
  - Google
    - stack overflow
    - documentation
    - videos (better when short)
  - People
    - coach
    - peers


``` ruby
def add(a,b)
a + b
end
a and b are arguments

c = 3, d = 4

add(c,d)

c and d are arguments
```

## python Fizzbuzz

``` python
def fizzbuzz(n):
    arr = []
    i = 1
    while i <= n:
        if i % 3 == 0 and i % 5 == 0:
            arr.append("FizzBuzz")
        elif i % 5 == 0:
            arr.append("Buzz")
        elif i % 3 == 0:
            arr.append("Fizz")
        else:
            arr.append(i)
        i += 1
    return arr


```

``` C
using System;
using System.Collections.Generic;

public class FizzBuzz
{
	public static string[] GetFizzBuzzArray(int n)
	{

  if (n <= 0)
      throw new ArgumentOutOfRangeException();
    string[] result = new string[n];
     for (int i = 1; i <= n; i++)
    {
    if (i % 3 == 0 && i % 5 == 0)
        result[i - 1] = "FizzBuzz";
      else if (i % 3 == 0)
        result[i - 1] = "Fizz";
      else if (i % 5 == 0)
        result[i - 1] = "Buzz";
      else
        result[i - 1] = i.ToString();
    }
    return result;


	}
}
```

JS - relies on {}
``` javascript
function add(a,b) {

}
```

Ruby - relies on end
``` ruby
def add(a,b)

end
```

Python - relies on indentation
``` python
def add(a,b):
  a fdsfdsaf
  afsdsad

end
```


## Types of language
* dynamic type (ruby, js)  can create a variable - can contain any type `a = 3`
* static type --> explicit type (me) `int a = 3`

## Overall process
* read about language - summary
* learn with a goal - fizzbuzz is good
* understand a structured piece of code - look at examples, look at returning a value, what starts and ends a function
* printing or logging
* get an error - google error and figure it out


## Javascript


ECMAScript 6 - ES6 - js standard v. 6.  Recommended way of doing it

ECMAScript - standard
6 - version

Interpreters created by browser devs.  Need to make sure conforms.

can translate into older version (e.g. Babel)

working with codebases - may not implement newer standard - so good to learn the other versions of JS. - ES2 and ES5

** Current try to use ES5 **

minify
