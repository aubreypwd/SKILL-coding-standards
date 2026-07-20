# Aubrey Portwood's Personal Coding Standards

This is Aubrey Portwood's personal coding standards skill for Codex. It tells an agent how to write, review, refactor, format, and explain code in the way Aubrey expects.

The skill applies to every code fragment an agent produces, including project files, examples, explanations, pseudocode, plans, diffs, patches, commands, configuration, and UI output. The agent begins with the project's own rules and tooling, uses WordPress Coding Standards as the baseline, and then applies Aubrey's explicit preferences and personal principles.

## Foundation: WordPress Coding Standards (WPCS)

WPCS is the foundation. The [WordPress Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/) are treated as a strict baseline for accessibility, documentation, PHP, JavaScript, CSS, HTML, spacing, indentation, and visual formatting.

Aubrey follows WPCS completely wherever this skill does not state an explicit override. Project-specific standards remain authoritative when they conflict with a personal preference.

The baseline references are:

- [General WordPress Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/)
- [CSS](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/css/), [HTML](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/html/), [PHP](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/), and [JavaScript](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/javascript/)
- [PHP inline documentation](https://developer.wordpress.org/coding-standards/inline-documentation-standards/php/) and [JavaScript inline documentation](https://developer.wordpress.org/coding-standards/inline-documentation-standards/javascript/)

## Deviations from WordPress Coding Standards

These are the places where Aubrey intentionally chooses a different result from the documented WordPress baseline. Each comparison shows the WordPress form first and Aubrey's form second. When WPCS does not prescribe a contrary form, that distinction is stated instead of presenting a false disagreement.

### HTML void elements

The WPCS HTML reference uses XHTML-style self-closing syntax for void elements. Aubrey uses modern HTML syntax without the closing slash.

```html
<!-- WPCS reference style. -->
<img src="image.jpg" />

<!-- Aubrey style. -->
<img src="image.jpg">
```

### Multiline HTML opening tags

WPCS requires readable, logically indented HTML but does not prescribe Aubrey's exact multiline-tag layout. Aubrey puts the closing `>` immediately after the final attribute and indents the contents one additional tab beyond the attributes.

```html
<!-- Baseline-style layout. -->
<button
	aria-controls="command-palette"
	aria-expanded="false"
	id="command-palette-trigger"
	type="button"
>
	Open command palette
</button>

<!-- Aubrey style. -->
<button
	aria-controls="command-palette"
	aria-expanded="false"
	id="command-palette-trigger"
	type="button">
		Open command palette
</button>
```

### PHP return-type spacing

WPCS requires no space between the closing parameter parenthesis and the return-type colon. Aubrey puts a space before that colon. This does not apply to the colon in PHP alternative syntax.

```php
// WPCS.
function get_label( string $value ): string {
	return $value;
}

// Aubrey.
function get_label( string $value ) : string {
	return $value;
}
```

### PHP and data-format details

When PHP controls HTML, Aubrey prefers PHP alternative syntax and indents the controlled HTML according to the surrounding template. A standalone `<?php` tag gets a blank line before a following DocBlock or statement, while the DocBlock remains directly adjacent to the declaration it documents.

WPCS does not prescribe this missing-key guard. Aubrey protects optional PHP array keys with a deliberately parenthesized null-coalescing expression before comparison.

```php
// Without Aubrey's guard.
if ( false === $subscription['enabled'] ) {
	return 'Disabled';
}

// Aubrey style.
if ( false === ( $subscription['enabled'] ?? false ) ) {
	return 'Disabled';
}
```

JSON is formatted with spaces rather than tabs and must remain valid JSON; JavaScript-only syntax such as trailing commas is not added.

Good:

```json
{
  "items": [
    "one",
    "two"
  ]
}
```

Bad:

```json
{
  "items": [
    "one",
    "two",
  ],
}
```

### CSS selector underscores

WPCS recommends lowercase, hyphenated selectors and advises against underscores. Aubrey allows underscores in CSS names.

```css
/* WPCS preference. */
.account-panel {
	margin: 0;
}

/* Aubrey allows. */
.account_panel {
	margin: 0;
}
```

### CSS function-parenthesis spacing

The WPCS CSS reference uses compact function parentheses. Aubrey applies the same interior spacing used for normal function calls to CSS functions.

```css
/* WPCS reference style. */
grid-template-columns: minmax(0, 1fr);

/* Aubrey style. */
grid-template-columns: minmax( 0, 1fr );
```

### CSS property ordering

WPCS recommends meaningful logical grouping and also documents alphabetization as an accepted approach. Aubrey chooses functional grouping first and alphabetical ordering within each group.

```css
/* A general WPCS-style logical ordering. */
.card {
	position: relative;
	display: grid;
	background: #fff;
	padding: 1rem;
}

/* Aubrey's functional-group ordering. */
.card {

	display: grid;
	position: relative;

	background: #fff;

	padding: 1rem;
}
```

### CSS color notation

WPCS recommends particular color forms, while Aubrey does not require changing clear, supported color notation without a reason.

```css
/* WPCS preference. */
color: rgba(23, 32, 51, 0.12);

/* Aubrey permits a clear supported form. */
color: rgb( 23 32 51 / 12% );
```

### CSS nesting

The WPCS reference uses flat selectors. Aubrey encourages nesting around a cohesive component concept when the target tooling supports it.

Aubrey does not use nesting merely to reproduce generic HTML ancestors. Root declarations and root responsive overrides stay together before nested child concepts, media-query rules remain with the selector they modify, and nesting normally stops at one or two meaningful levels.

```css
/* WPCS-style flat selectors. */
.card {
	display: grid;
}

.card__actions {
	display: flex;
	gap: 1rem;
}

/* Aubrey's component-centered nesting. */
.card {

	display: grid;

	.card__actions {

		display: flex;
		gap: 1rem;
	}
}
```

CSS nesting must be supported by the project's target and tooling. Otherwise, use the supported preprocessor or equivalent flat selectors.

### CSS comment placement

The WPCS CSS reference commonly places comments above the selector. Aubrey places comments inside the selector or at-rule they describe, immediately before the declarations or child blocks they explain.

```css
/* WPCS reference style. */
/* Explain the layout. */
.card {
	display: grid;
}

/* Aubrey style. */
.card {

	/* Explain the layout. */

	display: grid;
}
```

A trivial one-property CSS block remains compact. When a comment is useful, put it at the end of the declaration line rather than creating a full comment block around a single property.

### JavaScript string style

WPCS documents single-quoted JavaScript strings. Aubrey prefers template literals by default.

```js
// WPCS preference.
const message = 'Hello, world!';

// Aubrey preference.
const message = `Hello, world!`;
```

For known booleans, Aubrey uses explicit comparisons instead of bang negation. For simple type tests, Aubrey does not add grouping parentheses when operator precedence is already unambiguous. Variables are declared separately, intentional object and array order is preserved, associative-array arrows are not vertically aligned, and trailing commas in function calls are used only when the target supports them. Function calls retain WordPress spacing.

```js
// Common baseline form.
if ( ! panel.hidden ) {
	openPanel( panel );
}

// Aubrey form.
if ( false === panel.hidden ) {
	openPanel( panel );
}

// Common over-grouped form.
if ( false === ( panel instanceof HTMLElement ) ) {
	return;
}

// Aubrey form.
if ( false === panel instanceof HTMLElement ) {
	return;
}
```

The following rules remain WPCS-aligned rather than being deviations: accessibility, tabs for indentation, semicolons, general readability, and logical formatting. Aubrey's independent comment and code-structure principles are described next.

## Other principles

These are Aubrey's own coding principles. They are not presented as deviations from WPCS. Each section explains the intent and shows the same scenario written in an acceptable and unacceptable way.

### Comments

Comments are strongly encouraged when they explain useful context, non-obvious intent, domain behavior, constraints, accessibility behavior, state changes, intentional ordering, or why work is performed once. They should add information rather than restate what the code already makes clear.

Prefer concise comments immediately before the code they explain. Use a block comment for a genuinely multiline explanation, and reserve DocBlocks for declarations. Do not use comments to explain obvious code or to teach a coding principle; the code should demonstrate the principle.

Good:

```js
// Keep the workflow order aligned with its lifecycle.
const workflow = {
	queued: queueWorkflow,
	running: runWorkflow,
	complete: finishWorkflow,
};
```

Bad:

```js
// Create the workflow object.
const workflow = {
	queued: queueWorkflow,
	running: runWorkflow,
	complete: finishWorkflow,
};
```

The code is identical. Only the comment changes: one explains information the object does not reveal, while the other restates the declaration.

### Breathing Room Principle

Whitespace should make the logical structure of code visible. Add a blank line after a function-like opening brace when the body contains multiple logical statements or sections, or when its first statement is complex or multiline. Apply the same idea to parent blocks with multiple child sections in CSS, PHP, JavaScript, and HTML, and to callbacks or closures. HTML comments count as child blocks and need breathing room from both the parent and the child they explain. Short lists with simple `li` content remain compact. A one-statement function or callback may remain compact.

Good:

```css
.account_panel {

	display: grid;
	gap: 1rem;
	grid-template-columns: minmax( 0, 1fr ) auto;

	font-size: 1rem;
	line-height: 1.5;

	background-color: #f7f7f7;
	color: #1f2937;

	border: 1px solid #d1d5db;
	border-radius: 0.5rem;
	padding: 1.25rem;
}
```

Bad:

```css
.account_panel {
	display: grid;
	gap: 1rem;
	grid-template-columns: minmax( 0, 1fr ) auto;
	font-size: 1rem;
	line-height: 1.5;
	background-color: #f7f7f7;
	color: #1f2937;
	border: 1px solid #d1d5db;
	border-radius: 0.5rem;
	padding: 1.25rem;
}
```

Both blocks have the same declarations. The good version makes the layout, typography, color, and box-model groups visible.

### Strings and interpolation

Prefer native interpolation, templating, formatting, or parameterized strings. In JavaScript, use template literals by default.

Backticks are JavaScript's template-literal delimiters. They allow a value such as `${name}` to be inserted directly into the string, avoid manually joining separate fragments, and avoid unnecessary escaping when text contains apostrophes or quotation marks.

Good:

```js
showGreeting( `Hello, ${name}!` );
```

Bad:

```js
showGreeting( 'Hello, ' + name + '!' );
```

Both calls produce the same greeting. The good version expresses the value inside the string instead of assembling it from separate pieces.

### Avoid Concatenation at All Costs

Do not assemble strings with concatenation when a native alternative exists. Use interpolation, formatting, templating, or parameterized strings instead. Use concatenation only when the language or target version provides no viable alternative.

Good:

```js
const notice = `Hello, ${userName}!`;
```

Bad:

```js
const notice = `Hello, ` + userName + `!`;
```

Both assignments produce the same notice. The good version keeps the complete string expression in one interpolated template literal.

### Guard Runtime Data and Return Early and Often

Do not blindly trust data entering a function when the language cannot enforce its type. Use one practical guard for the expected type, then return as soon as invalid, missing, already-completed, or otherwise terminal conditions make the remaining work unnecessary. Keep the normal path flat and visible. Guard clauses are a technique within this principle, not its name.

Good:

```js
/**
 * Binds keyboard behavior to a dismissible panel.
 *
 * @since Unknown
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

Bad:

```js
/**
 * Binds keyboard behavior to a dismissible panel.
 *
 * @since Unknown
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

The event behavior is the same. The good version refuses an unexpected runtime value before calling a method on it.

### Ternaries

When a simple conditional only chooses a value to assign, use a ternary and assign the result once. Use `if` for multiple branches, multiple statements, side effects, or early returns. Long ternaries put the question mark and colon on continuation lines.

Good:

```js
groupedTasks[ day ] = undefined === groupedTasks[ day ]
	? []
	: groupedTasks[ day ];
```

Bad:

```js
if ( undefined === groupedTasks[ day ] ) {
	groupedTasks[ day ] = [];
}
```

Both forms initialize the same missing group. The ternary is the appropriate form because the condition only selects the value assigned.

### Practical documentation

Every named function and method gets a directly preceding DocBlock or JSDoc block. Document accurate broad runtime types and explain the expected contents in plain language. Do not encode an exhaustive internal object or array schema when a simple type is enough. The implementation and documentation must agree: a `.map()` result is an array, an object literal is an object, and a fetch chain is a Promise.

Use project-established named types only when they are already required as a public or shared contract. Do not invent one-off typedefs or nested inline schemas. Every named function and method has an `@since` tag; preserve existing tags and add a dated entry when modifying code. Add `@return` only for functions that return a value. Do not identify an agent or model as an author.

Good:

```php
<?php

/**
 * Returns a customer label.
 *
 * @since Unknown
 *
 * @param array $customer Customer data.
 * @return string Customer label.
 */
function get_customer_label( array $customer ) : string {

	return sprintf(
		'%s (%s)',
		$customer['name'],
		$customer['code']
	);
}
```

Bad:

```php
<?php

/**
 * Returns a customer label.
 *
 * @since Unknown
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

The implementation is identical. The bad documentation incorrectly claims that the function returns an array and over-specifies an internal object shape.

### Don't Use Variables Un-necessarily (Inline Temp)

Do not create a variable merely to rename, relocate, or relay a value that is already available and only needed once. This applies to properties, literals, arguments, return values, constructed values, and pure computations.

A variable is justified when it is reused, prevents repeated expensive or side-effectful work, captures a required snapshot, represents necessary state, or is required by the language or API.

Good:

```php
$account = get_account( $account_id );

return [
	'account' => $account,
	'contact' => $account['contact'],
	'refreshed_at' => time(),
];
```

Bad:

```php
$account = get_account( $account_id );
$contact = $account['contact'];

return [
	'account' => $account,
	'contact' => $contact,
	'refreshed_at' => time(),
];
```

Both payloads are identical. The bad version creates `$contact` only to relay a value into one array entry. The good version uses the canonical `build_sync_payload()` pattern from the reference examples.

### DRY, with practical exceptions

Do not repeat expensive work, side effects, transformations, or behavior. Reuse a computed result when it is needed more than once. DRY does not mean every repeated character needs a variable or abstraction, and superficial similarity does not require an abstraction.

Good:

```js
const profile = loadProfile( userId );

return {
	profile,
	permissions: getPermissions( profile ),
	lastSeen: formatLastSeen( profile ),
};
```

Bad:

```js
return {
	profile: loadProfile( userId ),
	permissions: getPermissions( loadProfile( userId ) ),
	lastSeen: formatLastSeen( loadProfile( userId ) ),
};
```

Both forms build the same profile summary. The bad version repeats the profile-loading operation three times.

Repeating a simple literal can still be clearer than introducing a shared alias:

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

## How the skill guides an agent

Before writing code, the agent inspects the project's instructions, documentation, configuration, tooling, nearby code, and supported runtime. Explicit project standards take priority.

When the project is silent, the agent reads the relevant WPCS and inline-documentation references, compares the intended code with the canonical good and bad example sets, and applies Aubrey's overrides only where this skill explicitly states them.

The agent checks the exact final code for project compatibility, documentation, accessibility, whitespace, comments, function-call spacing, strings, variables, object order, ternaries, runtime guards, early returns, and the other rules above. If a required check cannot be completed, it must disclose that instead of claiming compliance. If two credible standards conflict, it reports the conflict and asks before choosing silently.

## Install

Clone this repository into Codex's local skills directory:

```bash
git clone <repository-url> ~/.codex/skills/aubreypwd-coding-standards
```

The skill is then available as `$aubreypwd-coding-standards`.
