# JavaScript Inline Documentation Standards

> Snapshot source: https://developer.wordpress.org/coding-standards/inline-documentation-standards/javascript/
> Retrieved: 2026-07-18
> Conversion: HTML to Markdown with Pandoc.

<div id="wp--skip-link--target"
class="wp-block-group alignwide is-layout-flow wp-block-group-is-layout-flow"
role="main">

# JavaScript Documentation Standards

<div class="wp-block-wporg-sidebar-container is-layout-flow wp-block-wporg-sidebar-container-is-layout-flow"
breakpoint="1300px">

<div class="wp-block-wporg-table-of-contents">

<div class="wporg-table-of-contents__header">

## In this article

<span class="screen-reader-text">Table of Contents</span>

</div>

- [What Should Be Documented](#what-should-be-documented)
- [Documenting Tips](#documenting-tips)
  - [Language](#language)
  - [Grammar](#grammar)
- [Formatting Guidelines](#formatting-guidelines)
  - [Line wrapping](#line-wrapping)
  - [Aligning comments](#aligning-comments)
- [Functions](#functions)
- [Backbone classes](#backbone-classes)
- [Local functions](#local-functions)
- [Local ancestors](#local-ancestors)
- [Class members](#class-members)
- [Namespaces](#namespaces)
- [Inline Comments](#inline-comments)
  - [Single line comments](#single-line-comments)
  - [Multi-line comments](#multi-line-comments)
- [File Headers](#file-headers)
- [Supported JSDoc Tags](#supported-jsdoc-tags)
- [Unsupported JSDoc Tags](#unsupported-jsdoc-tags)

</div>

[<span class="wp-exclude-emoji" aria-hidden="true">↑</span> Back to
top](#wp--skip-link--target)

</div>

<div class="entry-content wp-block-post-content is-layout-flow wp-block-post-content-is-layout-flow">

WordPress follows the [JSDoc 3 standard](http://jsdoc.app/) for inline
JavaScript documentation.

## [What Should Be Documented](#what-should-be-documented)

JavaScript documentation in WordPress takes the form of either formatted
blocks of documentation or inline comments.

The following is a list of what should be documented in WordPress
JavaScript files:

- Functions and class methods
- Objects
- Closures
- Object properties
- Requires
- Events
- File headers

## [Documenting Tips](#documenting-tips)

### [Language](#language)

Short descriptions should be clear, simple, and brief. Document “what”
and “when” – “why” should rarely need to be included. The “why” can go
in the long description if needed. For example:

Functions and closures are *third-person singular* elements, meaning
*third-person singular verbs* should be used to describe what each does.

<div class="wp-block-wporg-notice is-tip-notice">

<div class="wp-block-wporg-notice__icon">

</div>

<div class="wp-block-wporg-notice__content">

  
Need help remembering how to conjugate for third-person singular verbs?
Imagine prefixing the function, hook, class, or method summary with
“It”:

- *Good*: “(It) Does something.”
- *Bad:* “(It) Do something.”

</div>

</div>

**Functions**: What does the function do?

- *Good*: Handles a click on X element.
- *Bad*: Included for back-compat for X element.

**`@since`**: The recommended tool to use when searching for the version
something was added to WordPress is
[`svn blame`](https://make.wordpress.org/core/handbook/svn/code-history/#using-subversion-annotate).

If, after using these tools, the version number cannot be determined,
use `@since Unknown`.

**Code Refactoring**: Do not refactor code in the file when changes to
the documentation.

### [Grammar](#grammar)

Descriptive elements should be written as complete sentences. The one
exception to this standard is file header summaries, which are intended
as file “titles” more than sentences.

The serial (Oxford) comma should be used when listing elements in
summaries, descriptions, and parameter or return descriptions.

## [Formatting Guidelines](#formatting-guidelines)

The following guidelines should be followed to ensure that the content
of the doc blocks can be parsed properly for inclusion in the code
reference.

**Short descriptions**:

Short descriptions should be a single sentence and contain no markup of
any kind. If the description refers to an HTML element or tag, then it
should be written as “link tag”, not “\<a\>”. For example: “Fires when
printing the link tag in the header”.

**Long descriptions**:

Markdown can be used, if needed, in a long description.

**`@param` and `@return` tags**:

No HTML or markdown is permitted in the descriptions for these tags.
HTML elements and tags should be written as “audio element” or “link
tag”.

### [Line wrapping](#line-wrapping)

DocBlock text should wrap to the next line after 80 characters of text.
If the DocBlock itself is indented on the left 20 character positions,
the wrap could occur at position 100, but should not extend beyond a
total of 120 characters wide.

### [Aligning comments](#aligning-comments)

Related comments should be spaced so that they align to make them more
easily readable.

For example:

``` javascript
/**
 * @param {very_long_type} name           Description.
 * @param {type}           very_long_name Description.
 */
```

## [Functions](#functions)

Functions should be formatted as follows:

- **Summary**: A brief, one line explanation of the purpose of the
  function. Use a period at the end.
- **Description**: A supplement to the summary, providing a more
  detailed description. Use a period at the end.
- **`@deprecated x.x.x`**: Only use for deprecated functions, and
  provide the version the function was deprecated which should always be
  3-digit (e.g. `@deprecated 3.6.0`), and the function to use instead.
- **`@since x.x.x`**: Should be 3-digit for initial introduction (e.g.
  `@since 3.6.0`). If significant changes are made, additional `@since`
  tags, versions, and descriptions should be added to serve as a
  changelog.
- **`@access`**: Only use for functions if private. If the function is
  private, it is intended for internal use only, and there will be no
  documentation for it in the code reference.
- **`@class`**: Use for class constructors.
- **`@augments`**: For class constructors, list direct parents.
- **`@mixes`**: List mixins that are mixed into the object.
- **`@alias`**: If this function is first assigned to a temporary
  variable this allows you to change the name it’s documented under.
- **`@memberof`**: Namespace that this function is contained within if
  JSDoc is unable to resolve this automatically.
- **`@static`**: For classes, used to mark that a function is a static
  method on the class constructor.
- **`@see`**: A function or class relied on.
- **`@link`**: URL that provides more information.
- **`@fires`**: Event fired by the function. Events tied to a specific
  class should list the class name.
- **`@listens`**: Events this function listens for. An event must be
  prefixed with the event namespace. Events tied to a specific class
  should list the class name.
- **`@global`**: Marks this function as a global function to be included
  in the global namespace.
- **`@param`**: Give a brief description of the variable; denote
  particulars (e.g. if the variable is optional, its default) with
  [JSDoc `@param` syntax](http://jsdoc.app/tags-param.html). Use a
  period at the end.
- **`@yield`**: For [generator
  functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*),
  a description of the values expected to be yielded from this function.
  As with other descriptions, include a period at the end.
- **`@return`**: Note the period after the description.

``` javascript
/**
 * Summary. (use period)
 *
 * Description. (use period)
 *
 * @since      x.x.x
 * @deprecated x.x.x Use new_function_name() instead.
 * @access     private
 *
 * @class
 * @augments parent
 * @mixes    mixin
 *
 * @alias    realName
 * @memberof namespace
 *
 * @see  Function/class relied on
 * @link URL
 * @global
 *
 * @fires   eventName
 * @fires   className#eventName
 * @listens event:eventName
 * @listens className~event:eventName
 *
 * @param {type}   var           Description.
 * @param {type}   [var]         Description of optional variable.
 * @param {type}   [var=default] Description of optional variable with default variable.
 * @param {Object} objectVar     Description.
 * @param {type}   objectVar.key Description of a key in the objectVar parameter.
 *
 * @yield {type} Yielded value description.
 *
 * @return {type} Return value description.
 */
```

## [Backbone classes](#backbone-classes)

Backbone’s `extend` calls should be formatted as follows:

- **`@lends`** This tag will allow JSDoc to recognize the `extend`
  function from Backbone as a class definition. This should be placed
  right before the Object that contains the class definition.

Backbone’s `initialize` functions should be formatted as follows:

- **Summary**: A brief, one line explanation of the purpose of the
  function. Use a period at the end.
- **Description**: A supplement to the summary, providing a more
  detailed description. Use a period at the end.
- **`@deprecated x.x.x`**: Only use for deprecated classes, and provide
  the version the class was deprecated which should always be 3-digit
  (e.g. `@deprecated 3.6.0`), and the class to use instead.
- **`@since x.x.x`**: Should be 3-digit for initial introduction (e.g.
  `@since 3.6.0`). If significant changes are made, additional `@since`
  tags, versions, and descriptions should be added to serve as a
  changelog.
- **`@constructs`**: Marks this function as the constructor of this
  class.
- **`@augments`**: List direct parents.
- **`@mixes`**: List mixins that are mixed into the class.
- **`@requires`**: Lists modules that this class requires. Multiple
  `@requires` tags can be used.
- **`@alias`**: If this class is first assigned to a temporary variable
  this allows you to change the name it’s documented under.
- **`@memberof`**: Namespace that this class is contained within if
  JSDoc is unable to resolve this automatically.
- **`@static`**: For classes, used to mark that a function is a static
  method on the class constructor.
- **`@see`**: A function or class relied on.
- **`@link`**: URL that provides more information.
- **`@fires`**: Event fired by the constructor. Should list the class
  name.
- **`@param`**: Document the arguments passed to the constructor even if
  not explicitly listed in `initialize`. Use a period at the end.
  - Backbone Models are passed `attributes` and `options` parameters.
  - Backbone Views are passed an `options` parameter.

``` javascript
Class = Parent.extend( /** @lends namespace.Class.prototype */{
    /**
     * Summary. (use period)
     *
     * Description. (use period)
     *
     * @since      x.x.x
     * @deprecated x.x.x Use new_function_name() instead.
     * @access     private
     *
     * @constructs namespace.Class
     * @augments   Parent
     * @mixes      mixin
     *
     * @alias    realName
     * @memberof namespace
     *
     * @see   Function/class relied on
     * @link  URL
     * @fires Class#eventName
     *
     * @param {Object} attributes     The model's attributes.
     * @param {type}   attributes.key One of the model's attributes.
     * @param {Object} [options]      The model's options.
     * @param {type}   options.key One of the model's options.
     */
    initialize: function() {
        //Do stuff.
    }
} );
```

If a Backbone class does not have an initialize function it should be
documented by using `@inheritDoc` as follows:

``` javascript
/**
 * Summary. (use period)
 *
 * Description. (use period)
 *
 * @since      x.x.x
 * @deprecated x.x.x Use new_function_name() instead.
 * @access     private
 *
 * @constructs namespace.Class
 * @memberOf   namespace
 * @augments   Parent
 * @mixes      mixin
 * @inheritDoc
 *
 * @alias    realName
 * @memberof namespace
 *
 * @see   Function/class relied on
 * @link  URL
 */
Class = Parent.extend( /** @lends namespace.Class.prototype */{
// Functions and properties.
} );
```

> Note: This currently doesn’t provide the expected functionality due to
> a bug with JSDoc’s inheritDoc tag. See [JSDocs3 issue
> 1012](https://github.com/jsdoc3/jsdoc/issues/1012).

## [Local functions](#local-functions)

At times functions will be assigned to a local variable before being
assigned as a class member.  
Such functions should be marked as inner functions of the namespace that
uses them using `~`.  
The functions should be formatted as follows:

``` javascript
/**
 * Function description, you can use any JSDoc here as long as the function remains private.
 *
 * @alias namespace~doStuff
 */
var doStuff = function () {
// Do stuff.
};

Class = Parent.extend( /** @lends namespace.Class.prototype */{
    /**
     * Class description
     *
     * @constructs namespace.Class
     *
     * @borrows namespace~doStuff as prototype.doStuff
     */
    initialize: function() {
    //Do stuff.
    },

    /*
     * This function will automatically have it's documentation copied from above.
     * You should make a comment ( not a DocBlock using /**, instead use /* or // )
     * noting that you're describing this function using @borrows.
     */
    doStuff: doStuff,
} );
```

## [Local ancestors](#local-ancestors)

At times classes will have Ancestors that are only assigned to a local
variable. Such classes should be assigned to the namespace their
children are and be made inner classes using `~`.

## [Class members](#class-members)

Class members should be formatted as follows:

- **Short description**: Use a period at the end.
- **`@since x.x.x`**: Should be 3-digit for initial introduction (e.g.
  `@since 3.6.0`). If significant changes are made, additional `@since`
  tags, versions, and descriptions should be added to serve as a
  changelog.
- **`@access`**: If the members is private, protected or public. Private
  members are intended for internal use only.
- **`@type`**: List the type of the class member.
- **`@property`** List all properties this object has in case it’s an
  Object. Use a period at the end.
- **`@member`**: Optionally use this to override JSDoc’s member
  detection in place of `@type` to force a class member.
- **`@memberof`**: Optionally use this to override what class this is a
  member of.

``` javascript
/**
 * Short description. (use period)
 *
 * @since  x.x.x
 * @access (private, protected, or public)
 *
 * @type     {type}
 * @property {type} key Description.
 *
 * @member   {type} realName
 * @memberof className
 */
```

## [Namespaces](#namespaces)

Namespaces should be formatted as follows:

- **Short description**: Use a period at the end.
- **`@namespace`**: Marks this symbol as a namespace, optionally provide
  a name as an override.
- **`@since x.x.x`**: Should be 3-digit for initial introduction (e.g.
  `@since 3.6.0`). If significant changes are made, additional `@since`
  tags, versions, and descriptions should be added to serve as a
  changelog.
- **`@memberof`**: Namespace that this namespace is contained in.
- **`@property`**: Properties that this namespace exposes. Use a period
  at the end.

``` javascript
/**
 * Short description. (use period)
 *
 * @namespace realName
 * @memberof  parentNamespace
 *
 * @since x.x.x
 *
 * @property {type} key Description.
 */
```

## [Inline Comments](#inline-comments)

Inline comments inside methods and functions should be formatted as
follows:

### [Single line comments](#single-line-comments)

``` javascript
// Extract the array values.
```

### [Multi-line comments](#multi-line-comments)

``` javascript
/*
 * This is a comment that is long enough to warrant being stretched over
 * the span of multiple lines. You'll notice this follows basically
 * the same format as the JSDoc wrapping and comment block style.
 */
```

**Important note:** Multi-line comments must not begin with `/**`
(double asterisk). Use `/*` (single asterisk) instead.

## [File Headers](#file-headers)

The JSDoc file header block is used to give an overview of what is
contained in the file.

Whenever possible, all WordPress JavaScript files should contain a
header block.

WordPress uses JSHint for general code quality testing. Any inline
configuration options should be placed at the end of the header block.

``` javascript
/**
 * Summary. (use period)
 *
 * Description. (use period)
 *
 * @link   URL
 * @file   This files defines the MyClass class.
 * @author AuthorName.
 * @since  x.x.x
 */

/** jshint {inline configuration here} */
```

## [Supported JSDoc Tags](#supported-jsdoc-tags)

<figure class="wp-block-table is-style-borderless">
<table>
<thead>
<tr>
<th>Tag</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>@abstract</code></td>
<td>This method can be implemented (or overridden) by the
inheritor.</td>
</tr>
<tr>
<td><code>@access</code></td>
<td>Specify the access level of this member (private, public, or
protected).</td>
</tr>
<tr>
<td><code>@alias</code></td>
<td>Treat a member as if it had a different name.</td>
</tr>
<tr>
<td><code>@augments</code></td>
<td>This class inherits from a parent class.</td>
</tr>
<tr>
<td><code>@author</code></td>
<td>Identify the author of an item.</td>
</tr>
<tr>
<td><code>@borrows</code></td>
<td>This object uses something from another object.</td>
</tr>
<tr>
<td><code>@callback</code></td>
<td>Document a callback function.</td>
</tr>
<tr>
<td><code>@class</code></td>
<td>This function is a class constructor.</td>
</tr>
<tr>
<td><code>@classdesc</code></td>
<td>Use the following text to describe the entire class.</td>
</tr>
<tr>
<td><code>@constant</code></td>
<td>Document an object as a constant.</td>
</tr>
<tr>
<td><code>@constructs</code></td>
<td>This function member will be the constructor for the previous
class.</td>
</tr>
<tr>
<td><code>@copyright</code></td>
<td>Document some copyright information.</td>
</tr>
<tr>
<td><code>@default</code></td>
<td>Document the default value.</td>
</tr>
<tr>
<td><code>@deprecated</code></td>
<td>Document that this is no longer the preferred way.</td>
</tr>
<tr>
<td><code>@description</code></td>
<td>Describe a symbol.</td>
</tr>
<tr>
<td><code>@enum</code></td>
<td>Document a collection of related properties.</td>
</tr>
<tr>
<td><code>@event</code></td>
<td>Document an event.</td>
</tr>
<tr>
<td><code>@example</code></td>
<td>Provide an example of how to use a documented item.</td>
</tr>
<tr>
<td><code>@exports</code></td>
<td>Identify the member that is exported by a JavaScript module.</td>
</tr>
<tr>
<td><code>@external</code></td>
<td>Document an external class/namespace/module.</td>
</tr>
<tr>
<td><code>@file</code></td>
<td>Describe a file.</td>
</tr>
<tr>
<td><code>@fires</code></td>
<td>Describe the events this method may fire.</td>
</tr>
<tr>
<td><code>@function</code></td>
<td>Describe a function or method.</td>
</tr>
<tr>
<td><code>@global</code></td>
<td>Document a global object.</td>
</tr>
<tr>
<td><code>@ignore</code></td>
<td>[todo] Remove this from the final output.</td>
</tr>
<tr>
<td><code>@inner</code></td>
<td>Document an inner object.</td>
</tr>
<tr>
<td><code>@instance</code></td>
<td>Document an instance member.</td>
</tr>
<tr>
<td><code>@kind</code></td>
<td>What kind of symbol is this?</td>
</tr>
<tr>
<td><code>@lends</code></td>
<td>Document properties on an object literal as if they belonged to a
symbol with a given name.</td>
</tr>
<tr>
<td><code>@license</code></td>
<td>[todo] Document the software license that applies to this code.</td>
</tr>
<tr>
<td><code>@link</code></td>
<td>Inline tag – create a link.</td>
</tr>
<tr>
<td><code>@member</code></td>
<td>Document a member.</td>
</tr>
<tr>
<td><code>@memberof</code></td>
<td>This symbol belongs to a parent symbol.</td>
</tr>
<tr>
<td><code>@mixes</code></td>
<td>This object mixes in all the members from another object.</td>
</tr>
<tr>
<td><code>@mixin</code></td>
<td>Document a mixin object.</td>
</tr>
<tr>
<td><code>@module</code></td>
<td>Document a JavaScript module.</td>
</tr>
<tr>
<td><code>@name</code></td>
<td>Document the name of an object.</td>
</tr>
<tr>
<td><code>@namespace</code></td>
<td>Document a namespace object.</td>
</tr>
<tr>
<td><code>@param</code></td>
<td>Document the parameter to a function.</td>
</tr>
<tr>
<td><code>@private</code></td>
<td>This symbol is meant to be private.</td>
</tr>
<tr>
<td><code>@property</code></td>
<td>Document a property of an object.</td>
</tr>
<tr>
<td><code>@protected</code></td>
<td>This member is meant to be protected.</td>
</tr>
<tr>
<td><code>@public</code></td>
<td>This symbol is meant to be public.</td>
</tr>
<tr>
<td><code>@readonly</code></td>
<td>This symbol is meant to be read-only.</td>
</tr>
<tr>
<td><code>@requires</code></td>
<td>This file requires a JavaScript module.</td>
</tr>
<tr>
<td><code>@return</code></td>
<td>Document the return value of a function.</td>
</tr>
<tr>
<td><code>@see</code></td>
<td>Refer to some other documentation for more information.</td>
</tr>
<tr>
<td><code>@since</code></td>
<td>Documents the version at which the function was added, or when
significant changes are made.</td>
</tr>
<tr>
<td><code>@static</code></td>
<td>Document a static member.</td>
</tr>
<tr>
<td><code>@this</code></td>
<td>What does the ‘this’ keyword refer to here?</td>
</tr>
<tr>
<td><code>@throws</code></td>
<td>Describe what errors could be thrown.</td>
</tr>
<tr>
<td><code>@todo</code></td>
<td>Document tasks to be completed.</td>
</tr>
<tr>
<td><code>@tutorial</code></td>
<td>Insert a link to an included tutorial file.</td>
</tr>
<tr>
<td><code>@type</code></td>
<td>Document the type of an object.</td>
</tr>
<tr>
<td><code>@typedef</code></td>
<td>Document a custom type.</td>
</tr>
<tr>
<td><code>@variation</code></td>
<td>Distinguish different objects with the same name.</td>
</tr>
<tr>
<td><code>@version</code></td>
<td>Documents the version number of an item.</td>
</tr>
<tr>
<td><code>@yield</code></td>
<td>Document the yielded values of a generator function.</td>
</tr>
</tbody>
</table>
</figure>

## [Unsupported JSDoc Tags](#unsupported-jsdoc-tags)

<figure class="wp-block-table is-style-borderless">
<table>
<thead>
<tr>
<th>Tag</th>
<th>Why it’s not supported</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>@summary</code></td>
<td>Should not be used. See the example of how to separate a summary
from the full description.</td>
</tr>
<tr>
<td><code>@virtual</code></td>
<td>An unsupported synonym. Use <code>@abstract</code> instead.</td>
</tr>
<tr>
<td><code>@extends</code></td>
<td>An unsupported synonym. Use <code>@augments</code> instead.</td>
</tr>
<tr>
<td><code>@constructor</code></td>
<td>An unsupported synonym. Use <code>@class</code> instead.</td>
</tr>
<tr>
<td><code>@const</code></td>
<td>An unsupported synonym. Use <code>@constant</code> instead.</td>
</tr>
<tr>
<td><code>@defaultvalue</code></td>
<td>An unsupported synonym. Use <code>@default</code> instead.</td>
</tr>
<tr>
<td><code>@desc</code></td>
<td>An unsupported synonym. Use <code>@description</code> instead.</td>
</tr>
<tr>
<td><code>@host</code></td>
<td>An unsupported synonym. Use <code>@external</code> instead.</td>
</tr>
<tr>
<td><code>@fileoverview</code></td>
<td>An unsupported synonym. Use <code>@file</code> instead.</td>
</tr>
<tr>
<td><code>@overview</code></td>
<td>An unsupported synonym. Use <code>@file</code> instead.</td>
</tr>
<tr>
<td><code>@emits</code></td>
<td>An unsupported synonym. Use <code>@fires</code> instead.</td>
</tr>
<tr>
<td><code>@func</code></td>
<td>An unsupported synonym. Use <code>@function</code> instead.</td>
</tr>
<tr>
<td><code>@method</code></td>
<td>An unsupported synonym. Use <code>@function</code> instead.</td>
</tr>
<tr>
<td><code>@var</code></td>
<td>An unsupported synonym. Use <code>@member</code> instead.</td>
</tr>
<tr>
<td><code>@emits</code></td>
<td>An unsupported synonym. Use <code>@fires</code> instead.</td>
</tr>
<tr>
<td><code>@arg</code></td>
<td>An unsupported synonym. Use <code>@param</code> instead.</td>
</tr>
<tr>
<td><code>@argument</code></td>
<td>An unsupported synonym. Use <code>@param</code> instead.</td>
</tr>
<tr>
<td><code>@prop</code></td>
<td>An unsupported synonym. Use <code>@property</code> instead.</td>
</tr>
<tr>
<td><code>@returns</code></td>
<td>An unsupported synonym. Use <code>@return</code> instead.</td>
</tr>
<tr>
<td><code>@exception</code></td>
<td>An unsupported synonym. Use <code>@throws</code> instead.</td>
</tr>
</tbody>
</table>
</figure>

</div>

<div class="wp-block-group entry-meta is-content-justification-left is-nowrap is-layout-flex wp-container-core-group-is-layout-9c958f0d wp-block-group-is-layout-flex"
style="border-top-color:var(--wp--preset--color--light-grey-1);border-top-width:1px;margin-top:var(--wp--preset--spacing--40);margin-bottom:var(--wp--preset--spacing--40);padding-top:var(--wp--preset--spacing--40)">

<div class="wp-block-group is-layout-constrained wp-container-core-group-is-layout-979f74b1 wp-block-group-is-layout-constrained">

First published

<div class="wp-block-post-date">

April 25, 2019

</div>

</div>

<div class="wp-block-group is-layout-constrained wp-container-core-group-is-layout-979f74b1 wp-block-group-is-layout-constrained">

Last updated

<div class="wp-block-post-date__modified-date wp-block-post-date">

January 16, 2024

</div>

</div>

<div class="wp-block-group is-layout-constrained wp-container-core-group-is-layout-979f74b1 wp-block-group-is-layout-constrained">

Edit article

[Improve it on GitHub<span class="screen-reader-text">: JavaScript
Documentation
Standards</span>](https://github.com/WordPress/wpcs-docs/edit/master/inline-documentation-standards/javascript.md)

</div>

<div class="wp-block-group is-layout-constrained wp-container-core-group-is-layout-979f74b1 wp-block-group-is-layout-constrained">

Changelog

[See list of changes<span class="screen-reader-text">: JavaScript
Documentation
Standards</span>](https://github.com/WordPress/wpcs-docs/commits/master/inline-documentation-standards/javascript.md)

</div>

</div>

<div class="wp-block-group alignfull is-content-justification-space-between is-nowrap is-layout-flex wp-container-core-group-is-layout-11b2f7b6 wp-block-group-is-layout-flex"
style="border-top-color:var(--wp--preset--color--light-grey-1);border-top-style:solid;border-top-width:1px;margin-top:var(--wp--preset--spacing--20);margin-bottom:var(--wp--preset--spacing--20);padding-top:var(--wp--preset--spacing--30);padding-bottom:var(--wp--preset--spacing--30)">

<div class="post-navigation-link-previous wp-block-post-navigation-link">

<a
href="https://developer.wordpress.org/coding-standards/inline-documentation-standards/"
rel="prev"><span class="post-navigation-link__label"
aria-hidden="true">Previous</span> <span
class="post-navigation-link__title" aria-hidden="true">Inline
Documentation Standards</span> <span
class="screen-reader-text">Previous: Inline Documentation
Standards</span></a>

</div>

<div class="post-navigation-link-next wp-block-post-navigation-link">

<a
href="https://developer.wordpress.org/coding-standards/inline-documentation-standards/php/"
rel="next"><span class="post-navigation-link__label"
aria-hidden="true">Next</span> <span class="post-navigation-link__title"
aria-hidden="true">PHP Documentation Standards</span> <span
class="screen-reader-text">Next: PHP Documentation Standards</span></a>

</div>

</div>

</div>

