# Aubrey Portwood's Personal Coding Standards

This is Aubrey Portwood's personal coding standards skill for Codex.

It is primarily based on the [WordPress Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/). Aubrey follows that foundation with a small set of personal differences and principles.

## Foundation: WordPress Coding Standards

The baseline is the WordPress Coding Standards family:

- [General WordPress Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/)
- [CSS](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/css/), [HTML](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/html/), [PHP](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/), and [JavaScript](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/javascript/)
- [PHP inline documentation](https://developer.wordpress.org/coding-standards/inline-documentation-standards/php/) and [JavaScript inline documentation](https://developer.wordpress.org/coding-standards/inline-documentation-standards/javascript/)

## Comments

Comments are strongly encouraged when they explain useful context, non-obvious intent, domain behavior, constraints, or state changes. They should add information rather than restate what the code already makes clear.

## Personal differences and preferences

- Use tabs for indentation. Use spaces only when a language, data format, or tool requires them.
- Do not use XHTML-style self-closing tags in HTML. Use `<img>` rather than `<img />`.
- Prefer CSS nesting for cohesive component concepts when the project tooling supports it. Do not use nesting merely to reproduce the HTML ancestor tree.
- In PHP return type declarations, put a space before the colon: `function example( string $value ) : string`.

## Personal principles

### Breathing Room Principle

Code should have enough whitespace to make its logical sections easy to see. Add a blank line after an opening brace or tag when a block contains multiple meaningful sections, and between sibling sections. Keep genuinely trivial blocks compact and do not add a trailing blank line before a closing curly brace.

The JavaScript examples assume a runtime that supports `const` and template literals.

```js
/**
 * Updates a panel with a message.
 *
 * @since July 18, 2026
 *
 * @param {HTMLElement} panel Panel element to update.
 * @param {string} message Message to display.
 */
function updatePanel( panel, message ) {

	if ( false === panel instanceof HTMLElement ) {
		return;
	}

	panel.textContent = message;
}
```

### Strings and interpolation

Prefer the language's native interpolation, templating, formatting, or parameterized-string mechanism. In JavaScript, use template literals by default.

```js
showGreeting( `Hello, ${name}!` );
```

### Avoid Concatenation at All Costs

Do not assemble strings with concatenation when a native interpolation, formatting, templating, or parameterized-string alternative exists. Use concatenation only when the language or target version provides no viable alternative.

### Guard Runtime Data and Return Early and Often

Do not blindly trust data entering a function when the language cannot enforce its type. Use one practical type guard when needed, then return as soon as invalid, missing, already-completed, or otherwise terminal conditions make the remaining work unnecessary. Keep the normal path flat and visible.

### Ternaries

Use a ternary when a simple conditional only chooses a value to assign. Use `if` for multiple branches, side effects, multiple statements, or early returns.

```js
const label = value ? `Yes` : `No`;
```

### Practical documentation

Document named functions and methods with accurate broad runtime types and plain-language descriptions of their contents. Keep documentation practical: do not encode an exhaustive internal object or array schema when a simple type and useful description are enough. Add `@return` only when the function returns a value.

### Don't Use Variables Un-necessarily (Inline Temp)

Do not create a variable merely to rename, relocate, or relay a value that is already available and only needed once. A variable is justified when it is reused, captures required state, prevents repeated work, or represents something that cannot be safely or clearly inlined.

Bad:

```js
const title = card.heading;

renderCardTitle( title );
```

Good:

```js
renderCardTitle( card.heading );
```

### DRY, with practical exceptions

Do not repeat expensive work, side effects, transformations, or behavior. Reuse a computed result when it is needed more than once. Do not create abstractions merely because simple literals happen to repeat.

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

## Install

Copy this directory into Codex's local skills directory:

```bash
cp -R /path/to/aubreypwd-coding-standards ~/.codex/skills/
```

The skill is then available as `$aubreypwd-coding-standards`.

The complete agent-facing instructions are in [`SKILL.md`](SKILL.md), and the local WordPress references are in [`references/`](references/).
