---
layout: post
title: "How to use exports in NodeJS"
date: 2012-06-04 16:58
comments: true
categories:
---

In any substantial project, it is necessary to separate your code in different files. Node.js implements the [CommonJS API standard to load modules](http://www.commonjs.org/specs/modules/1.0/) from other files. Using __exports__ can be a source of much confusion in Node.js. Let us explore how __exports__ works.

<!-- more -->

### Use Case 1: Exporting as an Object of functions

The most common use case is to export your functions using __exports__.

For example, I have a script that wants to use functions from another Node.js file.

``` javascript
// make_sandwich.js

var fridge = require ('./fridge');

function makeSandwich() {
  return {sandwich: fridge.bread() + ' ' + fridge.egg};
}

console.log(makeSandwich());
// => { sandwich: 'bread: 2 egg: 1' }
```

``` javascript
// fridge.js

exports.bread = function bread() {
  return 'bread: 2';
};

exports.egg = 'egg: 1';
```

Using __require__, Node.js will evaluate the file and load the functions defined in the __exports__ object.

Behind the scenes, __exports__ is just an object. Here is how it works.

``` javascript
// fridge.js

//var exports = {};

exports.bread = function bread() {
  return 'bread: 2';
};

exports.egg = 'egg: 1';

// exports = {
//              bread: function bread() { ... },
//              egg: 'egg: 1'
//           };
```

You can define variables and functions in the __exports__ object. It is exposed and can be used in other files when it is __required__.

What __require__ does is to return the __exports__ object defined in the file.

``` javascript
// make_sandwich.js
// subtituting require for the exports object

var fridge = require ('./fridge');

// var fridge = {
//                bread: function() { ... },
//                egg: 'egg: 1'
//              };

...
```

The variable fridge has the properties defined in the __exports__ object.

You don't have to use the same function name when exporting.

``` javascript
// fridge.js

// originally the function is called bread
exports.bread = function wholemealBread() {
  return 'bread: 2';
};

exports.egg = 'egg: 1';
```

It returns the same result. Only the name defined in the __exports__ object is used.

### Use Case 2: Exporting with module.exports

__module.exports__ can be used to export the interfaces directly. For example,

``` javascript
// bread.js

module.exports = function() {
  return 'bread: 2';
};
```

``` javascript
// make_sandwich.js

var bread = require ('./bread');

function makeSandwich() {
  return {sandwich: bread() + ' with some eggs'};
}

console.log(makeSandwich());
// => { sandwich: 'bread: 2 with some eggs' }
```

When is this pattern used? It is used when you want to expose __one__ variable. It could be a function, string, or any valid variable.

The code below is valid and will yield the same result if you call bread as a variable.

``` javascript
// bread.js
// assigning module.exports to a string

module.exports = 'bread: 2';
```

``` javascript
// make_sandwich.js

var bread = require ('./bread');

function makeSandwich() {
  return {sandwich: bread + ' with some eggs'};
}

console.log(makeSandwich());
// => { sandwich: 'bread: 2 with some eggs' }
```

The code below is the equivalent of the first example of fridge.js using __module.exports__.

``` javascript
// fridge.js

module.exports = {
  bread: function bread() {
    return 'bread: 2';
  },

  egg: 'egg: 1'
};
```

So what happens to the __exports__ variable then? If __module.exports__ is defined, the __exports__ object will be ignored. For example, if I add another definition of bread

``` javascript
// bread.js

module.exports = 'bread: 2';
exports.bread = 'wheat bread: 4';
```

``` javascript
// make_sandwich.js

var bread = require ('./bread');

function makeSandwich() {
  return {sandwich: bread + ' with some eggs'};
}

console.log(makeSandwich());
// => { sandwich: 'bread: 2 with some eggs' }
```

It uses the definition defined in __module.exports__.

What if you assign a function to __exports__?

``` javascript
// bread.js

exports = 'bread: 2';
```

``` javascript
var bread = require ('./bread');

console.log(bread);
// => {}
```

Why didn't __exports__ have the assigned string value? __exports__ is actually a property inside the __module__.

When you assign a function or string directly to __exports__, Javascript will treat __exports__ as another variable. Not the __exports__ property inside the module.

When you change the property of __exports__, Javascript will access the __exports__ property inside the module and apply the changes.

A deeper explanation can be found in this [Stackoverflow question](http://stackoverflow.com/questions/518000/is-javascript-a-pass-by-reference-or-pass-by-value-language).
