# marklib

[![Circle CI](https://circleci.com/gh/BowlingX/marklib.svg?style=svg)](https://circleci.com/gh/BowlingX/marklib)
[![Codacy Badge](https://www.codacy.com/project/badge/d31afc00c2f04ba2848f3991d6b17e47)](https://www.codacy.com/app/billing/marklib)
[![Coverage Status](https://coveralls.io/repos/BowlingX/marklib/badge.svg?branch=master)](https://coveralls.io/r/BowlingX/marklib?branch=master)
[![npm](https://img.shields.io/npm/v/marklib.svg?style=flat-square)](https://www.npmjs.com/package/marklib)
[![Join the chat at https://gitter.im/BowlingX/marklib](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/BowlingX/marklib?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

A simple and fast zero-dependencies-library to transform text-selections into serializable markings.

[Demo](http://bowlingx.github.io/marklib/)

![Demo-Gif](https://raw.githubusercontent.com/BowlingX/marklib/master/marklib.gif)

## Install

marklib can be installed with `npm` or `bower`.

`npm install --save-dev marklib`

`bower install marklib --save`

## Usage

### Render by selection

``` javascript

// obtain a selection from document
var selection = document.getSelection();

// create a new rendering based on the current document
var renderer = new Marklib.Rendering(document, options, context)
renderer.setId('myRenderId') // if an ID is not provided, a autogenerated one will be used

// renders the given selection and returns a result (`RenderResult`).
var result = renderer.renderWithRange(selection.getRangeAt(0));

```

**Important**: After a Rendering has been used to render a selection/serialized result, 
it can't be used to render something again. You need to create a new Instance of `Rendering`.

## Options

You can pass options to each rendering instance, the following shows the default options

```javascript

var renderer = new Marklib.Rendering(document, {
            hoverClass: 'marklib--hover',
            treeClass: 'marklib--tree',
            // Supports arrays and/or strings
            className: ['marking']
});

```

## Events

Marklib triggers events that can be listened to with `instance.on('event-name')`. Events are build with 
`wolfy87-eventemitter` (https://github.com/Olical/EventEmitter). The following Events are available:

Before you can actually receive events, you need to register the event handler with `registerEvents` (use `import { registerEvents } from 'marklib/src/main/RenderingEvents';` on your application bootstrap code.)

| Event-Name    | Description | Arguments |
| ------------- |-------------|-------------|
| `click`         | triggered when clicked on a marking.  | `(originalEvent, instanceHierarchy)`
| `hover-enter`   | triggered when a pointer-device starts hovering over a marking      | `(originalEvent, instanceHierarchy)`
| `hover-leave`   | triggered when a pointer-device leaves a marking                    | `(originalEvent, instanceHierarchy)`

Additionally, marklib will add hover classes to the current hovered marking.

#### Constructor Arguments

- 1) `HTMLDocument` document -> the document instance used
- 2) `Object` [options], optional -> an object containing setting for marklib (see Options)
- 3) `HTMLElement` [context], optional -> 
    the context used to serialize / deserialize the rendering, if not given the document instance.


### Render by serialized result

A Serialized results consist of 2 strings (start end end) in the following form

```

'body>section;0;1'`
-▲------------▲-▲ 

```

- ▲ The first part defines a css-selector (queryable with document.querySelector).
- ▲ The second part defines the text-node inside the given selector
- ▲ The third part defines the string-offset inside this text-node

#### Example

``` javascript

 // This is the result we get from `RenderResult#serialize()`
 
 var result = {
    startContainerPath: 'body>section;0',
    endContainerPath: 'body>section;1',
    startOffset: 2,
    endOffset: 5
 }

 var rendering = new Marklib.Rendering(document);
 
 rendering.renderWithResult(result);

```

## Use-Cases

- Annotations
- Collaboration tools
- Inline-Commenting (I actually started a project that will do something like this: https://github.com/BowlingX/commentp)

## Develop

`npm run develop` or `npm run tdd` (to start karma in watch mode)

## License

The MIT License (MIT)

Copyright (c) 2015 David Heidrich

Any contribution is welcome, just issue a pull-request or bug/feature if you found something :)

