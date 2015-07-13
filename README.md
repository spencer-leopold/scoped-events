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

You can use ScopedEvents directly, as an Event Dispatcher (singleton), or as a mixin to add the functionality to your own protoypes or objects.

# Use as a Dispatcher

```
var Dispatcher = require('scoped-events').Dispatcher;

var myObject = {
  buttons: {
    registOpen: '.open--register-dialog',
    registerClose: '.close--register-dialog'
  },

  bindUIEvents: function() {
    var buttons = this.buttons;
    $(buttons.registerOpen).on('click', Dispatcher.trigger('open:RegisterDialog', 'opening dialog'));
  }
};

myObject.bindUIEvents();


// In a separate file

var Dispatcher = require('scoped-events').Dispatcher;

var myDialogFactory = {
  init: function() {
    Dispatcher.on('open', this.openDialog, this);
    Dispatcher.on('close', this.closeDialog, this);
  },

  // Child scope will always be passed as the first
  // argument to subscribers
  openDialog: function(name, msg) {
    if (name === 'RegisterDialog') {
      // do something to create the dialog
    }
  },

  // Child scope will always be passed as the first
  // argument to subscribers
  closeDialog: function(name, e) {}
};

myDialogFactory.init();
```

# Use as a mixin

```
var eventMixin = require('scoped-events').mixin;

function myClass() {
  this.name = 'Anonymous';
}

myClass.prototype.setName = function(name) {
  this.name = name;
}

eventMixin(myClass);

module.exports = myClass;
```

