# Bad Examples

These examples are recognition fixtures. Each one intentionally breaks the named rule while keeping the surrounding scenario and other standards as consistent as possible. Never copy them into code presented as compliant.

Documentation examples apply when maintaining existing blocks or writing new blocks explicitly requested by the user. Missing DocBlocks or JSDoc on new declarations are acceptable by default. Existing blocks must be preserved and kept accurate; their presence does not authorize new blocks elsewhere.

## CSS

### Padded function and condition parentheses

```css
.account-panel {

	--foreground-color: var( --color-olive );

	color: var( --foreground-color );

	width: max( 45%, 600px );

	@media ( max-width: 619px ) {
		width: 100%;
	}
}
```

Do not pad function or condition parentheses with optional spaces, and do not separate an at-rule keyword from its opening condition parenthesis. Keep meaningful spaces such as the space after a property colon and between comma-separated arguments: `max(45%, 600px)`.

### Comment outside the block it describes

```css
/* Keep the panel readable when its contents become dense. */
.account-panel {
	display: grid;
	gap: 1rem;
}
```

The comment describes declarations inside the selector, so it belongs inside the selector block immediately before those declarations.

### Vertical comment for a one-property block

```css
.account-panel__heading {

	/* Remove the browser margin from this compact heading block. */

	margin: 0;
}
```

Keep a trivial one-property block compact and put a useful explanation at the end of the declaration line.

### Properties without functional grouping

```css
.account-panel {
	background-color: #f7f7f7;
	padding: 1.25rem;
	position: relative;
	display: grid;
	color: #1f2937;
}
```

Group properties by function and alphabetize within each group.

### Generic-ancestor nesting

```css
body {

	.page {

		.content {

			.booking-card {
				padding: 1rem;
			}
		}
	}
}
```

Do not mirror the HTML tree with generic ancestors. Nest around a portable component concept.

### Mixed component levels in one media query

```css
.booking-card {

	display: grid;

	@media(min-width: 48rem) {

		grid-template-columns: 2fr 1fr;

		.booking-card__hero {
			grid-template-columns: 1fr;
		}
	}

	.booking-card__hero {

		display: grid;
		grid-template-columns: auto 1fr;
	}
}
```

Keep root responsive changes with the root and child responsive changes with the child.

### Trailing blank line before a closing brace

```css
.account-panel {

	display: grid;

	background-color: #f7f7f7;

}
```

Close the block directly after its final declaration or child.

### Undocumented direct tag selector

```css
button {
	font: inherit;
}
```

Use a component class for project-specific styling. A direct tag selector requires an intentional exception and an explanation inside the CSS block.

## HTML

### XHTML-style void elements

```html
<input name="email" type="email" />
<img alt="Account badge" src="badge.png" />
```

Use modern HTML void elements without the closing slash.

### Standalone closing angle bracket

```html
<button
	aria-controls="account-menu"
	aria-expanded="false"
	id="account-menu-toggle"
	type="button"
>
		Open account menu
</button>
```

Put the closing angle bracket directly after the final attribute.

### Contents aligned with attributes

```html
<button
	aria-controls="account-menu"
	aria-expanded="false"
	id="account-menu-toggle"
	type="button">
	Open account menu
</button>
```

Indent the contents one additional tab beyond the attributes by default.

### Comment attached to parent and child

```html
<nav aria-label="Project sections">
	<!-- Explain the navigation structure. -->
	<ul>
		<li><a href="/overview">Overview</a></li>
	</ul>
</nav>
```

Treat an HTML comment as its own child block. Give it breathing room from the parent opening tag and the child it explains.

### Over-formatted simple list

```html
<ul>

	<li>
		<a href="/overview">Overview</a>
	</li>

	<li>
		<a href="/activity">Activity</a>
	</li>
</ul>
```

Short list items with one simple link should remain compact and adjacent.

## PHP

### Missing space before a return-type colon

```php
<?php

/**
 * Returns an invoice label.
 *
 * @since August 13, 2026
 *
 * @param string $status Invoice status.
 * @return string Invoice status.
 */
function get_invoice_label( string $status ): string {
	return $status;
}
```

Put one space before the return-type colon.

### Unprotected optional array key

```php
if ( false === $subscription['enabled'] ) {
	return 'Disabled';
}
```

Use a parenthesized null-coalescing expression before comparing an optional key.

### Over-specified or inaccurate documentation

```php
<?php

/**
 * Returns a customer label.
 *
 * @since August 13, 2026
 *
 * @param array{ name: string, code: string } $customer Customer data.
 * @return array Customer label.
 */
function get_customer_label( array $customer ) : string {

	return sprintf(
		'%s (%s)',
		$customer['name'],
		$customer['code']
	);
}
```

Use an accurate broad runtime type unless a named shape is an established shared contract. Document the actual return type.

### `@return` on a function with no return value

```php
<?php

/**
 * Registers the customer label filter.
 *
 * @since August 13, 2026
 *
 * @return void
 */
function register_customer_label_filter() : void {
	add_filter( 'customer_label', 'get_customer_label' );
}
```

Do not add `@return` when the function does not return a value.

### Vertically aligned associative-array arrows

```php
return [
	'account'      => $account,
	'contact'      => $account['contact'],
	'refreshed_at' => time(),
];
```

Use one space on each side of `=>`. Alignment creates a maintenance burden when a key changes length.

### Trailing blank line before a closing brace

```php
<?php

/**
 * Returns an account label.
 *
 * @since August 13, 2026
 *
 * @param string $label Account label.
 * @return string Account label.
 */
function get_account_label( string $label ) : string {
	return $label;

}
```

Close the function directly after its final statement.

## JavaScript

### String concatenation

```js
const message = 'Hello, ' + name + '!';
```

Use a template literal when the target supports it.

### Bang negation for a known boolean

```js
if ( ! panel.hidden ) {
	openPanel( panel );
}
```

Use an explicit comparison for a known boolean: `false === panel.hidden`.

### Unnecessary grouping in a type test

```js
if ( false === ( panel instanceof HTMLElement ) ) {
	return;
}
```

Do not add grouping parentheses when operator precedence already makes the comparison unambiguous.

### Meaningless single-use alias

```js
const accountName = account.profile.displayName;

showAccountName( accountName );
```

Pass the directly available value when the alias is not reused or needed for state.

### Repeating computed work

```js
return {
	profile: loadProfile( userId ),
	permissions: getPermissions( loadProfile( userId ) ),
	lastSeen: formatLastSeen( loadProfile( userId ) ),
};
```

Compute the profile once and reuse the result.

### `if` for a simple assignment choice

```js
if ( undefined === groupedTasks[ day ] ) {
	groupedTasks[ day ] = [];
}
```

Use a ternary when the condition only chooses the value assigned.

### Missing runtime guard

```js
/**
 * Binds keyboard behavior to a dismissible panel.
 *
 * @since August 13, 2026
 *
 * @param {HTMLElement} panel Panel element.
 */
function bindDismissiblePanel( panel ) {

	panel.addEventListener( `keydown`, function ( event ) {

		if ( `Escape` !== event.key ) {
			return;
		}

		panel.hidden = true;
	} );
}
```

Guard the expected runtime type before calling its methods.

### Incorrect function-call spacing

```js
return fetch(requestUrl, {
	method: `POST`,
	body: formData,
});
```

Use WordPress spacing and close a multiline object argument as `} );`.

### Unsupported trailing comma in a function call

```js
queueJob(
	jobId,
	payload,
);
```

Use a trailing comma in a function call only when the target language and version support it.
