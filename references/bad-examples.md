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
function store_diagnostic_message( string $message ): void {
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
function get_active_workspace(): array {
	return get_workspace();
}
```

The DocBlock is crammed against the standalone PHP opening tag. The tag must be separated from the DocBlock by a blank line.

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

The first child block is jammed against the parent at-rule. The last child is also jammed against the parent closing brace. A parent with multiple child blocks needs breathing room at both boundaries.

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
