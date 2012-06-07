# My coding conventions

This is a collection of conventions I apply when coding on my [projects](/js-coder/). I plan to write down and publish my conventions for HTML, CSS, PHP and Ruby, too.

**Note**: The document is just a very early draft of my coding conventions for JavaScript. I commented out a lot of the stuff I haven't quite decided about or was too lazy to write about. Stay tuned for more. :)

## JavaScript

### General stuff

 - Put a semicolon after every statement. Do not rely on automatic semicolon insertion (ASI).
 - Always use `===` instead of `==`. There is just one exception: Use `==` to compare if something is either `undefined` or `null`.
 - Always prefer clear code over short code. The following may look incredibly smart to you but it will confuse a lot people:

    ```javascript
    if (~string.indexOf(substring)) doStuff();
    ```
    
    A lot better to understand and easier to read is this:
    
    ```javascript
    if (string.indexOf(substring) > -1) doStuff();
    ```
    
 - Wrap parentheses around operators if it helps to understand the code better. That's a lot of times the case when using the `!` operator.
 - Use literals instead of constructor functions when creating *strings*, *numbers*, *booleans*, *regular expressions*, *arrays* and *objects*. 
    
     Good:    

     ```javascript
     var number = 42;
     var string = 'Hello world';
     var boolean = true;
     var regExp = /hello world/i;
     var array = [1, 2, 3];
     var object = {};
     ```
     
     Bad:
 
     ```javascript
     var number = new Number(42);
     var string = new String('Hello World');
     var boolean = new Boolean(true);
     var regExp = new RegExp('hello world', 'i')
     var array = new Array(1, 2, 3);
     var object = new Object();
     ```
    
    Why? Here are three good reasons:
    - All of these create a wrapper object around the actual value, `typeof new Number(42)` will be `'object'` not `'number'`. These wrapper objects make the `typeof` operator even more useless.
    - The `Array` constructor is a bit odd. You can take advantage of this (see below) but most of the time it's not what you want.
    
    ```javascript
    new Array(5); // Returns an array with 5 fields that have the value `undefined`.
    new Array(5, 6); // Returns an array containing `5` and `6`.
    ```
    
    - The constructor functions give you rarely an advantage (see below), but are longer than the literals.

    There are two exceptions:
    - Use the RegExp constructor function when creating a regular expression that contains the value of a variable. 
    - Use the Array constructor function to create an array with a specific length that only contains empty elements. 
    

 - Wrap all your code in an IIEF (immediately invoked function expression).
     - It will prevent unwanted global variables which is great for code that will be used on third party sites.
     - It’s also great for minification since the arguments passed to the function will be renamed to shorter names.
    
     ```javascript
     !function (window, document, undefined) { // Adjust these parameters to your needs.
       // Your code here.
     }(window, document);
     ```

### Comments

 - Do **not** comment obvious things. You will waste your time and the time of the people that will read your code.
 - Use Markdown like formatting for comments: The backtick character for code references and `*` to emphasize something.
 - You can prefix your comments with `TODO: ` or `BUG: `. This will make it easier for people to contribute to your project because they will know what they can fix or add. It's also nice because you can easily search through bugs and todos by using your editor's search function.

### Quotes

 - Use single quotes instead of double quotes. This will make it easier to work with HTML in JavaScript because double quotes are what everyone uses in HTML.
 - Only quote object keys if the key is a reserved word.

### Whitespace

 - Lines should be no longer than 80 characters.
 - Use tabs as indentation. This allows all developers to use the indentation width they prefer. 
 - Put one space before `{` and `(` if it’s not a function call. Put one space before and after any operator except the comma operator. Obmit the trailing space if a `(` is following the operator for readability. Put one space after every comma.

### Blocks

 - Only use curly brackets if the following code has more than one line. This will make it easier to scroll through the file.
 
    Good:
  
    ```javascript
    if (condition) doStuff();
    
    if (condition) {
      doThis();
      doThat();
    }
    
    ```
    
    Bad:

    ```javascript
    
    if (condition) {
      doStuff();
    }
    
    if (condition)
       doStuff();
    ```
    
 - Use the [K&R](http://en.wikipedia.org/wiki/Indent_style#K.26R_style) indent style:
   
    ```javascript
    if (condition) {
      // Some code.
    } else {
      // Some code.
    }

    try {
      // Some code.
    } catch (e) {
      // Handle error.
    }    
    ```
    
    If the `else` body only contains one line of code:
    
    ```javascript
    if (condition) {
      // Some code.
    } else oneStatement();
    ```

### Naming conventions

 - If a variable name contains several words, then you should follow the `camelCase` convention. That means capitalize the initial letter of all words, but leave the very first letter of the name lowercase. This also the way JavaScript’s built-in stuff is named: e.g. `getElementsByTagName`.
 - Capitalize the initial letter of constructor functions – `Constructor`. Again, this is also how JavaScript names the built-in constructor functions.
 - Capitalize all letters of constants – `CONSTANT`.
 - Try to prefix all boolean variables with `is`, `has` or similar prefixes.
 - Prefix variables that contain DOM node(s), or wrapper objects for DOM nodes e.g. a jQuery object, with the `$` sign – `var $body = document.body`

```javascript
var myVariable;
var ConstructorFunction = function () { /* ... */ };
var CONSTANT = 42;
var isActive = false;
var hasContent = true;
var $body = document.body;
var $paragraphs = jQuery('p');
```

<!--
### Code quality tools

 - jsPerf: To be written.
 - JSLint: To be written.
-->

### Variable declarations

 - You may use one var keyword for every variable assignment. There are good reasons to do so: http://benalman.com/news/2012/05/multiple-var-statements-javascript/ However, only use one var keyword if you are not assigning something to variables, but just declaring them: `var a, b, c;` instead of `var a; var b; var c;`.
 - Define variables, that are used through-out the scope, at the beginning of the scope. Define the other variables when they are used, for example the index of a loop.
 - If you use global variables, specify them explicitly. `window.globalVariable = 42;`

<!--
### Type casting

 - **Note**: Think twice about casting to a boolean. A lot of times you won’t need to cast to a boolean because of JavaScript’s truthy and falsy values.
-->

<!--
### Type checks

 - …
-->

### Functions

 - Prefer function expressions over function declarations.

<!--
### Prototypes

 - **Note on notation**: `#` stands for `.prototype.`. E.g.: `Array#push` ➞ `Array.prototype.push`. See http://mathiasbynens.be/notes/javascript-prototype-notation
-->

### Regular expressions

<!-- - Use RegExp#exec instead of String#match. -->
 - In JavaScript `.` matches everything except the new line character. Use `(.|\n)` to truly match everything.

### Other stuff

 - Rethink your problem if you want to use `eval`. While there are certainly a few use cases for `eval`, the chance that yours is one of them is very small.
 - Always cache the length when iterating over an array, it can be a huge performance win.

    ```javascript
    for (var i = 0, l = array.length; i < l; i++) {
      // Your code.
    }
    ```