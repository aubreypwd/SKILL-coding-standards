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

	.booking-button__icon {

		height: 1rem;
		width: 1rem;
	}

	&.booking-button--gold {

		background-color: #c08b12;

		.booking-button__icon {
			color: #1f2937;
		}
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

	.account-panel__heading {
		margin: 0; /* Remove the browser margin from this compact heading block. */
	}
}
```

### Easy To Manage CSS Organization

Keep ordinary component selectors flat under one root. Group same-element variation descendants inside their `&.` block. Put responsive conditions at the bottom and show the whole component's changes for each condition together.

The examples below deliberately show the pattern in several forms. The organization is the invariant; breakpoint syntax, responsive direction, class spelling, and nesting support come from the project.

#### Complete desktop-first component

This component starts with its widest/base layout and orders `max-width` conditions from widest to narrowest. Ordinary elements are flat. The featured variation owns only the descendants it extends.

```css
.schedule-card {

	display: grid;
	gap: 1rem;
	grid-template-columns: 2fr 1fr;

	.schedule-card__hero {

		display: grid;
		gap: 0.75rem;
		grid-template-columns: auto 1fr;
	}

	.schedule-card__title {
		font-weight: 700;
	}

	&.schedule-card--featured {

		border-color: #c08b12;

		.schedule-card__title {
			color: #7a5600;
		}
	}

	@media(max-width: 64rem) {

		grid-template-columns: 1fr;

		.schedule-card__hero {
			grid-template-columns: 1fr;
		}

		&.schedule-card--featured {

			.schedule-card__title {
				font-size: 1.25rem;
			}
		}
	}

	@media(max-width: 40rem) {

		gap: 0.75rem;

		.schedule-card__title {
			font-size: 1rem;
		}
	}
}
```

#### Variation recognition is structural while authored names use BEM

An additional class on the same element creates a variation when it is expressed with `&.`. Recognize that structure even in an existing project with another naming convention, but use the skill's canonical BEM modifier syntax when authoring generic or new code.

```css
.alert-banner {

	display: grid;
	gap: 1rem;
	grid-template-columns: 1fr auto;

	background-color: #f5f5f5;
	color: #1f2937;

	border: 1px solid #d1d5db;
	padding: 1rem;

	.alert-banner__message {
		font-weight: 600;
	}

	.alert-banner__actions {
		display: flex;
		gap: 0.5rem;
	}

	&.alert-banner--critical {

		background-color: #fff1f1;
		border-color: #b91c1c;

		.alert-banner__message {
			color: #991b1b;
		}
	}

	&.alert-banner--with-actions {

		padding-block: 1.25rem;

		.alert-banner__actions {
			justify-content: flex-end;
		}
	}
}
```

Both `&.alert-banner--critical` and `&.alert-banner--with-actions` are variations because each additional class applies to `.alert-banner` itself. Their BEM spelling follows the skill's naming standard; the `&.` same-element structure is what determines their organizational role. `.alert-banner__actions` is an ordinary element and therefore remains flat outside the variations until a variation extends it.

#### Multiple variation descendants stay together

When a variation changes several descendants, keep the complete extension inside one variation block. Do not repeat the compound variation selector for every descendant.

```css
.pricing-card {

	display: grid;
	gap: 1rem;

	background-color: #ffffff;

	border: 1px solid #d1d5db;
	padding: 1.5rem;

	.pricing-card__name {
		font-size: 1.25rem;
		font-weight: 700;
	}

	.pricing-card__price {
		font-size: 2rem;
	}

	.pricing-card__features {
		display: grid;
		gap: 0.5rem;
	}

	&.pricing-card--recommended {

		background-color: #fffaf0;
		border-color: #c08b12;

		.pricing-card__name {
			color: #7a5600;
		}

		.pricing-card__price {
			font-weight: 700;
		}

		.pricing-card__features {
			gap: 0.75rem;
		}
	}
}
```

#### Mobile-first responsive component

A mobile-first project uses the same component organization but orders `min-width` conditions from narrowest to widest. Each responsive condition shows every affected component level together.

```css
.filter-panel {

	display: grid;
	gap: 0.75rem;
	grid-template-columns: 1fr;

	.filter-panel__fields {
		display: grid;
		gap: 0.75rem;
		grid-template-columns: 1fr;
	}

	.filter-panel__actions {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
	}

	&.filter-panel--compact {

		gap: 0.5rem;

		.filter-panel__actions {
			font-size: 0.875rem;
		}
	}

	@media(min-width: 40rem) {

		grid-template-columns: 2fr 1fr;

		.filter-panel__fields {
			grid-template-columns: repeat(2, minmax(0, 1fr));
		}

		.filter-panel__actions {
			flex-direction: row;
		}
	}

	@media(min-width: 64rem) {

		gap: 1.25rem;

		.filter-panel__fields {
			grid-template-columns: repeat(3, minmax(0, 1fr));
		}

		&.filter-panel--compact {

			.filter-panel__actions {
				justify-content: flex-end;
			}
		}
	}
}
```

#### Established custom-media aliases

Use a project's existing aliases when they communicate established ranges. Do not invent aliases merely to satisfy this organization. This example is desktop-first, so the effective ranges still move from widest to narrowest.

```css
@custom-media --below-wide (width < 75rem);
@custom-media --compact (width < 48rem);

.profile-panel {

	display: grid;
	gap: 1.5rem;
	grid-template-columns: 18rem 1fr;

	.profile-panel__summary {
		display: grid;
		gap: 0.75rem;
	}

	.profile-panel__details {
		display: grid;
		gap: 1rem;
	}

	&.profile-panel--highlighted {

		border-color: #c08b12;

		.profile-panel__summary {
			background-color: #fffaf0;
		}
	}

	@media(--below-wide) {

		grid-template-columns: 14rem 1fr;

		.profile-panel__details {
			gap: 0.75rem;
		}
	}

	@media(--compact) {

		grid-template-columns: 1fr;

		.profile-panel__summary {
			gap: 0.5rem;
		}

		&.profile-panel--highlighted {

			.profile-panel__summary {
				border-left: 0;
			}
		}
	}
}
```

#### Bounded and overlapping responsive conditions

Bounded or overlapping conditions may encode real component behavior. Keep them, order them by effective range where possible, and preserve source order when changing it would alter the cascade.

```css
.navigation-strip {

	display: flex;
	flex-wrap: wrap;
	gap: 1rem;

	.navigation-strip__item {
		flex: 0 1 auto;
	}

	.navigation-strip__label {
		display: inline;
	}

	@media(min-width: 48rem) and (max-width: 70rem) {

		gap: 0.75rem;

		.navigation-strip__label {
			font-size: 0.875rem;
		}
	}

	@media(max-width: 47.99rem) {

		flex-direction: column;

		.navigation-strip__item {
			width: 100%;
		}
	}
}
```

#### Equivalent organization without native nesting

When project tooling does not support native nesting, preserve the same conceptual order with fully qualified selectors. The organization matters more than forcing unsupported syntax.

```css
.product-summary {

	display: grid;
	gap: 1rem;
	grid-template-columns: 1fr 1fr;
}

.product-summary__title {
	font-weight: 700;
}

.product-summary__details {
	display: grid;
	gap: 0.75rem;
}

.product-summary.product-summary--featured {
	border-color: #c08b12;
}

.product-summary.product-summary--featured .product-summary__title {
	color: #7a5600;
}

@media(max-width: 48rem) {

	.product-summary {
		grid-template-columns: 1fr;
	}

	.product-summary__details {
		gap: 0.5rem;
	}

	.product-summary.product-summary--featured .product-summary__title {
		font-size: 1.25rem;
	}
}
```

The fully qualified fallback cannot group variation descendants syntactically, but it preserves the same reading order: base component, ordinary elements, variations, then responsive behavior.

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
	}

	.command-palette__dialog[hidden] {
		display: none;
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
