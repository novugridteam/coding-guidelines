# JSDocs

JSDoc may be used on all classes, fields, and methods.

General form
------------
The basic formatting of JSDoc blocks is as seen in this example:
``` js
/**
 * Multiple lines of JSDoc text are written here,
 * wrapped normally.
 * @param {number} arg A number to do something to.
 */
function doSomething(arg) { … }
or in this single-line example:

/** @const @private {!Foo} A short bit of JSDoc. */
this.foo_ = foo;
```

If a single-line comment overflows into multiple lines, it must use the multi-line style with `/**` and `*/` on their own lines.

Many tools extract metadata from JSDoc comments to perform code validation and optimization. As such, these comments must be well-formed.

Markdown
--------
JSDoc is written in Markdown, though it may include HTML when necessary.

Note that tools that automatically extract JSDoc (e.g. JsDossier) will often ignore plain text formatting, so if you did this:
``` js
/**
 * Computes weight based on three factors:
 *   items sent
 *   items received
 *   last timestamp
 */
```

it would come out like this:

```
Computes weight based on three factors: items sent items received last timestamp
```

Instead, write a Markdown list:

``` js
/**
 * Computes weight based on three factors:
 *
 *  - items sent
 *  - items received
 *  - last timestamp
 */
```

JSDoc tags
----------
Most tags must occupy their own line, with the tag at the beginning of the line.

Disallowed:
``` js
/**
 * The "param" tag must occupy its own line and may not be combined.
 * @param {number} left @param {number} right
 */
function add(left, right) { ... }
```

Simple tags that do not require any additional data (such as `@private`, `@const`, `@final`, `@export`) may be combined onto the same line, along with an optional type when appropriate.

``` js
/**
 * Place more complex annotations (like "implements" and "template")
 * on their own lines.  Multiple simple tags (like "export" and "final")
 * may be combined in one line.
 * @export @final
 * @implements {Iterable<TYPE>}
 * @template TYPE
 */
class MyClass {
  /**
   * @param {!ObjType} obj Some object.
   * @param {number=} num An optional number.
   */
  constructor(obj, num = 42) {
    /** @private @const {!Array<!ObjType|number>} */
    this.data_ = [obj, num];
  }
}
```

There is no hard rule for when to combine tags, or in which order, but be consistent.

Line wrapping
-------------

Line-wrapped block tags are indented four spaces. Wrapped description text may be lined up with the description on previous lines, but this horizontal alignment is discouraged.

``` js
/**
 * Illustrates line wrapping for long param/return descriptions.
 * @param {string} foo This is a param with a description too long to fit in
 *     one line.
 * @return {number} This returns something that has a description too long to
 *     fit in one line.
 */
exports.method = function(foo) {
  return 5;
};
```

Do not indent when wrapping a @desc or @fileoverview description.

Top/file-level comments
-----------------------

A file may have a top-level file overview. A copyright notice , author information, and default visibility level are optional. File overviews are generally recommended whenever a file consists of more than a single class definition. The top level comment is designed to orient readers unfamiliar with the code to what is in this file. If present, it may provide a description of the file's contents and any dependencies or compatibility information. Wrapped lines are not indented.

Example:
``` js
/**
 * @fileoverview Description of file, its uses and information
 * about its dependencies.
 * @package
 */
```

Class comments
--------------

Classes, interfaces and records must be documented with a description and any template parameters, implemented interfaces, visibility, or other appropriate tags. The class description should provide the reader with enough information to know how and when to use the class, as well as any additional considerations necessary to correctly use the class. Textual descriptions may be omitted on the constructor. @constructor and @extends annotations are not used with the class keyword unless the class is being used to declare an @interface or it extends a generic class.

``` js
/**
 * A fancier event target that does cool things.
 * @implements {Iterable<string>}
 */
class MyFancyTarget extends EventTarget {
  /**
   * @param {string} arg1 An argument that makes this more interesting.
   * @param {!Array<number>} arg2 List of numbers to be processed.
   */
  constructor(arg1, arg2) {
    // ...
  }
};

/**
 * Records are also helpful.
 * @extends {Iterator<TYPE>}
 * @record
 * @template TYPE
 */
class Listable {
  /** @return {TYPE} The next item in line to be returned. */
  next() {}
}
```

Enum and typedef comments
-------------------------

All enums and typedefs must be documented with appropriate JSDoc tags (@typedef or @enum) on the preceding line. Public enums and typedefs must also have a description. Individual enum items may be documented with a JSDoc comment on the preceding line.
``` js
/**
 * A useful type union, which is reused often.
 * @typedef {!Bandersnatch|!BandersnatchType}
 */
let CoolUnionType;


/**
 * Types of bandersnatches.
 * @enum {string}
 */
const BandersnatchType = {
  /** This kind is really frumious. */
  FRUMIOUS: 'frumious',
  /** The less-frumious kind. */
  MANXOME: 'manxome',
};
```

Typedefs are useful for defining short record types, or aliases for unions, complex functions, or generic types. Typedefs should be avoided for record types with many fields, since they do not allow documenting individual fields, nor using templates or recursive references. For large record types, prefer @record.

Method and function comments
----------------------------

In methods and named functions, parameter and return types must be documented, except in the case of same-signature @overrides, where all types are omitted. The this type should be documented when necessary. Return type may be omitted if the function has no non-empty return statements.

Method, parameter, and return descriptions (but not types) may be omitted if they are obvious from the rest of the method’s JSDoc or from its signature.

Method descriptions begin with a verb phrase that describes what the method does. This phrase is not an imperative sentence, but instead is written in the third person, as if there is an implied This method ... before it.

If a method overrides a superclass method, it must include an @override annotation. Overridden methods inherit all JSDoc annotations from the super class method (including visibility annotations) and they should be omitted in the overridden method. However, if any type is refined in type annotations, all @param and @return annotations must be specified explicitly.

``` js
/** A class that does something. */
class SomeClass extends SomeBaseClass {
  /**
   * Operates on an instance of MyClass and returns something.
   * @param {!MyClass} obj An object that for some reason needs detailed
   *     explanation that spans multiple lines.
   * @param {!OtherClass} obviousOtherClass
   * @return {boolean} Whether something occurred.
   */
  someMethod(obj, obviousOtherClass) { ... }

  /** @override */
  overriddenMethod(param) { ... }
}

/**
 * Demonstrates how top-level functions follow the same rules.  This one
 * makes an array.
 * @param {TYPE} arg
 * @return {!Array<TYPE>}
 * @template TYPE
 */
function makeArray(arg) { ... }
```
If you only need to document the param and return types of a function, you may optionally use inline JSDocs in the function's signature. These inline JSDocs specify the return and param types without tags.

``` js
function /** string */ foo(/** number */ arg) {...}
```
If you need descriptions or tags, use a single JSDoc comment above the method. For example, methods which return values need a @return tag.

``` js
class MyClass {
  /**
   * @param {number} arg
   * @return {string}
   */
  bar(arg) {...}
}
// Illegal inline JSDocs.

class MyClass {
  /** @return {string} */ foo() {...}
}

/** Function description. */ bar() {...}
```
In anonymous functions annotations are generally optional. If the automatic type inference is insufficient or explicit annotation improves readability, then annotate param and return types like this:
``` js
promise.then(
    /** @return {string} */
    (/** !Array<string> */ items) => {
      doSomethingWith(items);
      return items[0];
    });
```

Property comments
-----------------
Property types must be documented. The description may be omitted for private properties, if name and type provide enough documentation for understanding the code.

Publicly exported constants are commented the same way as properties.

``` js
/** My class. */
class MyClass {
  /** @param {string=} someString */
  constructor(someString = 'default string') {
    /** @private @const {string} */
    this.someString_ = someString;

    /** @private @const {!OtherType} */
    this.someOtherThing_ = functionThatReturnsAThing();

    /**
     * Maximum number of things per pane.
     * @type {number}
     */
    this.someProperty = 4;
  }
}

/**
 * The number of times we'll try before giving up.
 * @const {number}
 */
MyClass.RETRY_COUNT = 33;
```

Type annotations
----------------

Type annotations are found on @param, @return, @this, and @type tags, and optionally on @const, @export, and any visibility tags. Type annotations attached to JSDoc tags must always be enclosed in braces.

### Nullability
The type system defines modifiers ! and ? for non-null and nullable, respectively. These modifiers must precede the type.

Nullability modifiers have different requirements for different types, which fall into two broad categories:

Type annotations for primitives (`string`, `number`, `boolean`, `symbol`, `undefined`, `null`) and literals (`{function(...): ...}` and `{{foo: string...}}`) are always non-nullable by default. Use the `?` modifier to make it nullable, but omit the redundant `!`.
Reference types (generally, anything in UpperCamelCase, including some.namespace.ReferenceType) refer to a class, enum, record, or typedef defined elsewhere. Since these types may or may not be nullable, it is impossible to tell from the name alone whether it is nullable or not. Always use explicit `?` and `!` modifiers for these types to prevent ambiguity at use sites.
Bad:

``` js
const /** MyObject */ myObject = null; // Non-primitive types must be annotated.
const /** !number */ someNum = 5; // Primitives are non-nullable by default.
const /** number? */ someNullableNum = null; // ? should precede the type.
const /** !{foo: string, bar: number} */ record = ...; // Already non-nullable.
const /** MyTypeDef */ def = ...; // Not sure if MyTypeDef is nullable.

// Not sure if object (nullable), enum (non-nullable, unless otherwise
// specified), or typedef (depends on definition).
const /** SomeCamelCaseName */ n = ...;
```

Good:

``` js
const /** ?MyObject */ myObject = null;
const /** number */ someNum = 5;
const /** ?number */ someNullableNum = null;
const /** {foo: string, bar: number} */ record = ...;
const /** !MyTypeDef */ def = ...;
const /** ?SomeCamelCaseName */ n = ...;
```

### Type Casts
In cases where the compiler doesn't accurately infer the type of an expression, it is possible to tighten the type by adding a type annotation comment and enclosing the expression in parentheses. Note that the parentheses are required.

``` js
/** @type {number} */ (x)
```

### Template Parameter Types
Always specify template parameters. This way compiler can do a better job and it makes it easier for readers to understand what code does.

Bad:

``` js
const /** !Object */ users = {};
const /** !Array */ books = [];
const /** !Promise */ response = ...;
```

Good:

``` js
const /** !Object<string, !User> */ users = {};
const /** !Array<string> */ books = [];
const /** !Promise<!Response> */ response = ...;

const /** !Promise<undefined> */ thisPromiseReturnsNothingButParameterIsStillUseful = ...;
const /** !Object<string, *> */ mapOfEverything = {};
```

Cases when template parameters should not be used:

-   Object is used for type hierarchy and not as map-like structure.

### Function type expressions
Terminology Note: function type expression refers to a type annotation for function types with the keyword function in the annotation (see examples below).

Where the function definition is given, do not use a function type expression. Specify parameter and return types with @param and @return, or with inline annotations ( see [Method and function comments](#method-and-function-comments) ). This includes anonymous functions and functions defined and assigned to a const (where the function jsdoc appears above the whole assignment expression).

Function type expressions are needed, for example, inside `@typedef`, `@param` or `@return`. Use it also for variables or properties of function type, if they are not immediately initialized with the function definition.

``` js
  /** @private {function(string): string} */
  this.idGenerator_ = myFunctions.identity;
```

When using a function type expression, always specify the return type explicitly. Otherwise the default return type is unknown (?), which leads to strange and unexpected behavior, and is rarely what is actually desired.

Bad - type error, but no warning given:

``` js
/** @param {function()} generateNumber */
function foo(generateNumber) {
  const /** number */ x = generateNumber();  // No compile-time type error here.
}

foo(() => 'clearly not a number');
```
Good:

``` js
/**
 * @param {function(): *} inputFunction1 Can return any type.
 * @param {function(): undefined} inputFunction2 Definitely doesn't return
 *      anything.
 * NOTE: the return type of `foo` itself is safely implied to be {undefined}.
 */
function foo(inputFunction1, inputFunction2) {...}
```

### Whitespace

Within a type annotation, a single space or line break is required after each comma or colon. Additional line breaks may be inserted to improve readability or avoid exceeding the column limit. These breaks should be chosen and indented following the applicable guidelines (e.g. 4.5 Line-wrapping and 4.2 Block indentation: +2 spaces). No other whitespace is allowed in type annotations.

Good:

``` js
/** @type {function(string): number} */

/** @type {{foo: number, bar: number}} */

/** @type {number|string} */

/** @type {!Object<string, string>} */

/** @type {function(this: Object<string, string>, number): string} */

/**
 * @type {function(
 *     !SuperDuperReallyReallyLongTypedefThatForcesTheLineBreak,
 *     !OtherVeryLongTypedef): string}
 */

/**
 * @type {!SuperDuperReallyReallyLongTypedefThatForcesTheLineBreak|
 *     !OtherVeryLongTypedef}
 */
```
Bad:

``` js
// Only put a space after the colon
/** @type {function(string) : number} */

// Put spaces after colons and commas
/** @type {{foo:number,bar:number}} */

// No space in union types
/** @type {number | string} */
```

Visibility annotations
----------------------

Visibility annotations (`@private`, `@package`, `@protected`) may be specified in a @fileoverview block, or on any exported symbol or property. Do not specify visibility for local variables, whether within a function or at the top level of a module. All `@private` names must end with an underscore.

