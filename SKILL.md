---
name: aubreypwd-coding-standards
description: Apply project-established coding standards first, then use WordPress Coding Standards and the user's personal code-formatting preferences as the fallback for writing, reviewing, refactoring, or formatting PHP, JavaScript, CSS, HTML, and inline documentation.
---

# Aubrey Portwood's Personal Coding Standards

Use this skill whenever writing, reviewing, refactoring, or formatting code.

## 1. Discover the project's standard first

Before editing code, inspect the repository for its actual standards:

1. Read applicable `AGENTS.md`, `CONTRIBUTING.md`, `README` guidance, and project documentation.
2. Inspect formatter, linter, type-checker, compiler, and test configuration.
3. Inspect `package.json`, Composer files, build configuration, CI configuration, and language/runtime versions.
4. Read representative nearby source files to identify repeated conventions, but treat inferred patterns as evidence rather than authoritative rules.
5. Identify which rules apply to the files being changed.

Explicit project instructions and enforced configuration take priority over this skill. If two credible project sources conflict, report the sources and affected decision, then ask the user before making the disputed change. Do not silently override a local standard.

When the project does not settle a decision, read the relevant local WordPress reference in `references/` and apply the personal overrides below. The local references are snapshots; their official URLs and refresh information are in `references/source-index.md`. Check the official source when current guidance matters or when refreshing the snapshots.

## 2. Baseline and personal overrides

Use WordPress Coding Standards as the preferred baseline, especially for accessibility, documentation, PHP, JavaScript, CSS, HTML, spacing, indentation, and visual formatting. Apply these personal overrides when they differ from the baseline:

- Follow WordPress accessibility guidance completely.
- Use tabs for indentation. Use spaces only when a syntax or data format requires them.
- For JSON, use spaces for indentation and remember that JSON does not support JavaScript trailing commas.
- Do not use XHTML-style self-closing syntax in HTML. Use `<img>` rather than `<img />`.
- When PHP controls an HTML region, prefer PHP alternative syntax and indent the controlled HTML in context:

  ```php
  <?php if ( $visible ) : ?>
  	<p>Visible</p>
  <?php endif; ?>
  ```

- In PHP, do not add indentation inside every PHP tag automatically. Indent according to the surrounding template and context.
- In PHP documentation, add `@return` only when the function actually returns a value.
- CSS underscores are allowed.
- CSS property order is functional first, alphabetical second. Group related properties such as layout, flex, typography, colors, spacing, and positioning; alphabetize only within each group. Do not alphabetize across functional groups.
- CSS colors may use any clear supported notation; do not change color notation without a reason.
- Add a blank line after a CSS opening brace when the block has meaningful property groups. Keep a trivial one-property block compact:

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

- Small JavaScript objects and arrays may stay compact. Once they are large enough to benefit from reordering or scanning, put each item on its own line and use a trailing comma when the syntax and target version support it.
- Preserve intentional object and array order. Do not alphabetize them automatically.
- Declare variables separately. Do not use comma-separated variable declarations.
- Prefer JavaScript template literals for strings by default. This avoids deciding between quote types and avoids unnecessary escaping. Do not use `+` for JavaScript string interpolation.
- Format a simple ternary without unnecessary parentheses. Use parentheses when combining multiple tests. For long ternaries, put the question mark and colon on continuation lines:

  ```js
  const result = condition
	? valueWhenTrue
	: valueWhenFalse;
  ```

- Keep one- and two-parameter function calls inline when they remain readable. For calls with more than two parameters, put one parameter on each line and add a trailing comma only when the target language and version support trailing commas in function calls.

## 3. Avoid meaningless variables

Do not create a local variable merely for convenience, to rename a directly available value, or to relay a value into a function. This is the refactoring commonly called **Inline Variable** or **Inline Temp**, and the style rule is to avoid needless indirection.

Prefer direct access:

```js
replaceCardContent(card.element, data.content);
```

Avoid convenience aliases:

```js
const cardElement = card.element;
const content = data.content;

replaceCardContent(cardElement, content);
```

This applies to literals, properties, function arguments, return values, constructed values, and nested expressions—not only to object properties.

Create a variable when it provides real value, such as when it:

- stores a computed, fetched, expensive, or side-effectful result;
- prevents the same work from happening repeatedly;
- is used in multiple places;
- captures a required snapshot;
- represents meaningful state that improves understanding; or
- is required by the language or API.

Do not create a variable solely because a literal is repeated. Repeating a simple literal can be clearer and easier to edit than introducing a shared alias:

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

Reuse a computed result:

```js
function renderUser( user ) {
	const displayName = getDisplayName(user);

	return {
		name: displayName,
		label: `User: ${displayName}`,
		ariaLabel: displayName,
	};
}
```

If direct code needs explanation, prefer a focused comment above the direct expression over a meaningless alias.

## 4. Compatibility and correctness

Before using syntax whose support varies, determine the target language and version from project configuration or explicit user information. Do not add trailing commas to function calls unless support is confirmed. The same compatibility check applies to template literals, language features, and formatting accepted by the project toolchain.

Do not apply this skill mechanically when a project has stronger local rules. Preserve behavior, intentional ordering, semantic comments, and established framework conventions. Keep changes focused and avoid unrelated cleanup.

## 5. References

Read only the relevant local reference when needed:

- `references/source-index.md` — official sources and snapshot metadata.
- `references/wordpress-coding-standards.md` — general baseline.
- `references/accessibility.md` — accessibility baseline.
- `references/css.md` — CSS baseline.
- `references/html.md` — HTML baseline.
- `references/php.md` — PHP baseline.
- `references/javascript.md` — JavaScript baseline.
- `references/javascript-documentation.md` — JavaScript documentation baseline.
- `references/php-documentation.md` — PHP documentation baseline.
- `references/examples.md` — conversation-derived bad and good examples.

Use `references/examples.md` when deciding whether a variable, line break, property order, string form, or function-call layout matches the user's intent.
