# ECMAScript Questions

## 1. Basic

1. Primitive types include: `Null`, `Undefined`, `Boolean`, `String`, `Number` and `Symbol`.
2. Built-in objects include: `Global`, `Object`, `Function`, `Boolean`, `Symbol`, `Error(s)`, `Math`, `Number`, `Date`, `String`, `RegExp`, `Array`, `Map`, `Set`, `JSON`, `ArrayBuffer`, `DataView`, `Promise`, `Proxy`, `Reflect`.
3. Number type has __2\*\*64-2\*\*53+3__(NaN, +Infinity, -Infinity) values, including positive zero and negative zero.
4. **An Object is logically a collection of properties. Each property is either a data property, or an accessor property.**
5. A *function object*  is an object that supports the **[[Call]]** internal method; A *constructor* is a function object that supports the **[[Construct]]** internal method (think about *generator* function).

## 2. Fundamental Objects

1. `Object.create` accept a *prototype* argument and a *properties* argument:

```javascript
let proto = {bar: 'max'};
let obj = Object.create(proto, {
  foo: {
    value: 28,
    writable: true,
    enumerable: true,
    configurable: true
  }
});
```

2. `Object.is` can distinguish *+0/-0* and *NaN*:

```javascript
Object.is(+0, -0)// false
Object.is(NaN, NaN)// true
```

3. `Object.keys` returns names of  *own enumerabel* properties.`Object.getOwnPropertyNames` returns all.
4. `new Symbol()` throws a *TypeError* because it's not a constructor.
5. The  `Symbol.for(key)` method searches for existing symbols in a runtime-wide symbol registry with the given key and returns it if found. Otherwise a new symbol gets created in the global symbol registry with this key. The `Symbol.keyFor(sym)` method retrieves a shared symbol key from the global symbol registry for the given symbol.

```javascript
Symbol.for("bar") === Symbol.for("bar"); // true
Symbol("bar") === Symbol("bar"); // false
Symbol.keyFor(Symbol.for('bar')); // bar
Symbol.keyFor(Symbol('bar')); // undefined
```

