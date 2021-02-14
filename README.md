# Novugrid's Coding Guidelines for Node JS projects

 This document contains the coding guidelines for Novugrid. It contains four major sections:

-   [What Do Code Reviewers Look For?](#what-do-code-reviewers-look-for)
-   [Style Guidelines](#style-guidelines), on how to organize your source code.
-   [Git Workflow](#git-workflow) discusses how our workflow has changed now that Mono is hosted on GitHub.
-   [Best Practices](#best-practices), used in the project.

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

...-   jquery, using the $ prefix
...-   threejs, using the THREE prefix

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

Indentation
-----------

Use tabs (and configure your IDE to show a size of 4 spaces for them) for writing your code (hopefully we can keep this consistent). If you are modifying som`eone else's code, try to keep the coding style similar.

Performance and Readability
---------------------------

It is more important to be correct than to be fast.

It is more important to be maintainable than to be fast.

Fast code that is difficult to maintain is likely going to be looked down upon.

Git Workflow
====================

One Line Summaries
------------------

Before your full commit message, use a one-line summary of the change as this improves the output of GitHub's rendering of changes done to the project.

After this one line change, you can add the regular, full-documented version of the change.

Personal Work Branches
----------------------

blah blah blah
