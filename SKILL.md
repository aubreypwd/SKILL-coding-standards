---
name: aubreypwd-coding-standards
description: Apply project-established coding standards first, then use WordPress Coding Standards and the user's personal code-formatting preferences as the fallback for writing, reviewing, refactoring, or formatting PHP, JavaScript, CSS, HTML, and inline documentation.
---

# Aubrey Portwood's Personal Coding Standards

Use this skill whenever writing, reviewing, refactoring, or formatting code.

## Required workflow

Do not treat these standards as a loose checklist. Complete this workflow before presenting code as standards-compliant:

1. Discover the project rules, enforced tooling, and target language/runtime version.
2. Read the applicable WordPress Coding Standards and inline documentation reference.
3. Apply Aubrey's explicit overrides only where they are stated below.
4. Generate the code.
5. Run the compliance gate in section 4. Correct every failure before presenting the result.

If a check cannot be completed, state the unresolved compatibility or standards question. Do not claim that the code follows these standards based on a partial review.

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

Use the applicable WordPress Coding Standards as a strict baseline for accessibility, documentation, PHP, JavaScript, CSS, HTML, spacing, indentation, and visual formatting. These are not suggestions. Apply Aubrey's personal overrides only where they are explicitly listed below:

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
- Avoid string concatenation in every supported language. Prefer the language's native interpolation, templating, formatting, or parameterized-string mechanism instead of assembling a string with `+`, `.`, or an equivalent concatenation operator. In JavaScript, use template literals; in PHP, use interpolation or the applicable formatting mechanism. Use concatenation only when the language or target version provides no viable alternative, and state that constraint.
- Prefer JavaScript template literals for strings by default. This avoids deciding between quote types and avoids unnecessary escaping.
- Format a simple ternary without unnecessary parentheses. Use parentheses when combining multiple tests. For long ternaries, put the question mark and colon on continuation lines:

  ```js
  const result = condition
	? valueWhenTrue
	: valueWhenFalse;
  ```

- Keep one- and two-parameter function calls inline when they remain readable. For calls with more than two parameters, put one parameter on each line and add a trailing comma only when the target language and version support trailing commas in function calls.
- Apply WordPress function-call spacing exactly: no space between a function or method name and its opening parenthesis, and one space inside the opening and closing parentheses. Use `fetch( requestUrl, { ... } );`, not `fetch(requestUrl, { ... });` and not `fetch ( requestUrl, { ... } );`.
- For multiline calls, put a space between the final argument expression and the closing parenthesis. An object argument therefore closes as `} );`, never `});`.
- Apply the same spacing to nested calls, constructors, methods, and calls with one or two parameters: `getDisplayName( user )`, `new FormData( form )`, and `replaceCardContent( card.element, data.content )`.
- Keep comments before the code they describe and precede inline comments with a blank line, as required by the WordPress JavaScript standard.

## 3. Avoid meaningless variables

Do not create a local variable merely for convenience, to rename a directly available value, or to relay a value into a function. This is the refactoring commonly called **Inline Temp**, and the style rule is **Don't Use Variables Un-necessarily**.

Prefer direct access:

```js
replaceCardContent( card.element, data.content );
```

Avoid convenience aliases:

```js
const cardElement = card.element;
const content = data.content;

replaceCardContent( cardElement, content );
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

If direct code needs explanation, prefer a focused comment above the direct expression over a meaningless alias.

## 4. Compliance gate

Before presenting code as compliant, verify all of the following:

- Project-local instructions and enforced formatter, linter, compiler, type-checker, and runtime rules were inspected.
- The applicable WordPress language and documentation references were read.
- The target language/runtime version is known, or the assumption is stated before version-dependent syntax is used; never silently assume support.
- Every new or modified named function and method has a directly preceding language-appropriate DocBlock or JSDoc block, unless an explicit project standard says otherwise.
- Function documentation follows the applicable WordPress summary, parameter, return, and version-tag rules. Do not invent version values; use project evidence or the documented unknown convention when required.
- `@return` appears only when the function actually returns a value. Do not add `@return void` unless the applicable project standard explicitly requires it.
- JavaScript and PHP function-call spacing follows the exact WordPress form, including spaces inside parentheses and `} );` for multiline calls.
- One- and two-parameter calls are not split unnecessarily; larger calls use one parameter per line.
- Trailing commas in function argument lists are present only when target-language and target-version support is confirmed.
- Tabs, semicolons, whitespace, line wrapping, braces, comments, and blank lines follow the applicable WordPress rules.
- Strings in every supported language use native interpolation, templating, formatting, or parameterization instead of concatenation whenever an alternative exists. JavaScript uses template literals by default, and variables are declared separately.
- Objects and arrays preserve intentional order and are not alphabetized without instruction.
- No meaningless single-use variables or convenience aliases were introduced.
- Repeated computation, fetching, side effects, and transformations are not repeated unnecessarily.
- Simple literal repetition has not been turned into a needless alias or abstraction.
- Ternaries are kept compact when simple and broken at `?` and `:` only when long or complex.
- Accessibility behavior is complete and accurate before it is described as accessibility-compliant.
- Every example included in the answer follows all unrelated standards too; an example must not teach one rule while violating another.

Run available project validation tools after this static review. If no tooling is available, perform the review manually and disclose that limitation.

## 5. Compatibility and correctness

Before using syntax whose support varies, determine the target language and version from project configuration or explicit user information. Do not add trailing commas to function calls unless support is confirmed. The same compatibility check applies to template literals, language features, and formatting accepted by the project toolchain.

If no target version can be established, ask for it or state a deliberate compatibility assumption before generating code. Do not silently use `export`, `const`, arrow functions, template literals, trailing commas in calls, or other version-dependent syntax.

Do not apply this skill mechanically when a project has stronger local rules. Preserve behavior, intentional ordering, semantic comments, and established framework conventions. Keep changes focused and avoid unrelated cleanup. Do not call code standards-compliant until the compliance gate passes.

## 6. References

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
