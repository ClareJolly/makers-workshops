# Javascript Module Patterns

## Encapsulation in JS

```
                    Encapsulation
                    -------------

  constructor/prototype      ***** module pattern *****    


```              

## Constructor/Prototype functions
Constructor function + prototype (kind of like classes and class methods in Ruby)

Problems with this:
- :x: prototype (having to write it)
- :x: syntactically ugly
- :x: can't make private functions - can put _ before it but not really private - this is just to indicate to others

- :white_check_mark: advantage of using prototype = just exist in one place, not additionally creating the functions again on 'new'.  More memory efficient


```javascript
function Bank () {

}

Bank.prototype.deposit = function () {

}

Bank.prototype.fraudChecker = function () {
  //we want this to be private but it can't be
}

```

## Module pattern

** Immediately Invoked Function Expression (IIFE) **

- :white_check_mark: public
- :white_check_mark: private
- :white_check_mark: don't have to write prototype
- :white_check_mark: all contained in 1 place

```javascript

//anonymous function
var hi = function() {
  console.log('hi')
}

//returns a function
(function() {console.log('hi')})

//add brackets at the end to run the console.log
(function() {console.log('hi')})()

//Achieve privacy by subtly using this pattern

//IIFE - Immediately Invoked Function Expression


```

```javascript

(function(exports) {
  function exclaim(string) {
    return string + "!".repeat(exclamation_count())
  }

  //private as not assigned to global scope
  function exclamation_count() {
    return(2)
  }


  //assigning exclaim to global scope
  exports.x = exclaim
  //abc.y = exclamation_count
  })(this)

  //run
  x("hello, world!!")
  // >> hello, world!

  exclamation_count(2)
  // >> not defined
  // with the abc.y line, running y(2) returns >> 2

```

```javascript
//interrobang
(function(exports) {
  function interrobang(exclaim, question, string) {
    question_string = question(string)
    return exclaim(question_string)
    // could do return exclaim(question(string)) - not as readable
  }

  exports.interrobang = interrobang
})(this)
```
