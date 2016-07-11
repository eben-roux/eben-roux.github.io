---
layout: post
title: "JavaScript modules for C# developers"
date: 2016-07-09 12:00:00
comments: false
categories:
- design
tags:
- donejs
- stealjs
- canjs
---

I have decided to make use of [DoneJS](https://donejs.com/) for my web development.  In turn, DoneJS makes use of a host of other technologies.  The one that drew me to this environment was [CanJS](https://canjs.com/) as I had spent some 3 years using [Ember.js](http://emberjs.com/) whilst working for a former employer and I didn't quite like it.  After some investigation I decided on *CanJS* and that led me to *DoneJS*.

JavaScript is a rather odd language and the ecosystem is rather huge, as with the .Net environment.  There are just *so* many choices.  As a result I have largely ignored anything that did not directly relate to my work.  As such I ended up missing the entire JavaScript ***module*** train.  In the upcoming EcmaScript 6 [JavaScript Modules](http://exploringjs.com/es6/ch_modules.html) are pretty well defined.  Now, if you are familiar with JavaScript modules you may as well ignore this post.

## Dependencies in C\#

When adhering to the [dependency inverson principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle) of the [SOLID principles](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)) one would depend on an abstraction.  That abstraction is typically an interface:

~~~ c#
public interface IRandomNumberGenerator
{
	int Next(int minimum, int maximum);
}
~~~    

We could then make use of this abstraction in some class without worrying too much as to how it is going to be implemented:

~~~ c#
public class RandomNumberConsumer : IRandomNumberConsumer
{
	private readonly IRandomNumberGenerator _randomNumberGenerator;

	public RandomNumberConsumer(IRandomNumberGenerator randomNumberGenerator)
	{
		_randomNumberGenerator = randomNumberGenerator;
	}

	public bool Probability()
	{
		return _randomNumberGenerator.Next(0, 100) > 50;
	}
}
~~~

We have inverted the dependency by not depending on a specific implementation.  It is possible to have multiple implementations with each working in a slightly different way.  For instance:

~~~ c#
public class DefaultRandomNumberGenerator : IRandomNumberGenerator
{
	private static readonly Random _random = new Random(DateTime.Now.Millisecond);

	public int Next(int minimum, int maximum)
	{
		return _random.Next(minimum, maximum);
	}
}

public class DoubleRandomNumberGenerator : IRandomNumberGenerator
{
	private static readonly Random _random = new Random(DateTime.Now.Millisecond);

	public int Next(int minimum, int maximum)
	{
		return (_random.Next(minimum, maximum) + _random.Next(minimum, maximum)) / 2;
	}
}
~~~

Making use of an interface for our dependency inversion also means that we can test the interaction by specifying known values that result in a predictable result:

~~~ c#
[TestFixture]
public class RandomNumberConsumerFixture
{
	[Test]
	public void Should_return_true_for_values_above_50()
	{
		var generator = new Mock<IRandomNumberGenerator>();

		generator.Setup(m => m.Next(It.IsAny<int>(), It.IsAny<int>())).Returns(60);

		var consumer = new RandomNumberConsumer(generator.Object);

		Assert.IsTrue(consumer.Probability());
	}

	[Test]
	public void Should_return_false_for_values_below_51()
	{
		var generator = new Mock<IRandomNumberGenerator>();

		generator.Setup(m => m.Next(It.IsAny<int>(), It.IsAny<int>())).Returns(30);

		var consumer = new RandomNumberConsumer(generator.Object);

		Assert.IsFalse(consumer.Probability());
	}
}
~~~

We could then make use of a dependency injection container to map a specific implementation to the required interface.  To make things simpler I have added an `IRandomNumberConsumer` interface as well, even though most DI containers should be able to register concrete types without an interface.  In logical terms we would then register our components in our container as follows:

~~~ c#
var container = new MyDependencyContainerOfChoice();

container.WhenAskingForInterfaceType<IRandomNumberGenerator>().ReturnAnInstanceOf<DoubleRandomNumberGenerator>();
container.WhenAskingForInterfaceType<IRandomNumberConsumer>().ReturnAnInstanceOf<RandomNumberConsumer>();

Console.WriteLine(container.GetImplementationOf<IRandomNumberConsumer>().Probability());
~~~

The container will see that the `RandomNumberConsumer` implementation of the `IRandomNumberConsumer` requires an instance of `IRandomNumberGenerator` and will locate the type that should be returned and inject it into the `RandomNumberConsumer`.

This, of course, brings us to *how* any required instance is created.  Typically a container provides a default *lifestyle* for the instance.  In most cases the container will return a *singleton* so that each request for the implementation will return the same instance.  However, we may instruct the container to act a bit like a *factory* and set the lifestyle to `Transient` in order for the container to return a ***new*** instance of the requested implementation each time we request it.

These dependencies can exist in one or more files within one or more dependencies.  When we need a dependency we will simply reference the relevant assembly, either directly or using NuGet.

## Dependencies in JavaScript

*JavaScript does not work like this*.  Is is a dynamically typed language that is also weakly typed.

This means that, in essence, there can be *no* dependency inversion.  It is not possible to define an abstraction to depend on.  You would simply depend on the implementation and with some luck the implemetation that you selected would implement all the required public interface bits.

There is a certain elegance in the way JavaScript works in that you can override just about everything.  Execute the following in the console of your browser:

~~~ javascript
alert('hello world!');
~~~

The result is somewhat predictable.  And now this:

~~~ javascript
window.alert = function() {}
~~~

Let's give that *hello world* business another go, shall we:

~~~ javascript
alert('hello world!');
~~~

Well, that escalated quickly!  Absolutely nothing happens and we have managed to break some pretty fundamental JavaScript functionality.

However, this can come in handy when we really *do* need to swap out some functionality on an object.  In the following code we will reproduce the C# code to a certain degree.

~~~ javascript
RandomNumberGenerator = function() {
	this.next = function(minimum, maximum) {
		return Math.floor(Math.random()*(maximum-minimum+1)+minimum);
	}
}

RandomNumberConsumer = function(randomNumberGenerator) {
	this._randomNumberGenerator = randomNumberGenerator;
	
	this.probability = function() {
		return this._randomNumberGenerator.next(0,100) > 50;
	}
}

// to test we'll create testing assertions
assertTrue = function(assertion) {
	if (assertion) {
		console.log("OK!")
		return;
	}
	
	throw new Error("Assertion failed.")
}

assertFalse = function(assertion) {
	if (!assertion) {
		console.log("OK!")
		return;
	}
	
	throw new Error("Assertion failed.")
}

// now to test we'll use a "mock"
var mock = { next: function() { return 60; } };
var consumer = new RandomNumberConsumer(mock);

assertTrue(consumer.probability());

mock.next = function() { return 30; };

assertFalse(consumer.probability());
~~~

We can achieve the same outcome in JavaScript that we had in C# but it works in a *very* different way.  There is no dependency injection container though since there is no interface-to-implementation mapping required.  In this way, a dynamic language has no need for a dependency injection container and it doesn't even make sense to have one.

Howe, we *do* still have dependencies.  There are not assemblies since JavaScript is *interpreted*.  This means that we ever only have source files that we work with.  Therefore, in order to use a dependency we have to use the *source code file* containing the code we wish to make use of.  This is where we end up with a whole host of `<script>` tags.  It is also important that the script tags be ordered correctly to represent the *dependency graph* correctly since we cannot make use of a dependency unless the code for that dependency has been executed in the JavaScript environment.

If you have ever done any JavaScript development you will know about the `<script>` tag.  In this article I will use the script tag to synchronously execute external JavaScript by using the `src` attribute.  There are some variations on the use of the `<script>` tag but they are beyond the scope of this article.

Our dependencies are usually added in an `html` file.  Since the scripts are loaded immediately and block any rendering they should be placed at the bottom of the page just before the closing `</body>` tag.  On a side note, we need to add any stylesheets to the top of the page in the `<head>` tag to apply styling to the page while we wait for those `<script>` tags to load:

~~~ html
<html>
<head>
	<link href='css/bootstrap-responsive.css?{cache-buster}' rel='stylesheet' type='text/css' />
	<link href='css/site.css?{cache-buster}' rel='stylesheet' type='text/css' />
	<link href='css/datepicker.css?{cache-buster}' rel='stylesheet' type='text/css' />
	<link href='css/another-one.css?{cache-buster}' rel='stylesheet' type='text/css' />
</head>
<body>	
	<script src='js/site/configuration.js?{cache-buster}' type='text/javascript'></script>
	<script src='js/external/jQuery/jquery.js?{cache-buster}' type='text/javascript'></script>
	<script src='js/external/Moment/moment.js?{cache-buster}' type='text/javascript'></script>
	<script src='js/external/LawnChair/lawnchair.js?{cache-buster}' type='text/javascript'></script>
	<script src='js/external/LawnChair/plugins/aggregation.js?{cache-buster}' type='text/javascript'></script>
	<script src='js/external/LawnChair/plugins/callbacks.js?{cache-buster}' type='text/javascript'></script>
	<script src='js/external/LawnChair/plugins/pagination.js?{cache-buster}' type='text/javascript'></script>
	<script src='js/external/LawnChair/plugins/query.js?{cache-buster}' type='text/javascript'></script>
	<script src='js/external/i18Next/i18next.js?{cache-buster}' type='text/javascript'></script>
	<script src='js/external/Bootstrap/bootstrap.js?{cache-buster}' type='text/javascript'></script>
	<script src='js/external/Bootstrap/bootstrap-datepicker.js?{cache-buster}' type='text/javascript'></script>
	
	<script src='js/site/app.js?{cache-buster}' type='text/javascript'></script>
</body>	
</html>
~~~

For many web applications this would be fine since we have a couple of "global" dependencies that we expect to be available in every bit of JavaScript that we execute.  However, eventually we will run into a bit of a nightmare w.r.t. dependency ordering and how one goes about bundling all the files.  In case you are not aware of it: it is faster to download one bigger resource (script, css, image) than many smaller resources.

This means that we are going to need some mechanism to merge all these files we depend on and then minify then into the least number of files possible.  This, in itself, may be somewhat of a challenge.

When dealing with many JavaScript files, such as when on develops a single page application, the number of files can be quite substantial  With Visual Sutdio one can drag a JavaScript file into another and the following *reference* entry will be placed at the top of the file that is referening the other:

~~~ javascript
/// <reference path="relative-path-to/this-dependency.js" />

var value = UseDependency('value');
~~~

It is important to note that this `reference` entry is purely informational.  Visual Studio may be able to use it to determine some intellisense but other than that there is no tooling that I am aware of that makes use of this entry.  I have developed a bundler for a previous project that uses these entries to build up a dependency graph.  This means that the entries have to be correct and circular references are prevented by the tooling.

## Why use modules?

As we have seen above absolutely all dependencies are globally scoped.  This means that when the files execute something is defined *globally*.  All variables in JavaScript (ES5 / current as at July 2016) are scoped either globally or in a function:

~~~ javascript
// this declaration...
var shuttle = {};

// ...is the same as this
window.shuttle = {};
~~~

Anything, therefore, *not* defined in a function gets attached to the global `window` object.  In contrast, anything defined anywhere (yes, anywhere) in a function is scoped to the function:

~~~ javascript
window.shuttle = {};

window.shuttle.sayHello = function(to) {
	var localTo = to.toUpperCase();

	alert('hello ' + localTo + '!');
	
	for(var i = 0; i < 2; i++) {
		console.log('variable "i" is hoisted to the top of the function.');
	}
}
~~~

You may be wondering why this is relevant.  Well, when importing, say, jQuery using a `<script>` tag the library assigns itself to variable `$` which we have seen is now equivalent to `window.$`.  This is where things get interesting when another library decides that it, too, would like to use the `$` variable.  There are ways around this but modules can help us here.

A JavaScript module is a singleton and is represented by a file.  The module can `import` other modules and `export` functionality in a variety of ways.  This means that our dependencies are still file-based since we have not type information.  The following is an example of importing jquery as a module:

~~~ javascript
import $ from 'jquery';

$('div').css('font-weight', 'bold');
~~~

You may be thinking: "What's the big deal?" and if this is all you are going to do then you are definitely not going to find much return on investment.

However, consider the following:

~~~ javascript
import $old from 'jquery-1.8';
import $new from 'jquery-2.0';

$old('div').css('font-weight', 'bold');
$new('div').css('font-weight', 'bold');
~~~

Now *that* is somewhat simpler than having to use `<script>` tags.  However, you *cannot* use this file directly in a browser without having the code **transpiled**; else you would probably only receive an error since todays browsers do not yet implement this syntax.  A JavaScript transpiler takes source that *is not* pure JavaScript and changes it *to be* pure JavaScript.

Logically (implementations are going to vary), a transpiler may take the above code and inspect it for import statements.  It will find the `import $old from 'jquery-1.8';` bit and check an internal registry in the for of a hash for the `jquery-1.8` identifier and return it or, if necessary, first load it:

~~~ javascript
if (!window.MODULES['jquery-1.8']) {
	this._fetchModule('jquery-1.8.js');
}

return window.MODULES['jquery-1.8'];
~~~

Now it can assign the functionality returned by the module to the variable you want.  The eventual, proper, JavaScript code could resemble this:

~~~ javascript
var $old = window.moduleLoader.Get('jquery-1.8');
var $new = window.moduleLoader.Get('jquery-2.0');

$old('div').css('font-weight', 'bold');
$new('div').css('font-weight', 'bold');
~~~

The beauty of this is that the above JavaScript would be in a function as well.  This means that the variable are locally scoped to the function and do not *polute* the global (window) namespace.

### Module dependencies

It may still not appear that big of a deal but once we get to module dependencies things may start to look a bit clearer.

Traditionally we would either have rather large chunks of JavaScript files or we would have to manage the depend out-of-band since tooling for this, especially in the Visual Studio / .Net space, is rather lacking.

**app.js**:

~~~ javascript
import a from 'dependency-a';

a.doSomethingA();
~~~

**dependency-a.js**:

~~~ javascript
import b from 'dependency-b';

var a = {
	doSomethingA: function() {
		b.doSomethingB();
	}
}

export default a;
~~~

**dependency-b.js**:

~~~ javascript
var b = {
	doSomethingB: function() {
		alert('doing... something...');
	}
}

export default b;
~~~

Now, another quick point to remember is that each module that requires another should import that module since, even though modules are singletons, they are *no longer global*.  You have to assign it to a variable and, being singular, they are only loaded once.

From the above it is rather easy, given the correct tooling, to build a dependency graph and then merge and minify the files.

## Node / NPM

If you are *au fait* with node/npm you can skip this section.

There has been a major shift for web development tooling to [Node.js](https://nodejs.org/en/) along with the [npmjs](https://www.npmjs.com/).  Npm is a package manage for node and JavaScript.  It can be somewhat odd to use npm for front-end JavaScript as all the code is stored in a sub-folder called `node_modules`.  However, not all module *are* node.js modules but packages targeting the tooling side of things most certainly are node modules.  The node and browser JavaScript files are, therefore, mingled.  This can take some getting used to.

Npm is somewhat like NuGet and, as such, needs some place to store the package dependencies.  When using NuGet the dependencies are stored in the `packages.config` file and it has the following structure:

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Shuttle.Esb" version="6.0.0" targetFramework="net45" />
  <package id="Shuttle.Esb.RabbitMQ" version="6.0.1" targetFramework="net45" />
</packages>
~~~

You will notice that the `packages.config` contains *only* the dependency information.  The npm package information is stored in a file called `package.json` and has the following basic structure:

~~~ json
{
  "name": "nodejs",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
~~~

You can get this basic structure by executing the following in a console and just pressing `enter` to answer all questions:

~~~ console
npm init
~~~

You will realise that there is more to this structure than just dependencies.  Let's add a dependency by running the following in a console:

~~~ console
npm install steal --save
npm install steal-tools --save-dev
~~~

The `package.json` now looks like this:

~~~ json
{
  "name": "nodejs",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "steal": "^0.16.23"
  },
  "devDependencies": {
    "steal-tools": "^0.16.5"
  }
}
~~~

The `--save` argument tells npm to add the dependency to the `dependencies` hash and keep track of it.  The `--save-dev` keeps track of the dependency in the `devDependencies` hash.  The difference between these two is that the *normal* `dependencies` refer to packages that your application requires.  This is akin to the packages in NuGet.  However, the `devDependencies` refer to packages you need for tooling.  There isn't really anything in .Net that relates to this although you could think of something like `msbuild` as a `devDependencies` candidate.  The `devDependencies` will, therefore, *not* be distributed/packaged with your application.

The main thing to understand is that the `package.json` does *more* than just track dependencies.  However, when someone else checks out your repository and runs `npm install` the `package.json` file will be used to download the requisite packages.

**Note to Windows users**: the `node_modules` folder can become *very* deep and you will undoubtedly run into path length errors at some stage when using node/npm.  There is a node package called [flatten-packages](https://www.npmjs.com/package/flatten-packages) that will sort you out.

## StealJS

This brings me to [StealJS](http://stealjs.com/):

> Futuristic JavaScript dependency loader and builder. Speeds up application load times. Works with ES6, CommonJS, AMD, CSS, LESS and more. Simplifies modular workflows.

As you can tell from the above, StealJS enables loading modules that have been imlemented using a variety of approaches.  I would suggest using the EcmaScript 6 module format for any future developement.

There are two parts to steal:

- The module loader that is included in your application
- The bundler (steal-tools) that creates release/production versions of your application and related dependencies

Steal also allows the progressive loading of modules using code at any point.  This is useful when you only determine the module to load at runtime.

You will find all the information you need to use steal on the site and you can get help on [DoneJS forum](http://forums.donejs.com/).
