# HTML Coding Standards

> Snapshot source: https://developer.wordpress.org/coding-standards/wordpress-coding-standards/html/
> Retrieved: 2026-07-18
> Conversion: HTML to Markdown with Pandoc.

<div id="wp--skip-link--target"
class="wp-block-group alignwide is-layout-flow wp-block-group-is-layout-flow"
role="main">

# HTML Coding Standards

<div class="wp-block-wporg-sidebar-container is-layout-flow wp-block-wporg-sidebar-container-is-layout-flow"
breakpoint="1300px">

<div class="wp-block-wporg-table-of-contents">

<div class="wporg-table-of-contents__header">

## In this article

<span class="screen-reader-text">Table of Contents</span>

</div>

- [HTML](#html)
  - [Validation](#validation)
  - [Self-closing Elements](#self-closing-elements)
  - [Attributes and Tags](#attributes-and-tags)
  - [Quotes](#quotes)
  - [Indentation](#indentation)
- [Credits](#credits)

</div>

[<span class="wp-exclude-emoji" aria-hidden="true">↑</span> Back to
top](#wp--skip-link--target)

</div>

<div class="entry-content wp-block-post-content is-layout-flow wp-block-post-content-is-layout-flow">

## [HTML](#html)

### [Validation](#validation)

All HTML pages should be verified against [the W3C
validator](https://validator.w3.org/) to ensure that the markup is well
formed. This in and of itself is not directly indicative of good code,
but it helps to weed out problems that are able to be tested via
automation. It is no substitute for manual code review. (For other
validators, see [HTML
Validation](https://codex.wordpress.org/Validating_a_Website#HTML_-_Validation)
in the Codex.)

### [Self-closing Elements](#self-closing-elements)

All tags must be properly closed. For tags that can wrap nodes such as
text or other elements, termination is a trivial enough task. For tags
that are self-closing, the forward slash should have exactly one space
preceding it:

``` html
<br />
```

rather than the compact but incorrect:

``` html
<br/>
```

The W3C specifies that a single space should precede the self-closing
slash ([source](https://w3.org/TR/xhtml1/#C_2)).

### [Attributes and Tags](#attributes-and-tags)

All tags and attributes must be written in lowercase. Additionally,
attribute values should be lowercase when the purpose of the text
therein is only to be interpreted by machines. For instances in which
the data needs to be human readable, proper title capitalization should
be followed.

For machines:

``` html
<meta http-equiv="content-type" content="text/html; charset=utf-8" />
```

For humans:

``` html
<a href="http://example.com/" title="Description Here">Example.com</a>
```

### [Quotes](#quotes)

According to the W3C specifications for XHTML, all attributes must have
a value, and must use double- or single-quotes
([source](https://www.w3.org/TR/xhtml1/#h-4.4)). The following are
examples of proper and improper usage of quotes and attribute/value
pairs.

Correct:

``` html
<input type="text" name="email" disabled="disabled" />
<input type='text' name='email' disabled='disabled' />
```

Incorrect:

``` html
<input type=text name=email disabled>
```

In HTML, attributes do not all have to have values, and attribute values
do not always have to be quoted. While all of the examples above are
valid HTML, *failing to quote attributes can lead to security
vulnerabilities*. Always quote attributes. Omitting the value on boolean
attributes is allowed. The values `true` and `false` are not valid on
boolean attributes ([HTML5
source](https://www.w3.org/TR/2011/WD-html5-20110405/common-microsyntaxes.html#boolean-attributes)).

Correct:

``` html
<input type="text" name="email" disabled />
```

Incorrect:

``` html
<input type="text" name="email" disabled="true" />
```

### [Indentation](#indentation)

As with PHP, HTML indentation should always reflect logical structure.
Use tabs and not spaces.

When mixing PHP and HTML together, indent PHP blocks to match the
surrounding HTML code. Closing PHP blocks should match the same
indentation level as the opening block.

Correct:

``` php
<?php if ( ! have_posts() ) : ?>
<div id="post-1" class="post">
    <h1 class="entry-title">Not Found</h1>
    <div class="entry-content">
        <p>Apologies, but no results were found.</p>
        <?php get_search_form(); ?>
    </div>
</div>
<?php endif; ?>
```

Incorrect:

``` php
<?php if ( ! have_posts() ) : ?>
<div id="post-0" class="post error404 not-found">
<h1 class="entry-title">Not Found</h1>
<div class="entry-content">
<p>Apologies, but no results were found.</p>
<?php get_search_form(); ?>
</div>
</div>
<?php endif; ?>
```

## [Credits](#credits)

- HTML code standards adapted from [Fellowship Tech Code
  Standards](https://developer.fellowshipone.com/patterns/code.php) ([CC
  license](https://creativecommons.org/licenses/by-nc-sa/3.0/)).

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

April 3, 2024

</div>

</div>

<div class="wp-block-group is-layout-constrained wp-container-core-group-is-layout-979f74b1 wp-block-group-is-layout-constrained">

Edit article

[Improve it on GitHub<span class="screen-reader-text">: HTML Coding
Standards</span>](https://github.com/WordPress/wpcs-docs/edit/master/wordpress-coding-standards/html.md)

</div>

<div class="wp-block-group is-layout-constrained wp-container-core-group-is-layout-979f74b1 wp-block-group-is-layout-constrained">

Changelog

[See list of changes<span class="screen-reader-text">: HTML Coding
Standards</span>](https://github.com/WordPress/wpcs-docs/commits/master/wordpress-coding-standards/html.md)

</div>

</div>

<div class="wp-block-group alignfull is-content-justification-space-between is-nowrap is-layout-flex wp-container-core-group-is-layout-11b2f7b6 wp-block-group-is-layout-flex"
style="border-top-color:var(--wp--preset--color--light-grey-1);border-top-style:solid;border-top-width:1px;margin-top:var(--wp--preset--spacing--20);margin-bottom:var(--wp--preset--spacing--20);padding-top:var(--wp--preset--spacing--30);padding-bottom:var(--wp--preset--spacing--30)">

<div class="post-navigation-link-previous wp-block-post-navigation-link">

<a
href="https://developer.wordpress.org/coding-standards/wordpress-coding-standards/github-actions/"
rel="prev"><span class="post-navigation-link__label"
aria-hidden="true">Previous</span> <span
class="post-navigation-link__title" aria-hidden="true">GitHub Actions
Workflow Standards</span> <span class="screen-reader-text">Previous:
GitHub Actions Workflow Standards</span></a>

</div>

<div class="post-navigation-link-next wp-block-post-navigation-link">

<a
href="https://developer.wordpress.org/coding-standards/wordpress-coding-standards/javascript/"
rel="next"><span class="post-navigation-link__label"
aria-hidden="true">Next</span> <span class="post-navigation-link__title"
aria-hidden="true">JavaScript Coding Standards</span> <span
class="screen-reader-text">Next: JavaScript Coding Standards</span></a>

</div>

</div>

</div>

