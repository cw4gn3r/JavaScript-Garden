## Objekte und Eigenschaften

Jeder Wert in JavaScript verhält sich wie ein Objekt, mit der Ausnahme von [`null`](#core.undefined) und [`undefined`](#core.undefined).

    false.toString(); // 'false'
    [1, 2, 3].toString(); // '1,2,3'
    
    function Foo(){}
    Foo.bar = 1;
    Foo.bar; // 1

Ein häufiges Missverständnis ist das Zahlenliterale keine Objekte sind. Das liegt daran, dass der Parser in JavaScript den Punkt als *Fließkommazahl* interpretiert.

    2.toString(); // verursacht einen SyntaxError

Es gibt jedoch mehrere Wege, um ein Zahlenliteral als Objekt anzusprechen:

    2..toString(); // Der zweite Punkt wird korrekt interpretier
    2 .toString(); // Leerzeichen vor dem Punkt
    (2).toString(); // Die '2' wird zuerst evaluiert

### Objekte als Datentyp

Objekte in JavaScript können auch als [*Hashmaps*][1] benutzt werden. In der Tat sind Objekte wenig mehr als .

Using an object literal - `{}` notation - it is possible to create a 
plain object. This new object [inherits](#object.prototype) from `Object.prototype` and 
does not have [own properties](#object.hasownproperty) defined.

    var foo = {}; // a new empty object

    // a new object with a 'test' property with value 12
    var bar = {test: 12}; 

### Accessing Properties

The properties of an object can be accessed in two ways, via either the dot
notation or the square bracket notation.
    
    var foo = {name: 'kitten'}
    foo.name; // kitten
    foo['name']; // kitten
    
    var get = 'name';
    foo[get]; // kitten
    
    foo.1234; // SyntaxError
    foo['1234']; // works

The notations work almost identically, with the only difference being that the
square bracket notation allows for dynamic setting of properties and
the use of property names that would otherwise lead to a syntax error.

### Deleting Properties

The only way to remove a property from an object is to use the `delete`
operator; setting the property to `undefined` or `null` only removes the
*value* associated with the property, but not the *key*.

    var obj = {
        bar: 1,
        foo: 2,
        baz: 3
    };
    obj.bar = undefined;
    obj.foo = null;
    delete obj.baz;

    for(var i in obj) {
        if (obj.hasOwnProperty(i)) {
            console.log(i, '' + obj[i]);
        }
    }

The above outputs both `bar undefined` and `foo null` - only `baz` was
removed and is therefore missing from the output.

### Notation of Keys

    var test = {
        'case': 'I am a keyword, so I must be notated as a string',
        delete: 'I am a keyword, so me too' // raises SyntaxError
    };

Object properties can be both notated as plain characters and as strings. Due to
another mis-design in JavaScript's parser, the above will throw 
a `SyntaxError` prior to ECMAScript 5.

This error arises from the fact that `delete` is a *keyword*; therefore, it must be 
notated as a *string literal* to ensure that it will be correctly interpreted by
older JavaScript engines.

[1]: http://en.wikipedia.org/wiki/Hashmap

