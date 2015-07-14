[![travis-ci status](https://api.travis-ci.org/spencer-leopold/scoped-events.png)](http://travis-ci.org/#!/spencer-leopold/scoped-events/builds)
[![Dependency Status](https://david-dm.org/spencer-leopold/scoped-events.png)](https://david-dm.org/spencer-leopold/scoped-events)
[![Coverage Status](https://coveralls.io/repos/spencer-leopold/scoped-events/badge.svg?branch=master&service=github)](https://coveralls.io/github/spencer-leopold/scoped-events?branch=master)

# ScopedEvents

Small event library that lets you namespace/scope your events. Also includes a mixin to add the functionality to any other object.

# Install

With [npm](http://npmjs.org) do:

```
npm install scoped-events
```

# Usage

You can use ScopedEvents directly, as an Event Dispatcher (singleton), or as a mixin to add the functionality to your own prototypes or objects.

# Details

When you trigger an event, if it is a child scope of another event, it will trigger all of it's parents as well.  So if you have the events "routes" and "routes:add" when you trigger "routes:add" it will also trigger "routes" - The first argument to any listener callbacks is either the child scope that triggered it, or null.  So when "routes:add" triggers "routes", the first argument the callback for "routes" receives is "add".

```javascript
// Normal use
var ScopedEvents = require('scoped-events');
var Events = new ScopedEvents();

Events.on('event', callback);

// Dispatcher use
var EventDispatcher = require('scoped-events').Dispatcher;
EventDispatcher.on('event', callback);

// Mixin use
// Works on plain objects too
var eventMixin = require('scoped-events').mixin;

function PersonClass() {
  this.name = 'Anonymous';

  this.on('all', function(event) {
    console.log("Called event: %s", event);
  });
}

eventMixin(PersonClass);

PersonClass.prototype.setName = function(name) {
  this.name = name;
  this.trigger('name:set');
};

var Person = new PersonClass();
Person.trigger('event:to:trigger');
```
