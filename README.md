# Aubrey Portwood's Personal Coding Standards

This is Aubrey Portwood's personal coding standards skill for Codex. It is used whenever an agent writes, reviews, refactors, formats, or explains code—not only when it edits a project file. Code in examples, plans, diffs, patches, commands, configuration, and other responses is held to the same standard.

## Foundation: WordPress Coding Standards (WPCS)

WPCS is the foundation: the [WordPress Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/) are treated as a strict baseline for accessibility, documentation, PHP, JavaScript, CSS, HTML, spacing, indentation, and visual formatting.

The baseline references are:

- [General WordPress Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/)
- [CSS](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/css/), [HTML](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/html/), [PHP](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/), and [JavaScript](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/javascript/)
- [PHP inline documentation](https://developer.wordpress.org/coding-standards/inline-documentation-standards/php/) and [JavaScript inline documentation](https://developer.wordpress.org/coding-standards/inline-documentation-standards/javascript/)

## Comments

Comments are strongly encouraged when they explain useful context, non-obvious intent, domain behavior, constraints, accessibility behavior, state changes, intentional ordering, or why work is performed once. They should add information rather than restate what the code already makes clear.

Prefer concise comments immediately before the code they explain. Use a block comment for a genuinely multiline explanation, and reserve DocBlocks for declarations. Do not use comments to explain obvious code or to teach a coding principle; the code should demonstrate the principle.

## Deviations from WordPress Coding Standards

These are Aubrey's intentional differences from the WordPress baseline. The detailed operational rules and examples remain in [`SKILL.md`](SKILL.md).

### General formatting

- Use tabs for indentation. Use spaces only when a language, data format, or tool requires them.
- JSON uses spaces for indentation and must not contain JavaScript-only syntax such as trailing commas.
- Add breathing room around meaningful groups in CSS, PHP, JavaScript, and HTML. Keep trivial blocks compact, and do not add a trailing blank line before a closing curly brace.

### HTML and PHP templates

- Do not use XHTML-style self-closing syntax in HTML. Use `<img>` rather than `<img />`.
- For multiline HTML opening tags, put the closing `>` immediately after the final attribute. Indent the contents one additional tab beyond the attributes.
- When PHP controls HTML, use PHP alternative syntax and indent the HTML according to its surrounding template. Do not automatically indent PHP code inside every opening PHP tag.
- Give HTML parent blocks breathing room after their opening tag and between meaningful sibling blocks. Keep short lists with simple `li` content compact.
- After a standalone `<?php` tag, leave a blank line before a DocBlock or statement. Keep the DocBlock directly adjacent to the declaration it documents.
- In PHP return type declarations, put a space before the colon: `function example( string $value ) : string`.
- In PHP documentation, add `@return` only when a function actually returns a value.
- When comparing an optional PHP array key, use a deliberately parenthesized null-coalescing expression such as `( $array['key'] ?? false )` or `( $array['key'] ?? '' )`.

### CSS

- Underscores are allowed in CSS names, and existing color notation does not need to be changed without a reason.
- Group CSS properties by function first and alphabetize only within each group.
- Put one space inside CSS function parentheses, such as `minmax( 0, 1fr )`.
- Prefer nesting around a cohesive component concept, including its states and component-owned responsive behavior. Do not use nesting merely to reproduce the HTML tree.
- Keep root component declarations and root-level responsive overrides together before nested child concepts. Keep each media-query override with the selector it changes.
- Prefer a root component plus one nested level and generally stop at two levels. Deeper nesting must represent meaningful concepts and be supported by the project tooling.
- Put CSS comments inside the selector or at-rule they describe. Keep a trivial one-property block compact with an end-of-line comment when needed.

### JavaScript and function calls

- Declare variables separately and preserve the intentional order of objects and arrays. Do not vertically align associative-array arrows.
- Keep one- and two-parameter calls inline when readable. Put larger calls one parameter per line, and use trailing commas only when the target language and version support them.
- Use WordPress function-call spacing: no space before the opening parenthesis, one space inside the parentheses, and `} );` for a multiline call.
- For known boolean values, use explicit comparisons such as `false === value` instead of bang negation such as `! value`. For nullable or missing values, use the known value such as `null === value` or `undefined === value`.
- Do not add grouping parentheses when a simple type or boolean test is already unambiguous.

## Personal principles

### Breathing Room Principle

Whitespace should make the logical structure of code visible. Add a blank line after a function-like opening brace when the body contains multiple logical statements or sections, or when its first statement is complex or multiline. Apply the same idea to parent blocks with multiple child sections and to callbacks or closures. A one-statement function or callback may remain compact.

```css
.card {

	display: grid;

	padding: 1rem;
}
```

### Strings and interpolation

Prefer native interpolation, templating, formatting, or parameterized strings. In JavaScript, use template literals by default.

```js
showGreeting( `Hello, ${name}!` );
```

### Avoid Concatenation at All Costs

Do not assemble strings with concatenation when a native alternative exists. Use concatenation only when the language or target version provides no viable alternative.

### Guard Runtime Data and Return Early and Often

Do not blindly trust data entering a function when the language cannot enforce its type. Use one practical guard for the expected type, then return as soon as invalid, missing, already-completed, or otherwise terminal conditions make the remaining work unnecessary. Keep the normal path flat and visible. Guard clauses are a technique within this principle, not its name.

```js
/**
 * Updates a panel with a message unless the panel is already complete.
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

	if ( `complete` === panel.dataset.status ) {
		return;
	}

	panel.textContent = message;
}
```

### Ternaries

When a simple conditional only chooses a value to assign, use a ternary and assign the result once. Use `if` for multiple branches, multiple statements, side effects, or early returns. Long ternaries put the question mark and colon on continuation lines.

```js
state.label = value ? `Yes` : `No`;
```

### Practical documentation

Every named function and method gets a directly preceding DocBlock or JSDoc block. Document accurate broad runtime types and explain the expected contents in plain language. Do not encode an exhaustive internal object or array schema when a simple type is enough. The implementation and documentation must agree: a `.map()` result is an array, an object literal is an object, and a fetch chain is a Promise.

Use project-established named types only when they are already required as a public or shared contract. Do not invent one-off typedefs or nested inline schemas. Every named function and method has an `@since` tag; preserve existing tags and add a dated entry when modifying code. Add `@return` only for functions that return a value. Do not identify an agent or model as an author.

### Don't Use Variables Un-necessarily (Inline Temp)

Do not create a variable merely to rename, relocate, or relay a value that is already available and only needed once. This applies to properties, literals, arguments, return values, constructed values, and pure computations. A variable is justified when it is reused, captures required state, prevents repeated work, or represents something that cannot be safely or clearly inlined.

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

Do not repeat expensive work, side effects, transformations, or behavior. Reuse a computed result when it is needed more than once. DRY does not mean every repeated character needs a variable or abstraction, and superficial similarity does not require an abstraction.

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

Repeating a simple literal can be clearer than introducing a shared alias.

## How the skill guides an agent

The agent first inspects the project's instructions, configuration, tooling, nearby code, and supported runtime. Explicit project standards take priority. When the project is silent, the agent uses the WordPress references, Aubrey's explicit overrides, and the skill's good and bad example sets. It checks target compatibility before using version-dependent syntax and never silently assumes support.

The skill applies to every code fragment the agent produces. Before presenting code as compliant, the agent checks the final output for project compatibility, documentation, accessibility, whitespace, comments, function-call spacing, strings, variables, object order, ternaries, runtime guards, early returns, and the other rules above. If a required check cannot be completed, the agent must disclose that instead of claiming compliance. If two credible standards conflict, the agent reports the conflict and asks before making the disputed choice.

## Install

Clone this repository into Codex's local skills directory:

```bash
git clone <repository-url> ~/.codex/skills/aubreypwd-coding-standards
```

The skill is then available as `$aubreypwd-coding-standards`.
