# Bad Examples

These examples are the reject list for Aubrey Portwood's coding standards. They are intentionally incorrect. Never copy them into code presented as compliant.

## Meaningless variables

```js
const accountName = account.profile.displayName;

showAccountName( accountName );
```

```js
const cutoff = currentTime + 30 * 60 * 1000;

return reminders.filter( function ( reminder ) {
	return reminder.startsAt <= cutoff;
} );
```

```js
const filteredEntries = entries
	.filter( function ( entry ) {
		return entry.visible;
	} );

return filteredEntries.map( function ( entry ) {
	return formatEntry( entry );
} );
```

```js
function submitPreferences( form, settings ) {
	const endpoint = settings.api.baseUrl;
	const formData = new FormData( form );
	const requestUrl = `${endpoint}/preferences/${settings.userId}`;

	return fetch( requestUrl, {
		method: `POST`,
		body: formData,
	} );
}
```

```js
const title = card.heading;
const status = card.status;

renderCard( title, status );
```

## Repeating work instead of applying DRY

```js
return {
	profile: loadProfile( userId ),
	permissions: getPermissions( loadProfile( userId ) ),
	lastSeen: formatLastSeen( loadProfile( userId ) ),
};
```

```php
return [
	'customer' => get_customer( $customer_id ),
	'email' => get_customer( $customer_id )['email'],
	'label' => get_customer_label( get_customer( $customer_id ) ),
];
```

## Repeating simple literals unnecessarily through a variable

```js
const region = `Southwest`;

const office = {
	primary: {
		region,
	},
	backup: {
		region,
	},
};
```

## Vertically aligned associative-array arrows

```php
return [
	'account'      => $account,
	'contact'      => $account['contact'],
	'refreshed_at' => time(),
];
```

Do not add padding to align `=>`. Keep one space on each side so changing one key does not require realigning the other entries.

## Comma-separated declarations

```js
const firstLabel = `Primary`, secondLabel = `Secondary`, count = 2;
```

## Obsolete variable declarations

```js
var pendingCount = 0;
```

## String concatenation and avoidable escaping

```js
const notice = `Hello, ` + userName + `!`;
```

```js
const notice = 'The user can\'t continue.';
```

```php
$message = 'The export for ' . $email . ' is ready.';
```

## Incorrect function-call spacing

```js
return fetch(requestUrl, {
	method: `POST`,
	body: formData,
});
```

```js
return fetch ( requestUrl, {
	method: `POST`,
	body: formData,
} );
```

```js
return fetch( requestUrl, {
	method: `POST`,
	body: formData,
});
```

The final object argument must close as `} );`, not `});`.

## Unsupported trailing commas in function calls

```js
queueJob(
	jobId,
	payload,
);
```

Use this only when the target language and version explicitly support trailing commas in function calls.

## Over-explained and misclassified comments

```js
// Comment one
// Continuing comment
// More information about the comment.

/**
 * This is an inline explanation of the next statement.
 */
const requestState = getRequestState();
```

Do not stack multiple single-line comments to over-explain one idea. If multiple lines are genuinely necessary, use `/* ... */`, not `/** ... */`; reserve DocBlocks for declarations. Prefer one concise comment that explains the construction or context of the code.

## Redundant comment that repeats the declaration

```php
<?php

/**
 * Registers the invoice status filter.
 *
 * @since Unknown
 */
function register_invoice_status_filter() : void {

	// Register the formatter at the point where the status hook is declared.
	add_filter( 'invoice_status_label', 'format_invoice_status_label' );
}
```

The function name, DocBlock, and direct call already explain this code. Do not add a comment that merely paraphrases them. Add a comment only when it contributes non-obvious rationale, a constraint, accessibility or state context, intentional ordering, or another detail the code does not already communicate.

## Comment explaining an obvious assignment or coding principle

```php
// Fetch the account once because its data populates two payload fields.
$account = get_account( $account_id );
```

The assignment already clearly says that the account is being retrieved and stored. The comment explains the DRY principle and adds no reader-facing domain context. Do not comment obvious assignments or use code comments to teach a coding standard.

## CSS: comment outside the block it describes

```css
/* Keep layout declarations visibly grouped. */
.account_panel {

	display: grid;
	gap: 1rem;
}
```

When a CSS comment describes declarations inside a selector, put it inside the selector block after the opening brace and before those declarations. Use the same placement inside an at-rule when the comment describes its child blocks.

## CSS: unnecessary vertical comment for one declaration

```css
.account_panel__heading {

	/* Remove the browser margin from this compact heading block. */

	margin: 0;
}
```

Keep a trivial one-property CSS block compact. Put a concise explanation at the end of the declaration line: `margin: 0; /* Remove the browser margin. */`.

## CSS: nesting the markup tree instead of a component concept

```css
body {

	.page {

		.content {

			.card {
				padding: 1rem;
			}
		}
	}
}
```

Do not nest CSS under generic ancestors merely because the HTML is nested that way. Nest around a stable component concept such as `.card`, keep the component portable, and generally stop at two levels. Deeper nesting requires distinct nested concepts and a real need for the additional structure.

## CSS: separating root properties from nested component behavior

```css
.card {

	display: grid;

	.card__body {
		padding: 1rem;
	}

	@media (min-width: 48rem) {

		grid-template-columns: 1fr auto;
	}
}
```

Keep the component's own declarations and root-level responsive overrides together before nested child concepts. Do not make readers search past child blocks to find properties that apply to the root component.

## CSS: one media query mixing component levels

```css
.card {

	display: grid;

	@media (min-width: 48rem) {

		grid-template-columns: 1fr auto;

		.card__hero {
			grid-template-columns: 1fr;
		}
	}

	.card__hero {
		grid-template-columns: auto 1fr;
	}
}
```

Keep the root media query limited to `.card` changes. Put the `.card__hero` media override inside `.card__hero` so each selector owns its responsive behavior.

## Vague or incorrect documentation

```js
function readLedger( records ) {
	return records.map( function ( record ) {
		return {
			id: record.id,
			label: record.label,
		};
	} );
}
```

```js
/**
 * Reads ledger records.
 *
 * @since Unknown
 *
 * @param {array} records Ledger records.
 * @return {object} Ledger records.
 */
function readLedger( records ) {
	return records.map( function ( record ) {
		return record;
	} );
}
```

The first function has no DocBlock. The second uses the correct broad input type but incorrectly claims that the mapped result is an object; `.map()` returns an array.

## Over-specified array and object documentation

```js
/**
 * Groups tasks by their calendar date.
 *
 * @since Unknown
 *
 * @param {{ id: string, dueDate: string, title: string }[]} tasks Task records.
 * @return {Record<string, { id: string, dueDate: string, title: string }[]>} Tasks keyed by calendar date.
 */
function groupTasksByDay( tasks ) {

	return tasks.reduce( function ( groupedTasks, task ) {

		const day = task.dueDate.slice( 0, 10 );

		if ( undefined === groupedTasks[ day ] ) {
			groupedTasks[ day ] = [];
		}

		groupedTasks[ day ].push( task );

		return groupedTasks;
	}, {} );
}
```

Do not document every internal property and array element inline when the function only needs a broad runtime type. Use `@param {array}` and `@return {object}` for this function, then explain the expected contents in plain language. Use a project-established named type only when it is required as a shared or public contract.

## JavaScript: `if` for a simple assignment choice

```js
if ( undefined === groupedTasks[ day ] ) {
	groupedTasks[ day ] = [];
}
```

When a simple conditional only chooses the value to assign, use a ternary and assign the result once. Keep `if` for multiple branches, multiple statements, side effects, or early returns.

```php
/**
 * Stores a diagnostic message.
 *
 * @since Unknown
 *
 * @param string $message Diagnostic message.
 * @return void
 */
function store_diagnostic_message( string $message ) : void {
	error_log( $message );
}
```

Do not add `@return` when the function does not return a value.

## PHP opening-tag spacing

```php
<?php
/**
 * Gets the active workspace.
 *
 * @since Unknown
 *
 * @return array Active workspace.
 */
function get_active_workspace() : array {
	return get_workspace();
}
```

The DocBlock is crammed against the standalone PHP opening tag. The tag must be separated from the DocBlock by a blank line.

## PHP: missing space before a return type colon

```php
<?php

/**
 * Returns an invoice label.
 *
 * @since Unknown
 *
 * @param string $status Invoice status.
 * @return string Invoice status.
 */
function get_invoice_label( string $status ): string {
	return $status;
}
```

Put a space before the colon in PHP return type declarations: `function get_invoice_label( string $status ) : string`.

## CSS block breathing room

```css
@media (prefers-contrast: more) {
	.billing_panel {

		border-color: CanvasText;
		color: CanvasText;
	}

	.billing_panel__total {

		font-weight: 700;
	}
}
```

The first child block is jammed against the parent at-rule. Add breathing room after the parent opening brace and between sibling blocks, but do not add a blank line before the parent closing brace.

## CSS: compact function parentheses

```css
.dashboard_grid {
	grid-template-columns: minmax(0, 1fr) repeat(2, minmax(0, 1fr));
}
```

CSS function notation follows the same visual spacing preference as normal function calls. Use `minmax( 0, 1fr )` and `repeat( 2, minmax( 0, 1fr ) )`.

## CSS: trailing blank line before a closing brace

```css
@media (prefers-color-scheme: dark) {

	.report_card {

		background-color: #111827;
		color: #f9fafb;
	}

	.report_card__meta {
		font-size: 0.875rem;
	}

}
```

The blank line before the final `}` is unnecessary. The closing brace should follow the final child block directly.

## PHP and JavaScript block spacing

```js
function revealDetails( panel ) {
	if ( panel.hidden ) {
		panel.hidden = false;
	}
	panel.focus();
}
```

```php
<?php if ( $show_details ) : ?>
	<section>
		<h2>Details</h2>
		<p><?php echo esc_html( $details ); ?></p>
	</section>
<?php endif; ?>
```

The logical child block is visually crammed against the surrounding parent syntax. Separate multiple logical sections with blank lines while preserving the required adjacency of a DocBlock and its declaration.

## JavaScript: unnecessary boolean alias and bang negation

```js
/**
 * Toggles the command palette.
 *
 * @since Unknown
 *
 * @param {HTMLButtonElement} trigger Palette trigger.
 * @param {HTMLElement} palette Palette element.
 */
function toggleCommandPalette( trigger, palette ) {

	const isHidden = palette.hidden;

	palette.hidden = ! isHidden;
	trigger.setAttribute( `aria-expanded`, `${isHidden}` );

	if ( isHidden ) {
		const searchInput = palette.querySelector( `#command-palette-search` );

		if ( searchInput instanceof HTMLInputElement ) {
			searchInput.focus();
		}

		return;
	}

	trigger.focus();
}
```

Do not create `isHidden` merely to rename a directly available boolean property. Use `false === palette.hidden` for the toggle and reference `palette.hidden` directly. Use explicit boolean comparisons instead of bang negation.

## JavaScript: unnecessary grouping parentheses in a type test

```js
function bindDismissiblePanel( panel ) {

	if ( false === ( panel instanceof HTMLElement ) ) {
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

When `instanceof` is being compared as a boolean and operator precedence is unambiguous, do not add grouping parentheses around the type test. Use `false === panel instanceof HTMLElement`.

## JavaScript: trusting a runtime parameter

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

Do not use a dynamically typed parameter without one practical expected-type guard. Test the parameter once, return early when it is not the expected type, and keep the specific `@param` type. Do not replace it with `unknown` or add exhaustive property checks when the type guard is sufficient.

## PHP: unprotected optional array keys

```php
<?php

/**
 * Gets a subscription label.
 *
 * @since Unknown
 *
 * @param array{enabled?: bool, status?: string} $subscription Subscription data.
 * @return string Subscription label.
 */
function get_subscription_label( array $subscription ) : string {

	if ( false === $subscription['enabled'] ) {
		return 'Disabled';
	}

	if ( 'trial' === $subscription['status'] ) {
		return 'Trial';
	}

	return 'Active';
}
```

Do not directly read an optional array key. Compare the null-coalesced value instead: `( $subscription['enabled'] ?? false )` or `( $subscription['status'] ?? '' )`.

## Breathing Room: complex named function body starts immediately

```js
/**
 * Returns a workspace when one is available.
 *
 * @since Unknown
 *
 * @param {object|null} workspace Workspace data.
 * @return {object} Workspace data.
 */
function getCurrentWorkspace( workspace ) {
	if ( null === workspace ) {
		return {};
	}

	return readWorkspace( workspace );
}
```

This function has multiple logical sections but no breathing room after its opening brace. Trivial one-statement functions may remain compact; functions with multiple sections or complex first statements need the blank line.

## JavaScript: trailing blank line before a closing brace

```js
/**
 * Reveals a drawer.
 *
 * @since Unknown
 *
 * @param {HTMLElement} drawer Drawer element.
 * @param {HTMLButtonElement} trigger Drawer trigger.
 */
function revealDrawer( drawer, trigger ) {

	if ( drawer.hidden ) {
		drawer.hidden = false;
	}

	trigger.focus();

}
```

The blank line before the function's closing brace adds no useful separation. Keep the opening and sibling spacing, then close the function directly after its final statement.

## HTML: no breathing room after an opening element

```html
<main id="billing-summary" aria-labelledby="billing-summary-title">
	<header>
		<h1 id="billing-summary-title">Billing summary</h1>
	</header>

	<p>Your current balance is shown below.</p>
</main>
```

The header is a separate child block and is jammed against the containing `main` element. Add a blank line after the opening parent tag before the first child block. This applies to every HTML parent with multiple logical child blocks, not only `main`. HTML does not need a trailing blank line before the closing tag.

## HTML: comment jammed against its parent or child

```html
<nav aria-label="Project sections">
	<!-- Explain the navigation structure. -->
	<ul>
		<li><a href="/overview">Overview</a></li>
	</ul>
</nav>
```

An HTML comment is its own logical child block. Add a blank line after the opening parent tag before the comment and another blank line between the comment and the child it explains.

## HTML: over-formatting a simple list

```html
<nav aria-label="Project sections">
	<ul>

		<li>
			<a href="/overview">Overview</a>
		</li>

		<li>
			<a href="/activity">Activity</a>
		</li>
	</ul>
</nav>
```

Short list items with one simple link do not need individual multiline blocks or blank lines between siblings. Keep each `li` compact and keep the list items adjacent.

## HTML: standalone closing angle bracket on a multiline opening tag

```html
<button
	aria-controls="command-palette"
	aria-expanded="false"
	id="command-palette-trigger"
type="button"
>
		Open command palette
</button>
```

When a multiline HTML opening tag is used, put the closing `>` immediately after the final attribute: `type="button">`.

## HTML: multiline-tag contents aligned with attributes

```html
<button
	aria-controls="command-palette"
	aria-expanded="false"
	id="command-palette-trigger"
	type="button">
	Open command palette
</button>
```

Indent the contents of a multiline opening tag one additional tab beyond its attributes by default. The text above is aligned with the attributes instead of being nested beneath them.

## PHP: trailing blank line before a closing brace

```php
<?php

/**
 * Builds a compact report summary.
 *
 * @since Unknown
 *
 * @param array{title: string, count: int} $report Report data.
 * @return array{title: string, count: int} Report summary.
 */
function build_report_summary( array $report ) : array {

	$summary = [
		'title' => $report['title'],
		'count' => $report['count'],
	];

	return $summary;

}
```

The blank line before the closing PHP brace is unnecessary. Keep the separation between the computation and return value, then close the function directly after the final statement.

## XHTML-style HTML

```html
<input type="email" name="email" />
<img src="badge.png" alt="Verified account" />
<br />
```

Use modern HTML void elements without closing forward slashes.

## Inaccessible HTML

```html
<div onclick="openMenu()">Open menu</div>
<input placeholder="Search here">
<span class="error">Invalid value</span>
```

Use the appropriate interactive element, an accessible label, and an announced error relationship.

## Alphabetizing intentional object order

```js
const lifecycle = {
	complete: finishProcess,
	initialize: initializeProcess,
	running: runProcess,
};
```

The object has been alphabetized instead of preserving the intentional lifecycle order.
