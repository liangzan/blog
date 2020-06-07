---
layout: post
title: "How to write a command line application in Node.js"
date: 2012-07-30 06:44
comments: true
categories:
---

What is a [command line](http://en.wikipedia.org/wiki/Command-line_interface) application? Usually it means a program that is ran via the command line. It can be a simple script that changes your working directory. Or it can be a complicated program that has a multitude of options and arguments. Examples of command line applications are [mutt](http://www.mutt.org), [irssi](http://irssi.org), [htop](htop.sourceforge.net).

<!-- more -->

You can write a command line application using most programming languages. Today, [dynamically typed](http://en.wikipedia.org/wiki/Type_system) programming languagues are popular choices for writing such scripts. Reason being they tend to be shorter to write and easy to learn. A short startup time is also important. If a script only takes a fraction of a second to run, and startup time takes 5s, it makes the script feels slow. Examples are the [JVM](http://stackoverflow.com/questions/844143/why-is-the-jvm-slow-to-start) languages. [Node.js](http://nodejs.org) fits the bill for all these cases. Let's take a look to see how we can write a command line application using [Node.js](http://nodejs.org).

### Library shopping

In [Node.js](http://nodejs.org), there are many libraries that can help you to write a command line application. Which to choose? There are a few main features that all these libraries provide:

 - Option parsing
 - Interactive prompt
 - Usage/help message helpers
 - Interface elements(progress bars, coloring, password input, spinners, etc)

Let us classify the libraries.

#### All in one

These libraries provides all the major features. [CLI](https://github.com/chriso/cli) provides input parsing as well.

 - [Celeri](https://github.com/crcn/celeri)
 - [CLI](https://github.com/chriso/cli)
 - [Commander.js](https://github.com/visionmedia/commander.js)
 - [Nomnom](https://github.com/harthur/nomnom)

#### Specialist option parsers

These libraries only do option parsing

 - [Nopt](https://github.com/isaacs/nopt)
 - [Optimist](https://github.com/substack/node-optimist)

#### Interactive prompt

Interactive prompt is like a sub shell. You can run custom commands in it. Good examples are node interactive shell and [irb](http://en.wikipedia.org/wiki/Interactive_Ruby_Shell).

 - [Prompt](https://github.com/flatiron/prompt)

#### Interface elements

 - [Ansi](https://github.com/TooTallNate/ansi.js)
 - [Charm](https://github.com/substack/node-charm)
 - [Colors](https://github.com/Marak/colors.js)
 - [Term-CSS](https://github.com/visionmedia/node-term-css)

## Components of a command line application

We can break a command line application into separate components. Namely, the entry point, arguments and options, output, documentation, configuration and distribution. I'll be using the [twitter search app](https://github.com/liangzan/cli-app) as an example for illustrating the components.

### Entry point

Entry point is the executable script which starts the program. Typically there're 3 ways to run a command line application.

One way is to indicate both the program loader and the path to the script. Assuming you have installed the example application and is now on the project root.

``` sh
node bin/birdie
```

A shorter way is to indicate just the path to the script. Don't forget to make the script executabe by changing the permissions. Another caveat is, if the path to the program loader is wrong, the program won't run correctly.

``` sh
bin/birdie
```

It is possible to do that because of the [shebang](http://en.wikipedia.org/wiki/Shebang_(Unix)) line present on the first line of the script. The shebang line hints to the operating system which program loader to use. You are encouraged to use it in your own scripts.

Here is an example of the shebang line used in the example application

``` sh
#!/usr/local/bin/node
```

It can be even shorter if the script is placed in the [PATH](http://en.wikipedia.org/wiki/PATH_(variable) directories.

``` sh
birdie
```

The operating system will find the script and run it. The only caveat is you have to install the script there.

If your program affects the whole system, install it in the [PATH](http://en.wikipedia.org/wiki/PATH_(variable)). A good example is [NPM](http://npmjs.org) and [Coffeescript](http://coffeescript.org).

By convention, scripts meant to be executed are placed in the __bin__ directory of your project.

### Arguments and options

A complex command line application usually takes in arguments and options. An option(or flag or switch) is used by the program to modify the operation. In Unix like systems, it is usually indicated by a hyphen-minus followed by a letter or word. Using __node__ as an example.

``` sh
node -v
```

__node__ is the command and __-v__ is the option.

For the example application, I chose [Optimst](https://github.com/substack/node-optimist) because of its simplicity. Accessing the options is very easy as illustrated below

``` javascript
var resultTypeOpt = argv.r || argv.resulttype || 'mixed';
```

[Optimist](https://github.com/substack/node-optimist) parses the option and makes it accessible via the __argv__ object. For example when running the command

``` sh
node bin/birdie -r foo bar
```

[Optimist](https://github.com/substack/node-optimist) parses the arguments and options and returns __argv__ in an object like this:

``` javascript
{
  _: [ 'bar' ],
  '$0': '/path/to/bin/birdie',
  r: 'foo'
}
```

The __r__ option value is placed under the __r__ property and the rest of the arguments are in the underscore array.

### Output

If your command line application need to show output, you can do so using plain old [console.log](http://nodejs.org/docs/latest/api/stdio.html).

Another option is to use [Winston](https://github.com/flatiron/winston). [Winston](https://github.com/flatiron/winston) is a logging libarary that can be used to log to [STDOUT](http://en.wikipedia.org/wiki/Standard_streams). It is almost the same as __console.log__.

``` javascript
winston.log('foo');
```

It comes with logging levels like __info__, __warn__ and __error__ which also comes in color. Applying them is as easy as

``` javascript
winston.info('bar');
```

There're plenty of other features to [Winston](https://github.com/flatiron/winston). Feel free to explore the library.

If you want to add colors to your text output, you can make use of [Colors.js](https://github.com/Marak/colors.js). Applying colors is very easy.

``` javascript
console.log("foo".green);
```

That will output the text in green. [Colors.js](https://github.com/Marak/colors.js) does that by overriding the [Javascript String](https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/String/) [prototype](http://stackoverflow.com/questions/572897/how-does-javascript-prototype-work). So if you are also [overriding the String prototype](http://stackoverflow.com/questions/892595/javascript-prototypal-inheritance), be careful when using [Colors.js](https://github.com/Marak/colors.js).

### Documentation

Typically a command line application comes with documentation in the form of [man pages](http://en.wikipedia.org/wiki/Man_page). In fact [NPM](http://npmjs.org) does comes with man pages. [NPM](http://npmjs.org) generates its man pages via the [RonnJS](https://github.com/isaacs/ronnjs) package. It generates man pages from a [Markdown](http://daringfireball.net/projects/markdown/) file. You can also use the [Ronn](https://github.com/rtomayko/ronn) ruby gem directly. [DailyJS](http://dailyjs.com) did a good [write up](http://dailyjs.com/2012/02/16/unix-node-community/) on generating man pages in [Node.js](http://nodejs.org).

Your application should also come with HTML documentation.

### Configuration

If your command line application has a lot of options, a configuration file could help. Let's take a look at [Mocha](https://github.com/visionmedia/mocha). Mocha has a __mocha.opts__ file which you can state the common options that you want included.

Adding a configuration file feature is as easy as reading from file. But do note that by convention, the option stated on the command line has higher precendence. Which means if I stated an option to print the output in green, but the option in the configuration file says it should be red, you should render it in green. Here's the conventional order of precedence in overriding configurations.

 1. Command line options
 2. Configuration file options
 3. Application default options

### Distribution

If you want to distribute your command line application, using [NPM](http://npmjs.org) is the best option. [NPM](http://npmjs.org) helps to install your executable script if you [add the bin option](http://npmjs.org/doc/json.html#bin) in your package.json manifest.

If the user installs the package locally like this

``` sh
npm install foo
```

The program's executable script would be found in 2 places:

 1. ./node_modules/.bin
 2. ./node_modules/foo/bin

NPM adds a symbolic link from the __.bin__ directory to your script.

If the user chose to install your package globally like this:

``` sh
npm install -g foo
```

The package will install your executable script under the [PATH](http://en.wikipedia.org/wiki/PATH_(variable) directories. More information can be found in the [global vs local](http://blog.nodejs.org/2011/03/23/npm-1-0-global-vs-local-installation/) npm blog article.

## Conclusion

Despite the relative youth of the [Node.js](http://nodejs.org) eco-system, all the pieces required for building a command line application are already there. Feel free to give the [example application](https://github.com/liangzan/cli-app) a run to see how easy it is to build one in [Node.js](http://nodejs.org).
