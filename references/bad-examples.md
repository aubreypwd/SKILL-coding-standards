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
 * @param {Array<Object>} records Ledger records.
 * @return {Object} Ledger records.
 */
function readLedger( records ) {
	return records.map( function ( record ) {
		return record;
	} );
}
```

The first function has no DocBlock. The second says the parameter is a generic array of objects and claims the mapped array is an object.

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

## Breathing Room: named function body starts immediately

```js
/**
 * @typedef {Object} Workspace
 * @property {string} id Workspace identifier.
 */

/**
 * Returns the current workspace.
 *
 * @since Unknown
 *
 * @return {Workspace} Current workspace.
 */
function getCurrentWorkspace() {
	return readWorkspace();
}
```

Named function and method bodies always get a blank line immediately after the opening brace, even when the body contains only one statement.

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
