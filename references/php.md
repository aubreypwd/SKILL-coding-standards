# PHP Coding Standards

> Snapshot source: https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/
> Retrieved: 2026-07-18
> Conversion: HTML to Markdown with Pandoc.

<div id="wp--skip-link--target"
class="wp-block-group alignwide is-layout-flow wp-block-group-is-layout-flow"
role="main">

# PHP Coding Standards

<div class="wp-block-wporg-sidebar-container is-layout-flow wp-block-wporg-sidebar-container-is-layout-flow"
breakpoint="1300px">

<div class="wp-block-wporg-table-of-contents">

<div class="wporg-table-of-contents__header">

## In this article

<span class="screen-reader-text">Table of Contents</span>

</div>

[General](#general)

- [Opening and Closing PHP Tags](#opening-and-closing-php-tags)
- [No Shorthand PHP Tags](#no-shorthand-php-tags)
- [Single and Double Quotes](#single-and-double-quotes)
- [Writing require/include
  statements](#writing-require-include-statements)

[Naming](#naming)

- [Naming Conventions](#naming-conventions)
- [Interpolation for Naming Dynamic
  Hooks](#interpolation-for-naming-dynamic-hooks)

[Whitespace](#whitespace)

- [Space Usage](#space-usage)
- [Indentation](#indentation)
- [Remove Trailing Spaces](#remove-trailing-spaces)

[Formatting](#formatting)

- [Brace Style](#brace-style)
- [Declaring Arrays](#declaring-arrays)
- [Multiline Function Calls](#multiline-function-calls)
- [Type declarations](#type-declarations)
- [Magic constants](#magic-constants)
- [Spread operator …](#spread-operator)

[Declare Statements, Namespace, and Import
Statements](#declare-statements-namespace-and-import-statements)

- [Namespace declarations](#namespace-declarations)
- [Using import use statements](#using-import-use-statements)

[Object-Oriented Programming](#object-oriented-programming)

- [Only One Object Structure (Class/Interface/Trait/Enum) per
  File](#only-one-object-structure-class-interface-trait-enum-per-file)
- [Trait Use Statements](#trait-use-statements)
- [Visibility should always be
  declared](#visibility-should-always-be-declared)
- [Visibility and modifier order](#visibility-and-modifier-order)
- [Object Instantiation](#object-instantiation)

[Control Structures](#control-structures)

- [Use elseif, not else if](#use-elseif-not-else-if)
- [Yoda Conditions](#yoda-conditions)

[Operators](#operators)

- [Ternary Operator](#ternary-operator)
- [Error Control Operator @](#error-control-operator)
- [Increment/decrement operators](#increment-decrement-operators)

[Database](#database)

- [Database Queries](#database-queries)
- [Formatting SQL statements](#formatting-sql-statements)

[Recommendations](#recommendations)

- [Self-Explanatory Flag Values for Function
  Arguments](#self-explanatory-flag-values-for-function-arguments)
- [Clever Code](#clever-code)
- [Closures (Anonymous Functions)](#closures-anonymous-functions)
- [Regular Expressions](#regular-expressions)
- [Don’t extract()](#dont-extract)
- [Shell commands](#shell-commands)

</div>

[<span class="wp-exclude-emoji" aria-hidden="true">↑</span> Back to
top](#wp--skip-link--target)

</div>

<div class="entry-content wp-block-post-content is-layout-flow wp-block-post-content-is-layout-flow">

These PHP coding standards are intended for the WordPress community as a
whole. They are mandatory for WordPress Core and we encourage you to use
them for your themes and plugins as well.

While themes and plugins may choose to follow a different *coding
style*, these ***coding standards*** are not just about *code style*,
but also encompass established best practices regarding
interoperability, translatability, and security in the WordPress
ecosystem, so, even when using a different *code style*, we recommend
you still adhere to the WordPress Coding Standards in regard to these
best practices.

While not all code may fully comply with these standards (yet), all
newly committed and/or updated code should fully comply with these
coding standards.

Also see the [PHP Inline Documentation
Standards](https://developer.wordpress.org/coding-standards/inline-documentation-standards/php/)
for further guidelines.

If you want to automatically check your code against this standard, you
can use the official [WordPress Coding
Standards](https://github.com/WordPress/WordPress-Coding-Standards)
tooling, which is run using
[PHP_CodeSniffer](https://github.com/PHPCSStandards/PHP_CodeSniffer/).

## [General](#general)

### [Opening and Closing PHP Tags](#opening-and-closing-php-tags)

When embedding multi-line PHP snippets within an HTML block, the PHP
open and close tags must be on a line by themselves.

Correct (Multiline):

``` php
function foo() {
    ?>
    <div>
        <?php
        echo esc_html(
            bar(
                $baz,
                $bat
            )
        );
        ?>
    </div>
    <?php
}
```

Correct (Single Line):

``` php
<input name="<?php echo esc_attr( $name ); ?>" />
```

Incorrect:

``` php
if ( $a === $b ) { ?>
<some html>
<?php }
```

### [No Shorthand PHP Tags](#no-shorthand-php-tags)

**Important:** Never use shorthand PHP start tags. Always use full PHP
tags.

Correct:

``` php
<?php ... ?>
<?php echo esc_html( $var ); ?>
```

Incorrect:

``` php
<? ... ?>
<?= esc_html( $var ) ?>
```

### [Single and Double Quotes](#single-and-double-quotes)

Use single and double quotes when appropriate. If you’re not evaluating
anything in the string, use single quotes. You should almost never have
to escape quotes in a string, because you can just alternate your
quoting style, like so:

``` php
echo '<a href="/static/link" class="button button-primary">Link name</a>';
echo "<a href='{$escaped_link}'>text with a ' single quote</a>";
```

Text that goes into HTML or XML attributes should be escaped so that
single or double quotes do not end the attribute value and invalidate
the HTML, causing a security issue. See [Data
Validation](https://developer.wordpress.org/plugins/security/data-validation/)
in the Plugin Handbook for further details.

### [Writing require/include statements](#writing-require-include-statements)

Because `require[_once]` and `include[_once]` are language constructs,
they do not need parentheses around the path, so those shouldn’t be
used. There should only be one space between the path and the
require/include keywords.

It is *strongly recommended* to use `require[_once]` for unconditional
includes. When using `include[_once]`, PHP will throw a warning when the
file is not found but will continue execution, which will almost
certainly lead to other errors/warnings/notices being thrown if your
application depends on the file loaded, potentially leading to security
leaks. For that reason, `require[_once]` is generally the better choice
as it will throw a `Fatal Error` if the file cannot be found.

``` php
// Correct.
require_once ABSPATH . 'file-name.php';

// Incorrect.
include_once  ( ABSPATH . 'file-name.php' );
require_once     __DIR__ . '/file-name.php';
```

## [Naming](#naming)

### [Naming Conventions](#naming-conventions)

Use lowercase letters in variable, action/filter, and function names
(never `camelCase`). Separate words via underscores. Don’t abbreviate
variable names unnecessarily; let the code be unambiguous and
self-documenting.

``` php
function some_name( $some_variable ) {}
```

For function parameter names, it is *strongly recommended* to avoid
reserved keywords as names, as it leads to hard to read and confusing
code when using the PHP 8.0 “named parameters in function calls”
feature.  
Also keep in mind that renaming a function parameter should be
considered a breaking change since PHP 8.0, so name function parameters
with due care!

Class, trait, interface and enum names should use capitalized words
separated by underscores. Any acronyms should be all upper case.

``` php
class Walker_Category extends Walker {}
class WP_HTTP {}

interface Mailer_Interface {}
trait Forbid_Dynamic_Properties {}
enum Post_Status {}
```

Constants should be in all upper-case with underscores separating words:

``` php
define( 'DOING_AJAX', true );
```

Files should be named descriptively using lowercase letters. Hyphens
should separate words.

``` php
my-plugin-name.php
```

Class file names should be based on the class name with `class-`
prepended and the underscores in the class name replaced with hyphens,
for example, `WP_Error` becomes:

``` php
class-wp-error.php
```

This file-naming standard is for all current and new files with classes,
except test classes.  
For files containing test classes, the file name should reflect the
class name exactly, as per PSR4. This is to ensure cross-version
[compatibility with all supported PHPUnit
versions](https://github.com/sebastianbergmann/phpunit/pull/4109).

Files containing template tags in the `wp-includes` directory should
have `-template` appended to the end of the name so that they are
obvious.

``` php
general-template.php
```

### [Interpolation for Naming Dynamic Hooks](#interpolation-for-naming-dynamic-hooks)

Dynamic hooks should be named using interpolation rather than
concatenation for readability and discoverability purposes.

Dynamic hooks are hooks that include dynamic values in their tag name,
e.g. `{$new_status}_{$post->post_type}` (publish_post).

Variables used in hook tags should be wrapped in curly braces `{` and
`}`, with the complete outer tag name wrapped in double quotes. This is
to ensure PHP can correctly parse the given variables’ types within the
interpolated string.

``` php
do_action( "{$new_status}_{$post->post_type}", $post->ID, $post );
```

Where possible, dynamic values in tag names should also be as succinct
and to the point as possible. `$user_id` is much more self-documenting
than, say, `$this->id`.

## [Whitespace](#whitespace)

### [Space Usage](#space-usage)

Always put spaces after commas, and on both sides of logical,
arithmetic, comparison, string and assignment operators.

``` php
SOME_CONST === 23;
foo() && bar();
! $foo;
array( 1, 2, 3 );
$baz . '-5';
$term .= 'X';
if ( $object instanceof Post_Type_Interface ) {}
$result = 2 ** 3; // 8.
```

Put spaces on both sides of the opening and closing parentheses of
control structure blocks.

``` php
foreach ( $foo as $bar ) { ...
```

When defining a function, do it like so:

``` php
function my_function( $param1 = 'foo', $param2 = 'bar' ) { ...

function my_other_function() { ...
```

When calling a function, do it like so:

``` php
my_function( $param1, func_param( $param2 ) );
my_other_function();
```

When performing logical comparisons, do it like so:

``` php
if ( ! $foo ) { ...
```

[Type
casts](https://www.php.net/manual/en/language.types.type-juggling.php#language.types.typecasting)
must be lowercase. Always prefer the short, canonical form of type
casts, `(int)` instead of `(integer)` and `(bool)` rather than
`(boolean)`. For float casts use `(float)`.

``` php
$foo = (bool) $bar; // Correct.
$foo = (boolean) $bar; // Incorrect.
```

When referring to array items, only include a space around the index if
it is a variable, for example:

``` php
$x = $foo['bar']; // Correct.
$x = $foo[ 'bar' ]; // Incorrect.

$x = $foo[0]; // Correct.
$x = $foo[ 0 ]; // Incorrect.

$x = $foo[ $bar ]; // Correct.
$x = $foo[$bar]; // Incorrect.
```

In a `switch` block, there must be no space between the `case` condition
and the colon.

``` php
switch ( $foo ) {
    case 'bar': // Correct.
    case 'bar' : // Incorrect.
}
```

Unless otherwise specified, parentheses should have spaces inside them.

``` php
if ( $foo && ( $bar || $baz ) ) { ...

my_function( ( $x - 1 ) * 5, $y );
```

When using increment (`++`) or decrement (`--`) operators, there should
be no spaces between the operator and the variable it applies to.

``` php
// Correct.
for ( $i = 0; $i < 10; $i++ ) {}

// Incorrect.
for ( $i = 0; $i < 10; $i ++ ) {}
++   $b; // Multiple spaces.
```

### [Indentation](#indentation)

Your indentation should always reflect logical structure. Use **real
tabs**, **not spaces**, as this allows the most flexibility across
clients.

Exception: if you have a block of code that would be more readable if
things are aligned, use spaces:

``` php
[tab]$foo   = 'somevalue';
[tab]$foo2  = 'somevalue2';
[tab]$foo34 = 'somevalue3';
[tab]$foo5  = 'somevalue4';
```

For arrays with explicit keys, *each item* should start on a new line
when the array contains more than one item:

``` php
$query = new WP_Query( array( 'ID' => 123 ) );
```

``` php
$args = array(
[tab]'post_type'   => 'page',
[tab]'post_author' => 123,
[tab]'post_status' => 'publish',
);

$query = new WP_Query( $args );
```

Note the comma after the last array item: this is recommended because it
makes it easier to change the order of the array, and makes for cleaner
diffs when new items are added.

``` php
$my_array = array(
[tab]'foo'   => 'somevalue',
[tab]'foo2'  => 'somevalue2',
[tab]'foo3'  => 'somevalue3',
[tab]'foo34' => 'somevalue3',
);
```

For `switch` control structures, `case` statements should be indented
one tab from the `switch` statement and the contents of the `case`
should be indented one tab from the `case` condition statement.

``` php
switch ( $type ) {
[tab]case 'foo':
[tab][tab]some_function();
[tab][tab]break;
[tab]case 'bar':
[tab][tab]some_function();
[tab][tab]break;
}
```

**Rule of thumb:** Tabs should be used at the beginning of the line for
indentation, while spaces can be used mid-line for alignment.

### [Remove Trailing Spaces](#remove-trailing-spaces)

Remove trailing whitespace at the end of each line. Omitting the closing
PHP tag at the end of a file is preferred. If you use the tag, make sure
you remove trailing whitespace.

There should be no trailing blank lines at the end of a function body.

## [Formatting](#formatting)

### [Brace Style](#brace-style)

Braces shall be used for all blocks in the style shown here:

``` php
if ( condition ) {
    action1();
    action2();
} elseif ( condition2 && condition3 ) {
    action3();
    action4();
} else {
    defaultaction();
}
```

If you have a really long block, consider whether it can be broken into
two or more shorter blocks, functions, or methods, to reduce complexity,
improve ease of testing, and increase readability.

Braces should always be used, even when they are not required:

``` php
if ( condition ) {
    action0();
}

if ( condition ) {
    action1();
} elseif ( condition2 ) {
    action2a();
    action2b();
}

foreach ( $items as $item ) {
    process_item( $item );
}
```

Note that requiring the use of braces means that *single-statement
inline control structures* are prohibited. You are free to use the
[alternative syntax for control
structures](https://www.php.net/manual/en/control-structures.alternative-syntax.php)
(e.g. `if`/`endif`, `while`/`endwhile`)—especially in templates where
PHP code is embedded within HTML, for instance:

``` php
<?php if ( have_posts() ) : ?>
    <div class="hfeed">
        <?php while ( have_posts() ) : the_post(); ?>
            <article id="<?php echo esc_attr( 'post-' . get_the_ID() ); ?>" class="<?php echo esc_attr( get_post_class() ); ?>">
                <!-- ... -->
            </article>
        <?php endwhile; ?>
    </div>
<?php endif; ?>
```

### [Declaring Arrays](#declaring-arrays)

Using long array syntax ( `array( 1, 2, 3 )` ) for declaring arrays is
generally more readable than short array syntax ( `[ 1, 2, 3 ]` ),
particularly for those with vision difficulties. Additionally, it’s much
more descriptive for beginners.

Arrays must be declared using long array syntax.

### [Multiline Function Calls](#multiline-function-calls)

When splitting a function call over multiple lines, each parameter must
be on a separate line. Single line inline comments can take up their own
line.

Each parameter must take up no more than a single line. Multi-line
parameter values must be assigned to a variable and then that variable
should be passed to the function call.

``` php
$bar = array(
    'use_this' => true,
    'meta_key' => 'field_name',
);
$baz = sprintf(
    /* translators: %s: Friend's name */
    __( 'Hello, %s!', 'yourtextdomain' ),
    $friend_name
);

$a = foo(
    $bar,
    $baz,
    /* translators: %s: cat */
    sprintf( __( 'The best pet is a %s.' ), 'cat' )
);
```

### [Type declarations](#type-declarations)

Type declarations must have exactly one space before and after the type.
The nullability operator (`?`) is regarded as part of the type
declaration and there should be no space between this operator and the
actual type. Class/interface/enum name based type declarations should
use the case of the class/interface/enum name as declared, while the
keyword-based type declarations should be lowercased.

Return type declarations should have no space between the closing
parenthesis of the function declaration and the colon starting a return
type.

These rules apply to all structures allowing for type declarations:
functions, closures, enums, catch conditions as well as the PHP 7.4
arrow functions and typed properties.

``` php
// Correct.
function foo( Class_Name $parameter, callable $callable, int $number_of_things = 0 ) {
    // Do something.
}

function bar(
    Interface_Name&Concrete_Class $param_a,
    string|int $param_b,
    callable $param_c = 'default_callable'
): User|false {
    // Do something.
}

// Incorrect.
function baz(Class_Name $param_a, String$param_b,      CALLABLE     $param_c )   :   ?   iterable   {
    // Do something.
}
```

<div class="wp-block-wporg-notice is-info-notice">

<div class="wp-block-wporg-notice__icon">

</div>

<div class="wp-block-wporg-notice__content">

Type declaration usage has the following restrictions based on the
minimum required PHP version of an application, whether it is WordPress
Core, a plugin or a theme:

- The scalar `bool`, `int`, `float`, and `string` type declarations
  cannot be used until the minimum PHP version is PHP 7.0 or higher.
- Return type declarations cannot be used until the minimum PHP version
  is PHP 7.0 or higher.
- Nullable type declarations cannot be used until the minimum PHP
  version is PHP 7.1 or higher.
- The `iterable` and `void` type declarations cannot be used until the
  minimum PHP version is PHP 7.1 or higher. The `void` type can only be
  used as a return type.
- The `object` type declaration cannot be used until the minimum PHP
  version is PHP 7.2 or higher.
- Property type declarations cannot be used until the minimum PHP
  version is PHP 7.4 or higher.
- `static` (return type only) cannot be used until the minimum PHP
  version is PHP 8.0 or higher.
- `mixed` type cannot be used until the minimum PHP version is PHP 8.0
  or higher. Take note that the `mixed` type includes `null`, so cannot
  be made nullable.
- Union types cannot be used until the minimum PHP version is PHP 8.0 or
  higher.
- Intersection types cannot be used until the minimum PHP version is PHP
  8.1 or higher.
- `never` (return type only) cannot be used until the minimum PHP
  version is PHP 8.1 or higher.
- Disjunctive normal form types (combining union types and intersection
  types) cannot be used until the minimum PHP version is PHP 8.2 or
  higher.  

*Usage in WordPress Core*

<div class="wp-block-wporg-notice is-warning-notice">

<div class="wp-block-wporg-notice__icon">

</div>

<div class="wp-block-wporg-notice__content">

  
Adding type declarations to existing WordPress Core functions should be
done with extreme care.  

</div>

</div>

The function signature of any function (method) which can be overloaded
by plugins or themes should not be touched.  
This leaves, for now, only unconditionally declared functions in the
global namespace, `private` class methods, and code new to Core, as
candidates for adding type declarations.

Note: Using the `array` keyword in type declarations is **strongly
discouraged** for now, as most often, it would be better to use
`iterable` to allow for more flexibility in the implementation and that
keyword is not yet available for use in WordPress Core until the minimum
requirements are raised to PHP 7.1.

### [Magic constants](#magic-constants)

The [PHP native `__*__` magic
constants](https://www.php.net/manual/en/language.constants.magic.php),
like `__CLASS__` and `__DIR__`, should be written in uppercase when
used.

When using the `::class` constant for class name resolution, the `class`
keyword should be in lowercase and there should be no spaces around the
`::` operator.

``` php
// Correct.
add_action( 'action_name', array( __CLASS__, 'method_name' ) );
add_action( 'action_name', array( My_Class::class, 'method_name' ) );

// Incorrect.
require_once __dIr__ . '/relative-path/file-name.php';
add_action( 'action_name', array( My_Class :: CLASS, 'method_name' ) );
```

### [Spread operator …](#spread-operator)

When using the spread operator, there should be one space or a new line
with the appropriate indentation before the spread operator. There
should be no spaces between the spread operator and the
variable/function call it applies to. When combining the spread operator
with the reference operator (`&`), there should be no spaces between
them.

``` php
// Correct.
function foo( &...$spread ) {
    bar( ...$spread );

    bar(
        array( ...$foo ),
        ...array_values( $keyed_array )
    );
}

// Incorrect.
function fool( &   ... $spread ) {
    bar(...
             $spread );

    bar(
        [...  $foo ],... array_values( $keyed_array )
    );
}
```

<div class="wp-block-wporg-notice is-info-notice">

<div class="wp-block-wporg-notice__icon">

</div>

<div class="wp-block-wporg-notice__content">

  
The spread operator (or splat operator as it’s known in other languages)
can be used for packing arguments in function declarations (variadic
functions) and unpacking them in function calls as of PHP 5.6. Since PHP
7.4, the spread operator is also used for unpacking numerically-indexed
arrays, with string-keyed array unpacking available since PHP 8.1.  
When used in a function declaration, the spread operator can only be
used with the last parameter.  

</div>

</div>

## [Declare Statements, Namespace, and Import Statements](#declare-statements-namespace-and-import-statements)

### [Namespace declarations](#namespace-declarations)

Each part of a namespace name should consist of capitalized words
separated by underscores.

Namespace declarations should have exactly one blank line before the
declaration and at least one blank line after.

``` php
namespace Prefix\Admin\Domain_URL\Sub_Domain\Event; // Correct.
```

There should be only one namespace declaration per file, and it should
be at the top of the file. Namespace declarations using curly brace
syntax are not allowed. Explicit global namespace declaration (namespace
declaration without name) are also not allowed.

``` php
// Incorrect: namespace declaration using curly brace syntax.
namespace Foo {
    // Code.
}

// Incorrect: namespace declaration for the global namespace.
namespace {
    // Code.
}
```

*There is currently no timeline for introducing namespaces to WordPress
Core.*

The use of namespaces in plugins and themes is strongly encouraged. It
is a great way to prefix a lot of your code to prevent naming conflicts
with other plugins, themes and/or WordPress Core.

Please do make sure you use a unique and long enough namespace prefix to
actually prevent conflicts. Generally speaking, using a namespace prefix
along the lines of `Vendor\Project_Name` is a good idea.

<div class="wp-block-wporg-notice is-warning-notice">

<div class="wp-block-wporg-notice__icon">

</div>

<div class="wp-block-wporg-notice__content">

  
The `wp` and `WordPress` namespace prefixes are reserved for WordPress
itself.  

</div>

</div>

<div class="wp-block-wporg-notice is-info-notice">

<div class="wp-block-wporg-notice__icon">

</div>

<div class="wp-block-wporg-notice__content">

  
Namespacing has no effect on variables, constants declared with
`define()` or non-PHP native constructs, like the hook names as used in
WordPress.  
Those still need to be prefixed individually.  

</div>

</div>

### [Using import use statements](#using-import-use-statements)

Using import `use` statements allows you to refer to constants,
functions, classes, interfaces, namespaces, enums and traits that live
outside of the current namespace.

Import `use` statements should be at the top of the file and follow the
(optional) `namespace` declaration. They should follow a specific order
based on the **type** of the import:

1.  `use` statements for namespaces, classes, interfaces, traits and
    enums
2.  `use` statements for functions
3.  `use` statements for constants

Aliases can be used to prevent name collisions (two classes in different
namespaces using the same class name).  
When using aliases, make sure the aliases follow the WordPress naming
convention and are unique.

The following examples showcase the correct and incorrect usage of
import `use` statements regarding things like spacing, groupings,
leading backslashes, etc.

Correct:

``` php
namespace Project_Name\Feature;

use Project_Name\Sub_Feature\Class_A;
use Project_Name\Sub_Feature\Class_C as Aliased_Class_C;
use Project_Name\Sub_Feature\{
    Class_D,
    Class_E as Aliased_Class_E,
}

use function Project_Name\Sub_Feature\function_a;
use function Project_Name\Sub_Feature\function_b as aliased_function;

use const Project_Name\Sub_Feature\CONSTANT_A;
use const Project_Name\Sub_Feature\CONSTANT_D as ALIASED_CONSTANT;

// Rest of the code.
```

Incorrect:

``` php
namespace Project_Name\Feature;

use   const   Project_Name\Sub_Feature\CONSTANT_A; // Superfluous whitespace after the "use" and the "const" keywords.
use function Project_Name\Sub_Feature\function_a; // Function import after constant import.
use \Project_Name\Sub_Feature\Class_C as aliased_class_c; // Leading backslash shouldn't be used, alias doesn't comply with naming conventions.
use Project_Name\Sub_Feature\{Class_D, Class_E   as   Aliased_Class_E} // Extra spaces around the "as" keyword, incorrect whitespace use inside the brace opener and closer.
use Vendor\Package\{ function function_a, function function_b,
     Class_C,
        const CONSTANT_NAME}; // Combining different types of imports in one use statement, incorrect whitespace use within group use statement.

class Foo {
    // Code.
}

use const \Project_Name\Sub_Feature\CONSTANT_D as Aliased_constant; // Import after class definition, leading backslash, naming conventions violation.
use function Project_Name\Sub_Feature\function_b as Aliased_Function; // Import after class definition, naming conventions violation.

// Rest of the code.
```

<div class="wp-block-wporg-notice is-alert-notice">

<div class="wp-block-wporg-notice__icon">

</div>

<div class="wp-block-wporg-notice__content">

  
Import `use` statements have no effect on dynamic class, function or
constant names.  
Group `use` statements are available from PHP 7.0, and trailing commas
in group `use` statements are available from PHP 7.2.  

</div>

</div>

<div class="wp-block-wporg-notice is-info-notice">

<div class="wp-block-wporg-notice__icon">

</div>

<div class="wp-block-wporg-notice__content">

  
Note that, unless you have implemented
[autoloading](https://www.php.net/manual/en/language.oop5.autoload.php),
the `use` statement won’t automatically load whatever is being imported.
For OO constructs, you’ll either need to set up autoloading or load the
file(s) containing the OO declaration(s) using `require[_once]` or
`include[_once]` statement(s).  
Autoloading is only applicable to OO constructs; for functions and
constants, you must always use `require[_once]` or `include[_once]`.  

</div>

</div>

**Note about WordPress Core usage**

While import `use` statements can already be used in WordPress Core, it
is, for the moment, **strongly discouraged**.

Import `use` statements are most useful when combined with namespaces
and a class autoloading implementation.  
As neither of these are currently in place for WordPress Core and
discussions about this are ongoing, holding off on adding import `use`
statements to WordPress Core is the sensible choice for now.

## [Object-Oriented Programming](#object-oriented-programming)

### [Only One Object Structure (Class/Interface/Trait/Enum) per File](#only-one-object-structure-class-interface-trait-enum-per-file)

For instance, if we have a file called `class-example-class.php` it can
only contain one class in that file.

``` php
// Incorrect: file class-example-class.php.
class Example_Class {}

class Example_Class_Extended {}
```

The second class should be in its own file called
`class-example-class-extended.php`.

``` php
// Correct: file class-example-class.php.
class Example_Class {}
```

``` php
// Correct: file class-example-class-extended.php.
class Example_Class_Extended {}
```

### [Trait Use Statements](#trait-use-statements)

Trait `use` statements should be at the top of a class and should have
exactly one blank line before the first `use` statement, and at least
one blank line after the last statement. The only exception is when the
class only contains trait `use` statements, in which case the blank line
after may be omitted.

The following code examples show the formatting requirements for trait
`use` statements regarding things like spacing, grouping and
indentation.

``` php
// Correct.
class Foo {

    use Bar_Trait;
    use Foo_Trait,
        Bazinga_Trait {
        Bar_Trait::method_name insteadof Bar_Trait;
        Bazinga_Trait::method_name as bazinga_method;
    }
    use Loopy_Trait {
        eat as protected;
    }

    public $baz = true;

    ...
}

// Incorrect.
class Foo {
    // No blank line before trait use statement, multiple spaces after the use keyword.
    use       Bar_Trait;

    /*
     * Multiple spaces when importing traits, no new line after opening brace.
     * Aliasing should be done on the same line as the method it's replacing.
     */
    use Foo_Trait,   Bazinga_Trait{Bar_Trait::method_name    insteadof     Foo_Trait; Bazinga_Trait::method_name
        as     bazinga_method;
            }; // Wrongly indented brace.
    public $baz = true; // Missing blank line after trait import.

    ...
}
```

### [Visibility should always be declared](#visibility-should-always-be-declared)

For all constructs that allow it (properties, methods, class constants
since PHP 7.1), visibility should be explicitly declared.  
Using the `var` keyword for property declarations is not allowed.

``` php
// Correct.
class Foo {
    public $foo;

    protected function bar() {}
}

// Incorrect.
class Foo {
    var $foo;

    function bar() {}
}
```

*Usage in WordPress Core*

Visibility for class constants can not be used in WordPress Core until
the minimum PHP version has been raised to PHP 7.1.

### [Visibility and modifier order](#visibility-and-modifier-order)

When using multiple modifiers for a *class declaration*, the order
should be as follows:

1.  First the optional `abstract` or `final` modifier.
2.  Next, the optional `readonly` modifier.

When using multiple modifiers for a *constant declaration* inside
object-oriented structures, the order should be as follows:

1.  First the optional `final` modifier.
2.  Next, the visibility modifier.

When using multiple modifiers for a *property declaration*, the order
should be as follows:

1.  First a visibility modifier.
2.  Next, the optional `static` or `readonly` modifier (these keywords
    are mutually exclusive).
3.  Finally, the optional `type` declaration.

When using multiple modifiers for a *method declaration*, the order
should be as follows:

1.  First the optional `abstract` or `final` modifier.
2.  Then, a visibility modifier.
3.  Finally, the optional `static` modifier.

``` php
// Correct.
abstract readonly class Foo {
    private const LABEL = 'Book';

    public static $foo;

    private readonly string $bar;

    abstract protected static function bar();
}

// Incorrect.
class Foo {
    protected final const SLUG = 'book';

    static public $foo;

    static protected final function bar() {
        // Code.
    };
}
```

<div class="wp-block-wporg-notice is-info-notice">

<div class="wp-block-wporg-notice__icon">

</div>

<div class="wp-block-wporg-notice__content">

  
– Visibility for OO constants can be declared since PHP 7.1.  
– Typed properties are available since PHP 7.4.  
– Readonly properties are available since PHP 8.1.  
– `final` modifier for constants in object-oriented structures is
available since PHP 8.1.  
– Readonly classes are available since PHP 8.2.  

</div>

</div>

### [Object Instantiation](#object-instantiation)

When instantiating a new object instance, parenthesis must always be
used, even when not strictly necessary.  
There should be no space between the name of the class being
instantiated and the opening parenthesis.

``` php
// Correct.
$foo = new Foo();
$anonymous_class = new class( $parameter ) { ... };
$instance = new static();

// Incorrect.
$foo = new Foo;
$anonymous_class = new class ( $input ) { ... };
```

## [Control Structures](#control-structures)

### [Use elseif, not else if](#use-elseif-not-else-if)

`else if` is not compatible with the colon syntax for `if|elseif`
blocks. For this reason, use `elseif` for conditionals.

### [Yoda Conditions](#yoda-conditions)

``` php
if ( true === $the_force ) {
    $victorious = you_will( $be );
}
```

When doing logical comparisons involving variables, always put the
variable on the right side and put constants, literals, or function
calls on the left side. If neither side is a variable, the order is not
important. (In [computer science
terms](https://en.wikipedia.org/wiki/Value_(computer_science)#Assignment:_l-values_and_r-values),
in comparisons always try to put l-values on the right and r-values on
the left.)

In the above example, if you omit an equals sign (admit it, it happens
even to the most seasoned of us), you’ll get a parse error, because you
can’t assign to a constant like `true`. If the statement were the other
way around `( $the_force = true )`, the assignment would be perfectly
valid, returning `1`, causing the if statement to evaluate to `true`,
and you could be chasing that bug for a while.

A little bizarre, it is, to read. Get used to it, you will.

This applies to `==`, `!=`, `===`, and `!==`. Yoda conditions for `<`,
`>`, `<=` or `>=` are significantly more difficult to read and are best
avoided.

## [Operators](#operators)

### [Ternary Operator](#ternary-operator)

[Ternary
operators](https://www.php.net/manual/en/language.operators.comparison.php#language.operators.comparison.ternary)
are fine, but always have them test if the statement is true, not false.
Otherwise, it just gets confusing. (An exception would be using
`! empty()`, as testing for false here is generally more intuitive.)

The short ternary operator must not be used.

For example:

``` php
// (if statement is true) ? (do this) : (else, do this);
$musictype = ( 'jazz' === $music ) ? 'cool' : 'blah';
// (if field is not empty ) ? (do this) : (else, do this);
```

### [Error Control Operator @](#error-control-operator)

As noted in the [PHP
docs](https://www.php.net/manual/en/language.operators.errorcontrol.php):

> PHP supports one error control operator: the at sign (@). When
> prepended to an expression in PHP, any diagnostic error that might be
> generated by that expression will be suppressed.

While this operator does exist in Core, it is often used lazily instead
of doing proper error checking. Its use is highly discouraged, as even
the PHP docs also state:

> Warning: Prior to PHP 8.0.0, it was possible for the @ operator to
> disable critical errors that will terminate script execution. For
> example, prepending @ to a call of a function that did not exist, by
> being unavailable or mistyped, would cause the script to terminate
> with no indication as to why.

### [Increment/decrement operators](#increment-decrement-operators)

Pre-increment/decrement should be favored over post-increment/decrement
for stand-alone statements.

Pre-increment/decrement will increment/decrement and then return, while
post-increment/decrement will return and then increment/decrement.  
Using the “pre” version is slightly more performant and can prevent
future bugs when code gets moved around.

``` php
// Correct: Pre-decrement.
--$a;

// Incorrect: Post-decrement.
$a--;
```

## [Database](#database)

### [Database Queries](#database-queries)

Avoid touching the database directly. If there is a defined function
that can get the data you need, use it. Database abstraction (using
functions instead of queries) helps keep your code forward-compatible
and, in cases where results are cached in memory, it can be many times
faster.

If you must touch the database, consider creating a
[Trac](https://core.trac.wordpress.org/) ticket. There you can discuss
the possibility of adding a new function to cover the functionality you
wanted, for a future version of WordPress.

### [Formatting SQL statements](#formatting-sql-statements)

When formatting SQL statements you may break them into several lines and
indent if it is sufficiently complex to warrant it. Most statements work
well as one line though. Always capitalize the SQL parts of the
statement like `UPDATE` or `WHERE`.

Functions that update the database should expect their parameters to
lack SQL slash escaping when passed. Escaping should be done as close to
the time of the query as possible, preferably by using
`$wpdb->prepare()`

`$wpdb->prepare()` is a method that handles escaping, quoting, and
int-casting for SQL queries. It uses a subset of the `sprintf()` style
of formatting. Example :

``` php
$var = "dangerous'"; // Raw data that may or may not need to be escaped.
$id = some_foo_number(); // Data we expect to be an integer, but we're not certain.

$wpdb->query( $wpdb->prepare( "UPDATE $wpdb->posts SET post_title = %s WHERE ID = %d", $var, $id ) );
```

The following placeholders can be used in the query string:

- %d (integer)
- %f (float)
- %s (string)
- %i (identifier, e.g. table/field names)

Note that these placeholders should not be ‘quoted’! `$wpdb->prepare()`
will take care of escaping and quoting for us. This makes it easy to see
at a glance whether something has been escaped or not because it happens
right when the query happens.

See [Data
Validation](https://developer.wordpress.org/plugins/security/data-validation/)
in the Plugin Handbook for further details.

## [Recommendations](#recommendations)

### [Self-Explanatory Flag Values for Function Arguments](#self-explanatory-flag-values-for-function-arguments)

Prefer string values to just `true` and `false` when calling functions.

``` php
// Incorrect.
function eat( $what, $slowly = true ) {
...
}
eat( 'mushrooms' );
eat( 'mushrooms', true ); // What does true mean?
eat( 'dogfood', false ); // What does false mean? The opposite of true?
```

PHP only supports named arguments as of PHP 8.0. However, as WordPress
currently still supports older PHP versions, we cannot yet use those.  
Without named arguments, the values of the flags are meaningless, and
each time we come across a function call like the examples above, we
have to search for the function definition. The code can be made more
readable by using descriptive string values, instead of booleans.

``` php
// Correct.
function eat( $what, $speed = 'slowly' ) {
...
}
eat( 'mushrooms' );
eat( 'mushrooms', 'slowly' );
eat( 'dogfood', 'quickly' );
```

When more words are needed to describe the function parameters, an
`$args` array may be a better pattern.

``` php
function eat( $what, $args ) {
...
}
eat ( 'noodles', array( 'speed' => 'moderate' ) );
```

Be careful when using this pattern, as it can lead to “Undefined array
index” notices if input isn’t validated before use. Use this pattern
only where it makes sense (i.e. multiple possible arguments), not just
for the sake of it.

### [Clever Code](#clever-code)

In general, readability is more important than cleverness or brevity.

``` php
isset( $var ) || $var = some_function();
```

Although the above line is clever, it takes a while to grok if you’re
not familiar with it. So, just write it like this:

``` php
if ( ! isset( $var ) ) {
    $var = some_function();
}
```

Unless absolutely necessary, loose comparisons should not be used, as
their behavior can be misleading.

Correct:

``` php
if ( 0 === strpos( $text, 'WordPress' ) ) {
    echo esc_html__( 'Yay WordPress!', 'textdomain' );
}
```

Incorrect:

``` php
if ( 0 == strpos( 'WordPress', 'foo' ) ) {
    echo esc_html__( 'Yay WordPress!', 'textdomain' );
}
```

Assignments must not be placed in conditionals.

Correct:

``` php
$data = $wpdb->get_var( '...' );
if ( $data ) {
    // Use $data.
}
```

Incorrect:

``` php
if ( $data = $wpdb->get_var( '...' ) ) {
    // Use $data.
}
```

In a `switch` statement, it’s okay to have multiple empty cases fall
through to a common block. If a case contains a block, then falls
through to the next block, however, this must be explicitly commented.

``` php
switch ( $foo ) {
    case 'bar':                // Correct, an empty case can fall through without comment.
    case 'baz':
        echo esc_html( $foo ); // Incorrect, a case with a block must break, return, or have a comment.
    case 'cat':
        echo 'mouse';
        break;                 // Correct, a case with a break does not require a comment.
    case 'dog':
        echo 'horse';
        // no break            // Correct, a case can have a comment to explicitly mention the fall through.
    case 'fish':
        echo 'bird';
        break;
}
```

The `goto` statement must never be used.

The `eval()` construct is *very dangerous* and is impossible to secure.
It must not be used.

The `create_function()` function internally performs an `eval()`. It was
deprecated in PHP 7.2 and removed in PHP 8.0. It must not be used.

### [Closures (Anonymous Functions)](#closures-anonymous-functions)

Where appropriate, closures may be used as an alternative to creating
new functions to pass as callbacks. For example:

``` php
$caption = preg_replace_callback(
    '/<[a-zA-Z0-9]+(?: [^<>]+>)*/',
    function ( $matches ) {
        return preg_replace( '/[\r\n\t]+/', ' ', $matches[0] );
    },
    $caption
);
```

Closures should not be passed as filter or action callbacks, as removing
these via `remove_action()` / `remove_filter()` is complex (at this
time) (see
[\#46635](https://core.trac.wordpress.org/ticket/46635 "Improve identifying of non–trivial callbacks in hooks")
for a proposal to address this).

### [Regular Expressions](#regular-expressions)

Perl compatible regular expressions ([PCRE](https://www.php.net/pcre),
`preg_` functions) should be used.  
Never use the `/e` switch, use `preg_replace_callback` instead.

It’s most convenient to use single-quoted strings for regular
expressions since, contrary to double-quoted strings, they have only two
metasequences which need escaping: `\'` and `\\`.

### [Don’t extract()](#dont-extract)

Per
[\#22400](https://core.trac.wordpress.org/ticket/22400 "Remove all, or at least most, uses of extract() within WordPress"):

> `extract()` is a terrible function that makes code harder to debug and
> harder to understand. We should discourage it’s \[sic\] use and remove
> all of our uses of it.

Joseph Scott has [a good write-up of why it’s
bad](https://blog.josephscott.org/2009/02/05/i-dont-like-phps-extract-function/).

### [Shell commands](#shell-commands)

Use of the [backtick
operator](https://www.php.net/manual/en/language.operators.execution.php)
is not allowed.

Use of the backtick operator is identical to
[`shell_exec()`](https://www.php.net/manual/en/function.shell-exec.php),
and most hosts disable this function in the `php.ini` file for security
reasons.

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

February 8, 2026

</div>

</div>

<div class="wp-block-group is-layout-constrained wp-container-core-group-is-layout-979f74b1 wp-block-group-is-layout-constrained">

Edit article

[Improve it on GitHub<span class="screen-reader-text">: PHP Coding
Standards</span>](https://github.com/WordPress/wpcs-docs/edit/master/wordpress-coding-standards/php.md)

</div>

<div class="wp-block-group is-layout-constrained wp-container-core-group-is-layout-979f74b1 wp-block-group-is-layout-constrained">

Changelog

[See list of changes<span class="screen-reader-text">: PHP Coding
Standards</span>](https://github.com/WordPress/wpcs-docs/commits/master/wordpress-coding-standards/php.md)

</div>

</div>

<div class="wp-block-group alignfull is-content-justification-space-between is-nowrap is-layout-flex wp-container-core-group-is-layout-11b2f7b6 wp-block-group-is-layout-flex"
style="border-top-color:var(--wp--preset--color--light-grey-1);border-top-style:solid;border-top-width:1px;margin-top:var(--wp--preset--spacing--20);margin-bottom:var(--wp--preset--spacing--20);padding-top:var(--wp--preset--spacing--30);padding-bottom:var(--wp--preset--spacing--30)">

<div class="post-navigation-link-previous wp-block-post-navigation-link">

<a
href="https://developer.wordpress.org/coding-standards/wordpress-coding-standards/javascript/"
rel="prev"><span class="post-navigation-link__label"
aria-hidden="true">Previous</span> <span
class="post-navigation-link__title" aria-hidden="true">JavaScript Coding
Standards</span> <span class="screen-reader-text">Previous: JavaScript
Coding Standards</span></a>

</div>

<div class="post-navigation-link-next wp-block-post-navigation-link">

<a
href="https://developer.wordpress.org/coding-standards/inline-documentation-standards/"
rel="next"><span class="post-navigation-link__label"
aria-hidden="true">Next</span> <span class="post-navigation-link__title"
aria-hidden="true">Inline Documentation Standards</span> <span
class="screen-reader-text">Next: Inline Documentation
Standards</span></a>

</div>

</div>

<div class="wp-block-wporg-sidebar-container is-layout-flow wp-block-wporg-sidebar-container-is-layout-flow"
breakpoint="768px">

<div class="wporg-chapter-list__header">

## Chapters

<span class="screen-reader-text">Chapter list</span>

</div>

- [Best Practices](https://developer.wordpress.org/coding-standards/)
- [WordPress Coding
  Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/)
  - [Accessibility Coding
    Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/accessibility/)
  - [CSS Coding
    Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/css/)
  - [GitHub Actions Workflow
    Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/github-actions/)
  - [HTML Coding
    Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/html/)
  - [JavaScript Coding
    Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/javascript/)
  - <a
    href="https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/"
    aria-current="page">PHP Coding Standards</a>
- [Inline Documentation
  Standards](https://developer.wordpress.org/coding-standards/inline-documentation-standards/)
  - [JavaScript Documentation
    Standards](https://developer.wordpress.org/coding-standards/inline-documentation-standards/javascript/)
  - [PHP Documentation
    Standards](https://developer.wordpress.org/coding-standards/inline-documentation-standards/php/)
- [Changelog](https://developer.wordpress.org/coding-standards/changelog/)
- [Markdown Style
  Guide](https://developer.wordpress.org/coding-standards/styleguide/)

</div>

</div>

</div>

<div class="wporg-footer wp-block-template-part">

- [About](https://wordpress.org/about/)
- [News](https://wordpress.org/news/)
- [Hosting](https://wordpress.org/hosting/)
- [Privacy](https://wordpress.org/about/privacy/)

<!-- -->

- [Showcase](https://wordpress.org/showcase/)
- [Themes](https://wordpress.org/themes/)
- [Plugins](https://wordpress.org/plugins/)
- [Patterns](https://wordpress.org/patterns/)

<!-- -->

- [Learn](https://learn.wordpress.org/)
- [Documentation](https://wordpress.org/documentation/)
- [Developers](https://developer.wordpress.org/)
- [WordPress.tv <span class="wp-exclude-emoji"
  aria-hidden="true">↗</span>](https://wordpress.tv/)

<!-- -->

- [Get Involved](https://make.wordpress.org/)
- [Events](https://events.wordpress.org/)
- [Donate <span class="wp-exclude-emoji"
  aria-hidden="true">↗</span>](https://wordpressfoundation.org/donate/)
- [Five for the Future](https://wordpress.org/five-for-the-future/)

<!-- -->

- [WordPress.com <span class="wp-exclude-emoji"
  aria-hidden="true">↗</span>](https://wordpress.com/?ref=wporg-footer)
- [Matt <span class="wp-exclude-emoji"
  aria-hidden="true">↗</span>](https://ma.tt/)
- [bbPress <span class="wp-exclude-emoji"
  aria-hidden="true">↗</span>](https://bbpress.org/)
- [BuddyPress <span class="wp-exclude-emoji"
  aria-hidden="true">↗</span>](https://buddypress.org/)

<div class="wp-block-group global-footer__logos-container is-layout-flow wp-block-group-is-layout-flow">

<div class="wp-block-group is-content-justification-left is-nowrap is-layout-flex wp-container-core-group-is-layout-e1f0195b wp-block-group-is-layout-flex">

<figure class="wp-block-image global-footer__wporg-logo-mark">
<a href="https://wordpress.org/"><img
src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHJvbGU9ImltZyIgd2lkdGg9IjI4IiBoZWlnaHQ9IjI4IiB2aWV3Ym94PSIwIDAgMjggMjgiPgogICAgPHRpdGxlPldvcmRQcmVzcy5vcmc8L3RpdGxlPgogICAgPHBhdGggZmlsbD0iY3VycmVudENvbG9yIiBkPSJNMTMuNjA1MiAwLjkyMzUyNUMxNi4xNDMyIDAuOTIzNTI1IDE4LjYxMzcgMS42Nzk1MyAyMC43MDYyIDMuMDk3MDNDMjIuNzQ0NyA0LjQ3NDAzIDI0LjM1MTIgNi40MTgwMyAyNS4zMDk3IDguNjg2MDNDMjYuOTgzNyAxMi42NDE1IDI2LjUzODIgMTcuMTY0IDI0LjEzNTIgMjAuNzE0NUMyMi43NTgyIDIyLjc1MyAyMC44MTQyIDI0LjM1OTUgMTguNTQ2MiAyNS4zMThDMTQuNTkwNyAyNi45OTIgMTAuMDY4MiAyNi41NDY1IDYuNTE3NzIgMjQuMTQzNUM0LjQ3OTIyIDIyLjc2NjUgMi44NzI3MiAyMC44MjI1IDEuOTE0MjIgMTguNTU0NUMwLjI0MDIyNSAxNC41OTkgMC42ODU3MjUgMTAuMDc2NSAzLjA4ODcyIDYuNTI2MDNDNC40NjU3MiA0LjQ4NzUzIDYuNDA5NzMgMi44ODEwMyA4LjY3NzcyIDEuOTIyNTNDMTAuMjMwMiAxLjI2MTAzIDExLjkxNzcgMC45MjM1MjUgMTMuNjA1MiAwLjkyMzUyNVpNMTMuNjA1MiAwLjExMzUyNUM2LjE1MzIyIDAuMTEzNTI1IDAuMTA1MjI1IDYuMTYxNTMgMC4xMDUyMjUgMTMuNjEzNUMwLjEwNTIyNSAyMS4wNjU1IDYuMTUzMjIgMjcuMTEzNSAxMy42MDUyIDI3LjExMzVDMjEuMDU3MiAyNy4xMTM1IDI3LjEwNTIgMjEuMDY1NSAyNy4xMDUyIDEzLjYxMzVDMjcuMTA1MiA2LjE2MTUzIDIxLjA1NzIgMC4xMTM1MjUgMTMuNjA1MiAwLjExMzUyNVoiIC8+CiAgICA8cGF0aCBmaWxsPSJjdXJyZW50Q29sb3IiIGQ9Ik0yLjM2MDExIDEzLjYxMzNDMi4zNjAxMSAxNy45MTk4IDQuODE3MTEgMjEuODYxOCA4LjcwNTExIDIzLjczODNMMy4zMzIxMSA5LjAzNjg0QzIuNjg0MTEgMTAuNDgxMyAyLjM2MDExIDEyLjAzMzggMi4zNjAxMSAxMy42MTMzWk0yMS4yMDYxIDEzLjA0NjNDMjEuMjA2MSAxMS42NTU4IDIwLjcwNjYgMTAuNjk3MyAyMC4yNzQ2IDkuOTQxMzRDMTkuODQyNiA5LjE4NTM0IDE5LjE2NzYgOC4yMjY4NCAxOS4xNjc2IDcuMzA4ODRDMTkuMTY3NiA2LjM5MDg0IDE5Ljk1MDYgNS4zMTA4NCAyMS4wNTc2IDUuMzEwODRIMjEuMjA2MUMxNi42Mjk2IDEuMTEyMzQgOS41MTUxMSAxLjQyMjg0IDUuMzE2NjEgNi4wMTI4NEM0LjkxMTYxIDYuNDU4MzQgNC41MzM2MSA2LjkzMDg0IDQuMjA5NjEgNy40MzAzNEg0LjkzODYxQzYuMTEzMTEgNy40MzAzNCA3LjkzNTYxIDcuMjgxODQgNy45MzU2MSA3LjI4MTg0QzguNTQzMTEgNy4yNDEzNCA4LjYxMDYxIDguMTMyMzQgOC4wMDMxMSA4LjIxMzM0QzguMDAzMTEgOC4yMTMzNCA3LjM5NTYxIDguMjgwODQgNi43MjA2MSA4LjMyMTM0TDEwLjgxMTEgMjAuNTExOEwxMy4yNjgxIDEzLjEyNzNMMTEuNTEzMSA4LjMyMTM0QzEwLjkwNTYgOC4yODA4NCAxMC4zMzg2IDguMjEzMzQgMTAuMzM4NiA4LjIxMzM0QzkuNzMxMTEgOC4xNzI4NCA5Ljc5ODYxIDcuMjU0ODQgMTAuNDA2MSA3LjI4MTg0QzEwLjQwNjEgNy4yODE4NCAxMi4yNjkxIDcuNDMwMzQgMTMuMzYyNiA3LjQzMDM0QzE0LjQ1NjEgNy40MzAzNCAxNi4zNTk2IDcuMjgxODQgMTYuMzU5NiA3LjI4MTg0QzE2Ljk2NzEgNy4yNDEzNCAxNy4wMzQ2IDguMTMyMzQgMTYuNDI3MSA4LjIxMzM0QzE2LjQyNzEgOC4yMTMzNCAxNS44MTk2IDguMjgwODQgMTUuMTQ0NiA4LjMyMTM0TDE5LjIwODEgMjAuNDE3M0wyMC4zNjkxIDE2Ljc0NTNDMjAuODgyMSAxNS4xMzg4IDIxLjE5MjYgMTQuMDA0OCAyMS4xOTI2IDEzLjAzMjhMMjEuMjA2MSAxMy4wNDYzWk0xMy43OTQ2IDE0LjU4NTNMMTAuNDE5NiAyNC4zOTk4QzEyLjY4NzYgMjUuMDYxMyAxNS4xMDQxIDI1LjAwNzMgMTcuMzMxNiAyNC4yMjQzTDE3LjI1MDYgMjQuMDc1OEwxMy43OTQ2IDE0LjU4NTNaTTIzLjQ3NDEgOC4yMTMzNEMyMy41MjgxIDguNTkxMzQgMjMuNTU1MSA4Ljk4Mjg0IDIzLjU1NTEgOS4zNzQzNEMyMy41NTUxIDEwLjUyMTggMjMuMzM5MSAxMS44MDQzIDIyLjcwNDYgMTMuMzk3M0wxOS4yNjIxIDIzLjMzMzNDMjQuNTI3MSAyMC4yNjg4IDI2LjQwMzYgMTMuNTU5MyAyMy40NzQxIDguMjEzMzRaIiAvPgo8L3N2Zz4=" /></a>
</figure>

<figure class="wp-block-image global-footer__wporg-logo-full">
<a href="https://wordpress.org/"><img
src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHJvbGU9ImltZyIgd2lkdGg9IjMyOSIgaGVpZ2h0PSI1MiIgdmlld2JveD0iMCAwIDMyOSA1MiI+CiAgICA8dGl0bGU+V29yZFByZXNzLm9yZzwvdGl0bGU+CiAgICA8cGF0aCBmaWxsPSJjdXJyZW50Q29sb3IiIGQ9Ik00LjMzIDI2YTIxLjY4IDIxLjY4IDAgMCAwIDEyLjIyIDE5LjVMNi4yMSAxNy4xOEEyMS42NiAyMS42NiAwIDAgMCA0LjMzIDI2Wk0yNi4zOCAyNy44OWwtNi41IDE4Ljg5YTIxLjMxIDIxLjMxIDAgMCAwIDYuMTIuODkgMjEuNzcgMjEuNzcgMCAwIDAgNy4yLTEuMjMgMS40MjkgMS40MjkgMCAwIDEtLjE2LS4zbC02LjY2LTE4LjI1WiIgLz4KICAgIDxwYXRoIGZpbGw9ImN1cnJlbnRDb2xvciIgZD0iTTI2IDBhMjYgMjYgMCAxIDAgMCA1MiAyNiAyNiAwIDAgMCAwLTUyWm0yMC4yNyAzOS42NmEyNC40NyAyNC40NyAwIDAgMS0yOS43OCA4Ljg2IDI0LjQ5IDI0LjQ5IDAgMCAxLTEzLTEzIDI0LjQgMjQuNCAwIDAgMSA1LjIzLTI2LjggMjQuNDYgMjQuNDYgMCAwIDEgMjYuNzktNS4yNCAyNC40OSAyNC40OSAwIDAgMSAxMyAxMyAyNC40MiAyNC40MiAwIDAgMS0yLjI1IDIzLjE3bC4wMS4wMVoiIC8+CiAgICA8cGF0aCBmaWxsPSJjdXJyZW50Q29sb3IiIGQ9Ik00NSAxNS42MWMuMTAzLjczNi4xNTMgMS40NzcuMTUgMi4yMmEyMC4zOCAyMC4zOCAwIDAgMS0xLjY1IDcuNzZsLTYuNjEgMTkuMTRBMjEuNjUgMjEuNjUgMCAwIDAgNDUgMTUuNjFaTTQwLjYzIDI0LjkxYTExLjQ1IDExLjQ1IDAgMCAwLTEuNzktNmMtMS4xLTEuNzgtMi4xMy0zLjI5LTIuMTMtNS4wOEEzLjc2IDMuNzYgMCAwIDEgNDAuMzUgMTBoLjI4QTIxLjY1IDIxLjY1IDAgMCAwIDcuOSAxNC4xaDEuMzljMi4yNyAwIDUuNzgtLjI3IDUuNzgtLjI3YS45LjkgMCAwIDEgLjEzIDEuNzlzLTEuMTcuMTMtMi40Ny4ybDcuODggMjMuNDcgNC43NS0xNC4yMkwyMiAxNS44NGMtMS4xNy0uMDctMi4yNy0uMi0yLjI3LS4yYS45LjkgMCAwIDEgLjE0LTEuNzlzMy41Ny4yNyA1LjcuMjdjMi4xMyAwIDUuNzgtLjI3IDUuNzgtLjI3YS45LjkgMCAwIDEgLjE0IDEuNzlzLTEuMTguMTMtMi40OC4ybDcuODMgMjMuMjkgMi4yMy03LjA4YTI1LjE3MSAyNS4xNzEgMCAwIDAgMS41Ni03LjE0Wk0xNDUuODMgMTkuM2gtMTAuMzR2MS4xYzMuMjMgMCAzLjc1LjY5IDMuNzUgNC43OXY3LjRjMCA0LjEtLjUyIDQuODUtMy43NSA0Ljg1LTIuNDgtLjM1LTQuMTYtMS42OC02LjQ3LTQuMjJsLTIuNjYtMi44OWMzLjU4LS42MyA1LjQ5LTIuODkgNS40OS01LjQzIDAtMy4xOC0yLjcyLTUuNi03LjgtNS42aC0xMC4xN3YxLjFjMy4yNCAwIDMuNzYuNjkgMy43NiA0Ljc5djcuNGMwIDQuMS0uNTIgNC44NS0zLjc2IDQuODV2MS4xaDExLjV2LTEuMWMtMy4yNCAwLTMuNzYtLjc1LTMuNzYtNC44NXYtMi4wOGgxbDYuNDIgOGgxNi44MWM4LjI2IDAgMTEuODUtNC4zOSAxMS44NS05LjY1IDAtNS4yNi0zLjYxLTkuNTYtMTEuODctOS41NlptLTI0LjIxIDkuNDJWMjFIMTI0YTMuNTUxIDMuNTUxIDAgMCAxIDMuNzYgMy44NyAzLjUzNiAzLjUzNiAwIDAgMS0zLjc2IDMuODVoLTIuMzhabTI0LjM4IDhoLS40Yy0yLjA4IDAtMi4zNy0uNTItMi4zNy0zLjE4VjIxSDE0NmM2IDAgNy4xMSA0LjM5IDcuMTEgNy44UzE1MiAzNi43NSAxNDYgMzYuNzV2LS4wM1pNOTMuNDkgMTMuNTJIODIuNjJ2MS4xNmMzLjcgMCA0LjIyIDEgMy4wNyA0LjM5bC00IDExLjc4TDc2IDEzLjUyaC0xLjFsLTUuODUgMTcuMzMtMy44Ny0xMS43OGMtMS4yMi0zLjU5LS4yOS00LjM5IDMuMTItNC4zOXYtMS4xNkg1NS40N3YxLjE2YzMuMzUgMCA0LjI4Ljg2IDUuNjYgNS4wOGw2LjQyIDE5Ljc2aC43NWw2LTE4LjA4IDUuOSAxOC4wOGguOGw2LjU5LTE5Ljc2YzEuNDQtNC4yMiAyLjMxLTUuMDggNS45NS01LjA4bC0uMDUtMS4xNlpNMTAxLjM0IDE4LjU1Yy02LjM1IDAtMTEuNTUgNC42OC0xMS41NSAxMC4zNHM1LjIgMTAuNCAxMS41NSAxMC40YzYuMzUgMCAxMS41Ni00LjY4IDExLjU2LTEwLjQgMC01LjcyLTUuMi0xMC4zNC0xMS41Ni0xMC4zNFptMCAxOC44OWMtNS4zMSAwLTcuMTYtNC43NC03LjE2LTguNTUgMC0zLjgxIDEuODUtOC41NSA3LjE2LTguNTUgNS4zMSAwIDcuMjMgNC43OSA3LjIzIDguNTUgMCAzLjc2LTEuODUgOC41NS03LjIzIDguNTVaTTE3MC42NyAxMy41MmgtMTJ2MS4xNmMzLjg4IDAgNC41Ny45MiA0LjU3IDYuN3Y5LjI0YzAgNS43OC0uNjkgNi43Ni00LjU3IDYuNzZ2MS4xNkgxNzJ2LTEuMTZjLTMuODggMC00LjU3LTEtNC41Ny02Ljc2di0yLjgzaDMuMjljNiAwIDkuMjUtMy4xMiA5LjI1LTcuMTFzLTMuMzUtNy4xNi05LjMtNy4xNlptMCAxMi4xM2gtMy4yOXYtMTBoMy4yOWMzLjI0IDAgNC43NCAyLjMxIDQuNzQgNS4wOHMtMS41IDQuOTItNC43NCA0LjkyWk0yMTkuMzIgMzQuMTVjLS41MiAxLjktMS4xNSAyLjYtNS4yNiAyLjZoLS44MWMtMyAwLTMuNTItLjctMy41Mi00Ljh2LTIuNjZjNC41MSAwIDQuODUuNDEgNC44NSAzLjQxaDEuMXYtOC42MWgtMS4xYzAgMy0uMzQgMy40MS00Ljg1IDMuNDFWMjFoMy4xOGM0LjEgMCA0Ljc0LjY5IDUuMjYgMi42bC4yOCAxLjFoLjkzbC0uMzgtNS40aC0xN3YxLjFjMy4yMyAwIDMuNzUuNjkgMy43NSA0Ljc5djcuNGMwIDMuNzUtLjQ0IDQuNjktMyA0LjgzLTIuNDItLjM3LTQuMDktMS42OS02LjM3LTQuMmwtMi42NS0yLjg5YzMuNTgtLjYzIDUuNDktMi44OSA1LjQ5LTUuNDMgMC0zLjE4LTIuNzItNS42LTcuOC01LjZoLTEwLjE3djEuMWMzLjIzIDAgMy43NS42OSAzLjc1IDQuNzl2Ny40YzAgNC4xLS41MiA0Ljg1LTMuNzUgNC44NXYxLjFoMTEuNDl2LTEuMWMtMy4yMyAwLTMuNzUtLjc1LTMuNzUtNC44NXYtMi4wOGgxbDYuNDEgOGgyMy43NWwuMzUtNS40M2gtLjg3bC0uMzEgMS4wN1pNMTg5IDI4LjcyVjIxaDIuMzdhMy41NDIgMy41NDIgMCAwIDEgMy43NSAzLjg3IDMuNTMyIDMuNTMyIDAgMCAxLS45OTggMi43NyAzLjUzMiAzLjUzMiAwIDAgMS0yLjc1MiAxLjA1bC0yLjM3LjAzWk0yMzQuNTIgMjcuOTFsLTMuMTgtMS41NmMtMi43OC0xLjI3LTQtMi4wOC00LTMuNTkgMC0xLjUxIDEuNS0yLjM2IDMuMTItMi4zNiAzLjA2IDAgNC41NyAyLjI1IDUgNWgxLjIxdi02Ljg1aC0xLjA5YTMuNDE1IDMuNDE1IDAgMCAxLS43NSAxLjU2IDcuMjUgNy4yNSAwIDAgMC00LjUxLTEuNWMtMy41OCAwLTYuMTggMi4zNi02LjE4IDUuMTQgMCAyLjU0IDEuNzMgNC40NSA0IDUuNTRsMy4yOSAxLjU2YzIuMzcgMS4xIDMuNyAyLjI2IDMuNyAzLjc2IDAgMS43My0xLjUgMi43Ny0zLjM1IDIuNzctMy40MSAwLTYuMDctMi4yNS02LjUzLTYuMDZoLTEuMTV2OGgxLjA5YTQuMTk0IDQuMTk0IDAgMCAxIC45My0yIDguNDgxIDguNDgxIDAgMCAwIDUuMiAyYzMuODcgMCA3LTIuNTQgNy02LjE4LjA3LTEuNzctMS4wMy0zLjktMy44LTUuMjNaTTI1MiAyNy45MWwtMy4xOC0xLjU2Yy0yLjc4LTEuMjctNC0yLjA4LTQtMy41OSAwLTEuNTEgMS41LTIuMzYgMy4xMi0yLjM2IDMuMDYgMCA0LjU3IDIuMjUgNSA1aDEuMjF2LTYuODVIMjUzYTMuNDE1IDMuNDE1IDAgMCAxLS43NSAxLjU2IDcuMjUgNy4yNSAwIDAgMC00LjUxLTEuNWMtMy41OCAwLTYuMTggMi4zNi02LjE4IDUuMTQgMCAyLjU0IDEuNzMgNC40NSA0IDUuNTRsMy4yOSAxLjU2YzIuMzcgMS4xIDMuNyAyLjI2IDMuNyAzLjc2IDAgMS43My0xLjUgMi43Ny0zLjM1IDIuNzctMy40MSAwLTYuMDctMi4yNS02LjUzLTYuMDZoLTEuMTV2OGgxLjA5YTQuMTk0IDQuMTk0IDAgMCAxIC45My0yIDguNDgxIDguNDgxIDAgMCAwIDUuMiAyYzMuODcgMCA3LjA1LTIuNTQgNy4wNS02LjE4LjA3LTEuNzctMS4wMy0zLjktMy43OS01LjIzWk0yNzcuNTYgMTguNzVhMTAuNDgxIDEwLjQ4MSAwIDAgMC0xMC42OCAxMC4xNyAxMC40NyAxMC40NyAwIDAgMCAxMC42OCAxMC4xNmM1LjkgMCAxMC43MS00LjU4IDEwLjcxLTEwLjE2cy00LjgxLTEwLjE3LTEwLjcxLTEwLjE3Wm0wIDE5Yy01LjUyIDAtNy42My00LjkxLTcuNjMtOC44OCAwLTMuOTcgMi4wNy04Ljg3IDcuNjMtOC44NyA1LjU2IDAgNy42NiA0Ljk0IDcuNjYgOC45MiAwIDMuOTgtMi4xMSA4Ljg4LTcuNjYgOC44OHYtLjA1Wk0zMDEuNzEgMzMuNzlsLTMuMTQtMy42OWMzLjYzLS4zOCA1LjcxLTIuNTkgNS43MS01LjM4IDAtMy0yLjQ0LTUuNDItNi44OS01LjQyaC04LjQ3di43YzIuNjYgMCAzLjA1LjUxIDMuMDUgMy43MnYxMC4zOWMwIDMuMjEtLjM5IDMuNzYtMy4wNSAzLjc2di42N2g4LjY2di0uNjdjLTIuNjYgMC0zLjA1LS41NS0zLjA1LTMuNzZ2LTRIMjk2bDYuMzUgOC40NGg1LjI5di0uNjdjLTEuODgtLjI0LTQuMDMtMS44OC01LjkzLTQuMDlaTTI5NC41MyAyOXYtOC41MmgyLjgyYzIuNzkgMCA0LjA4IDEuOTMgNC4wOCA0LjI0IDAgMi4zMS0xLjI5IDQuMjgtNC4wOCA0LjI4aC0yLjgyWk0zMTkuNiAzMC41OXYuNjRjMi4yMSAwIDMgLjcgMyAyLjA4IDAgMi44OS0yLjUgNC4zOS01LjI5IDQuMzktNS45MyAwLTcuNi00LjgxLTcuNi04Ljc4IDAtMy45NyAxLjg2LTguOTIgNy04LjkyIDMuNTkgMCA2LjA5IDIuNTQgNyA2LjdoLjY0di03aC0uNjRhMy4yODEgMy4yODEgMCAwIDEtMS4wOSAxLjgzIDguMjAzIDguMjAzIDAgMCAwLTYtMi43MyAxMC4xNjcgMTAuMTY3IDAgMCAwLTkuODUxIDEwLjE2NSAxMC4xNjkgMTAuMTY5IDAgMCAwIDkuODUxIDEwLjE2NWMzLjM0IDAgNC43OC0xLjY2IDguMzQtMS42NlYzNWMwLTMuMjEuMzktMy43NSAzLjA1LTMuNzV2LS42NGwtOC40MS0uMDJaTTI2MS45IDM0Ljc3YTIuMDYxIDIuMDYxIDAgMSAwIC4yODggNC4xMTIgMi4wNjEgMi4wNjEgMCAwIDAtLjI4OC00LjExMloiIC8+Cjwvc3ZnPg==" /></a>
</figure>

</div>

- <a href="https://www.x.com/WordPress"
  class="wp-block-social-link-anchor"><img
  src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdib3g9IjAgMCAyNCAyNCIgdmVyc2lvbj0iMS4xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGFyaWEtaGlkZGVuPSJ0cnVlIiBmb2N1c2FibGU9ImZhbHNlIj48cGF0aCBkPSJNMTMuOTgyIDEwLjYyMiAyMC41NCAzaC0xLjU1NGwtNS42OTMgNi42MThMOC43NDUgM0gzLjVsNi44NzYgMTAuMDA3TDMuNSAyMWgxLjU1NGw2LjAxMi02Ljk4OUwxNS44NjggMjFoNS4yNDVsLTcuMTMxLTEwLjM3OFptLTIuMTI4IDIuNDc0LS42OTctLjk5Ny01LjU0My03LjkzSDhsNC40NzQgNi40LjY5Ny45OTYgNS44MTUgOC4zMThoLTIuMzg3bC00Ljc0NS02Ljc4N1oiIC8+PC9zdmc+" /><span
  class="wp-block-social-link-label screen-reader-text">Visit our X
  (formerly Twitter) account</span></a>
- <a href="https://bsky.app/profile/wordpress.org"
  class="wp-block-social-link-anchor"><img
  src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdib3g9IjAgMCAyNCAyNCIgdmVyc2lvbj0iMS4xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGFyaWEtaGlkZGVuPSJ0cnVlIiBmb2N1c2FibGU9ImZhbHNlIj48cGF0aCBkPSJNNi4zLDQuMmMyLjMsMS43LDQuOCw1LjMsNS43LDcuMi45LTEuOSwzLjQtNS40LDUuNy03LjIsMS43LTEuMyw0LjMtMi4yLDQuMy45cy0uNCw1LjItLjYsNS45Yy0uNywyLjYtMy4zLDMuMi01LjYsMi44LDQsLjcsNS4xLDMsMi45LDUuMy01LDUuMi02LjctMi44LTYuNy0yLjgsMCwwLTEuNyw4LTYuNywyLjgtMi4yLTIuMy0xLjItNC42LDIuOS01LjMtMi4zLjQtNC45LS4zLTUuNi0yLjgtLjItLjctLjYtNS4zLS42LTUuOSwwLTMuMSwyLjctMi4xLDQuMy0uOWgwWiIgLz48L3N2Zz4=" /><span
  class="wp-block-social-link-label screen-reader-text">Visit our Bluesky
  account</span></a>
- <a href="https://mastodon.world/@WordPress"
  class="wp-block-social-link-anchor" rel="me"><img
  src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdib3g9IjAgMCAyNCAyNCIgdmVyc2lvbj0iMS4xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGFyaWEtaGlkZGVuPSJ0cnVlIiBmb2N1c2FibGU9ImZhbHNlIj48cGF0aCBkPSJNMjMuMTkzIDcuODc5YzAtNS4yMDYtMy40MTEtNi43MzItMy40MTEtNi43MzJDMTguMDYyLjM1NyAxNS4xMDguMDI1IDEyLjA0MSAwaC0uMDc2Yy0zLjA2OC4wMjUtNi4wMi4zNTctNy43NCAxLjE0NyAwIDAtMy40MTEgMS41MjYtMy40MTEgNi43MzIgMCAxLjE5Mi0uMDIzIDIuNjE4LjAxNSA0LjEyOS4xMjQgNS4wOTIuOTM0IDEwLjEwOSA1LjY0MSAxMS4zNTUgMi4xNy41NzQgNC4wMzQuNjk1IDUuNTM1LjYxMiAyLjcyMi0uMTUgNC4yNS0uOTcyIDQuMjUtLjk3MmwtLjA5LTEuOTc1cy0xLjk0NS42MTMtNC4xMjkuNTM5Yy0yLjE2NS0uMDc0LTQuNDQ5LS4yMzMtNC43OTktMi44OTFhNS40OTkgNS40OTkgMCAwIDEtLjA0OC0uNzQ1czIuMTI1LjUyIDQuODE3LjY0M2MxLjY0Ni4wNzUgMy4xOS0uMDk3IDQuNzU4LS4yODMgMy4wMDctLjM1OSA1LjYyNS0yLjIxMiA1Ljk1NC0zLjkwNS41MTctMi42NjUuNDc1LTYuNTA3LjQ3NS02LjUwN3ptLTQuMDI0IDYuNzA5aC0yLjQ5N1Y4LjQ2OWMwLTEuMjktLjU0My0xLjk0NC0xLjYyOC0xLjk0NC0xLjIgMC0xLjgwMi43NzYtMS44MDIgMi4zMTJ2My4zNDloLTIuNDgzdi0zLjM1YzAtMS41MzYtLjYwMi0yLjMxMi0xLjgwMi0yLjMxMi0xLjA4NSAwLTEuNjI4LjY1NS0xLjYyOCAxLjk0NHY2LjExOUg0LjgzMlY4LjI4NGMwLTEuMjg5LjMyOC0yLjMxMy45ODctMy4wNy42OC0uNzU4IDEuNTY5LTEuMTQ2IDIuNjc0LTEuMTQ2IDEuMjc4IDAgMi4yNDYuNDkxIDIuODg2IDEuNDc0TDEyIDYuNTg1bC42MjItMS4wNDNjLjY0LS45ODMgMS42MDgtMS40NzQgMi44ODYtMS40NzQgMS4xMDQgMCAxLjk5NC4zODggMi42NzQgMS4xNDYuNjU4Ljc1Ny45ODYgMS43ODEuOTg2IDMuMDd2Ni4zMDR6IiAvPjwvc3ZnPg==" /><span
  class="wp-block-social-link-label screen-reader-text">Visit our Mastodon
  account</span></a>
- <a href="https://www.threads.net/@wordpress"
  class="wp-block-social-link-anchor"><img
  src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdib3g9IjAgMCAyNCAyNCIgdmVyc2lvbj0iMS4xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGFyaWEtaGlkZGVuPSJ0cnVlIiBmb2N1c2FibGU9ImZhbHNlIj48cGF0aCBkPSJNMTYuMyAxMS4zYy0uMSAwLS4yLS4xLS4yLS4xLS4xLTIuNi0xLjUtNC0zLjktNC0xLjQgMC0yLjYuNi0zLjMgMS43bDEuMy45Yy41LS44IDEuNC0xIDItMSAuOCAwIDEuNC4yIDEuNy43LjMuMy41LjguNSAxLjMtLjctLjEtMS40LS4yLTIuMi0uMS0yLjIuMS0zLjcgMS40LTMuNiAzLjIgMCAuOS41IDEuNyAxLjMgMi4yLjcuNCAxLjUuNiAyLjQuNiAxLjItLjEgMi4xLS41IDIuNy0xLjMuNS0uNi44LTEuNC45LTIuNC42LjMgMSAuOCAxLjIgMS4zLjQuOS40IDIuNC0uOCAzLjYtMS4xIDEuMS0yLjMgMS41LTQuMyAxLjUtMi4xIDAtMy44LS43LTQuOC0yUzUuNyAxNC4zIDUuNyAxMmMwLTIuMy41LTQuMSAxLjUtNS40IDEuMS0xLjMgMi43LTIgNC44LTIgMi4yIDAgMy44LjcgNC45IDIgLjUuNy45IDEuNSAxLjIgMi41bDEuNS0uNGMtLjMtMS4yLS44LTIuMi0xLjUtMy4xLTEuMy0xLjctMy4zLTIuNi02LTIuNi0yLjYgMC00LjcuOS02IDIuNkM0LjkgNy4yIDQuMyA5LjMgNC4zIDEycy42IDQuOCAxLjkgNi40YzEuNCAxLjcgMy40IDIuNiA2IDIuNiAyLjMgMCA0LS42IDUuMy0yIDEuOC0xLjggMS43LTQgMS4xLTUuNC0uNC0uOS0xLjItMS43LTIuMy0yLjN6bS00IDMuOGMtMSAuMS0yLS40LTItMS4zIDAtLjcuNS0xLjUgMi4xLTEuNmguNWMuNiAwIDEuMS4xIDEuNi4yLS4yIDIuMy0xLjMgMi43LTIuMiAyLjd6IiAvPjwvc3ZnPg==" /><span
  class="wp-block-social-link-label screen-reader-text">Visit our Threads
  account</span></a>
- <a href="https://www.facebook.com/WordPress/"
  class="wp-block-social-link-anchor"><img
  src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdib3g9IjAgMCAyNCAyNCIgdmVyc2lvbj0iMS4xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGFyaWEtaGlkZGVuPSJ0cnVlIiBmb2N1c2FibGU9ImZhbHNlIj48cGF0aCBkPSJNMTIgMkM2LjUgMiAyIDYuNSAyIDEyYzAgNSAzLjcgOS4xIDguNCA5Ljl2LTdINy45VjEyaDIuNVY5LjhjMC0yLjUgMS41LTMuOSAzLjgtMy45IDEuMSAwIDIuMi4yIDIuMi4ydjIuNWgtMS4zYy0xLjIgMC0xLjYuOC0xLjYgMS42VjEyaDIuOGwtLjQgMi45aC0yLjN2N0MxOC4zIDIxLjEgMjIgMTcgMjIgMTJjMC01LjUtNC41LTEwLTEwLTEweiIgLz48L3N2Zz4=" /><span
  class="wp-block-social-link-label screen-reader-text">Visit our Facebook
  page</span></a>
- <a href="https://www.instagram.com/wordpress/"
  class="wp-block-social-link-anchor"><img
  src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdib3g9IjAgMCAyNCAyNCIgdmVyc2lvbj0iMS4xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGFyaWEtaGlkZGVuPSJ0cnVlIiBmb2N1c2FibGU9ImZhbHNlIj48cGF0aCBkPSJNMTIsNC42MjJjMi40MDMsMCwyLjY4OCwwLjAwOSwzLjYzNywwLjA1MmMwLjg3NywwLjA0LDEuMzU0LDAuMTg3LDEuNjcxLDAuMzFjMC40MiwwLjE2MywwLjcyLDAuMzU4LDEuMDM1LDAuNjczIGMwLjMxNSwwLjMxNSwwLjUxLDAuNjE1LDAuNjczLDEuMDM1YzAuMTIzLDAuMzE3LDAuMjcsMC43OTQsMC4zMSwxLjY3MWMwLjA0MywwLjk0OSwwLjA1MiwxLjIzNCwwLjA1MiwzLjYzNyBzLTAuMDA5LDIuNjg4LTAuMDUyLDMuNjM3Yy0wLjA0LDAuODc3LTAuMTg3LDEuMzU0LTAuMzEsMS42NzFjLTAuMTYzLDAuNDItMC4zNTgsMC43Mi0wLjY3MywxLjAzNSBjLTAuMzE1LDAuMzE1LTAuNjE1LDAuNTEtMS4wMzUsMC42NzNjLTAuMzE3LDAuMTIzLTAuNzk0LDAuMjctMS42NzEsMC4zMWMtMC45NDksMC4wNDMtMS4yMzMsMC4wNTItMy42MzcsMC4wNTIgcy0yLjY4OC0wLjAwOS0zLjYzNy0wLjA1MmMtMC44NzctMC4wNC0xLjM1NC0wLjE4Ny0xLjY3MS0wLjMxYy0wLjQyLTAuMTYzLTAuNzItMC4zNTgtMS4wMzUtMC42NzMgYy0wLjMxNS0wLjMxNS0wLjUxLTAuNjE1LTAuNjczLTEuMDM1Yy0wLjEyMy0wLjMxNy0wLjI3LTAuNzk0LTAuMzEtMS42NzFDNC42MzEsMTQuNjg4LDQuNjIyLDE0LjQwMyw0LjYyMiwxMiBzMC4wMDktMi42ODgsMC4wNTItMy42MzdjMC4wNC0wLjg3NywwLjE4Ny0xLjM1NCwwLjMxLTEuNjcxYzAuMTYzLTAuNDIsMC4zNTgtMC43MiwwLjY3My0xLjAzNSBjMC4zMTUtMC4zMTUsMC42MTUtMC41MSwxLjAzNS0wLjY3M2MwLjMxNy0wLjEyMywwLjc5NC0wLjI3LDEuNjcxLTAuMzFDOS4zMTIsNC42MzEsOS41OTcsNC42MjIsMTIsNC42MjIgTTEyLDMgQzkuNTU2LDMsOS4yNDksMy4wMSw4LjI4OSwzLjA1NEM3LjMzMSwzLjA5OCw2LjY3NywzLjI1LDYuMTA1LDMuNDcyQzUuNTEzLDMuNzAyLDUuMDExLDQuMDEsNC41MTEsNC41MTEgYy0wLjUsMC41LTAuODA4LDEuMDAyLTEuMDM4LDEuNTk0QzMuMjUsNi42NzcsMy4wOTgsNy4zMzEsMy4wNTQsOC4yODlDMy4wMSw5LjI0OSwzLDkuNTU2LDMsMTJjMCwyLjQ0NCwwLjAxLDIuNzUxLDAuMDU0LDMuNzExIGMwLjA0NCwwLjk1OCwwLjE5NiwxLjYxMiwwLjQxOCwyLjE4NWMwLjIzLDAuNTkyLDAuNTM4LDEuMDk0LDEuMDM4LDEuNTk0YzAuNSwwLjUsMS4wMDIsMC44MDgsMS41OTQsMS4wMzggYzAuNTcyLDAuMjIyLDEuMjI3LDAuMzc1LDIuMTg1LDAuNDE4QzkuMjQ5LDIwLjk5LDkuNTU2LDIxLDEyLDIxczIuNzUxLTAuMDEsMy43MTEtMC4wNTRjMC45NTgtMC4wNDQsMS42MTItMC4xOTYsMi4xODUtMC40MTggYzAuNTkyLTAuMjMsMS4wOTQtMC41MzgsMS41OTQtMS4wMzhjMC41LTAuNSwwLjgwOC0xLjAwMiwxLjAzOC0xLjU5NGMwLjIyMi0wLjU3MiwwLjM3NS0xLjIyNywwLjQxOC0yLjE4NSBDMjAuOTksMTQuNzUxLDIxLDE0LjQ0NCwyMSwxMnMtMC4wMS0yLjc1MS0wLjA1NC0zLjcxMWMtMC4wNDQtMC45NTgtMC4xOTYtMS42MTItMC40MTgtMi4xODVjLTAuMjMtMC41OTItMC41MzgtMS4wOTQtMS4wMzgtMS41OTQgYy0wLjUtMC41LTEuMDAyLTAuODA4LTEuNTk0LTEuMDM4Yy0wLjU3Mi0wLjIyMi0xLjIyNy0wLjM3NS0yLjE4NS0wLjQxOEMxNC43NTEsMy4wMSwxNC40NDQsMywxMiwzTDEyLDN6IE0xMiw3LjM3OCBjLTIuNTUyLDAtNC42MjIsMi4wNjktNC42MjIsNC42MjJTOS40NDgsMTYuNjIyLDEyLDE2LjYyMnM0LjYyMi0yLjA2OSw0LjYyMi00LjYyMlMxNC41NTIsNy4zNzgsMTIsNy4zNzh6IE0xMiwxNSBjLTEuNjU3LDAtMy0xLjM0My0zLTNzMS4zNDMtMywzLTNzMywxLjM0MywzLDNTMTMuNjU3LDE1LDEyLDE1eiBNMTYuODA0LDYuMTE2Yy0wLjU5NiwwLTEuMDgsMC40ODQtMS4wOCwxLjA4IHMwLjQ4NCwxLjA4LDEuMDgsMS4wOGMwLjU5NiwwLDEuMDgtMC40ODQsMS4wOC0xLjA4UzE3LjQwMSw2LjExNiwxNi44MDQsNi4xMTZ6IiAvPjwvc3ZnPg==" /><span
  class="wp-block-social-link-label screen-reader-text">Visit our
  Instagram account</span></a>
- <a href="https://www.linkedin.com/company/wordpress"
  class="wp-block-social-link-anchor"><img
  src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdib3g9IjAgMCAyNCAyNCIgdmVyc2lvbj0iMS4xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGFyaWEtaGlkZGVuPSJ0cnVlIiBmb2N1c2FibGU9ImZhbHNlIj48cGF0aCBkPSJNMTkuNywzSDQuM0MzLjU4MiwzLDMsMy41ODIsMyw0LjN2MTUuNEMzLDIwLjQxOCwzLjU4MiwyMSw0LjMsMjFoMTUuNGMwLjcxOCwwLDEuMy0wLjU4MiwxLjMtMS4zVjQuMyBDMjEsMy41ODIsMjAuNDE4LDMsMTkuNywzeiBNOC4zMzksMTguMzM4SDUuNjY3di04LjU5aDIuNjcyVjE4LjMzOHogTTcuMDA0LDguNTc0Yy0wLjg1NywwLTEuNTQ5LTAuNjk0LTEuNTQ5LTEuNTQ4IGMwLTAuODU1LDAuNjkxLTEuNTQ4LDEuNTQ5LTEuNTQ4YzAuODU0LDAsMS41NDcsMC42OTQsMS41NDcsMS41NDhDOC41NTEsNy44ODEsNy44NTgsOC41NzQsNy4wMDQsOC41NzR6IE0xOC4zMzksMTguMzM4aC0yLjY2OSB2LTQuMTc3YzAtMC45OTYtMC4wMTctMi4yNzgtMS4zODctMi4yNzhjLTEuMzg5LDAtMS42MDEsMS4wODYtMS42MDEsMi4yMDZ2NC4yNDloLTIuNjY3di04LjU5aDIuNTU5djEuMTc0aDAuMDM3IGMwLjM1Ni0wLjY3NSwxLjIyNy0xLjM4NywyLjUyNi0xLjM4N2MyLjcwMywwLDMuMjAzLDEuNzc5LDMuMjAzLDQuMDkyVjE4LjMzOHoiIC8+PC9zdmc+" /><span
  class="wp-block-social-link-label screen-reader-text">Visit our LinkedIn
  account</span></a>
- <a href="https://www.tiktok.com/@wordpress"
  class="wp-block-social-link-anchor"><img
  src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdib3g9IjAgMCAzMiAzMiIgdmVyc2lvbj0iMS4xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGFyaWEtaGlkZGVuPSJ0cnVlIiBmb2N1c2FibGU9ImZhbHNlIj48cGF0aCBkPSJNMTYuNzA4IDAuMDI3YzEuNzQ1LTAuMDI3IDMuNDgtMC4wMTEgNS4yMTMtMC4wMjcgMC4xMDUgMi4wNDEgMC44MzkgNC4xMiAyLjMzMyA1LjU2MyAxLjQ5MSAxLjQ3OSAzLjYgMi4xNTYgNS42NTIgMi4zODV2NS4zNjljLTEuOTIzLTAuMDYzLTMuODU1LTAuNDYzLTUuNi0xLjI5MS0wLjc2LTAuMzQ0LTEuNDY4LTAuNzg3LTIuMTYxLTEuMjQtMC4wMDkgMy44OTYgMC4wMTYgNy43ODctMC4wMjUgMTEuNjY3LTAuMTA0IDEuODY0LTAuNzE5IDMuNzE5LTEuODAzIDUuMjU1LTEuNzQ0IDIuNTU3LTQuNzcxIDQuMjI0LTcuODggNC4yNzYtMS45MDcgMC4xMDktMy44MTItMC40MTEtNS40MzctMS4zNjktMi42OTMtMS41ODgtNC41ODgtNC40OTUtNC44NjQtNy42MTUtMC4wMzItMC42NjctMC4wNDMtMS4zMzMtMC4wMTYtMS45ODQgMC4yNC0yLjUzNyAxLjQ5NS00Ljk2NCAzLjQ0My02LjYxNSAyLjIwOC0xLjkyMyA1LjMwMS0yLjgzOSA4LjE5Ny0yLjI5NyAwLjAyNyAxLjk3NS0wLjA1MiAzLjk0OC0wLjA1MiA1LjkyMy0xLjMyMy0wLjQyOC0yLjg2OS0wLjMwOC00LjAyNSAwLjQ5NS0wLjg0NCAwLjU0Ny0xLjQ4NSAxLjM4NS0xLjgxOSAyLjMzMy0wLjI3NiAwLjY3Ni0wLjE5NyAxLjQyNy0wLjE4MSAyLjE0NSAwLjMxNyAyLjE4OCAyLjQyMSA0LjAyNyA0LjY2NyAzLjgyOCAxLjQ4OS0wLjAxNiAyLjkxNi0wLjg4IDMuNjkyLTIuMTQ1IDAuMjUxLTAuNDQzIDAuNTMyLTAuODk2IDAuNTQ3LTEuNDE3IDAuMTMxLTIuMzg1IDAuMDc5LTQuNzYgMC4wOTUtNy4xNDUgMC4wMTEtNS4zNzUtMC4wMTYtMTAuNzM1IDAuMDI1LTE2LjA5M3oiIC8+PC9zdmc+" /><span
  class="wp-block-social-link-label screen-reader-text">Visit our TikTok
  account</span></a>
- <a href="https://www.youtube.com/wordpress"
  class="wp-block-social-link-anchor"><img
  src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdib3g9IjAgMCAyNCAyNCIgdmVyc2lvbj0iMS4xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGFyaWEtaGlkZGVuPSJ0cnVlIiBmb2N1c2FibGU9ImZhbHNlIj48cGF0aCBkPSJNMjEuOCw4LjAwMWMwLDAtMC4xOTUtMS4zNzgtMC43OTUtMS45ODVjLTAuNzYtMC43OTctMS42MTMtMC44MDEtMi4wMDQtMC44NDdjLTIuNzk5LTAuMjAyLTYuOTk3LTAuMjAyLTYuOTk3LTAuMjAyIGgtMC4wMDljMCwwLTQuMTk4LDAtNi45OTcsMC4yMDJDNC42MDgsNS4yMTYsMy43NTYsNS4yMiwyLjk5NSw2LjAxNkMyLjM5NSw2LjYyMywyLjIsOC4wMDEsMi4yLDguMDAxUzIsOS42MiwyLDExLjIzOHYxLjUxNyBjMCwxLjYxOCwwLjIsMy4yMzcsMC4yLDMuMjM3czAuMTk1LDEuMzc4LDAuNzk1LDEuOTg1YzAuNzYxLDAuNzk3LDEuNzYsMC43NzEsMi4yMDUsMC44NTVjMS42LDAuMTUzLDYuOCwwLjIwMSw2LjgsMC4yMDEgczQuMjAzLTAuMDA2LDcuMDAxLTAuMjA5YzAuMzkxLTAuMDQ3LDEuMjQzLTAuMDUxLDIuMDA0LTAuODQ3YzAuNi0wLjYwNywwLjc5NS0xLjk4NSwwLjc5NS0xLjk4NXMwLjItMS42MTgsMC4yLTMuMjM3di0xLjUxNyBDMjIsOS42MiwyMS44LDguMDAxLDIxLjgsOC4wMDF6IE05LjkzNSwxNC41OTRsLTAuMDAxLTUuNjJsNS40MDQsMi44Mkw5LjkzNSwxNC41OTR6IiAvPjwvc3ZnPg==" /><span
  class="wp-block-social-link-label screen-reader-text">Visit our YouTube
  channel</span></a>
- <a href="https://wordpress.tumblr.com/"
  class="wp-block-social-link-anchor"><img
  src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdib3g9IjAgMCAyNCAyNCIgdmVyc2lvbj0iMS4xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGFyaWEtaGlkZGVuPSJ0cnVlIiBmb2N1c2FibGU9ImZhbHNlIj48cGF0aCBkPSJNMTcuMDQgMjEuMjhoLTMuMjhjLTIuODQgMC00Ljk0LTEuMzctNC45NC01LjAydi01LjY3SDYuMDhWNy41YzIuOTMtLjczIDQuMTEtMy4zIDQuMy01LjQ4aDMuMDF2NC45M2gzLjQ3djMuNjVIMTMuNHY0LjkzYzAgMS40Ny43MyAyLjAxIDEuOTIgMi4wMWgxLjczdjMuNzV6IiAvPjwvcGF0aD48L3N2Zz4=" /><span
  class="wp-block-social-link-label screen-reader-text">Visit our Tumblr
  account</span></a>

<figure class="wp-block-image is-resized global-footer__code_is_poetry">
<img src="https://s.w.org/style/images/code-is-poetry-for-dark-bg.svg"
width="188" height="13" alt="Code is Poetry" />
</figure>

</div>

The WordPress® trademark is the intellectual property of the WordPress
Foundation.

</div>

</div>

