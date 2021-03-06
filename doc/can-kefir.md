@module {Object} can-kefir
@parent can-ecosystem
@package ../package.json
@group can-kefir/symbols symbols
@group can-kefir/methods methods

@description Integrate [KefirJS](https://rpominov.github.io/kefir/) streams directly within [can-stache]
and other parts of CanJS.

@type {Object}

`can-kefir` exposes `value` and `error` key values on a stream that can be read and listened to
by [can-reflect] and therefore many other CanJS libraries like [can-stache] and [can-stache-bindings].

The `can-kefir` module imports [KefirJS](https://rpominov.github.io/kefir/) and mutates it in two ways:

First, it adds [can-symbol symbols]
to Kefir's types so Kefir streams can be read and listened to for changes by [can-reflect].  This allows integration with various parts of CanJS like [can-stache] and [can-stache-bindings].  

Second, it adds a [can-kefir/emitterProperty] method that creates a stream that is more easily writable.

The decorated `Kefir` object is exported by the `can-kefir` module.

@body

## Use

To use it with [can-stache], create a stream and use `.value` or `.error` to write out the  
last emitted value or error.


The following will show a number increasing to 3 over 3 seconds:

```js
var Kefir = require("can-kefir");

var countTo3Stream = Kefir.sequentially(1000,[1,2,3]);

var view = stache("<p>Number: {{countTo3Stream.value}}</p>");

var frag = view({
	countTo3Stream: countTo3Stream
});

document.body.appendChild(frag);
```

## emitterProperty

Use [can-kefir/emitterProperty] to create an stream object that also
has its emitter's methods.  The following creates an `age` stream that
we can emit events on with its `value()` method:

```js
var Kefir = require("can-kefir");

var age = Kefir.emitterProperty();

age.onValue(function(age){
  console.log(age)
});

age.value(20) //-> logs 20

age.value(30) //-> logs 30
```
