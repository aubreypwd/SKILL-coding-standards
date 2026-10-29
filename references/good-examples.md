# Good Examples

These are the canonical positive examples for Aubrey Portwood's coding standards. They are deliberately short and focused on the rule they demonstrate. A function example is complete; a smaller statement or declaration may be shown as an excerpt.

Every example still follows the other applicable standards. Generic CSS examples use lowercase kebab-case BEM. Native CSS nesting assumes that the project target and tooling support it. Function calls do not use trailing commas unless the target has been confirmed to support them.

DocBlocks and JSDoc in these examples illustrate existing documentation being maintained or new documentation explicitly requested by the user. Do not copy those blocks into new code by default, ask whether to add them, or reject an undocumented declaration solely because these examples include documentation. Preserve and update existing blocks.

## CSS

Use compact parentheses in CSS functions and media-query or other at-rule conditions. Do not put optional whitespace immediately inside the parentheses or between an at-rule keyword and its opening condition parenthesis. Preserve whitespace required for syntax or meaning, including the space after property colons, spaces between comma-separated values, arithmetic operators, and media-query separators. Preserve the existing indentation, breathing room, grouping, nesting, and comment rules. This rule applies only to CSS; PHP and JavaScript retain their strict spacing rules.

### Compact CSS parentheses

Keep function and condition parentheses compact while retaining meaningful spaces between arguments and in conditions.

```css
.account-panel {

	--foreground-color: var(--color-olive);

	color: var(--foreground-color);

	width: max(45%, 600px);

	@media(max-width: 619px) {
		width: 100%;
	}
}
```

### BEM markup and native nesting

The HTML and CSS use the same block, element, and modifier names. The modifier is an additional class on the block.

```html
<button class="booking-button booking-button--gold" type="button">

	<span aria-hidden="true" class="booking-button__icon"></span>

	Book appointment
</button>
```

```css
.booking-button {

	display: inline-flex;
	gap: 0.5rem;

	background-color: #1f2937;
	color: #ffffff;

	border: 0;
	padding: 0.75rem 1rem;

	&.booking-button--gold {

		background-color: #c08b12;
	}

	.booking-button__icon {

		height: 1rem;
		width: 1rem;
	}
}
```

### CSS property groups

Group properties by function and alphabetize within each group.

```css
.account-panel {

	display: grid;
	gap: 1rem;
	grid-template-columns: minmax(0, 1fr) auto;

	background-color: #f7f7f7;
	color: #1f2937;

	border: 1px solid #d1d5db;
	padding: 1.25rem;
}
```

### CSS comments

Comments describing declarations or child blocks live inside the selector or at-rule. A one-property block can use an end-of-line comment.

```css
.account-panel {

	/* Keep the panel readable when its contents become dense. */

	display: grid;
	gap: 1rem;
}

.account-panel__heading {
	margin: 0; /* Remove the browser margin from this compact heading block. */
}
```

### Component-owned responsive rules

Keep a root component's responsive changes with the root and a child concept's changes with that child.

```css
.schedule-card {

	display: grid;
	grid-template-columns: 1fr;

	@media(min-width: 48rem) {
		grid-template-columns: 2fr 1fr;
	}

	.schedule-card__hero {

		display: grid;
		gap: 0.75rem;

		@media(min-width: 48rem) {
			grid-template-columns: 1fr;
		}
	}
}
```

## HTML

### Parent spacing and accessible structure

Parent elements with multiple logical child blocks have breathing room. Short, simple list items remain compact.

```html
<main aria-labelledby="notification-settings-title" id="notification-settings">

	<!-- Keep the heading and help text together as the form's accessible context. -->

	<h1 id="notification-settings-title">Notification settings</h1>

	<p id="notification-settings-help">
		Choose which updates should reach your team.
	</p>

	<ul aria-describedby="notification-settings-help">
		<li>Email summaries</li>
		<li>Text alerts</li>
	</ul>
</main>
```

### Multiline tags and void elements

The closing angle bracket follows the final attribute. Void elements do not use XHTML-style closing slashes.

```html
<input
	aria-describedby="email-help"
	id="email"
	name="email"
	type="email">

<p id="email-help">Use the address for account notifications.</p>
```

## PHP

### Documentation and return types

When a named function already has a DocBlock or the user explicitly requests one, keep it directly preceding the declaration and use accurate broad types. Aubrey's return-type spacing applies with or without documentation. A function without a return value does not receive an `@return` tag.

```php
<?php

/**
 * Returns a customer label.
 *
 * @since August 13, 2026
 *
 * @param array $customer Customer data containing a name and code.
 * @return string Customer label.
 */
function get_customer_label( array $customer ) : string {

	return sprintf(
		'%s (%s)',
		$customer['name'],
		$customer['code']
	);
}

/**
 * Registers the customer label filter.
 *
 * @since August 13, 2026
 */
function register_customer_label_filter() : void {
	add_filter( 'customer_label', 'get_customer_label' );
}
```

### Optional array keys

Choose a null-coalescing default that matches the missing-value semantics.

```php
if ( false === ( $subscription['enabled'] ?? false ) ) {
	return 'Disabled';
}

if ( '' === ( $subscription['email'] ?? '' ) ) {
	return 'Email required';
}

if ( 'trial' === ( $subscription['status'] ?? '' ) ) {
	return 'Trial';
}
```

### Reuse meaningful work

Reuse a computed result when it is needed more than once. Do not vertically align associative-array arrows.

```php
$account = get_account( $account_id );

return [
	'account' => $account,
	'contact' => $account['contact'],
	'refreshed_at' => time(),
];
```

## JavaScript

### Runtime guards and early returns

Use one practical type guard before using a dynamically typed parameter. Return early for invalid input and terminal event state.

```js
/**
 * Binds keyboard behavior to a dismissible panel.
 *
 * @since August 13, 2026
 *
 * @param {HTMLElement} panel Panel element.
 */
function bindDismissiblePanel( panel ) {

	if ( false === panel instanceof HTMLElement ) {
		return;
	}

	panel.addEventListener( `keydown`, function ( event ) {

		if ( `Escape` !== event.key ) {
			return;
		}

		panel.hidden = true;
	} );
}
```

### Strings, ternaries, and order

Use template literals, ternaries for simple value selection, and preserve intentional object order.

```js
const message = `Hello, ${name}!`;

groupedTasks[ day ] = undefined === groupedTasks[ day ]
	? []
	: groupedTasks[ day ];

const workflow = {
	queued: queueWorkflow,
	running: runWorkflow,
	complete: finishWorkflow,
};
```

### DRY with practical exceptions

Store a result when repeating the operation would repeat work. Repeating a simple literal can remain clearer than introducing an alias.

```js
const profile = loadProfile( userId );

return {
	profile,
	permissions: getPermissions( profile ),
	lastSeen: formatLastSeen( profile ),
};
```

```js
const locations = {
	primary: {
		city: `Albuquerque`,
	},
	secondary: {
		city: `Albuquerque`,
	},
};
```

## Combined example

This small example keeps the BEM class names consistent across markup, CSS, and JavaScript.

```html
<div class="command-palette">

	<button
		aria-controls="command-palette-dialog"
		aria-expanded="false"
		class="command-palette__trigger"
		id="command-palette-trigger"
		type="button">
			Open command palette
	</button>

	<div
		aria-labelledby="command-palette-title"
		class="command-palette__dialog"
		hidden
		id="command-palette-dialog"
		role="dialog">

		<h2 id="command-palette-title">Command palette</h2>

		<button
			class="command-palette__close"
			id="command-palette-close"
			type="button">
				Close command palette
		</button>

		<input
			aria-label="Search commands"
			class="command-palette__search"
			id="command-palette-search"
			type="search">
	</div>
</div>
```

```css
.command-palette {

	display: grid;
	gap: 1rem;

	.command-palette__dialog {

		background-color: #111827;
		color: #f9fafb;

		border: 1px solid #374151;
		padding: 1rem;

		&[hidden] {
			display: none;
		}
	}

	.command-palette__close,
	.command-palette__search,
	.command-palette__trigger {
		font: inherit;
	}
}
```

```js
/**
 * Toggles the command palette and synchronizes its accessible state.
 *
 * @since August 13, 2026
 *
 * @param {HTMLButtonElement} trigger Palette trigger.
 * @param {HTMLElement} palette Palette element.
 */
function toggleCommandPalette( trigger, palette ) {

	if ( false === trigger instanceof HTMLButtonElement ) {
		return;
	}

	if ( false === palette instanceof HTMLElement ) {
		return;
	}

	palette.hidden = false === palette.hidden;
	trigger.setAttribute( `aria-expanded`, `${false === palette.hidden}` );

	if ( false === palette.hidden ) {

		const searchInput = palette.querySelector( `#command-palette-search` );

		if ( searchInput instanceof HTMLInputElement ) {
			searchInput.focus();
		}

		return;
	}

	trigger.focus();
}
```
