# generate [![NPM version](https://img.shields.io/npm/v/generate.svg?style=flat)](https://www.npmjs.com/package/generate) [![NPM downloads](https://img.shields.io/npm/dm/generate.svg?style=flat)](https://npmjs.org/package/generate) [![Build Status](https://img.shields.io/travis/generate/generate.svg?style=flat)](https://travis-ci.org/generate/generate)

<p align="center">
<a href="https://github.com/generate/generate">
<img height="250" width="250" src="https://raw.githubusercontent.com/generate/generate/master/docs/logo.png">
</a>
</p>

Command line tool and developer framework for scaffolding out new GitHub projects. Generate offers the robustness and configurability of Yeoman, the expressiveness and simplicity of Slush, and more powerful flow control and composability than either.

You might also be interested in [update](https://github.com/update/update).

## Table of Contents

- [What is "Generate"?](#what-is-generate)
  * [Why Generate?](#why-generate)
  * [Features](#features)
  * [Build workflow](#build-workflow)
- [Command line usage](#command-line-usage)
  * [Getting started](#getting-started)
  * [init](#init)
  * [Options](#options)
- [Generators](#generators)
  * [Discovering generators](#discovering-generators)
  * [Discovering plugins](#discovering-plugins)
  * [Authoring generators](#authoring-generators)
- [More information](#more-information)
- [Community](#community)
- [About](#about)
  * [Related projects](#related-projects)
  * [Contributing](#contributing)
  * [Running tests](#running-tests)
  * [Author](#author)
  * [License](#license)

_(TOC generated by [verb](https://github.com/verbose/verb) using [markdown-toc](https://github.com/jonschlinkert/markdown-toc))_

Please read our [contributing guide](.github/contributing.md) if you'd like to learn more about contributing to this project.

## What is "Generate"?

Generate itself is a command line tool and API for creating, resolving, registering, and running "generators". Along with methods for rendering tempalates, storing preferences, initiating user prompts, and so on. All actual "generating" is accomplished using plugins called [generators](docs/generators.md), which are run by command line or API, and can be installed globally, locally, or exported as a function from a local `generator.js`.

Generators can be found and installed through [npm](https://www.npmjs.com/), or you can create your own [generators](docs/generators.md) using Generate's API.

### Why Generate?

There are other project generators out there, why should you spend your time learning to use Generate?

* [Comparison to Yeoman and Slush](docs/why-use-generate.md)
* [Generator framework comparison table](docs/comparison-table.md)

### Features

* **exceptional flow control**: through the use of [generators](docs/generators.md) and [tasks](docs/tasks.md)
* **render templates**: use templates to create new files, or replace existing files
* **prompts**: It's easy to create custom prompts, and aswers to prompts can be used as context for rendering templates, for settings options, determining file names, directory structure, and anything else that requires user feedback.
* **any engine**: use any template engine to render templates, including [handlebars](http://www.handlebarsjs.com/), [lodash](https://lodash.com/), [swig](https://github.com/paularmstrong/swig) and [pug](http://jade-lang.com), and anything supported by [consolidate](https://github.com/visionmedia/consolidate.js).
* **data**: gather data from the user's environment for rendering templates, to populate "hints" in user prompts or for rendering templates, etc.
* **fs**: in the spirit of [gulp](http://gulpjs.com), use `.src` and `.dest` to read and write globs of files.
* **vinyl**: files and templates are [vinyl](http://github.com/gulpjs/vinyl) files
* **streams**: full support for [gulp](http://gulpjs.com) and [assemble](https://github.com/assemble/assemble) plugins
* **smart plugins**: Update is built on [base](https://github.com/node-base/base), so any "smart" plugin from the Base ecosystem can be used
* **stores**: persist configuration settings, global defaults, project-specific defaults, answers to prompts, and so on.
* much more!

### Build workflow

Generate can be used as a standalone library, but it can also be used as a part of a larger build workflow when used alongside the following libraries:

* [assemble](https://github.com/assemble/assemble): build projects
* [verb](https://github.com/verbose/verb): document projects
* [update](https://github.com/update/update): maintain projects

## Command line usage

### Getting started

The following instructions are intended to provide a basic demonstration of how Generate works. Visit the links after this section for more information.

**1. Install generate**

To use Generate's CLI, it must first be installed globally using [npm](https://www.npmjs.com/). You can do that now with the following command.

```sh
$ npm install --global generate
```

This adds the `gen` command to your system path, allowing it to be run from anywhere.

**2. Install an "generator"**

To see how generators work, install `generate-example`:

```sh
$ npm install --global generate-example
```

**3. Run**

Run the example generator with the following command:

```sh
$ gen example
```

This appends the string `foo` to the contents of `example.txt`. Visit the [generate-example](https://github.com/generate/generate-example) project for additional steps and guidance.

**Next steps**

* Browse the [documentation](docs)
* Learn about [generators](docs/generators.md)
* Learn about the [built-in generators](docs/cli/built-in-generators.md)
* Learn about [authoring generators](docs/generators.md#creating-generators)
* Learn about [setting options](docs/options.md#template-specific-options) in templates

### init

Tell Generate's CLI to automatically run certain generators every time the `gen` command is given:

```sh
$ gen init
```

You can run this command whenever you want to generate your preferences, like after installing new generators.

```console
$ gen help

  Usage: generate <command> [options]

  Command: generator or tasks to run

  Options:

    --config, -c      Save a configuration value to the `gen` object in package.json
    --cwd             Set or display the current working directory
    --data, -d        Define data. API equivalent of `app.data()`
    --disable         Disable an option. API equivalent of "app.disable('foo')"
    --enable          Enable an option. API equivalent of "app.enable('foo')"
    --global, -g      Save a global configuration value to use as a default
    --help, -h        Display this help menu
    --init, -i        Prompts for configuration values and stores the answers
    --option, -o      Define options. API equivalent of `app.option()`
    --run             Force tasks to run regardless of command line flags used
    --silent, -S      Silence all tasks and generators in the terminal
    --show <key>      Display the value of <key>
    --version, -V     Display the current version of generate
    --verbose, -v     Display all verbose logging messages

  Examples:

    # run generator "foo"
    $ gen foo

    # run task "bar" from generator "foo"
    $ gen foo:bar

    # run multiple tasks from generator "foo"
    $ gen foo:bar,baz,qux

    # run a sub-generator from generator "foo"
    $ gen foo.abc

    # run task "xyz" from sub-generator "foo.abc"
    $ gen foo.abc:xyz

    Generate attempts to automatically determine if "foo" is a task or generator.
    If there is a conflict, you can force generate to run generator "foo"
    by specifying its default task. Example: `$ gen foo:default`
```

### Options

Note that all command line options are preceded by `--`, or `-` for abbreviations. Only generator names or tasks can be used as commands (without `--` or `-`).

* `--no-install`: Don't automatically install `dependencies` or `devDependencies` after generating files.
* `--no-hints`: Don't use hints in prompts

See [options documentation](docs/options.md).

## Generators

### Discovering generators

* Find generators to install by [searching npm](https://www.npmjs.com/browse/keyword/generategenerator) for packages with the keyword `generategenerator`
* Visit [Generate's GitHub org](https://github.com/generate) to see the generators maintained by the core team

### Discovering plugins

Plugins from any applications built on [base](https://github.com/node-base/base) should work with Generate (and can be used in your generator):

* [base](https://www.npmjs.com/browse/keyword/baseplugin): find base plugins on npm using the `baseplugin` keyword
* [assemble](https://www.npmjs.com/browse/keyword/assembleplugin): find assemble plugins on npm using the `assembleplugin` keyword
* [generate](https://www.npmjs.com/browse/keyword/generateplugin): find generate plugins on npm using the `generateplugin` keyword
* [templates](https://www.npmjs.com/browse/keyword/templatesplugin): find templates plugins on npm using the `templatesplugin` keyword
* [update][update-plugin]: find update plugins on npm using the `updateplugin` keyword
* [verb](https://www.npmjs.com/browse/keyword/verbplugin): find verb plugins on npm using the `verbplugin` keyword

### Authoring generators

Visit the [generator documentation](docs/generators.md) guide to learn how to use, author and publish generators.

## More information

* [Documentation](docs)
* [API documentation](docs/api)
* [Generaters maintained by the core team](https://github.com/generate)

## Community

Are you using Generate in your project? Have you published an [generator](docs/generators.md) and want to share your Generate project with the world? Here are some suggestions:

* If you get like Generate and want to tweet about it, please use the hashtag `#generatejs`
* Get implementation help on [StackOverflow](http://stackoverflow.com/questions/tagged/generate) (please use the `gen` tag in questions)
* **Gitter** Discuss Generate with us on [Gitter](https://gitter.im/generate)
* If you publish an generator, thank you! To make your project as discoverable as possible, please add the keyword `generate-generator` to package.json.

## About

### Related projects

Generate shares a common architecture and plugin ecosystem with the following libraries:

* [assemble](https://www.npmjs.com/package/assemble): Assemble is a powerful, extendable and easy to use static site generator for node.js. Used… [more](https://github.com/assemble/assemble) | [homepage](https://github.com/assemble/assemble "Assemble is a powerful, extendable and easy to use static site generator for node.js. Used by thousands of projects for much more than building websites, Assemble is also used for creating themes, scaffolds, boilerplates, e-books, UI components, API docum")
* [base](https://www.npmjs.com/package/base): base is the foundation for creating modular, unit testable and highly pluggable node.js applications, starting… [more](https://github.com/node-base/base) | [homepage](https://github.com/node-base/base "base is the foundation for creating modular, unit testable and highly pluggable node.js applications, starting with a handful of common methods, like `set`, `get`, `del` and `use`.")
* [update](https://www.npmjs.com/package/update): Be scalable! Update is a new, open source developer framework and CLI for automating updates… [more](https://github.com/update/update) | [homepage](https://github.com/update/update "Be scalable! Update is a new, open source developer framework and CLI for automating updates of any kind in code projects.")
* [verb](https://www.npmjs.com/package/verb): Documentation generator for GitHub projects. Verb is extremely powerful, easy to use, and is used… [more](https://github.com/verbose/verb) | [homepage](https://github.com/verbose/verb "Documentation generator for GitHub projects. Verb is extremely powerful, easy to use, and is used on hundreds of projects of all sizes to generate everything from API docs to readmes.")

### Contributing

Pull requests and stars are always welcome. For bugs and feature requests, [please create an issue](../../issues/new).

Please read the [contributing guide](.github/contributing.md) for avice on opening issues, pull requests, and coding standards.

### Running tests

Install dev dependencies:

```sh
$ npm install -d && npm test
```

### Author

**Jon Schlinkert**

* [github/jonschlinkert](https://github.com/jonschlinkert)
* [twitter/jonschlinkert](http://twitter.com/jonschlinkert)

### License

Copyright © 2016, [Jon Schlinkert](https://github.com/jonschlinkert).
Released under the [MIT license](https://github.com/generate/generate/blob/master/LICENSE).

***

_This file was generated by [verb](https://github.com/verbose/verb), v0.9.0, on July 12, 2016._