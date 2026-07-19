---
name: aubreypwd-coding-standards
description: Use for every response or task that writes, edits, reviews, refactors, formats, or generates code in PHP, JavaScript, CSS, HTML, or any other language. Inspect project standards first, then strictly validate every code fragment—including chat examples, plans, diffs, CLI output, and UI responses—against WordPress Coding Standards and Aubrey Portwood's exact documentation, whitespace, interpolation, function-call, and Inline Temp rules.
---

# Aubrey Portwood's Personal Coding Standards

Use this skill whenever writing, reviewing, refactoring, or formatting code.

## Always-on code output rule

Before producing any response that contains meaningful code, ask internally: **Am I writing code right now?** If the answer is yes, apply this skill before generating the code, even when the code is only:

- an example or snippet in a normal chat response;
- pseudocode, a plan, a diff, or a patch;
- a command, configuration fragment, or generated file body;
- an error correction or explanation that includes replacement code; or
- output shown in a CLI, IDE, web, or other user interface rather than written to a project file.

Do not output generic illustrative code and assume these standards apply only to file edits. Every code fragment must pass the same standards workflow and compliance gate.

## Required workflow

Do not treat these standards as a loose checklist. Complete this workflow before presenting code as standards-compliant:

1. Discover the project rules, enforced tooling, and target language/runtime version.
2. Read the applicable WordPress Coding Standards and inline documentation reference.
3. Read `references/good-examples.md` for positive code shapes and `references/bad-examples.md` for reject patterns.
4. Apply Aubrey's explicit overrides only where they are stated below.
5. Generate the code.
6. Run the compliance gate in section 4. Correct every failure before presenting the result.

If a check cannot be completed, state the unresolved compatibility or standards question. Do not claim that the code follows these standards based on a partial review.

Do not present code that fails any applicable check. Revise it before showing it; if the check cannot be resolved without project information or user direction, stop before generating the code and state what is missing.

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
- For multiline HTML opening tags, put the closing `>` immediately after the final attribute on that same line. Do not put the closing `>` on its own line.
- For multiline HTML opening tags, indent the contents one additional tab beyond the attributes by default. Attributes are indented relative to the opening tag; nested text and elements are indented relative to the closed tag, so they do not strictly align with the attributes. Follow an explicit project convention when one exists.
- When PHP controls an HTML region, prefer PHP alternative syntax and indent the controlled HTML in context:

  ```php
  <?php if ( $visible ) : ?>
  	<p>Visible</p>
  <?php endif; ?>
  ```

- In PHP, do not add indentation inside every PHP tag automatically. Indent according to the surrounding template and context.
- When a standalone `<?php` opening tag is followed by a DocBlock or statement, leave one blank line after the tag. Keep a DocBlock directly adjacent to the declaration it documents; the blank line belongs between `<?php` and the DocBlock, not between the DocBlock and the declaration. This does not apply to inline alternative syntax such as `<?php if ( $visible ) : ?>`.
- In PHP return type declarations, put a space before the colon: `function example( string $value ) : string`. This is Aubrey's spacing override to the WordPress PHP baseline. Do not apply it to PHP alternative-syntax control statements such as `<?php if ( $visible ) : ?>`.
- In PHP documentation, add `@return` only when the function actually returns a value.
- Follow the **Breathing Room Principle** for function-like code: every non-empty named function or method body gets a blank line immediately after its opening brace, even when it contains only one statement. Apply the same spacing to an anonymous function, callback, or closure when its body contains multiple statements or logical sections; a one-statement callback may remain compact.
- Follow the **Return Early and Often** principle: exit a function as soon as an invalid, missing, already-completed, or otherwise terminal condition means the remaining work is unnecessary. Keep the normal path flat and visible. Use a bare `return;` when no value is returned. Guard clauses are one technique within this principle, not the name of the principle itself.
- Do not blindly trust runtime parameters in a dynamically typed language. When the expected type cannot be enforced by the language, use one practical guard that tests the parameter's expected type, then return early when it fails. Do not exhaustively test every property or API method when the type test is sufficient. In JavaScript, preserve the expected JSDoc type and use the native type test—for example, `@param {HTMLElement} panel` with `if ( false === panel instanceof HTMLElement ) { return; }`; do not weaken the parameter to `unknown` merely because a guard is present.
- When reading an optional or untrusted PHP array key, use null coalescing before comparing it: use `( $array['key'] ?? false )` when the missing-value default should be `false`, and `( $array['key'] ?? '' )` when comparing against a specific string. Parenthesize the coalesced expression because `??` has low precedence.
- CSS underscores are allowed.
- CSS property order is functional first, alphabetical second. Group related properties such as layout, flex, typography, colors, spacing, and positioning; alphabetize only within each group. Do not alphabetize across functional groups.
- CSS colors may use any clear supported notation; do not change color notation without a reason.
- In CSS function notation, put one space inside the parentheses, just as with normal function calls: `minmax( 0, 1fr )`, `repeat( 2, 1fr )`, and `rgb( 23 32 51 / 12% )`.
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

- Give a parent block breathing room when it contains multiple logical child blocks: leave a blank line after the opening brace or opening HTML tag before the first child and between sibling blocks. Do not add a trailing blank line immediately before the closing curly brace; close the block directly after its final child or statement. Apply this to CSS at-rules, PHP blocks, JavaScript blocks, and HTML elements when they contain multiple sections. HTML follows the opening and sibling-spacing rule, but has no curly-brace closing rule. Compact simple HTML lists are an exception: when each `li` contains short, simple content, keep each item on one line, keep sibling items adjacent, and do not add blank lines inside the `ul` or `ol`. Expand a list item only when its contents are complex or contain multiple meaningful child elements.

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

- In languages with ternary expressions, when a simple conditional only chooses a value to assign—especially when it initializes a missing value—use a ternary instead of an `if` block. Assign the ternary result once. Use `if` for multiple branches, multiple statements, side effects, or early returns:

  ```js
	groupedTasks[ day ] = undefined === groupedTasks[ day ]
		? []
		: groupedTasks[ day ];
  ```

- When testing a known boolean, compare explicitly with `false === value` instead of using bang negation such as `! value`. For nullable or missing values, use the known value such as `null === value` or `undefined === value`; do not change the data test's meaning merely to avoid `!`.
- Do not add parentheses around a simple type or boolean test when the language's operator precedence already makes the comparison unambiguous. For example, write `false === panel instanceof HTMLElement`, not `false === ( panel instanceof HTMLElement )`.

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

- prevents an expensive, fetched, side-effectful, or otherwise non-repeatable operation from happening repeatedly;
- is used in multiple places;
- captures a required snapshot;
- represents required state that cannot be safely or clearly inlined; or
- is required by the language or API.

Being computed is not, by itself, a reason to create a variable. A simple pure expression that is used once is still an unnecessary Inline Temp. Do not keep a one-use variable merely because the expression is arithmetic, a ternary, a property path, a string interpolation, or a constructed value. Inline it where it is used:

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

The same rule applies to a pure one-use transformation such as `visibleSignals`. Do not split a filter-and-map pipeline into a named intermediate value unless the separate binding is required for a snapshot, repeated use, or correctness.

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
 * @since Unknown
 *
 * @param {object} user User data.
 * @return {object} Rendered user data.
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

### Document practical data types

Function documentation must identify the actual broad runtime type and explain the expected contents in plain language. Use simple types such as `array`, `object`, `string`, `int`, `bool`, `HTMLElement`, or `Promise` when appropriate. Do not encode a complete nested array or object schema in every DocBlock; that creates a needless maintenance burden and falsely presents internal structure as a universal contract.

Inspect the source of the data before writing the DocBlock so the broad type and purpose are accurate. A function that uses `.map()` returns an array, an object literal returns an object, and a fetch chain returns a Promise. The implementation and the broad documentation must agree.

Use a project-established named type only when the project already requires it as a public or shared contract. Do not invent inline object-property shapes, array-element shapes, or standalone typedefs merely to enumerate internal fields in a single function or example. Explain those contents in the parameter or return description instead.

This is separate from runtime guarding. Documentation may say `@param {array} tasks Task records.` without claiming a complete schema; the implementation may still validate or use the fields it needs.

## 4. Compliance gate

Before presenting code as compliant, verify all of the following:

Review the exact final code that will be shown, not the intended design or an earlier draft. Treat examples in chat, diffs, plans, and explanations as production code for this gate. Inspect every local variable's use count, every function's documentation, every documented data shape, and every parent-child block boundary before output.

- Project-local instructions and enforced formatter, linter, compiler, type-checker, and runtime rules were inspected.
- The applicable WordPress language and documentation references were read.
- The target language/runtime version is known, or the assumption is stated before version-dependent syntax is used; never silently assume support.
- Every new or modified named function and method has a directly preceding language-appropriate DocBlock or JSDoc block, unless an explicit project standard says otherwise.
- Function documentation follows the applicable WordPress summary, parameter, return, and version-tag rules. Every named function and method has an `@since` tag. For new code, use the current date in `Month D, YYYY` format, such as `@since July 18, 2026`. Never replace an existing `@since`; when modifying existing code, add another `@since` line with the current date and a concise description of the change. Use `@since Unknown` only when the original introduction date cannot be established.
- Determine human authorship once when first working in a project by inspecting project metadata, available history, or asking the user. Do not add `@author` when the project is clearly authored by one person. When a project clearly has multiple human authors and the function's author is known, add `@author Name <email>` in that exact format. For Aubrey's documented identity, the exact tag is `@author Aubrey Portwood <aubreypwd@icloud.com>`, but never copy that identity into an unrelated project. If authorship is unclear, omit `@author`. Never identify Codex, Claude, an AI agent, or a model as the author.
- Function documentation uses a simple accurate runtime type and a plain-language description of its contents. Reject unnecessarily verbose nested array/object schemas and invented typedefs when a broad type is sufficient. Verify that array-versus-object documentation matches the actual return expression.
- `@return` appears only when the function actually returns a value. Do not add `@return void` unless the applicable project standard explicitly requires it.
- A standalone `<?php` tag has a blank line before a following DocBlock or statement, while the DocBlock remains directly adjacent to its declaration.
- Parent blocks and HTML elements with multiple logical child blocks have a blank line after their opening syntax and between sibling blocks. Curly-braced languages have no trailing blank line immediately before the closing brace; HTML has no corresponding trailing-curly rule.
- The Breathing Room Principle was applied to every non-empty named function or method, including one-statement bodies. Nested callbacks and closures with multiple statements or logical sections also have breathing room after their opening brace.
- Simple HTML lists remain compact: keep short `li` items on one line and adjacent to one another without blank lines inside the list. Do not apply the general parent-spacing rule mechanically to a short list.
- CSS, PHP, and JavaScript blocks have no trailing blank line immediately before a closing curly brace. HTML is not subject to this curly-brace rule.
- PHP return type declarations have a space before the colon. Do not confuse this with the colon in PHP alternative syntax.
- CSS function arguments have one space inside their parentheses; reject compact forms such as `minmax(0, 1fr)`.
- JavaScript and PHP function-call spacing follows the exact WordPress form, including spaces inside parentheses and `} );` for multiline calls.
- One- and two-parameter calls are not split unnecessarily; larger calls use one parameter per line.
- Trailing commas in function argument lists are present only when target-language and target-version support is confirmed.
- Tabs, semicolons, whitespace, line wrapping, braces, comments, and blank lines follow the applicable WordPress rules.
- Strings in every supported language use native interpolation, templating, formatting, or parameterization instead of concatenation whenever an alternative exists. JavaScript uses template literals by default, and variables are declared separately.
- Objects and arrays preserve intentional order and are not alphabetized without instruction.
- No meaningless single-use variables or convenience aliases were introduced. A pure one-use computation is rejected even when it has a descriptive name or is assigned to a variable for readability.
- Repeated computation, fetching, side effects, and transformations are not repeated unnecessarily.
- Simple literal repetition has not been turned into a needless alias or abstraction.
- Ternaries are kept compact when simple and broken at `?` and `:` only when long or complex.
- A simple conditional assignment uses a ternary and assigns the result once; `if` is reserved for multiple branches, multiple statements, side effects, or early returns.
- Known boolean false checks use explicit comparisons such as `false === value`, not bang negation. Nullable and missing-value checks use their actual known values.
- Simple boolean and type-test expressions do not have unnecessary grouping parentheses when operator precedence already makes their evaluation order clear.
- Runtime parameters in dynamically typed code have one practical expected-type guard before use when static typing cannot enforce the contract. Reject exhaustive defensive checks when that guard is sufficient, and keep the documented parameter type specific.
- Return Early and Often was applied: terminal conditions exit before later work, and the normal path is not unnecessarily nested inside conditionals.
- Optional PHP array keys are protected with `??` before comparison, with a default matching the intended value type and explicit parentheses around the coalesced expression.
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
- `references/good-examples.md` — canonical positive examples for CSS, HTML, PHP, JavaScript, and combined templates.
- `references/bad-examples.md` — canonical reject list of known failure patterns.
- `references/examples.md` — earlier conversation-derived examples retained as supporting history.

Read `references/good-examples.md` before generating code and compare the final output with `references/bad-examples.md` before presenting it. Use `references/examples.md` only as supporting history when a decision is not covered by the canonical files.

Any example explicitly labeled `Bad` is a recognition fixture only. Never copy it into code presented as compliant.
