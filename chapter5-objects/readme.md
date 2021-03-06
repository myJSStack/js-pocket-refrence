# Objects
- An object is an unordered collection of properties, each of which has a name and a value. Property names are strings, so we can say that objects map strings to values.
- This string-to-value mapping goes by various names: `hash`, `hashtable`, `dictionary`, `associative array`.
- In addition to maintaining its own set of properties, a JavaScript object also inherits the properties of another object, known as its “prototype.” 
- **Any value in JavaScript that is not a string, a number, true, false, null, or undefined is an object.**
- Objects are **mutable** and are manipulated **by reference** rather than by value

### Creating Objects
#### (I) Object Literals
```
var empty = {}; // An object with no properties
var point = { x:0, y:0 }; // Two properties
var point2 = { // Another object literal
 x:point.x, // With more complex properties
 y:point.y+1
};
var book = { // Nonidentifier property names are quoted
 "main title": "JavaScript", // space in property name
 'sub-title': "Pocket Ref", // punctuation in name
 "for": "all audiences", // reserved word name
};
```
#### (II) Creating Objects with new Object()
```
var o = new Object()
```

**PROTOTYPE: Every JavaScript object has a second JavaScript object (or null, but this is rare) associated with it. This second object is known as a prototype, and the first object inherits properties from the prototype**

#### (III) Object.create()
1.
```
// o1 inherits properties x and y.
var o1 = Object.create({x:1, y:2});
```
2. It has not prototype, or inherit anything.
```
// o2 inherits no properties or methods.
var o2 = Object.create(null);
```
3.
```
// o3 is like {} or new Object().
var o3 = Object.create(Object.prototype);
```

### Properties
#### Property Inheritance
- JavaScript objects have a set of “own properties,” and they also inherit a set of properties from their prototype object.
```
// o inherits object methods from Object.prototype
var o = {}
o.x = 1; // and has an own property x.
// p inherits properties from o and Object.prototype
var p = inherit(o);
p.y = 2; // and has an own property y.
```

#### Deleting Properties
- The delete operator removes a property from an object.
```
delete book.author; // book now has no author.
delete book["main title"]; // or a "main title", either.
```

#### Testing Properties
1. (Specific)`hasOwnProperty()`: method of an object tests whether that object has an own property with the given name. _It returns false for inherited properties._
2. (More Specific)`propertyIsEnumerable()`: method refines the hasOwnProperty() test. It returns true only if the named property is an own
property and its enumerable attribute is true.
3. (General)`in operator`: expects a property name (as a string) on its left side and an object on its right. It returns true if the object has
an own property or an inherited property by that name.

***Instead of using the in operator, it is often sufficient to simply query the property and use !== to make sure it is not undefined***

#### Enumerating Properties
- Properties created by normal JavaScript code are enumerable.
- Built-in methods that objects inherit are not enumerable, but the properties that your code adds to objects are enumerable (unless you use one of the functions described later to make them nonenumerable).
-  To guard against this, you might want to filter the properties returned by `for/in`. Here are two ways 
```
for(p in o) {
 if (!o.hasOwnProperty(p)) // Skip inherited props
 continue;
}
```

or 
```
for(p in o) {
 if (typeof o[p] === "function") // Skip methods
 continue;
}
```
- `Object.keys()`:  which returns an array of the names of the enumerable own properties of an object.
- `Object.getOwnPropertyNames()`:  It works like Object.keys() but returns the names of all the own properties of the specified object, not just the enumerable properties.

#### Serializing Properties and Objects
- Object serialization is the process of converting an object’s state to a string from which it can later be restored. `JSON.stringify()`, `JSON.parse()` to serialize and restore JavaScript objects. These functions use the JSON data interchange format.
- JSON stands for “JavaScript Object Notation,”
- Note that JSON syntax is a subset of JavaScript syntax, and it cannot represent all JavaScript values. Objects, arrays, strings,
finite numbers, true, false, and null are supported and can be serialized and restored.

#### Property Getters and Setters
```
var o = {
 // An ordinary data property
 data_prop: value,
  // An accessor property as a pair of functions
 get accessor_prop() { /* return value */ },
 set accessor_prop(value) { /* set value */ }
};
```
#### Property Attributes (ECMAScript 5): value, writable(boolean), enumerable (boolean), and configurable (boolean)
- Introduced with ECMAScript 5 API.
- The four attributes of a data property are `value`, `writable`(boolean), `enumerable` (boolean), and `configurable` (boolean).
- Four attributes of an accessor property are `get`, `set`, `enumerale` (boolean) and `configurable` (boolean)
- The ECMAScript 5 methods for this is  property descriptor.
- To get attribute descriptions use `Object.getOwnPropertyDescriptor(obj, "property_name")`
- To set attribute descriptions:
```
var o = {}; // Start with no properties at all
// Add a nonenumerable data property x with value 1.
Object.defineProperty(o, "x", { value : 1,
   writable: true,
   enumerable: false,
   configurable: true});
 
 // Check that the property is there but is nonenumerable
  o.x; // => 1
  Object.keys(o) // => []

// Now modify the property x so that it is read-only
Object.defineProperty(o, "x", { writable: false });
// Try to change the value of the property
o.x = 2; // Fails silently or TypeError in strict mode
o.x // => 1

// The property is still configurable,
// so we can change its value like this:
Object.defineProperty(o, "x", { value: 2 });
o.x // => 2

// Now change x to an accessor property
Object.defineProperty(o, "x", {
 get: function() { return 0; }
});
o.x // => 0
 ```
 - You do not have to include all four attributes to make a change.
 
### Object Attributes (ECMAScript 5): `prototype`, `class`, `extensible`

#### prototype
- `Object.getPrototypeOf()`, `isPrototypeOf()
- Note that isPrototypeOf() performs a function similar to the instanceof operator.

#### class
- An object’s class attribute is a string that provides informationabout the type of the object. 
- there is only an indirect technique for querying it. The default toString() method

#### extensible
- The extensible attribute of an object specifies whether new properties can be added to the object or not.
- Level one: `Object.isExtensible()`, `Object.preventExtensions()`
- Level two: (extension+config): `Object.seal()`, `Object.isSealed()`
- Level three:  (extension+config+nowrite): `Object.freeze()`

