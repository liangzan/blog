---
layout: post
title: "How to use Coffeescript in your Node.js project"
date: 2012-07-22 17:10
comments: true
categories:
---

[Coffeescript](http//coffeescript.org) is a popular language that compiles to Javascript. Many developers prefer to write in Coffeescript due to its elegant syntax. If you want to use Coffeescript to write your [Node.js](http://nodejs.org) project, you can do so easily. I'm going to write [a small project](https://github.com/liangzan/use-coffeescript) as an example of how to use Coffeescript in your project.

<!-- more -->

## Querying for the weather
We will make use of Node.js to query for the weather information using Google's unofficial weather API. It will be a simple command line script.

## Installing Coffeescript

There are a variety of ways to install Coffeescript. Let's make use of __package.json__ to install our packages. You could add Coffeescript to your list of dependencies in __package.json__

``` javascript
"dependencies": {
  "coffee-script": "~1.3.3",
}
```

And run

``` sh
npm install
```

to install it. If not you can install it directly with __npm__.

``` sh
npm install coffee-script
```

You can also install it via your package manager. Most of them should provide a Coffeescript package.

## Compiling Coffeescript

When you are done writing your code, you have to compile the Coffeescript file to Javascript. Compiling is as simple as running the following command

``` sh
coffee -c foo.coffee
```

That will output a __foo.js__.

Once you have a lot of Coffeescript files, compiling by hand becomes tedious. Currently many open source Node.js projects prefer to use [GNU Make](http://www.gnu.org/software/make/). You can make use of __make__ to compile your Coffeescript files into Javacript. Below is an example.

``` sh
compile:
	@find src -name '*.coffee' | xargs ./node_modules/.bin/coffee -c -o bin
```

Save the script as __Makefile__. This __compile__ taget assumes three things. One, your Coffeescript files are under the __src__ directory. Two, your __coffee__ executable is found under __node_modules__. Three, you want your compiled Javscript files to be saved under the __bin__ directory. Do replace if otherwise. Currently the convention is to put the Coffeescript files under the __src__ directory. And then compile to the __lib__ or __bin__ directory. You can compile all your Coffeescript files by running the command below.

``` sh
make compile
```

The compiled files can be found under the __bin__ directory.

## Automating the compilation

As you develop the code, you will find the process of compilation tedious. There are ways to automate the compilation.

### Guard

[Guard](https://github.com/guard/guard) is a [Ruby](http://ruby-lang.org/) [gem](http://rubygems.org) that can run scripts on file changes.

Below is a sample Guardfile that watches Coffeescript files for changes and compiles them.

``` ruby
guard :compile-coffeescript do
  watch(%r{^src/*.coffee$}) { `make compile` }
end
```

### Compile on run

Another way is to compile before running the script. We can add a run target in the __Makefile__ with a __compile__ dependency. You can add the below to your __Makefile__ to compile to Javascript before running the script.

``` sh
run: compile
	node ./bin/index.js singapore
```

You can run the following to see the results.

``` sh
make run
```

The only caveat is you cannot pass arguments to the script elegantly. There is a [workaround](http://stackoverflow.com/questions/2214575/passing-arguments-to-make-run) though.

## Conclusion

Using Coffeescript to develop your Node.js project is simple. Simply setup your project to compile your source and that is it.
