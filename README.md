# Aubrey Portwood's Personal Coding Standards

This is a Codex skill for writing, reviewing, refactoring, formatting, and explaining code in the way Aubrey expects.

The skill is an agent behavior contract, not a formatter or a replacement for a project's linter, tests, compiler, or accessibility review. When the skill is active, the agent applies it to code in project files and to code shown in chat, examples, plans, diffs, patches, commands, configuration, and other output.

## In one minute

The agent:

1. Reads the project's instructions, tooling, runtime, and nearby code first.
2. Follows explicit project rules and enforced tooling.
3. Uses WordPress Coding Standards (WPCS) as the baseline when the project does not settle a decision.
4. Applies Aubrey's explicit overrides and other principles.
5. Reviews the exact final code before calling it compliant.

If a compatibility, accessibility, standards, or project question cannot be resolved, the agent reports it instead of silently guessing.

## Rule precedence

The rules are applied in this order:

1. Project instructions and enforced configuration.
2. Explicit rules in this skill.
3. The applicable WordPress Coding Standards reference.
4. Nearby code patterns, only when the higher-priority sources are silent.

This means the skill does not overwrite a stronger project convention. It reports conflicting credible project sources and asks for a decision when the conflict cannot be resolved from the repository.

## Foundation: WordPress Coding Standards (WPCS)

WPCS is the foundation for accessibility, documentation, PHP, JavaScript, CSS, HTML, indentation, spacing, and visual formatting. Aubrey follows WPCS unless this skill states a specific override or personal principle.

The local reference snapshots are available in [`references/`](references/). They are source material for the agent, not a second set of Aubrey examples. The authored examples in [`references/good-examples.md`](references/good-examples.md) and [`references/bad-examples.md`](references/bad-examples.md) show how this skill applies the rules.

## What this skill changes from WPCS

The examples below show the distinction directly. A baseline example is not automatically an acceptable Aubrey result.

### HTML void elements

WPCS's HTML reference uses XHTML-style void-element syntax. Aubrey uses modern HTML syntax without the closing slash.

WordPress baseline:

```html
<img alt="Account badge" src="badge.png" />
```

Aubrey result:

```html
<img alt="Account badge" src="badge.png">
```

### Multiline HTML opening tags

Put the closing angle bracket directly after the final attribute. Contents are indented one additional tab beyond the attributes.

WordPress baseline form:

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

Aubrey result:

```html
<button
	aria-controls="account-menu"
	aria-expanded="false"
	id="account-menu-toggle"
	type="button">
		Open account menu
</button>
```

### PHP return-type spacing

WPCS puts no space before the return-type colon. Aubrey puts one space before it. This does not change the colon used by PHP alternative syntax.

WordPress baseline:

```php
<?php

/**
 * Returns the supplied label.
 *
 * @since August 13, 2026
 *
 * @param string $value Label.
 * @return string Label.
 */
function get_label( string $value ): string {
	return $value;
}
```

Aubrey result:

```php
<?php

/**
 * Returns the supplied label.
 *
 * @since August 13, 2026
 *
 * @param string $value Label.
 * @return string Label.
 */
function get_label( string $value ) : string {
	return $value;
}
```

### PHP templates and optional-key guards

Protect optional or untrusted array keys with null coalescing before comparing them. Choose the missing-value default from the intended meaning: use `false` for a false-like flag and `''` when missing should behave like an empty string.

When PHP controls HTML, prefer PHP alternative syntax and indent the controlled HTML in context. A standalone `<?php` opening tag gets a blank line before a following DocBlock or statement; the DocBlock remains directly adjacent to the declaration it documents.

```php
if ( false === ( $subscription['enabled'] ?? false ) ) {
	return 'Disabled';
}

if ( '' === ( $subscription['status'] ?? '' ) ) {
	return 'Status required';
}
```

### JSON formatting

JSON uses spaces for indentation and must remain valid JSON. JavaScript-only trailing commas are not added.

```json
{
  "items": [
    "one",
    "two"
  ]
}
```

### CSS naming: canonical BEM examples

Generic examples in this README use lowercase kebab-case BEM:

- A block stands alone: `.booking-button`.
- An element belongs to its block: `.booking-button__icon`.
- A modifier is an additional class: `.booking-button booking-button--gold`.

The canonical markup and CSS describe the same component:

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

		/* Keep the gold action readable against its surrounding surface. */

		background-color: #c08b12;
	}

	.booking-button__icon {

		height: 1rem;
		width: 1rem;
	}
}
```

The operational rule remains project-aware when a project already maps a CSS block to a JavaScript class. Preserve that class's casing in the mapped selectors, but do not mix multiple casing systems into one example or present an exception as the canonical style.

CSS underscores are allowed when an existing project convention uses them, but they are not a reason to introduce a second naming style into a generic example. Clear, supported color notation may remain unchanged; do not perform notation-only cleanup without a reason.

Use class selectors for component styling. Reserve direct tag selectors for intentional global defaults, resets, browser normalization, or unavoidable third-party markup, and explain each exception inside the CSS block.

### CSS property order and nesting

Group CSS properties by function, then alphabetize within each group. Keep a component's declarations and its root responsive changes together before nested child concepts. Place each child or state's media-query changes with that child or state.

Native CSS nesting is appropriate only when the project's target and tooling support it. Do not nest generic ancestors merely because the HTML is nested that way.

### CSS function spacing

Use one space inside CSS function parentheses, matching the function-call spacing used elsewhere in the skill:

```css
.booking-button {
	grid-template-columns: minmax( 0, 1fr ) repeat( 2, minmax( 0, 1fr ) );
}
```

### CSS comments

When a comment describes declarations or child blocks, place it inside the selector or at-rule it describes. A comment above the selector is not the Aubrey result for that case.

Bad under this skill:

```css
/* Keep the action readable against its surrounding surface. */
.booking-button {
	background-color: #c08b12;
}
```

Good:

```css
.booking-button {

	/* Keep the action readable against its surrounding surface. */

	background-color: #c08b12;
}
```

Keep a one-property block compact. Put a useful explanation at the end of the declaration instead of creating a separate vertical comment block.

```css
.booking-button__icon {
	margin: 0; /* Remove the browser margin from this compact icon block. */
}
```

### JavaScript strings and checks

JavaScript uses template literals by default. Known boolean values use explicit comparisons instead of bang negation, and simple type tests do not receive unnecessary grouping parentheses.

```js
const message = `Hello, ${name}!`;

if ( false === panel.hidden ) {
	openPanel( panel );
}

if ( false === panel instanceof HTMLElement ) {
	return;
}
```

Declare variables separately, preserve intentional object and array order, and reuse computed work without introducing meaningless aliases.

Function calls use WordPress spacing: no space before the opening parenthesis, one space inside the parentheses, and `} );` for a multiline object argument. Split larger calls only when readability requires it, and use trailing commas in function calls only when the target supports them.

## Other principles

These are Aubrey's own principles. They are not presented as WPCS deviations. Each example pair keeps the scenario the same and changes only the relevant choice.

### Comments

Comments should explain context, intent, constraints, accessibility behavior, state changes, or meaningful ordering. They should not restate the declaration or teach a coding principle.

Good:

```js
// Keep the workflow order aligned with its lifecycle.
const workflow = {
	queued: queueWorkflow,
	running: runWorkflow,
	complete: finishWorkflow,
};
```

Bad under this skill:

```js
// Create the workflow object.
const workflow = {
	queued: queueWorkflow,
	running: runWorkflow,
	complete: finishWorkflow,
};
```

### Breathing Room

Blank lines should make logical groups visible. Add them after an opening brace and between meaningful groups, but keep trivial one-statement functions and simple HTML lists compact. Do not add a trailing blank line before a closing curly brace.

Good:

```css
.account-panel {

	display: grid;
	gap: 1rem;

	background-color: #f7f7f7;
	color: #1f2937;

	border: 1px solid #d1d5db;
	padding: 1.25rem;
}
```

Bad under this skill:

```css
.account-panel {
	display: grid;
	gap: 1rem;
	background-color: #f7f7f7;
	color: #1f2937;
	border: 1px solid #d1d5db;
	padding: 1.25rem;
}
```

### Strings and interpolation

Prefer native interpolation, templating, formatting, or parameterized strings.

Good:

```js
showGreeting( `Hello, ${name}!` );
```

Bad under this skill:

```js
showGreeting( 'Hello, ' + name + '!' );
```

### Guard runtime data and return early

When a dynamically typed parameter cannot be enforced by the language, use one practical expected-type guard. Return as soon as invalid or terminal state makes the remaining work unnecessary.

Good:

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

Bad under this skill:

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

### Ternaries

Use a ternary when a simple conditional only chooses the value assigned. Use `if` for multiple statements, side effects, multiple branches, or early returns.

Good:

```js
groupedTasks[ day ] = undefined === groupedTasks[ day ]
	? []
	: groupedTasks[ day ];
```

Bad under this skill:

```js
if ( undefined === groupedTasks[ day ] ) {
	groupedTasks[ day ] = [];
}
```

### Practical documentation

Every named function and method gets a directly preceding DocBlock or JSDoc block. Use accurate broad runtime types and plain-language descriptions. Add `@return` only when a value is returned, and use a current `@since` entry for new code.

Good:

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
```

Bad under this skill:

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

The bad example over-specifies an internal shape and incorrectly documents the string return value as an array.

### Don't use variables unnecessarily

Do not create a variable merely to rename or relay a value used once. Keep a variable when it is reused, captures a required snapshot, represents state, or prevents repeated expensive or side-effectful work.

Good:

```php
$account = get_account( $account_id );

return [
	'account' => $account,
	'contact' => $account['contact'],
	'refreshed_at' => time(),
];
```

Bad under this skill:

```php
$account = get_account( $account_id );
$contact = $account['contact'];

return [
	'account' => $account,
	'contact' => $contact,
	'refreshed_at' => time(),
];
```

### DRY with practical exceptions

Do not repeat expensive work, side effects, transformations, or behavior. Repeating a simple literal can be clearer than introducing an alias.

Good:

```js
const profile = loadProfile( userId );

return {
	profile,
	permissions: getPermissions( profile ),
	lastSeen: formatLastSeen( profile ),
};
```

Bad under this skill:

```js
return {
	profile: loadProfile( userId ),
	permissions: getPermissions( loadProfile( userId ) ),
	lastSeen: formatLastSeen( loadProfile( userId ) ),
};
```

## How the agent applies the skill

Before generating or reviewing code, the agent inspects:

- Repository instructions and contribution guidance.
- Formatter, linter, compiler, type-checker, test, and build configuration.
- Supported language and runtime versions.
- Representative nearby source.
- The applicable WPCS and inline-documentation references.

The agent then applies the project rules, this skill's explicit overrides, and the relevant positive and reject examples. Before presenting code as compliant, it reviews the exact output for documentation, accessibility, compatibility, comments, whitespace, function-call spacing, strings, variables, object order, ternaries, runtime guards, early returns, and semantic correctness.

The skill does not silently claim success when a required check was unavailable. It reports the limitation or asks for the missing project decision.

## Reference map

- [`SKILL.md`](SKILL.md) — the complete agent-facing contract and compliance gate.
- [`references/good-examples.md`](references/good-examples.md) — canonical acceptable examples.
- [`references/bad-examples.md`](references/bad-examples.md) — canonical reject examples.
- [`references/examples.md`](references/examples.md) — supporting history and example index.
- [`references/source-index.md`](references/source-index.md) — WordPress snapshot sources and retrieval dates.

## Install

Place this repository at:

```text
~/.codex/skills/aubreypwd-coding-standards
```

The skill is then available to the Codex environment as `$aubreypwd-coding-standards`. The surrounding environment determines when it is invoked; once invoked, the workflow and rules above apply.
