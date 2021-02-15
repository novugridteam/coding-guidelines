# Novugrid's Coding Guidelines for Node JS projects

 This document contains the coding guidelines for Novugrid. It contains four major sections:

-   [What Do Code Reviewers Look For?](#what-do-code-reviewers-look-for)
-   [Style Guidelines](#style-guidelines), on how to organize your source code.
-   [Git Workflow](#git-workflow) discusses how our workflow has changed now that Mono is hosted on GitHub.
<!-- -   [Best Practices](#best-practices), used in the project. -->

What Do Code Reviewers Look For?
================================
Code reviews should look at:

-   Design: Is the code well-designed and appropriate for your system?
-   Functionality: Does the code behave as the author likely intended? Is the way the code behaves good for its users?
-   Complexity: Could the code be made simpler? Would another developer be able to easily understand and use this code when they come across it in the future?
-   Tests: Does the code have correct and well-designed automated tests?
-   Naming: Did the developer choose clear names for variables, classes, methods, etc.?
-   Comments: Are the comments clear and useful?
-   Style: Does the code follow our [style guides](#style-guidelines)?
-   Documentation: Did the developer also update relevant documentation?

Style Guidelines
================

In order to keep the code consistent, please use the following conventions.

You may also wish to read some [Node.js best practices](https://github.com/goldbergyoni/nodebestpractices) as well, however the guidelines below take precedent for your code if there is a conflict.

Identifiers
-----------
Identifiers must use only ASCII letters, digits, underscores (for constants and structured test method names), and the '\(' sign. Thus each valid identifier name is matched by the regular expression `[\)\w]+`.
|Style          |Category |
| ------------  | ------- |
|`UpperCamelCase`	|class / interface / type / enum / decorator / type parameters |
|`lowerCamelCase`	|variable / parameter / function / method / property / module alias |
|`CONSTANT_CASE`	|global constant values, including enum values |

-   **Abbreviations:** Treat abbreviations like acronyms in names as whole words, i.e. use `loadHttpUrl`, not ~~`loadHTTPURL,`~~ unless required by a platform name (e.g. `XMLHttpRequest`).
-   **Dollar sign:** Identifiers should not generally use `$`, except when aligning with naming conventions for third party frameworks. See below for more on using `$` with Observable values.
-   **Type parameters:** Type parameters, like in `Array<T>`, may use a single upper case character (`T`) or `UpperCamelCase`.
-   **Test names:** Test method names in Closure testSuites and similar xUnit-style test frameworks may be structured with `_` separators, e.g. `testX_whenY_doesZ()`.
-   **`_` prefix/suffix:** Identifiers must not use `_` as a prefix or suffix. This also means that `_` must not be used as an identifier by itself (e.g. to indicate a parameter is unused).
-   Imports: Module namespace imports are lowerCamelCase while files are snake_case, which means that imports correctly will not match in casing style, such as

    ``` js
    import * as fooBar from './foo_bar';
    ```

    Some libraries might commonly use a namespace import prefix that violates this naming scheme, but overbearingly common open source use makes the violating style more readable. The only libraries that currently fall under this exception are:

    -   jquery, using the $ prefix
    -   threejs, using the THREE prefix

Naming style
------------
TypeScript expresses information in types, so names should not be decorated with information that is included in the type. (See also Testing Blog for more about what not to include.)

Some concrete examples of this rule:

-   Do not use trailing or leading underscores for private properties or methods.
-   Do not use the `opt_` prefix for optional parameters.
-   Do not mark interfaces specially (~~`IMyInterface`~~ or ~~`MyFooInterface`~~) unless it's idiomatic in its environment. When introducing an interface for a class, give it a name that expresses why the interface exists in the first place (e.g. class TodoItem and interface TodoItemStorage if the interface expresses the format used for storage/serialization in JSON).
-   Suffixing Observables with `$` is a common external convention and can help resolve confusion regarding observable values vs concrete values. Judgement on whether this is a useful convention is left up to individual teams, but should be consistent within projects.


Descriptive names
-----------------
Names must be descriptive and clear to a new reader. Do not use abbreviations that are ambiguous or unfamiliar to readers outside your project, and do not abbreviate by deleting letters within a word.

-   **Exception:** Variables that are in scope for 10 lines or fewer, including arguments that are not part of an exported API, may use short (e.g. single letter) variable names.

File encoding: UTF-8
--------------------
For non-ASCII characters, use the actual Unicode character (e.g. `∞`). For non-printable characters, the equivalent hex or Unicode escapes (e.g. `\u221e`) can be used along with an explanatory comment.

Good: 
``` js
// Perfectly clear, even without a comment.
const units = 'μs';

// Use escapes for non-printable characters.
const output = '\ufeff' + content;  // byte order mark
```

Bad:
``` js
// Hard to read and prone to mistakes, even with the comment.
const units = '\u03bcs'; // Greek letter mu, 's'

// The reader has no idea what this is.
const output = '\ufeff' + content;
```

Comments & Documentation
------------------------

### JSDoc vs comments
There are two types of comments, JSDoc (`/** ... */`) and non-JSDoc ordinary comments (`// ...` or `/* ... */`).

Use `/** JSDoc */` comments for documentation, i.e. comments a user of the code should read.
Use // line comments for implementation comments, i.e. comments that only concern the implementation of the code itself.

JSDoc comments are understood by tools (such as editors and documentation generators), while ordinary comments are only for other humans.

### JSDoc rules
The style-guide for jsDocs can be found [here](js-docs.md). Exceptions to those rules may be found below

### Document all top-level exports of modules
Use /** JSDoc */ comments to communicate information to the users of your code. Avoid merely restating the property or parameter name. You should also document all properties and methods (exported/public or not) whose purpose is not immediately obvious from their name, as judged by your reviewer.

### Omit comments that are redundant with TypeScript
For example, do not declare types in @param or @return blocks, do not write @implements, @enum, @private etc. on code that uses the implements, enum, private etc. keywords.

### Do not use `@override`
Do not use `@override` in TypeScript source code.

`@override` is not enforced by the compiler, which is surprising and leads to annotations and implementation going out of sync. Including it purely for documentation purposes is confusing.

### Make comments that actually add information
For non-exported symbols, sometimes the name and type of the function or parameter is enough. Code will usually benefit from more documentation than just variable names though!

Avoid comments that just restate the parameter name and type, e.g.
``` ts
/** @param fooBarService The Bar service for the Foo application. */
```

Because of this rule, @param and @return lines are only required when they add information, and may otherwise be omitted.
``` ts
/**
 * POSTs the request to start coffee brewing.
 * @param amountLitres The amount to brew. Must fit the pot size!
 */
brew(amountLitres: number, logger: Logger) {
  // ...
}
```

### Parameter property comments
A parameter property is when a class declares a field and a constructor parameter in a single declaration, by marking a parameter in the constructor. E.g. constructor(private readonly foo: Foo), declares that the class has a foo field.

To document these fields, use JSDoc's @param annotation. Editors display the description on constructor calls and property accesses.
```ts
/** This class demonstrates how parameter properties are documented. */
class ParamProps {
  /**
   * @param percolator The percolator used for brewing.
   * @param beans The beans to brew.
   */
  constructor(
    private readonly percolator: Percolator,
    private readonly beans: CoffeeBean[]) {}
}
```

```ts
/** This class demonstrates how ordinary fields are documented. */
class OrdinaryClass {
  /** The bean that will be used in the next call to brew(). */
  nextBean: CoffeeBean;

  constructor(initialBean: CoffeeBean) {
    this.nextBean = initialBean;
  }
}
```

### Comments when calling a function
If needed, document parameters at call sites inline using block comments. Also consider named parameters using object literals and destructuring. The exact formatting and placement of the comment is not prescribed.
```ts
// Inline block comments for parameters that'd be hard to understand:
new Percolator().brew(/* amountLitres= */ 5);
// Also consider using named arguments and destructuring parameters (in brew's declaration):
new Percolator().brew({amountLitres: 5});
```

### Place documentation prior to decorators
When a class, method, or property have both decorators like `@Component` and JsDoc, please make sure to write the JsDoc before the decorator.

Do not write JsDoc between the Decorator and the decorated statement.
``` ts
@Component({
  selector: 'foo',
  template: 'bar',
})
/** Component that prints "bar". */
export class FooComponent {}
```
Write the JsDoc block before the Decorator.

``` ts
/** Component that prints "bar". */
@Component({
  selector: 'foo',
  template: 'bar',
})
export class FooComponent {}
```

Indentation
-----------

Use tabs (and configure your IDE to show a size of 4 spaces for them) for writing your code (hopefully we can keep this consistent). If you are modifying som`eone else's code, try to keep the coding style similar.

Performance and Readability
---------------------------

It is more important to be correct than to be fast.

It is more important to be maintainable than to be fast.

Fast code that is difficult to maintain is likely going to be looked down upon.

Source Organization
------------------
### Modules
#### Import Paths
TypeScript code must use paths to import other TypeScript code. Paths may be relative, i.e. starting with `.` or `..`, or rooted at the base directory, e.g. `root/path/to/file`.

Code should use relative imports (`./foo`) rather than absolute imports `path/to/foo` when referring to files within the same (logical) project.

Consider limiting the number of parent steps (`../../../`) as those can make module and path structures hard to understand.
``` ts
import {Symbol1} from 'google3/path/from/root';
import {Symbol2} from '../parent/file';
import {Symbol3} from './sibling';
```

#### Namespaces vs Modules
TypeScript supports two methods to organize code: namespaces and modules, but namespaces are disallowed. google3 code must use TypeScript modules (which are ECMAScript 6 modules). That is, your code must refer to code in other files using imports and exports of the form import {foo} from 'bar';

Your code must not use the namespace Foo { ... } construct. namespaces may only be used when required to interface with external, third party code. To semantically namespace your code, use separate files.

Code must not use require (as in import x = require('...');) for imports. Use ES6 module syntax.

``` ts
// Bad: do not use namespaces:
namespace Rocket {
  function launch() { ... }
}

// Bad: do not use <reference>
/// <reference path="..."/>

// Bad: do not use require()
import x = require('mydep');
```

NB: TypeScript namespaces used to be called internal modules and used to use the module keyword in the form `module Foo { ... }`. Don't use that either. Always use ES6 imports.

### Exports
Use named exports in all code:

``` ts
// Use named exports:
export class Foo { ... }
Do not use default exports. This ensures that all imports follow a uniform pattern.
```


### Imports
There are four variants of import statements in ES6 and TypeScript:

| Import type | Example                       | Use for |
| ----------- | --------------                |  -----  |
| module	    |`import * as foo from '...';`  |	TypeScript imports |
destructuring |`import {SomeThing} from '...';`|	TypeScript imports|
default	      |`import SomeThing from '...';`	|TypeScript imports|
side-effect   |`import '...';`	              |Only to import libraries for their side-effects on load (such as custom elements)|


``` ts
// Good: choose between two options as appropriate (see below).
import * as ng from '@angular/core';
import {Foo} from './foo';

// Only when needed: default imports.
import Button from 'Button';

// Sometimes needed to import libraries for their side effects:
import 'jasmine';
import '@polymer/paper-button';
```

### Organize By Features

Organize packages by feature, not by type. For example, an online shop should have packages named `products`, `checkout`, `categories`, not `views`, `models`, `controllers`.

Git Workflow
============
We would be using the [Github flow](https://guides.github.com/introduction/flow/)

One Line Summaries
------------------

Before your full commit message, use a one-line summary of the change as this improves the output of GitHub's rendering of changes done to the project.

After this one line change, you can add the regular, full-documented version of the change.

Feature Branches
----------------

You should create a feature branch for your work.
the naming standard for creating feature branches will follow the format of prefixing the branch name with the feature
keyword then followed by the work item name, which looks like this format -
`feature/feature-name/`. A more valid examples are as follows `feature/validations` or `feature/login` or `feature/collections-revert`

### Branch Naming
Please ensure the branch naming follows a small case and hyphen spaces convention e.g `branch-name1`, `branch-name2`, `another-branch-name`
