# JavaScript coding standards / best practices

#####Overview
This document’s primary motivation is two-fold 1) code consistency and 2) best practices.  By maintaining consistency in coding styles and conventions, we can ease the burden of code maintenance, and mitigate risk of breakage in the future. By adhering to best practices, we ensure optimized page loading, performance and maintainable code.

#####Pillars of Front-end Development
1. Separation of presentation, content, and behavior (no inline JavaScript / CSS)
2. Re-use variables and functions as much as possible

###General Coding Principles
* **Strict code:**  We should use ”use strict”;  once per scope (either function or global scope).
* **Avoid Inline code:** Add JavaScript (and CSS) through external files.  We should avoid inline JavaScript / CSS like the plague.  Inline code is very difficult to maintain and debug.
* **Code Indentation:** Use consistent indentation.  Using the tab key this should be equivalent to four spaces.
* **Variable & Function Naming:** All variables and functions should use camelCase.  Use meaningful, unique names.  All Boolean variables should start with an ‘is’. All functions should start with a verb. jQuery objects should begin with a dollar sign ($). 
> Do not use _ (underbar) as the first or last character of a name. It is sometimes intended to indicate privacy, but it does not actually provide privacy. - Douglas Crockford

  ```
  var isValid = (test.value >= 4 && test.success);
  function updateSettings () { … }
  var $accountForm = $(‘form#account’);
  ```
  
* **Reusability:** Strive to create functions which can be generalized, take parameters, and return values. This allows for substantial code reuse and, when combined with includes or external scripts, can reduce the overhead when scripts need to change.  For example, instead of hard-coding a pop-window with window size, options, and url, consider creating a function which takes size, url, and options as variables.
 
###Efficient / Faster Code
Using a single var statement at the top of your functions is a useful pattern to adopt.  It has the following benefits: 
* Provides a single place to find / modify all local variables needed by the function 
* Re-using variables is much more efficient especially HTMLCollection objects *example: $(‘#myDiv’))* 
* Prevents logical errors when a variable is used before it’s defined (more below regarding variable hoisting)
* Helps you remember to declare variables and therefore minimize globals
* Is less code (to type and to transfer over the wire)

'''js
sub-optimal - it looks like shorter code, but performance costs add up quickly
for (var i < 0; i < myDogs.length; i++) {
    $('div.dogList').css('background','red');
    $('div.dogList').append('<h1'>+myDogs[i]+'</h1>');
}
 
// fast, efficient, maintainable
var i,                                      // don't declare i within the loop
    tempList ='',
    myDogs = ['spot','stinky'],    
    $dogList = $('div.dogList');            // re-use variables
 
for (i = myDogs.length; i--;) {             // speed improvement when possible
    tempList += '<h1>'+myDogs[i]+'</h1>';   // DOM manipulation is expensive, build your data, then append it
}
$dogList.css('background','red').append(tempList);
'''

###Writing Comments
You have to comment your code, even if it’s unlikely that someone other than you will ever touch it.  You shouldn’t go overboard commenting the obvious: every single variable or every line.  But you usually need to document all functions, their arguments and return values, and also any interesting or unusual algorithm or technique. 
Think of comments as hints to future readers; the readers need to understand what your code does without reading much more than just the comments and the function and property names.  When you have, for example, five or six lines of code performing a specific task, the reader can skip to the code details if you provide a one-line description describing the purpose of the code and why it’s there.

###Code Documentation
Just like good comments, code documentation can be invaluable to other developers (and you) when reading/debugging your code later.  Documentation can also be leveraged by tools such as JSDocs to create API documentation.
Basic format:
 
 '''js
 /**
 *
 * Multiplies two numbers
 *
 * @method multi
 * @param {Number} a First number input
 * @param {Number} b Second number input
 * @return {Number} The two inputs multiplied
 */
 
multi: function (a, b) {
    return a * b;
}
 '''

Tag definitions (see JSDocs for more):
* @namespace – The global reference that contains your object
* @class – Used to mean either an object or a constructor function
* @method – Defines a method in an object and specifies the method name
* @para – Lists the arguments that a function takes. 
* @return – Like @param, only it describes the value returned by the method

###Variable Hoisting
JavaScript enables you to have multiple var statements anywhere in a function, and they all act as if the variables were declared at the top of the function.  This behavior is known as hoisting.  This can lead to logic errors when you use a variable and then you declare it further in the function.  For JavaScript, as long as a variable is in the same scope (same function), it’s considered declared, even when it’s used before the var declaration.  


###Avoid Implied Typecasting
JavaScript implicitly typecasts variables when you compare them.  That’s why comparisons such as:
* *false == 0 or “” == 0  return true.*
To avoid confusion caused by the implied typecasting, always use the === and !== operators that check both the values and the type of the expressions you compare.  *The **only** exception is when checking for undefined and null by way of null.*

'''js
// incorrect
var zero = 0;
if (zero == false) // this will return true
 
// correct
if (zero === false) // this will return false
 
// Check for both undefined and null values, for some important reason.
undefOrNull == null;
'''

###JavaScript debugging tools

Use *console.log* to debug instead of *alert()* – it’s much more powerful and there is much more you can do with it see Developer Tools tutorials.  It will also be less abrasive to others developing in the same sandbox.  Just don’t forget to comment it out or (preferably) remove it, before you commit your changes.  Note: In IE console.log won’t be available unless the developer tools window is open.
Even with the best of validators, inevitably browser quirks will cause issues.  It’s important to have debug tools available.  I recommend developing for Google Chrome or Firefox first, then Internet Explorer and Safari since tweaks and conditional comments might be needed.

The following is a list of helpful debuggers and speed analyzers:
* Chrome: [Developer Tools](https://developer.chrome.com/devtools/index) (built-in just hit F12)
* Firefox: [Firebug](http://getfirebug.com/)
* Opera: [Dragonfly](http://www.opera.com/dragonfly/)
* Great way to compare script speed: [JSPerf](http://jsperf.com/)
* Quick way to test a function:  [JSFiddle](http://www.jsfiddle.net/)




