# PHP Inline Documentation Standards

> Snapshot source: https://developer.wordpress.org/coding-standards/inline-documentation-standards/php/
> Retrieved: 2026-07-18
> Conversion: HTML to Markdown with Pandoc.

<div id="wp--skip-link--target"
class="wp-block-group alignwide is-layout-flow wp-block-group-is-layout-flow"
role="main">

# PHP Documentation Standards

<div class="wp-block-wporg-sidebar-container is-layout-flow wp-block-wporg-sidebar-container-is-layout-flow"
breakpoint="1300px">

<div class="wp-block-wporg-table-of-contents">

<div class="wporg-table-of-contents__header">

## In this article

<span class="screen-reader-text">Table of Contents</span>

</div>

- [What Should Be Documented](#what-should-be-documented)
  - [Documenting Tips](#documenting-tips)
  - [Formatting Guidelines](#formatting-guidelines)
- [DocBlock Formatting](#docblock-formatting)
  - [1. Functions & Class Methods](#1-functions-class-methods)
  - [2. Classes](#2-classes)
  - [3. Requires and Includes](#3-requires-and-includes)
  - [4. Hooks (Actions and Filters)](#4-hooks-actions-and-filters)
  - [5. Inline Comments](#5-inline-comments)
  - [6. File Headers](#6-file-headers)
  - [7. Constants](#7-constants)
- [PHPDoc Tags](#phpdoc-tags)
  - [Deprecated Tags](#deprecated-tags)
  - [Other Tags](#other-tags)
- [Resources](#resources)

</div>

[<span class="wp-exclude-emoji" aria-hidden="true">↑</span> Back to
top](#wp--skip-link--target)

</div>

<div class="entry-content wp-block-post-content is-layout-flow wp-block-post-content-is-layout-flow">

WordPress uses a customized documentation schema that draws inspiration
from PHPDoc, an evolving standard for providing documentation to PHP
code, which is maintained by [phpDocumentor](http://phpdoc.org/).

## [What Should Be Documented](#what-should-be-documented)

PHP documentation in WordPress mostly takes the form of either formatted
blocks of documentation or inline comments.

The following is a list of what should be documented in WordPress files:

- Functions and class methods
- Classes
- Class members (including properties and constants)
- Requires and includes
- Hooks (actions and filters)
- Inline comments
- File headers
- Constants

### [Documenting Tips](#documenting-tips)

#### [Language](#language)

Summaries should be clear, simple, and brief. Avoid describing “why” an
element exists, rather, focus on documenting “what” and “when” it does
something.

A function, hook, class, or method is a *third-person singular* element,
meaning that *third-person singular verbs* should be used to describe
what each does.

<div class="wp-block-wporg-notice is-tip-notice">

<div class="wp-block-wporg-notice__icon">

</div>

<div class="wp-block-wporg-notice__content">

  
Need help remembering how to conjugate for third-person singular verbs?
Imagine prefixing the function, hook, class, or method summary with
“It”:

- *Good*: “(It) Does something.”
- *Bad*: “(It) Do something.”

</div>

</div>

Summary examples:

- **Functions**: *What* does the function do?
  - Good: *Displays the last modified date for a post.*
  - Bad: *Display the date on which the post was last modified.*
- **Filters**: *What* is being filtered?
  - Good: *Filters the post content.*
  - Bad: *Lets you edit the post content that is output in the post
    template.*
- **Actions:** *When* does an action fire?
  - Good: *Fires after most of core is loaded, and the user is
    authenticated.*
  - Bad: *Allows you to register custom post types, custom taxonomies,
    and other general housekeeping tasks after a lot of WordPress core
    has loaded.*

#### [Grammar](#grammar)

Descriptive elements should be written as complete sentences. The one
exception to this standard is file header summaries, which are intended
as file “titles” more than sentences.

The serial (Oxford) comma should be used when listing elements in
summaries, descriptions, and parameter or return descriptions.

#### [Miscellaneous](#miscellaneous)

**`@since`**: The recommended tool to use when searching for the version
something was added to WordPress is
[`svn blame`](https://make.wordpress.org/core/handbook/svn/code-history/#using-subversion-annotate).
An additional resource for older hooks is the [WordPress Hooks
Database](http://adambrown.info/p/wp_hooks).

If the version number cannot be determined after using these tools, use
`@since Unknown`.

Anything ported over from WPMU should use `@since MU (3.0.0)`. Existing
`@since MU (3.0.0)` tags should not be changed.

**Code Refactoring**: It is permissible to space out the specific action
or filter lines being documented to meet the coding standards, but do
not refactor other code in the file.

### [Formatting Guidelines](#formatting-guidelines)

<div class="wp-block-wporg-notice is-info-notice">

<div class="wp-block-wporg-notice__icon">

</div>

<div class="wp-block-wporg-notice__content">

  
WordPress’ inline documentation standards for PHP are specifically
tailored for optimum output on the [official Code
Reference](https://developer.wordpress.org/reference/). As such,
following the standards in core and formatting as described below are
*extremely* important to ensure expected output.  

</div>

</div>

#### [General](#general)

DocBlocks should directly precede the hook, action, function, method, or
class line. There should not be any opening/closing tags or other things
between the DocBlock and the declarations to prevent the parser becoming
confused.

#### [Summary](#summary)

No HTML markup or Markdown of any kind should be used in the summary. If
the text refers to an HTML element or tag, then it should be written as
“image tag” or “img” element, not “`<img>`“. For example:

- Good: *Fires when printing the link tag in the header.*
- Bad: *Fires when printing the `<link>` tag in the header.*

Inline PHPDoc tags may be used.

#### [Description](#description)

HTML markup should never be used outside of code examples, though
Markdown can be used, as needed, in the description.

1.  Lists:

    Use a hyphen (-) to create an unordered list, with a blank line
    before and after.

    ``` php
     * Description which includes an unordered list:
     *
     * - This is item 1.
     * - This is item 2.
     * - This is item 3.
     *
     * The description continues on ...
    ```

    Use numbers to create an ordered list, with a blank line before and
    after.

    ``` php
     * Description which includes an ordered list:
     *
     * 1. This is item 1.
     * 2. This is item 2.
     * 3. This is item 3.
     *
     * The description continues on ...
    ```

2.  Code samples would be created by indenting every line of the code by
    4 spaces, with a blank line before and after. Blank lines in code
    samples also need to be indented by four spaces. Note that examples
    added in this way will be output in `<pre>` tags and *not*
    syntax-highlighted.

    ``` php
     * Description including a code sample:
     *
     *    $status = array(
     *        'draft'   => __( 'Draft' ),
     *        'pending' => __( 'Pending Review' ),
     *        'private' => __( 'Private' ),
     *        'publish' => __( 'Published' )
     *    );
     *
     * The description continues on ...
    ```

3.  Links in the form of URLs, such as related Trac tickets or other
    documentation, should be added in the appropriate place in the
    DocBlock using the `@link` tag:

    ``` php
     * Description text.
     *
     * @link https://core.trac.wordpress.org/ticket/20000
    ```

#### [@since Section (Changelogs)](#since-section-changelogs)

Every function, hook, class, and method should have a corresponding
`@since` version associated with it (more on that below).

No HTML should be used in the descriptions for `@since` tags, though
limited Markdown can be used as necessary, such as for adding backticks
around variables, arguments, or parameter names, e.g. `$variable`.

Versions should be expressed in the 3-digit `x.x.x` style:

``` php
 * @since 4.4.0
```

If significant changes have been made to a function, hook, class, or
method, additional `@since` tags, versions, and descriptions should be
added to provide a changelog for that function.

“Significant changes” include but are not limited to:

- Adding new arguments or parameters.
- Required arguments becoming optional.
- Changing default/expected behaviors.
- Functions or methods becoming wrappers for new APIs.
- Parameters which have been renamed (once PHP 8.0 support has been
  announced).

PHPDoc supports multiple `@since` versions in DocBlocks for this
explicit reason. When adding changelog entries to the `@since` block, a
version should be cited, and a description should be added in sentence
case and form and end with a period:

``` php
 * @since 3.0.0
 * @since 3.8.0 Added the `post__in` argument.
 * @since 4.1.0 The `$force` parameter is now optional.
```

#### [Other Descriptions](#other-descriptions)

`@param`, `@type`, `@return`: No HTML should be used in the descriptions
for these tags, though limited Markdown can be used as necessary, such
as for adding backticks around variables, e.g. `$variable`.

- Inline `@see` tags can also be used to auto-link hooks in core:
  - Hooks, e.g.
    [`'save_post'`](https://developer.wordpress.org/reference/hooks/save_post/)
  - Dynamic hooks, e.g.
    [`'$old_status_to_$new_status'`](https://developer.wordpress.org/reference/hooks/old_status_to_new_status/)
    (Note that any extra curly braces have been removed inside the
    quotes)
- Default or available values should use single quotes, e.g. ‘draft’.
  Translatable strings should be identified as such in the description.
- HTML elements and tags should be written as “audio element” or “link
  tag”.

#### [Line wrapping](#line-wrapping)

DocBlock text should wrap to the next line after 80 characters of text.
If the DocBlock itself is indented on the left 20 character positions,
the wrap could occur at position 100, but should not extend beyond a
total of 120 characters wide.

## [DocBlock Formatting](#docblock-formatting)

The examples provided in each section below show the expected DocBlock
content and tags, as well as the exact formatting. Use spaces inside the
DocBlock, not tabs, and ensure that items in each tag group are aligned
according to the examples.

### [1. Functions & Class Methods](#1-functions-class-methods)

Functions and class methods should be formatted as follows:

- **Summary**: A brief, one sentence explanation of the purpose of the
  function spanning a maximum of two lines. Use a period at the end.
- **Description**: A supplement to the summary, providing a more
  detailed description. Use a period at the end of sentences.
- **`@ignore`**: Used when an element is meant only for internal use and
  should be skipped from parsing.
- **`@since x.x.x`**: Should always be 3-digit (e.g. `@since 3.9.0`).
  Exception is `@since MU (3.0.0)`.
- **`@access`**: Only used for core-only functions or classes
  implementing “private” core APIs. If the element is private it will be
  output with a message stating its intention for internal use.
- **`@see`**: Reference to a function, method, or class that is
  heavily-relied on within. See the note above about inline `@see` tags
  for expected formatting.
- **`@link`**: URL that provides more information. This should never be
  used to reference another function, hook, class, or method, see
  `@see`.
- **`@global`**: List PHP globals that are used within the function or
  method, with an optional description of the global. If multiple
  globals are listed, they should be aligned by type, variable, and
  description, with each other as a group.
- **`@param`**: Note if the parameter is *Optional* before the
  description, and include a period at the end. The description should
  mention accepted values as well as the default. For example:
  *Optional. This value does something. Accepts ‘post’, ‘term’, or
  empty. Default empty.*
- **`@return`**: Should contain all possible return types and a
  description for each. Use a period at the end. Note: `@return void`
  should not be used outside the default bundled themes and the PHP
  compatibility shims included in WordPress Core.

``` php
/**
 * Summary.
 *
 * Description.
 *
 * @since x.x.x
 *
 * @see Function/method/class relied on
 * @link URL
 * @global type $varname Description.
 * @global type $varname Description.
 *
 * @param type $var Description.
 * @param type $var Optional. Description. Default.
 * @return type Description.
 */
```

#### [1.1 Parameters That Are Arrays](#1-1-parameters-that-are-arrays)

Parameters that are an array of arguments should be documented in the
“originating” function only, and cross-referenced via an `@see` tag in
corresponding DocBlocks.

Array values should be documented using WordPress’ flavor of hash
notation style similar to how
[Hooks](https://developer.wordpress.org/coding-standards/inline-documentation-standards/php/#4-hooks-actions-and-filters)
can be documented, each array value beginning with the `@type` tag, and
taking the form of:

``` php
*     @type type $key Description. Default 'value'. Accepts 'value', 'value'.
*                     (aligned with Description, if wraps to a new line)
```

An example of an “originating” function and re-use of an argument array
is
[`wp_remote_request|post|get|head()`](https://core.trac.wordpress.org/browser/tags/6.0/src/wp-includes/http.php/#L114).

``` php
/**
 * Summary.
 *
 * Description.
 *
 * @since x.x.x
 *
 * @param type  $var Description.
 * @param array $args {
 *     Optional. An array of arguments.
 *
 *     @type type $key Description. Default 'value'. Accepts 'value', 'value'.
 *                     (aligned with Description, if wraps to a new line)
 *     @type type $key Description.
 * }
 * @param type  $var Description.
 * @return type Description.
 */
```

In most cases, there is no need to mark individual arguments in a hash
notation as *optional*, as the entire array is usually optional.
Specifying “Optional.” in the hash notation description should suffice.
In the case where the array is NOT optional, individual key/value pairs
may be optional and should be marked as such as necessary.

#### [1.2 Deprecated Functions](#1-2-deprecated-functions)

If the function is deprecated and should not be used any longer, the
`@deprecated` tag, along with the version and description of what to use
instead, should be added. Note the additional use of an `@see` tag – the
Code Reference uses this information to attempt to link to the
replacement function.

``` php
/**
 * Summary.
 *
 * Description.
 *
 * @since x.x.x
 * @deprecated x.x.x Use new_function_name()
 * @see new_function_name()
 *
 * @param type $var Optional. Description.
 * @param type $var Description.
 * @return type Description.
 */
```

### [2. Classes](#2-classes)

Class DocBlocks should be formatted as follows:

- **Summary**: A brief, one sentence explanation of the **purpose** of
  the class spanning a maximum of two lines. Use a period at the end.
- **Description**: A supplement to the summary, providing a more
  detailed description. Use a period at the end.
- **`@since x.x.x`**: Should always be 3-digit (e.g. `@since 3.9.0`).
  Exception is `@since MU (3.0.0)`.

``` php
/**
 * Summary.
 *
 * Description.
 *
 * @since x.x.x
 */
```

If documenting a sub-class, it’s also helpful to include an `@see` tag
reference to the super class:

``` php
/**
 * Summary.
 *
 * Description.
 *
 * @since x.x.x
 *
 * @see Super_Class
 */
```

#### [2.1 Class Members](#2-1-class-members)

##### [2.1.1 Properties](#2-1-1-properties)

Class properties should be formatted as follows:

- **Summary**: Use a period at the end.
- **`@since x.x.x`**: Should always be 3-digit (e.g. `@since 3.9.0`).
  Exception is `@since MU (3.0.0)`.
- **`@var`**: Formatted the same way as `@param`, though the description
  may be omitted.

``` php
/**
 * Summary.
 *
 * @since x.x.x
 * @var type $var Description.
 */
```

##### [2.1.2 Constants](#2-1-2-constants)

**Summary**: Use a period at the end.

**`@since x.x.x`**: Should always be 3-digit (e.g. `@since 3.9.0`).
Exception is `@since MU (3.0.0)`.

**`@var`**: Formatted the same way as `@param`, though the description
may be omitted.

``` php
/**
 * Summary.
 *
 * @since x.x.x
 * @var type $var Description.
 */
const NAME = value;
```

### [3. Requires and Includes](#3-requires-and-includes)

Files required or included should be documented with a summary
description DocBlock. Optionally, this may apply to inline
`get_template_part()` calls as needed for clarity.

``` php
/**
 * Summary.
 */
require_once( ABSPATH . WPINC . '/filename.php' );
```

### [4. Hooks (Actions and Filters)](#4-hooks-actions-and-filters)

Both action and filter hooks should be documented on the line
immediately preceding the call to `do_action()` or
`do_action_ref_array()`, or `apply_filters()` or
`apply_filters_ref_array()`, and formatted as follows:

- **Summary**: A brief, one line explanation of the purpose of the hook.
  Use a period at the end.
- **Description**: A supplemental description to the summary, if
  warranted.
- **`@ignore`**: Used when a hook is meant only for internal use and
  should be skipped from parsing.
- **`@since x.x.x`**: Should always be 3-digit (e.g. `@since 3.9.0`).
  Exception is `@since MU (3.0.0)`.
- **`@param`**: If the parameter is an array of arguments, document each
  argument using a hash notation (described above in the *Parameters
  That Are Arrays* section), and include a period at the end of each
  line.

Note that `@return` is *not* used for hook documentation, because action
hooks return nothing, and filter hooks always return their first
parameter.

``` php
/**
 * Summary.
 *
 * Description.
 *
 * @since x.x.x
 *
 * @param type  $var Description.
 * @param array $args {
 *     Short description about this hash.
 *
 *     @type type $var Description.
 *     @type type $var Description.
 * }
 * @param type  $var Description.
 */
```

If a hook is in the middle of a block of HTML or a long conditional, the
DocBlock should be placed on the line immediately before the start of
the HTML block or conditional, even if it means forcing line-breaks/PHP
tags in a continuous line of HTML.

Tools to use when searching for the version a hook was added are
[`svn blame`](https://make.wordpress.org/core/handbook/svn/code-history/#using-subversion-annotate),
or the [WordPress Hooks Database](http://adambrown.info/p/wp_hooks) for
older hooks. If, after using these tools, the version number cannot be
determined, use `@since Unknown`.

#### [4.1 Duplicate Hooks](#4-1-duplicate-hooks)

Occasionally, hooks will be used multiple times in the same or separate
core files. In these cases, rather than list the entire DocBlock every
time, only the first-added or most logically-placed version of an action
or filter will be fully documented. Subsequent versions should have a
single-line comment.

For actions:

``` php
/** This action is documented in path/to/filename.php */
```

For filters:

``` php
/** This filter is documented in path/to/filename.php */
```

To determine which instance should be documented, search for multiples
of the same hook tag, then use
[`svn blame`](https://make.wordpress.org/core/handbook/svn/code-history/#using-subversion-annotate)
to find the first use of the hook in terms of the earliest revision. If
multiple instances of the hook were added in the same release, document
the one most logically-placed as the “primary”.

### [5. Inline Comments](#5-inline-comments)

Inline comments inside methods and functions should be formatted as
follows:

#### [5.1 Single line comments](#5-1-single-line-comments)

``` php
// Allow plugins to filter an array.
```

#### [5.2 Multi-line comments](#5-2-multi-line-comments)

``` php
/*
 * This is a comment that is long enough to warrant being stretched over
 * the span of multiple lines. You'll notice this follows basically
 * the same format as the PHPDoc wrapping and comment block style.
 */
```

**Important note**: Multi-line comments must not begin with `/**`
(double asterisk) as the parser might mistake it for a DocBlock. Use
`/*` (single asterisk) instead.

### [6. File Headers](#6-file-headers)

The file header DocBlock is used to give an overview of what is
contained in the file.

Whenever possible, **all** WordPress files should contain a header
DocBlock, regardless of the file’s contents – this includes files
containing classes.

``` php
/**
 * Summary (no period for file headers)
 *
 * Description. (use period)
 *
 * @link URL
 *
 * @package WordPress
 * @subpackage Component
 * @since x.x.x (when the file was introduced)
 */
```

The *Summary* section is meant to serve as a succinct description of
**what** specific purpose the file serves.

Examples:

- Good: *“Widgets API:
  <a href="https://developer.wordpress.org/reference/classes/wp_widget/"
  rel="class">WP_Widget</a> class”*
- Bad: *“Core widgets class”*

The *Description* section can be used to better explain overview
information for the file such as how the particular file fits into the
overall makeup of an API or component.

Examples:

- Good: *“The Widgets API is comprised of the
  <a href="https://developer.wordpress.org/reference/classes/wp_widget/"
  rel="class">WP_Widget</a> and <a
  href="https://developer.wordpress.org/reference/classes/wp_widget_factory/"
  rel="class">WP_Widget_Factory</a> classes in addition to a variety of
  top-level functionality that implements the Widgets and related
  sidebar APIs. WordPress registers a number of common widgets by
  default.”*

### [7. Constants](#7-constants)

The constant DocBlock is used to give a description of the constant for
better use and understanding.

Constants should be formatted as follows:

- **Summary**: Use a period at the end.
- **`@since x.x.x`**: Should always be 3-digit (e.g. `@since 3.9.0`).
  Exception is `@since MU (3.0.0)`.
- **`@var`**: Formatted the same way as `@param`. The description is
  optional.

``` php
/**
 * Summary.
 *
 * @since x.x.x (if available)
 * @var type $var Description.
 */
```

## [PHPDoc Tags](#phpdoc-tags)

Common PHPDoc tags used in WordPress include `@since`, `@see`, `@global`
`@param`, and `@return` (see table below for full list).

For the most part, tags are used correctly, but not all the time. For
instance, sometimes you’ll see an `@link` tag inline, linking to a
separate function or method. “Linking” to known classes, methods, or
functions is not necessary, as the Code Reference automatically links
these elements. For “linking” hooks inline, the proper tag to use is
`@see` – see the *Other Descriptions* section.

<figure class="wp-block-table is-style-borderless">
<table>
<thead>
<tr>
<th>Tag</th>
<th>Usage</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong><code>@access</code></strong></td>
<td>private</td>
<td>Only used in limited circumstances, like when visibility modifiers
cannot be used in the code, and only when private, such as for core-only
functions or core classes implementing “private” APIs. Used directly
below the <code>@since</code> line in block.</td>
</tr>
<tr>
<td><strong><code>@deprecated</code></strong></td>
<td>version x.x.x Use <em>replacement function name</em> instead</td>
<td>What version of WordPress the function/method was deprecated. Use
3-digit version number. Should be accompanied by a matching
<code>@see</code> tag.</td>
</tr>
<tr>
<td><strong><code>@global</code></strong></td>
<td>datatype $variable description</td>
<td>Document global(s) used in the function/method. For boolean and
integer types, use <code>bool</code> and <code>int</code>,
respectively.</td>
</tr>
<tr>
<td><strong><code>@internal</code></strong></td>
<td>information string</td>
<td>Typically used wrapped in <code>{}</code> for adding notes for
internal use only.</td>
</tr>
<tr>
<td><strong><code>@ignore</code></strong></td>
<td>(standalone)</td>
<td>Used to skip parsing of the entire element.</td>
</tr>
<tr>
<td><strong><code>@link</code></strong></td>
<td>URL</td>
<td>Link to additional information for the function/method. For an
external script/library, links to source. Not to be used for related
functions/methods; use <code>@see</code> instead.</td>
</tr>
<tr>
<td><strong><code>@method</code></strong></td>
<td>returntype description</td>
<td>Shows a “magic” method found inside the class.</td>
</tr>
<tr>
<td><strong><code>@package</code></strong></td>
<td>packagename</td>
<td>Specifies package that all functions, includes, and defines in the
file belong to. Found in DocBlock at top of the file. For core (and
bundled themes), this is always <strong>WordPress</strong>.</td>
</tr>
<tr>
<td><strong><code>@param</code></strong></td>
<td>datatype $variable description</td>
<td>Function/method parameter of the format: parameter type, variable
name, description, default behavior. For boolean and integer types, use
<code>bool</code> and <code>int</code>, respectively.</td>
</tr>
<tr>
<td><strong><code>@return</code></strong></td>
<td>datatype description</td>
<td>Document the return value of functions or methods.
<code>@return void</code> should not be used outside of the default
bundled themes. For boolean and integer types, use <code>bool</code> and
<code>int</code>, respectively.</td>
</tr>
<tr>
<td><strong><code>@see</code></strong></td>
<td>elementname</td>
<td>References another function/method/class the function/method relies
on. Should only be used inline for “linking” hooks.</td>
</tr>
<tr>
<td><strong><code>@since</code></strong></td>
<td>version x.x.x</td>
<td>Documents release version function/method was added. Use 3-digit
version number – this is to aid with version searches, and for use when
comparing versions in code. Exception is
<code>@since MU (3.0.0)</code>.</td>
</tr>
<tr>
<td><strong><code>@static</code></strong></td>
<td>(standalone)</td>
<td>Note: This tag has been used in the past, but should no longer be
used. Just using the static keyword in your code is enough for
phpDocumentor on PHP5+ to recognize static variables and methods, and
PhpDocumentor will mark them as static.</td>
</tr>
<tr>
<td><strong><code>@staticvar</code></strong></td>
<td>datatype $variable description</td>
<td>Note: This tag has been used in the past, but should no longer be
used. Document a static variable’s use in a function/method. For boolean
and integer types, use <code>bool</code> and <code>int</code>,
respectively.</td>
</tr>
<tr>
<td><strong><code>@subpackage</code></strong></td>
<td>subpackagename</td>
<td>For page-level DocBlock, specifies the Component that all functions
and defines in file belong to. For class-level DocBlock, specifies the
subpackage/component the class belongs to.</td>
</tr>
<tr>
<td><strong><code>@todo</code></strong></td>
<td>information string</td>
<td>Documents planned changes to an element that have not been
implemented.</td>
</tr>
<tr>
<td><strong><code>@type</code></strong></td>
<td>datatype description for an argument array value</td>
<td>Used to denote argument array value types. See the
<strong>Hooks</strong> or <strong>Parameters That Are Arrays</strong>
sections for example syntax.</td>
</tr>
<tr>
<td><strong><code>@uses</code></strong></td>
<td>class::methodname() / class::$variablename / functionname()</td>
<td>Note: This tag has been used in the past, but should no longer be
used. References a key function/method used. May include a short
description.</td>
</tr>
<tr>
<td><strong><code>@var</code></strong></td>
<td>datatype description</td>
<td>Data type for a class variable and short description. Callbacks are
marked callback.</td>
</tr>
</tbody>
</table>
</figure>

<div class="wp-block-wporg-notice is-info-notice">

<div class="wp-block-wporg-notice__icon">

</div>

<div class="wp-block-wporg-notice__content">

  
PHPDoc tags work with some text editors/IDEs to display more information
about a piece of code. It is useful to developers using those editors to
understand what the purpose is, and where they would use it in their
code. PhpStorm and Netbeans already support PHPDoc.

The following text editors/IDEs have extensions/bundles you can install
that will help you auto-create DocBlocks:

- Notepad++: [DocIt for
  Notepad++](http://sourceforge.net/projects/nppdocit/) (Windows)
- TextMate: [php.tmbundle](https://github.com/textmate/php.tmbundle)
  (Mac)
- SublimeText: [sublime
  packages](https://packagecontrol.io/search/phpdoc) (Windows, Mac,
  Linux)

Note: Even with help generating DocBlocks, most code editors don’t do a
very thorough job – it’s likely you’ll need to manually fill in certain
areas of any generated DocBlocks.  

</div>

</div>

### [Deprecated Tags](#deprecated-tags)

> **Preface:** For the time being, and for the sake of consistency,
> WordPress Core will continue to use `@subpackage` tags – both when
> writing new DocBlocks, and editing old ones.
>
> Only when the new – external – PSR-5 recommendations are finalized,
> will across-the-board changes be considered, such as deprecating
> certain tags.

As proposed in the [new
PSR-5](https://github.com/phpDocumentor/fig-standards/blob/master/proposed/phpdoc.md)
recommendations, the following PHPDoc tag should be deprecated:

- `@subpackage` (in favor of a unified package tag:
  `@package Package\Subpackage`)
- `@static` (no longer needed)
- `@staticvar` (no longer needed)

### [Other Tags](#other-tags)

**`@package` Tag in Plugins and Themes (bundled themes excluded)**

Third-party plugin and theme authors **must not** use
`@package WordPress` in their plugins or themes. The `@package` name for
plugins should be the plugin name; for themes, it should be the theme
name, spaced with underscores: `Twenty_Fifteen`.

**`@author` Tag**

It is WordPress’ policy not to use the `@author` tag, except in the case
of maintaining it in external libraries. We do not want to imply any
sort of “ownership” over code that might discourage contribution.

**`@copyright` and `@license` Tags**

The `@copyright` and `@license` tags are used in external libraries and
scripts, and should not be used in WordPress core files.

- `@copyright` is used to specify external script copyrights.
- `@license` is used to specify external script licenses.

## [Resources](#resources)

- [Wikipedia on PHPDoc](http://en.wikipedia.org/wiki/PHPDoc)
- [PEAR Standards](http://pear.php.net/manual/en/standards.sample.php)
- [phpDocumentor](http://www.phpdoc.org/)
- [phpDocumentor Tutorial
  Tags](http://manual.phpdoc.org/HTMLSmartyConverter/HandS/phpDocumentor/tutorial_tags.pkg.html)
- [Draft PSR-5
  recommendations](https://github.com/phpDocumentor/fig-standards/blob/master/proposed/phpdoc.md)
- [Draft PSR-19
  recommendations](https://github.com/phpDocumentor/fig-standards/blob/master/proposed/phpdoc-tags.md)

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

September 9, 2025

</div>

</div>

<div class="wp-block-group is-layout-constrained wp-container-core-group-is-layout-979f74b1 wp-block-group-is-layout-constrained">

Edit article

[Improve it on GitHub<span class="screen-reader-text">: PHP
Documentation
Standards</span>](https://github.com/WordPress/wpcs-docs/edit/master/inline-documentation-standards/php.md)

</div>

<div class="wp-block-group is-layout-constrained wp-container-core-group-is-layout-979f74b1 wp-block-group-is-layout-constrained">

Changelog

[See list of changes<span class="screen-reader-text">: PHP Documentation
Standards</span>](https://github.com/WordPress/wpcs-docs/commits/master/inline-documentation-standards/php.md)

</div>

</div>

<div class="wp-block-group alignfull is-content-justification-space-between is-nowrap is-layout-flex wp-container-core-group-is-layout-11b2f7b6 wp-block-group-is-layout-flex"
style="border-top-color:var(--wp--preset--color--light-grey-1);border-top-style:solid;border-top-width:1px;margin-top:var(--wp--preset--spacing--20);margin-bottom:var(--wp--preset--spacing--20);padding-top:var(--wp--preset--spacing--30);padding-bottom:var(--wp--preset--spacing--30)">

<div class="post-navigation-link-previous wp-block-post-navigation-link">

<a
href="https://developer.wordpress.org/coding-standards/inline-documentation-standards/javascript/"
rel="prev"><span class="post-navigation-link__label"
aria-hidden="true">Previous</span> <span
class="post-navigation-link__title" aria-hidden="true">JavaScript
Documentation Standards</span> <span
class="screen-reader-text">Previous: JavaScript Documentation
Standards</span></a>

</div>

<div class="post-navigation-link-next wp-block-post-navigation-link">

<a href="https://developer.wordpress.org/coding-standards/changelog/"
rel="next"><span class="post-navigation-link__label"
aria-hidden="true">Next</span> <span class="post-navigation-link__title"
aria-hidden="true">Changelog</span> <span
class="screen-reader-text">Next: Changelog</span></a>

</div>

</div>

</div>

