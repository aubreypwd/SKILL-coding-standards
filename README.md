# Aubrey Portwood's Personal Coding Standards (Codex SKILL)

These are Aubrey Portwood's personal coding standards for writing readable, direct, maintainable code.

They are primarily based on the [WordPress Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/), because WordPress pays unusually close attention to code readability, visual formatting, documentation, accessibility, and consistency. Aubrey agrees with most of those standards, but has several intentional differences and personal principles.

This repository packages the standards as a skill for coding agents—especially Codex—to discover and follow. The agent-facing instructions are in [`SKILL.md`](SKILL.md). The [`references/`](references/) directory contains local snapshots of the WordPress standards and examples from the conversations that defined these preferences.

## How the standards are applied

When working in a project, an agent should:

1. Find and follow the project's established instructions and enforced configuration first.
2. Treat repeated local code patterns as useful evidence, but not automatically as authoritative rules.
3. Use these Aubrey Portwood standards as the fallback when the project does not settle a decision.
4. Report genuine conflicts between credible standards and ask before making the disputed choice.
5. Check language and runtime support before using optional syntax.

If the target version is unknown, the agent must ask or state a deliberate compatibility assumption before using version-dependent syntax. It must not silently assume that `export`, `const`, arrow functions, template literals, or trailing commas in function calls are supported.

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

### JavaScript strings

Use template literals by default. This avoids repeatedly deciding whether a string should use single or double quotes, and avoids unnecessary escaping when a string contains an apostrophe or quotation mark.

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
 * @param string $message Message to log.
 */
function log_admin_message( $message ) {
	error_log( $message );
}
```

## Inline Temp: Don't Use Variables Un-necessarily

**Inline Temp** is the established refactoring name for replacing a needless temporary variable with its original expression. This standard applies it as **Don't Use Variables Un-necessarily**.

The rule is:

> Do not create a variable merely to rename, relocate, or relay a value that is already available and only needed once.

This is not limited to object properties. It applies to direct values, function arguments, return values, constructed values, nested expressions, and hard-coded strings.

Bad:

```js
const userName = user.profile.displayName;

renderUserName( userName );
```

Good:

```js
renderUserName( user.profile.displayName );
```

Bad:

```js
/**
 * Submits a form.
 *
 * @param {HTMLFormElement} form Form to submit.
 * @param {Object} settings Submission settings.
 * @return {Promise} Form submission request.
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
 * @param {HTMLFormElement} form Form to submit.
 * @param {Object} settings Submission settings.
 * @return {Promise} Form submission request.
 */
function submitForm( form, settings ) {
	return fetch( `${settings.api.baseUrl}/forms/${settings.formId}`, {
		method: `POST`,
		body: new FormData( form ),
	} );
}
```

The bad version creates three convenience variables that are each used only once. None of them prevents repeated work or adds meaningful information. The good version passes each value directly where it is needed.

A comment can provide context without creating a meaningless alias:

```js
/**
 * Updates a card.
 *
 * @param {Object} card Card data.
 * @param {Object} data Updated card data.
 */
function updateCard( card, data ) {

	// Replace the existing card content in the card element.
	replaceCardContent( card.element, data.content );
}
```

### When a variable is justified

The rule is not “never create variables.” A variable is appropriate when it stores a computed, fetched, expensive, side-effectful, or repeatedly used result:

```js
/**
 * Renders a user.
 *
 * @param {Object} user User data.
 * @return {Object} Rendered user data.
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

```js
/**
 * Builds a profile.
 *
 * @param {number} userId User ID.
 * @return {Object} Profile data.
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
 * @param {number} userId User ID.
 * @return {Object} Profile data.
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

This skill exists to make an agent's code look and read the way Aubrey expects. The agent should study the project's own standards first, then use these standards and the local WordPress references as the fallback. [`references/examples.md`](references/examples.md) contains more bad-versus-good examples for ambiguous cases.

The standards must be validated before code is presented as compliant. The agent must check project configuration, the applicable WordPress references, language-version compatibility, function-call spacing, documentation, indentation, comments, variables, strings, object order, ternaries, and accessibility behavior. If a check cannot be completed, the agent must say so instead of claiming that the code follows the standards.

When a project standard and a personal preference disagree, the project standard normally wins unless Aubrey explicitly asks for the personal standard. When two project standards conflict, the agent should explain the conflict and ask rather than guessing.
