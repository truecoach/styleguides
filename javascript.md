# JavaScript Style Guide

## Table Of Contents

### Grammar

* [Block Statements](#block-statements)
* [Conditional Statements](#conditional-statements)
* [Commas](#commas)
* [Semicolons](#semicolons)
* [Comments](#comments)
* [Variables](#variables)
* [Whitespace](#whitespace)

### Objects

* [Objects](#objects)
* [Properties](#properties)

### Strings

* [Strings](#strings)

### Arrays

* [Arrays](#arrays)

## Functions

* [Functions](#functions)
* [Function Arguments](#function-arguments)

## Block Statements

+ Use spaces before leading brace.

```javascript
// conditional
if (notFound) {
  return 0;
} else {
  return 1;
}

switch (condition) {
  case 'yes':
    // code
    break;
}

// loops
for (const key in keys) {
  // code
}

for (let i = 0, l = keys.length; i < l; i++) {
  // code
}

while (true) {
  // code
}

try {
  // code that throws an error
} catch(e) {
  // code that handles an error
}
```

+ Opening curly brace (`{`) should be on the same line as the beginning of a statement or declaration.

```javascript
function foo() {
  const obj = {
    val: 'test'
  };

  return {
    data: obj
  };
}

if (foo === 1) {
  foo();
}

for (let key in keys) {
  bar(e);
}

while (true) {
  foo();
}
```

+ Keep `else` and its accompanying braces on the same line.

```javascript
if (foo === 1) {
  bar = 2;
} else {
  bar = '2';
}

if (foo === 1) {
  bar = 2;
} else if (foo === 2) {
  bar = 1;
} else {
  bar = 3;
}
```

## Conditional Statements

+ Use strict equality (`===` and `!==`).
+ Use curly braces for all conditional blocks.

```javascript
if (notFound) {
  // code
}
```

+ Use explicit conditions when checking for non `null`, `undefined`, `true`, `false` values.

```javascript
if (arr.length > 0) {
  // code
}

if (foo !== '') {
  // code
}
```

+ Use multiline format.

```javascript
if (foo === 'bar') {
  return;
}
```

## Commas

+ Skip trailing commas.

```javascript
const foo = [1, 2, 3];
const bar = { a: 'a' };
```

+ Skip trailing and leading commas.

```javascript
const foo = [1, 2, 3];
const bar = {
  a: 'a',
  b: 'b'
};
```

## Semicolons

+ Use semicolons.

## Comments

+ Use multiline comments with two leading asterisks for documentation.

```javascript
/**
  This is documentation for something just below.
*/
```

+ Use [YUIDoc](http://yui.github.io/yuidoc/syntax/index.html) comments for
  documenting functions.
+ Use `//` for non-documenting comments (both single and multiline).

```javascript
function foo() {
  let bar = 5;

  // multiplies `bar` by 2.
  fooBar(bar);

  console.log(bar);
}
```

+ Pad comments with a space.

## Variables

+ Never use `var`. Prefer `const` to declare variables, unless explicitly
requiring mutability.

```javascript
const a = [1, 2, 3];
```

+ Note that both `let` and `const` are block scoped.

```javascript
{
  let a = 1;
  const b = 2;
}
console.log(a); // ReferenceError
console.log(b); // ReferenceError
```

+ Put all non-assigning declarations on one line.

```javascript
let a, b;
```

+ Use a single `const` declaration for each assignment.

```javascript
const a = 1;
const b = 2;
```

+ Declare variables at the top of their block scope.

```javascript
function foo() {
  let bar;

  console.log('foo bar!');

  bar = getBar();
}

function bar() {
  let coolList;

  // code

  for (let index = 0, length = coolThing.length; index < length; index++) {
    // code
  }
}
```

## Whitespace

+ Use soft tabs set to 2 spaces.

```javascript
function() {
∙∙const name;
}
```

+ Place 1 space before a leading brace (`{`).

```javascript
obj.set('foo', {
  foo: 'bar'
});

test('foo-bar', function() {
});
```

+ No spaces before semicolons.

```javascript
const foo = {};
```

+ Keep parenthesis adjacent to the function name when declared or called.

```javascript
function foo() {
}

foo();
```

+ No trailing whitespace.

## Objects

+ Use literal form for object creation.

```javascript
const foo = {};
```

+ Pad single-line objects with white-space.

```javascript
const bar = { color: 'orange' };
```

## Strings

+ Prefer single quotes, and use double quotes to avoid escaping.

```javascript
// BAD:
const foo = "bar";

// GOOD:
const foo = 'bar';
const baz = "What's this?";
```

+ When constructing strings with dynamic values, prefer template strings.

```javascript
const prefix = 'Hello';
const suffix = 'and have a good day.';

// BAD:
return prefix + ' world, ' + suffix;

// GOOD:
return `${prefix} world, ${suffix}`;
```

## Arrays

+ Use literal form for array creation (unless you know the exact length).

```javascript
const foo = [];
```

+ Use new Array if you know the exact length of the array and know that its length will not change.

```javascript
const foo = new Array(16);
```

+ Use `push` to add an item to an array.

```javascript
let foo = [];
foo.push('bar');
```

+ Use spread to join 2 arrays.

```javascript
let foo = [0, 1, 2];
let bar = [3, 4, 5];

foo.push(...bar);
```

+ Join single line array items with a space.

```javascript
const foo = ['a', 'b', 'c'];
```

+ Use array destructuring.

```javascript
let arr = [1, 2, 3, 4];

// BAD:
const head = arr.shift();
const tail = arr;

// GOOD:
const [head, ...tail] = arr;
```

## Properties

+ Use property value shorthand.

```javascript
const name = 'Derek Zoolander';
const age = 25;

// BAD:
const foo = {
  name: name,
  age: age
};

// GOOD:
const foo = {
  name,
  age
};
```

+ Use dot-notation when accessing properties.

```javascript
const foo = {
  bar: 'bar'
};

foo.bar;
```

+ Use `[]` when accessing properties via a variable.

```javascript
const propertyName = 'bar';
const foo = {
  bar: 'bar'
};

foo[propertyName];
```

+ Use object destructuring when accessing multiple properties on an object.

```javascript
// BAD:
function foo(person) {
  const name = person.name;
  const age = person.age;
  const height = person.height;

  return `${name} is ${age} years old and ${height} tall.`
}

// GOOD:
function foo(person) {
  const { name, age, height } = person;

  return `${name} is ${age} years old and ${height} tall.`
}
```

## Functions

+ Use object method shorthand.

```javascript
// BAD:
const foo = {
  value: 0,

  bar: function bar(value) {
    return foo.value + value;
  }
};
```

```javascript
// GOOD:
const foo = {
  value: 0,

  bar(value) {
    return foo.value + value;
  }
};
```

+ Use scope to lookup functions (not variables).

```javascript
// GOOD:
function foo() {
  function bar() {
    // code
  }

  bar();
}

// BAD:
function foo() {
  const bar = function bar() {
    // code
  }

  bar();
}
```

+ Use fat arrows to preserve `this` when using function expressions or
anonymous functions.

```javascript
const foo = {
  base: 0,

  // BAD:
  bar(items) {
    const _this = this;
    return items.map(function(item) {
      return _this.base + item.value;
    });
  },

  // GOOD:
  bar(items) {
    return items.map((item) => {
      return this.base + item.value;
    });
  }
};

## Function Arguments

+ Never use `arguments` – use rest instead.

```javascript
// BAD:
function foo() {
  const args = Array.prototype.slice.call(arguments);
  return args.join('');
}

// GOOD:
function foo(...args) {
  return args.join('');
}
```

`arguments` object must not be passed or leaked anywhere.
See the [reference](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers#3-managing-arguments).

+ Don't re-assign the arguments.

```javascript
// Don't re-assign the arguments
function fooBar() {
  arguments = 3;
}

// Don't re-assign the arguments
function fooBar(opt) {
  opt = 3;
}

// Use a new variable if you need to assign the value of an argument
function fooBar(_opt) {
  const opt = _opt;

  opt = 3;
}
```

+ Use default params instead of mutating them.

```javascript
// BAD:
function fooBar(obj, key, value) {
  obj = obj || {};
  key = key || 'id';
  // ...
}

// GOOD:
function fooBar(obj = {}, key = 'id', value = 0) {
  if (key) {
    return obj[key];
  } else {
    obj[key] = value;
    return obj[key];
  }
}
```
