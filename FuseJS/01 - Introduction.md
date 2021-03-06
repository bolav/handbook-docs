<documentProperties pageTitle="Learn FuseJS" />

# Introduction

$(FuseJS) is a $(JavaScript) framework for writing cross-platform mobile app business logic.

Note: FuseJS is currently [not supported for WebGL exports](/learn/guides/previewandexport/other).

## Getting started

FuseJS can be used in @(UX Markup) through the `<JavaScript>` tag, either by pointing to external JavaScript files, like this:

	<JavaScript File="SomeCode.js" />

Or by inlining the JavaScript code in the tag, like this:

	<JavaScript>
		console.log("Hello, FuseJS!");
	</JavaScript>

## Modules

FuseJS implements the <a href="http://www.commonjs.org/">CommonJS</a> module system. Each code file or inline snippet is a _module_.

For things inside the module to be visible on the outside, we use the `module.exports`-construct:

	<JavaScript>
		module.exports = {
			exportedSymbol: "Hello, rest of the world!"
		};
	</JavaScript>

Failing to export from modules will make it impossible to reach defined data inside the module:

	<JavaScript>
		var data = [1, 2, 3];
		var invisible = "I'm invisible";

		module.exports = {
			data: data
		};
	</JavaScript>

This is good for hiding implementation details from other calling JavaScript modules and UX code.

> ## Importing modules

Each code file (or inline snippet) is a module.

To make a module available to other modules through `$(require)`, mark your `<JavaScript>` tag with the `ux:Global` attribute.

	<JavaScript File="parse-1.5.0.min.js" ux:Global="Parse" />

You can then access this module in any other module in the same project like this:

	var Parse = require("Parse").Parse;

> Note: It is currently not possible to include modules from JS just by file path. We're on it, thanks for your patience!

Examples:

* <a href="https://www.fusetools.com/developers/examples/todoparseexample">TODO App with Parse backend example</a>

## Design and motivation

The key design goal of FuseJS is to keep your JS code small, clean and only concerned with the practical functions of your application. Meanwhile
all things UX-oriented such as layout, data presentation, animation and gesture response is left to declarative $(UX markup) and native UI components.

The way Fuse separates JavaScript business logic from UX markup presentation has some clear benefits:

* Performance - all the performance critical bits are handled in native code and based on native UI components.
* Easy - declarative code is easy to read, write and understand even with limited programming knowledge
* Less error prone - fewer states means less things can go wrong
  * Visual tooling - UX markup can be edited by Fuse tools such as inspectors, timelines and generally cool drag & droppy stuff.

Note that Fuse has tons of declarative APIs (designed UX markup) that replace the need for controlling animation from JavaScript (i.e. imperatively).

Many other JavaScript frameworks mix imperative UI code, animation and performance critical tasks into JavaScript, hence many people new to FuseJS tend to try
doing things this way in the beginning. While most of these things are technically possible in FuseJS, it is discouraged. We recommend taking some
time to study the Fuse examples to get a feel for the new way of doing things.

Purifying your code by separating view and logic into UX markup and JavaScript can shrink your code base significantly, make it more maintainable, and allow
more effective collaboration between UX designers and developers.

If you need to write performance-critical business logic, we recommend doing that in native code or alternatively @(Uno) code instead of JS.
