# ECMAScript Questions

## 1. Basic

1. Primitive types include: `Null`, `Undefined`, `Boolean`, `String`, `Number` and `Symbol`.
2. Built-in objects include: `Global`, `Object`, `Function`, `Boolean`, `Symbol`, `Error`, `EvalError`, `RangeError`, `ReferenceError`, `SyntaxError`, `TypeError`, `URIError`, `Math`, `Number`, `Date`, `String`, `RegExp`, `Array`, `Map`, `WeakMap`,`Set`,`WeakSet`, `JSON`, `ArrayBuffer`, `DataView`, `Promise`, `Proxy`, `Reflect`.
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

6. *Error('msg')* is equal to *new Error('msg')*.
7. The`Symbol.hasInstance` well-known symbol is used to determine if a constructor object recognizes an object as its instance. The instanceof operator behavior can be customized by this symbol:

```javascript
class MyArray {  
  static [Symbol.hasInstance](instance) {
    return Array.isArray(instance);
  }
}
console.log([] instanceof MyArray); // true
```

8. The `Symbol.isConcatSpreadable` well-known symbol is used to configure if an object should be flattened to its array elements when using the Array.prototype.concat() method:

```javascript
var alpha = ['a', 'b', 'c'], numeric = [1, 2, 3]; 

numeric[Symbol.isConcatSpreadable] = false;

console.log(alpha.concat(numeric)); // ['a', 'b', 'c', [1, 2, 3] ]
```

9. The `Symbol.iterator` well-known symbol specifies the default iterator for an object. Used by *for…of*:

```javascript
var obj = {};
// way one
obj[Symbol.iterator] = function () {
    return {
        next: function () {
            if (++this._idx < 10) {
                return {
                    value: this._idx,
                    done: false
                }
            } else {
                return {
                    done: true
                }
            }
        },
        _idx: 0
    };
};
// way two
obj[Symbol.iterator] = function* () {
    let _idx = 0;
    while (++_idx < 10) {
        yield _idx;
    }
};

for (let v of obj) {
    console.log(v);
}

console.log(...obj);
```

10. The `Symbol.toPrimitive` is a symbol that specifies a function valued property that is called to convert an object to a corresponding primitive value:

```javascript
var ten = {
  [Symbol.toPrimitive](hint) {
    if (hint === "number") {
      return 10;
    }
    if (hint === "string") {
      return "ten";
    }
    return true;
  }
};

console.log(Number(ten)); // 10
console.log(String(ten)); // ten
```

11. The `Symbol.toStringTag` well-known symbol is a string valued property that is used in the creation of the default string description of an object. It is accessed internally by the Object.prototype.toString() method:

```javascript
class Foo {
  get [Symbol.toStringTag]() {
    return 'Foo';
  }
}

console.log(Object.prototype.toString.call(new Foo())); // [object Foo]
```

12. The `Symbol.unscopables` well-known symbol is used to specify an object value of whose own and inherited property names are excluded from the with environment bindings of the associated object:

```javascript
var obj = { 
  foo: 1, 
  bar: 2 
};

obj[Symbol.unscopables] = { 
  foo: false, 
  bar: true 
};

with(obj) {
  console.log(foo); // 1
  console.log(bar); // ReferenceError: bar is not defined
}
```





## 3. Text Processing

1. *String.prototype.startsWith* accepts a *position* as the second argument, *String.prototype.endsWith* accepts a *endPosition* as the second argument:

```javascript
"ABCD".startsWith("BCD", 1) // true
"ABCD".endsWith("ABC", 3) // true
```

2. *String.prototype.substring(indexStart, indexEnd)*. If *indexStart* is greater than *indexEnd*, then the effect of substring() is as if the two arguments were swapped. For example:

```javascript
'AB'.substring(1, 0) == 'AB'.substring(0, 1) // 'A'
```

## 4. Indexed Collections

1. *Array*()* is equal to *new Array*()*.
2. The *Array.from()* method creates a new Array instance from an array-like or iterable object:

```javascript
// Array-like object (arguments) to Array
function f() {
  return Array.from(arguments);
}

f(1, 2, 3); 
// [1, 2, 3]


// Any iterable object...
// Set
var s = new Set(["foo", window]);
Array.from(s);   
// ["foo", window]


// Map
var m = new Map([[1, 2], [2, 4], [4, 8]]);
Array.from(m);                          
// [[1, 2], [2, 4], [4, 8]]  


// String
Array.from("foo");                      
// ["f", "o", "o"]


// Using an arrow function as the map function to
// manipulate the elements
Array.from([1, 2, 3], x => x + x);      
// [2, 4, 6]


// Generate a sequence of numbers
Array.from({length: 5}, (v, k) => k);    
// [0, 1, 2, 3, 4]
```

3. The difference between *Array.of()* and the Array constructor is in the handling of integer arguments: Array.of(7) creates an array with a single element, 7, whereas Array(7) creates an array with 7 elements, each of which is undefined:

```javascript
new Array(7); // [,,,,,,]
Array.of(7); // [7]
```

4. The **Array.prototype.entries** method returns a new Array Iterator object that contains the key/value pairs for each index in the array:

```javascript
var arr = ['a', 'b', 'c'];
var eArr = arr.entries();

console.log(eArr.next().value); // [0, 'a']
console.log(eArr.next().value); // [1, 'b']
console.log(eArr.next().value); // [2, 'c']

for (let kv of eArr) {
  console.log(kv);
}
```

5. The **Array.prototype.find** method returns a value in the array, if an element in the array satisfies the provided testing function. Otherwise undefined is returned:

```javascript
var inventory = [
    {name: 'apples', quantity: 2},
    {name: 'bananas', quantity: 0},
    {name: 'cherries', quantity: 5}
];

function findCherries(fruit) { 
    return fruit.name === 'cherries';
}

inventory.find(findCherries); // { name: 'cherries', quantity: 5 }
```

6. The **Array.prototype.findIndex** method returns an index in the array, if an element in the array satisfies the provided testing function. Otherwise -1 is returned:

```javascript
var inventory = [
    {name: 'apples', quantity: 2},
    {name: 'bananas', quantity: 0},
    {name: 'cherries', quantity: 5}
];

function findCherries(fruit) { 
    return fruit.name === 'cherries';
}

inventory.findIndex(findCherries); // 2
```

7. The **Array.prototype.includes** method intentionally differs from the similar indexOf method in two ways. First, it uses the SameValueZero algorithm, instead of Strict Equality Comparison, allowing it to detect NaN array elements. Second, it does not skip missing array elements, instead treating them as undefined.

```javascript
// NaN
[NaN].indexOf(NaN); // -1
[NaN].includes(NaN); // true
// Missing array element
[1,,3].indexOf(undefined); // -1
[1,,3].includes(undefined); // true
```

8. The **Array.prototype.keys/values** method returns a new Array Iterator object that contains the keys/values for each index in the array:

```javascript
var arr = ['w', 'y', 'k', 'o', 'p'];
var ks = arr.keys();
var vs = arr.values();

for (let key of ks) {
  console.log(key); // 1,2,3,4,5
}

for (let letter of vs) {
  console.log(letter); // w,y,k,o,p
}
```

## 5. Structured Data

1. Notice **reviver** argument in `JSON.parse(text[, reviver])`:

```javascript
JSON.parse('{"a":1,"b":2}', function(key, value) {
  if (key === "a") {
    return 3;
  }
  return value;
});
// {a: 3, b: 2}
```

2. Notice **replacer** argument in `JSON.stringify(value [,replacer [,space]])`:

```javascript
JSON.stringify({a:1, b:2}, function(key, value) {
  return (key==='a') ? undefined : value;
});
// {"b": 2}
```

3. Object.prototype.toString.call(JSON) == '[object JSON]'

