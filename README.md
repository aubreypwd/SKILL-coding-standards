# Aubrey Portwood's Personal Coding Standards (Codex SKILL)

These are Aubrey Portwood's personal coding standards for writing readable, direct, maintainable code.

They are primarily based on the [WordPress Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/), because WordPress pays unusually close attention to code readability, visual formatting, documentation, accessibility, and consistency. Aubrey agrees with most of those standards, but has several intentional differences and personal principles.

This repository packages the standards as a skill for coding agents—especially Codex—to discover and follow. The agent-facing instructions are in [`SKILL.md`](SKILL.md). The [`references/`](references/) directory contains local snapshots of the WordPress standards, a canonical good-example corpus, and a canonical bad-example reject list.

The standards apply whenever an agent produces code, not only when it edits a project file. That includes code in chat responses, examples, explanations, pseudocode, plans, diffs, patches, commands, configuration fragments, CLI output, IDE output, and other interfaces.

## How the standards are applied

When working in a project, an agent should:

1. Find and follow the project's established instructions and enforced configuration first.
2. Treat repeated local code patterns as useful evidence, but not automatically as authoritative rules.
3. Use these Aubrey Portwood standards as the fallback when the project does not settle a decision.
4. Report genuine conflicts between credible standards and ask before making the disputed choice.
5. Check language and runtime support before using optional syntax.

If the target version is unknown, the agent must ask or state a deliberate compatibility assumption before using version-dependent syntax. It must not silently assume that `export`, `const`, arrow functions, template literals, or trailing commas in function calls are supported.

Before producing any meaningful code, the agent must ask internally, “Am I writing code right now?” If yes, it must apply the full standards workflow and compliance gate before showing the code.

These standards are not intended to override a project's explicit conventions. They guide code when the project is silent or when the user explicitly requests these standards.

## Foundation: WordPress Coding Standards

The baseline is the complete WordPress standards family:

- [General WordPress Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/)
- [Accessibility](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/accessibility/)
- [CSS](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/css/)
- [HTML](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/html/)
- [PHP](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/)
- [JavaScript](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/javascript/)
- [JavaScript inline documentation](https://developer.wordpress.org/coding-standards/inline-documentation-standards/javascript/)
- [PHP inline documentation](https://developer.wordpress.org/coding-standards/inline-documentation-standards/php/)

Local snapshots and their retrieval information are listed in [`references/source-index.md`](references/source-index.md).

## Canonical example files

Before generating code, compare the intended code shape with [`references/good-examples.md`](references/good-examples.md). Before showing the result, check it against [`references/bad-examples.md`](references/bad-examples.md) and reject every matching failure pattern. The good examples are intentionally unique to this skill and cover CSS, HTML, PHP, JavaScript, and combined templates. The bad examples record the mistakes Aubrey has identified so far.

## Personal differences from WordPress standards

### Accessibility

Aubrey follows the WordPress accessibility standards completely.

### Tabs and indentation

Use real tabs for indentation. Spaces are appropriate only when a language, data format, or tool requires them. JSON uses spaces for indentation, and JSON must not receive JavaScript-only syntax such as trailing commas.

Indentation should reflect the logical context of the code. For example, HTML controlled by a PHP conditional is indented inside the conditional:

```php
<?php if ( $visible ) : ?>
	<p>Visible</p>
<?php endif; ?>
```

Do not automatically indent PHP code inside every opening PHP tag. Template context determines the correct visual structure.

### HTML

Do not use XHTML-style self-closing syntax. It is unnecessary in modern HTML:

```html
<!-- Avoid. -->
<img src="image.jpg" />

<!-- Prefer. -->
<img src="image.jpg">
```

When an HTML opening tag spans multiple lines, put the closing `>` immediately after the final attribute. Do not place it on a separate line.

### CSS

- Underscores are allowed in CSS names.
- Color notation is not important enough to change without a reason.
- Group properties by function first—for example, flex, typography, colors, spacing, and positioning.
- Alphabetize properties only within each functional group.
- Add breathing room after an opening brace when the block contains meaningful groups.
- Keep a trivial one-property block compact.

```css
#overlay {

	background: #fff;
	color: #777;

	padding: 10px;
	position: absolute;
	z-index: 1;
}

#overlay {
	background: #fff;
}
```

When a parent block contains multiple child blocks, add breathing room after the parent opening brace or opening HTML tag and between sibling blocks. Do not add a trailing blank line before a CSS, PHP, or JavaScript closing brace. This makes each block visibly separate without leaving an unnecessary gap before closing curly syntax:

```css
@media (forced-colors: active) {

	.orbit-card {

		border-color: CanvasText;
		background-color: Canvas;
		color: CanvasText;
	}

	.orbit-card__pulse {

		background-color: CanvasText;
		box-shadow: none;
	}

	.orbit-card__eyebrow {

		color: CanvasText;
	}
}
```

Short HTML lists are a deliberate compact exception. When each list item contains simple content, keep each `li` on one line and keep the items adjacent without blank lines inside the `ul` or `ol`. Expand an item only when it contains complex or multiple meaningful child elements.

The same visual separation applies to PHP, JavaScript, and HTML blocks. Do not cram a DocBlock against a standalone `<?php` tag:

```php
<?php

/**
 * Encodes binary data as unpadded URL-safe Base64.
 *
 * @since Unknown
 *
 * @param string $value Binary data.
 * @return string URL-safe Base64 data.
 */
function constellation_base64url_encode( string $value ) : string {

	return rtrim(
		strtr(
			base64_encode( $value ),
			'+/',
			'-_'
		),
		'='
	);
}
```

The DocBlock remains directly adjacent to the function declaration. The blank line separates the PHP opening tag from the documentation block.

For PHP return type declarations, Aubrey puts a space before the colon:

```php
/**
 * Returns an invoice label.
 *
 * @since Unknown
 *
 * @param string $status Invoice status.
 * @return string Invoice label.
 */
function get_invoice_label( string $status ) : string {

	return $status;
}
```

This spacing override applies to return type declarations, not to the colon in PHP alternative syntax such as `<?php if ( $visible ) : ?>`.

## Breathing Room Principle

Every non-empty named function or method gets a blank line immediately after its opening brace, even when its body contains only one statement. Anonymous functions, callbacks, and closures get the same spacing when they contain multiple statements or logical sections; a one-statement callback may remain compact.

The same principle applies to parent blocks with multiple logical child blocks in CSS, PHP, JavaScript, and HTML: separate the parent from its first child and separate sibling blocks. Keep compact exceptions such as simple one-property CSS blocks and short HTML lists. Never add a trailing blank line immediately before a closing curly brace.

### Strings and interpolation

Avoid string concatenation in every supported language. Prefer the language's native interpolation, templating, formatting, or parameterized-string mechanism instead of assembling a string with `+`, `.`, or an equivalent operator. Concatenation is acceptable only when the language or target version leaves no viable alternative.

In JavaScript, use template literals by default. This avoids repeatedly deciding whether a string should use single or double quotes, and avoids unnecessary escaping when a string contains an apostrophe or quotation mark.

```js
const message = `I can't save this file.`;
const greeting = `Hello, ${name}!`;
```

Do not concatenate strings with `+`:

```js
// Avoid.
const message = 'Hello, ' + name + '!';
```

The concatenation example is intentionally shown as bad code. It demonstrates the pattern agents should recognize and replace.

In PHP, use interpolation when it expresses the value clearly:

```php
// Avoid.
$message = 'Hello, ' . $name . '!';

// Prefer.
$message = "Hello, {$name}!";
```

Apply the equivalent native interpolation or formatting mechanism in other languages. If the language forces concatenation, use it only when necessary and document the constraint.

### JavaScript objects, arrays, and calls

- Very small objects and arrays may remain compact.
- Larger objects and arrays put each item on its own line.
- Preserve intentional order; do not alphabetize objects or arrays automatically.
- Use trailing commas when supported by the language and target version.
- For function calls, one or two parameters may remain on one line. More than two parameters are written one per line.
- Follow WordPress spacing exactly: `fetch( requestUrl, { ... } );`, with no space before the opening parenthesis and one space inside the parentheses.
- Close multiline calls as `} );`, never `});`.
- Never add a trailing comma to a function call unless support is confirmed.

```js
const point = { x: 10, y: 20 };

createCard(
	card.element,
	data.content,
	data.heading,
	data.status,
);
```

The trailing comma in the multiline call depends on language and version support. If support is uncertain, omit it.

### Ternaries

Do not add parentheses around a simple ternary test when they add no clarity. Use parentheses when combining tests. Long ternaries put the question mark and colon on continuation lines:

```js
const label = value ? `Yes` : `No`;

const result = condition
	? valueWhenTrue
	: valueWhenFalse;
```

### PHP documentation

Add `@return` only when a function actually returns a value:

```php
/**
 * Logs an administrative message.
 *
 * @since Unknown
 *
 * @param string $message Message to log.
 */
function log_admin_message( $message ) {

	error_log( $message );
}
```

## Exact documentation shapes

Every named function and method needs a directly preceding DocBlock or JSDoc block. Its parameter and return types must describe the actual shapes established by the project. Do not use bare `Object`, `Array`, `Array<Object>`, `any`, `mixed`, or an unqualified `Promise` when the source makes the shape knowable.

For an array, document the element type and the array container. For an object, document its concrete properties. If the shape is unknown, inspect the source, schema, or type declarations before documenting it; do not guess a generic type. A function that uses `.map()` returns an array, even though each element is an object.

The complete signal example in [`references/examples.md`](references/examples.md#pure-one-use-computations-and-exact-jsdoc-shapes) demonstrates this distinction with `BeaconSignal[]` input and `FoldedBeaconSignal[]` output. The canonical positive examples are in [`references/good-examples.md`](references/good-examples.md).

## Inline Temp: Don't Use Variables Un-necessarily

**Inline Temp** is the established refactoring name for replacing a needless temporary variable with its original expression. This standard applies it as **Don't Use Variables Un-necessarily**.

The rule is:

> Do not create a variable merely to rename, relocate, or relay a value that is already available and only needed once.

This is not limited to object properties. It applies to direct values, function arguments, return values, constructed values, nested expressions, and hard-coded strings.

A pure computation used once is also an Inline Temp. Being computed does not make a variable necessary. Do not create a name for a simple arithmetic expression, ternary, property path, interpolation, or constructed value when it can be placed directly at its only use.

Bad:

```js
const horizon = now + 15 * 60 * 1000;

return signals.filter( function ( signal ) {
	return signal.startsAt <= horizon;
} );
```

Good:

```js
return signals.filter( function ( signal ) {
	return signal.startsAt <= now + 15 * 60 * 1000;
} );
```

This rule also applies to a pure one-use transformation such as `visibleSignals`. Inline the filter-and-map chain unless a separate binding is required for a snapshot, repeated use, or correctness.

Bad:

```js
const userName = user.profile.displayName;

renderUserName( userName );
```

Good:

```js
renderUserName( user.profile.displayName );
```

The form examples use these explicit shapes:

```js
/**
 * @typedef {Object} SubmissionApiSettings
 * @property {string} baseUrl API base URL.
 */

/**
 * @typedef {Object} SubmissionSettings
 * @property {SubmissionApiSettings} api API settings.
 * @property {string} formId Form identifier.
 */
```

Bad:

```js
/**
 * Submits a form.
 *
 * @since Unknown
 *
 * @param {HTMLFormElement} form Form to submit.
 * @param {SubmissionSettings} settings Submission settings.
 * @return {Promise<Response>} Form submission request.
 */
function submitForm( form, settings ) {

	const endpoint = settings.api.baseUrl;
	const formData = new FormData( form );
	const requestUrl = `${endpoint}/forms/${settings.formId}`;

	return fetch( requestUrl, {
		method: `POST`,
		body: formData,
	} );
}
```

Good:

```js
/**
 * Submits a form.
 *
 * @since Unknown
 *
 * @param {HTMLFormElement} form Form to submit.
 * @param {SubmissionSettings} settings Submission settings.
 * @return {Promise<Response>} Form submission request.
 */
function submitForm( form, settings ) {

	return fetch( `${settings.api.baseUrl}/forms/${settings.formId}`, {
		method: `POST`,
		body: new FormData( form ),
	} );
}
```

The bad version creates three convenience variables that are each used only once. None of them prevents repeated work or adds meaningful information. The good version passes each value directly where it is needed.

A comment can provide context without creating a meaningless alias. The card examples use these explicit shapes:

```js
/**
 * @typedef {Object} CardData
 * @property {Element} element Card element.
 */

/**
 * @typedef {Object} UpdatedCardData
 * @property {string} content Updated card content.
 */
```

```js
/**
 * Updates a card.
 *
 * @since Unknown
 *
 * @param {CardData} card Card data.
 * @param {UpdatedCardData} data Updated card data.
 */
function updateCard( card, data ) {

	// Replace the existing card content in the card element.
	replaceCardContent( card.element, data.content );
}
```

### When a variable is justified

The rule is not “never create variables.” A variable is appropriate when it prevents repeated expensive or side-effectful work, is used in multiple places, captures a required snapshot, represents required state that cannot be safely or clearly be inlined, or is required by the language or API. A value being computed once is not enough.

The user example below uses explicit input and output shapes:

```js
/**
 * @typedef {Object} UserData
 * @property {string} displayName User display name.
 */

/**
 * @typedef {Object} RenderedUserData
 * @property {string} name User name.
 * @property {string} label User label.
 * @property {string} ariaLabel Accessible user label.
 */
```

```js
/**
 * Renders a user.
 *
 * @since Unknown
 *
 * @param {UserData} user User data.
 * @return {RenderedUserData} Rendered user data.
 */
function renderUser( user ) {

	const displayName = getDisplayName( user );

	return {
		name: displayName,
		label: `User: ${displayName}`,
		ariaLabel: displayName,
	};
}
```

Here `displayName` is used three times and prevents `getDisplayName()` from running three times. The variable has a real purpose.

## DRY, with practical exceptions

DRY means **Don't Repeat Yourself**. It is useful when repetition would repeat work, behavior, or a piece of knowledge that must stay synchronized.

Bad: repeat a computation or fetch:

These examples use explicit profile shapes:

```js
/**
 * @typedef {Object} UserProfile
 * @property {number} id User ID.
 * @property {string} name User name.
 */

/**
 * @typedef {Object} ProfileResult
 * @property {UserProfile} profile User profile.
 * @property {string[]} permissions User permissions.
 * @property {string} summary Profile summary.
 */
```

```js
/**
 * Builds a profile.
 *
 * @since Unknown
 *
 * @param {number} userId User ID.
 * @return {ProfileResult} Profile data.
 */
function buildProfile( userId ) {

	return {
		profile: fetchProfile( userId ),
		permissions: getPermissions( fetchProfile( userId ) ),
		summary: createSummary( fetchProfile( userId ) ),
	};
}
```

Good: compute it once and reuse the result:

```js
/**
 * Builds a profile.
 *
 * @since Unknown
 *
 * @param {number} userId User ID.
 * @return {ProfileResult} Profile data.
 */
function buildProfile( userId ) {

	const profile = fetchProfile( userId );

	return {
		profile,
		permissions: getPermissions( profile ),
		summary: createSummary( profile ),
	};
}
```

However, DRY does not mean every repeated character must become a variable or abstraction. Repeating a simple literal can be clearer than creating a shared alias:

```js
const locations = {
	primary: {
		city: `Albuquerque`,
		state: `New Mexico`,
	},
	secondary: {
		city: `Albuquerque`,
		state: `New Mexico`,
	},
};
```

The string is not computed, expensive, fetched, or behaviorally significant. Creating a `city` variable would add indirection without preventing work or preserving shared knowledge.

The practical rule is:

- Do not repeat expensive work, side effects, transformations, or behavior.
- Reuse computed results when they are needed in multiple places.
- Do not create variables solely to avoid repeating a simple literal.
- Do not introduce abstractions merely because two pieces of code look superficially similar.
- Preserve intentional object and array order.

## For coding agents

This skill exists to make an agent's code look and read the way Aubrey expects. The agent should study the project's own standards first, then use these standards and the local WordPress references as the fallback. [`references/good-examples.md`](references/good-examples.md) contains the canonical positive examples, and [`references/bad-examples.md`](references/bad-examples.md) contains the reject list for ambiguous cases.

The standards must be validated before code is presented as compliant. The agent must check project configuration, the applicable WordPress references, [`references/good-examples.md`](references/good-examples.md), [`references/bad-examples.md`](references/bad-examples.md), language-version compatibility, function-call spacing, documentation, indentation, comments, variables, strings, object order, ternaries, and accessibility behavior. If a check cannot be completed, the agent must say so instead of claiming that the code follows the standards.

When a project standard and a personal preference disagree, the project standard normally wins unless Aubrey explicitly asks for the personal standard. When two project standards conflict, the agent should explain the conflict and ask rather than guessing.
