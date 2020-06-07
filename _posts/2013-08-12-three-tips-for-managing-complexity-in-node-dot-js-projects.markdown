---
layout: post
title: "Three tips for managing complexity in Node.js projects"
date: 2013-08-12 05:33
comments: true
categories:
---

![Complexity. By DigitalMums](http://farm7.staticflickr.com/6045/6310508350_8deb30151d_n.jpg)

After working on various Node.js projects, I want to share three simple tips which helps to reduce complexity. If you are building an [Express](http://expressjs.com) or [Restify](https://github.com/mcavage/node-restify) project, ususally the framework does not tell you how to structure your application. You can put everything in one mega file, or you can(and should) extract different functionalities out.

<!-- more -->

## Folder structure

A clear folder structure helps you to find code. It is a form of documentation. There is no right or wrong. Do what suits your project best. I find the suggestions from [this discussion](https://gist.github.com/viatropos/1398757) worth applying. In general I like to separate the main parts of the app:

 - application logic
 - assets
 - view templates
 - configuration
 - tests
 - logging
 - third party vendor code

As long as your folder structure separates these functions, it is good enough.

## Index method

For example, when you are writing an API application, your controllers will increase as you add more functionality. You can break the controllers into their own file and place them all in a folder.

```
|-- controllers
|   |-- threads.js
|   |-- posts.coffee
|   |-- users.coffee
|   |-- polls.coffee
```

Now you need to make them available to the server. You can make use of __index.js__. Place an __index.js__ in your controller folder

```
|-- controllers
|   |-- index.js
|   |-- threads.js
|   |-- posts.coffee
|   |-- users.coffee
|   |-- polls.coffee
```

Write something like this. It finds all the files in the folder and adds them to the exports namespace.

``` javascript
var fs = require('fs');

/*
 * Modules are automatically loaded once they are declared
 * in the controller directory.
 */
fs.readdirSync(__dirname).forEach(function(file) {
  if (file != 'index.js') {
    var moduleName = file.substr(0, file.indexOf('.'));
    exports[moduleName] = require('./' + moduleName);
  }
});
```

Now you can get all the controllers in one variable. Which allows you to hook it to the server.

``` javascript
var controllers = require('./app/controllers');

server.get('/v1/accounts', controllers.accounts.index);
```

This technique helps to reduce the number of requires you need to write. Plus, making use of __index.js__ makes it look cleaner as you only need to write the folder name. In general, when you face a situation where you have many similar functions, extract them into separate files, and use an index file to bind them together.

## Passing the reference to an external file

If you haven't noticed in Express already, there are several [middleware](http://expressjs.com/api.html#middleware) initialization function calls when you initialize an Express server.

``` javascript
app.use(express.json());
app.use(express.urlencoded());
app.use(express.multipart());
```

It quickly gets unwieldy as your projects grow. My ideal case is to initialize all the middleware configurations in a separate file. To do that, I pass the server reference over.

On your former app file, use a single require for the external file.

``` javascript
require('../config/middleware')(app);
```

Export a single function which initializes all the middleware.

``` javascript
module.exports = function(app) {
  app.use(express.json());
  app.use(express.urlencoded());
  app.use(express.multipart());
});
```

This way, your app file will be much cleaner. It is a useful technique for extracting out large code blocks. Hope these three tips helps!
