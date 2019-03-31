---
layout: default
---

** Javascript Constructors **

So there are two ways to create 'classes' and 'instances'. With an Object OR a Function.

Remember though with an Object you DONT have the ability to do any of the whole prototype.constructor business.

So Objects are much more limited. You just change the Object.create and thats it.

Prototypes are only available on functions NOT objects. fact.

** Object **

```

// base object with methods including initialization
var Vehicle = {
  init: function(name) {
    this.name = name;
  },
  start: function() {
    return "engine of "+this.name + " starting...";
  }
}
// delegation link created between sub object and base object
var Car = Object.create(Vehicle);
// sub object method
Car.run = function() {
  console.log("Hello "+ this.start());
};
// instance object with delegation link point to sub object
var c1 = Object.create(Car);
c1.init('Fiesta');
var c2 = Object.create(Car);
c2.init('Baleno');
c1.run();   // "Hello engine of Fiesta starting..."
c2.run();   // "Hello engine of Baleno starting..."

```

** Function **

```

// Vehicle - superclass
function Vehicle(name) {
  this.name = name;
}
// superclass method
Vehicle.prototype.start = function() {
  return "engine of "+this.name + " starting...";
};
// Car - subclass
function Car(name) {
  Vehicle.call(this,name); // call super constructor.
}
// subclass extends superclass
Car.prototype = Object.create(Vehicle.prototype);
// subclass method
Car.prototype.run = function() {
  console.log("Hello "+ this.start());
};
// instances of subclass
var c1 = new Car("Fiesta");
var c2 = new Car("Baleno");
// accessing the subclass method which internally access superclass method
c1.run();   // "Hello engine of Fiesta starting..."
c2.run();   // "Hello engine of Baleno starting..."

```
