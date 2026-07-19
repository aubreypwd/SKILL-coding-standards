# Good Examples

These examples are the canonical positive reference for Aubrey Portwood's coding
standards. They are intentionally distinct from the examples in the WordPress
snapshots and the conversation history.

Verify the target language and version before using version-sensitive syntax.
These examples do not use trailing commas in function calls. CSS function notation uses one space inside its parentheses.
Associative-array arrows use one space on each side and are not vertically aligned.
Named function and method bodies have breathing room after their opening brace when they contain multiple logical statements or sections, or when their first statement is complex or multiline. Trivial one-statement bodies remain compact. Nested callbacks and closures have the same spacing when they contain multiple statements or logical sections.
Comments are intentionally included throughout the examples to explain construction, context, accessibility, state changes, and the purpose of direct calls. They remain concise and use language-appropriate comment syntax.
Nested component examples keep the component's own declarations and root-level responsive overrides together before nested child concepts.

## CSS

```css
.account_panel {

	display: grid;
	gap: 1rem;
	grid-template-columns: minmax( 0, 1fr ) auto;

	font-size: 1rem;
	line-height: 1.5;

	background-color: #f7f7f7;
	color: #1f2937;

	border: 1px solid #d1d5db;
	border-radius: 0.5rem;
	padding: 1.25rem;
}
```

```css
@container account_panel (min-width: 44rem) {

	/* Move the details and actions into separate columns when space allows. */

	.account_panel__details {

		display: grid;
		grid-template-columns: repeat( 2, minmax( 0, 1fr ) );
		gap: 1rem;
	}

	.account_panel__actions {

		align-items: center;
		display: flex;
		justify-content: flex-end;
	}
}
```

```css
.account_panel__heading {
	margin: 0; /* Remove the browser margin from this compact heading block. */
}
```

```css
@media (prefers-reduced-motion: reduce) {

	/* Respect the user's motion preference for carousel feedback. */

	.carousel_track {
		scroll-behavior: auto;
	}

	.carousel_indicator {
		transition: none;
	}
}
```

```css
.inventory_card {

	align-items: start;
	display: flex;
	gap: 0.75rem;

	font-size: 0.9375rem;
	font-weight: 600;

	background: #ffffff;
	color: #172033;

	box-shadow: 0 0.25rem 1rem rgb( 23 32 51 / 12% );
	padding: 1rem;
}

.inventory_card:focus-within {
	outline: 0.1875rem solid #2563eb;
	outline-offset: 0.125rem;
}
```

```css
.schedule_grid {

	display: grid;
	grid-template-areas:
		"date title"
		"date detail";
	grid-template-columns: 8rem minmax( 0, 1fr );

	column-gap: 1.25rem;
	row-gap: 0.375rem;
}

.schedule_grid__date {
	grid-area: date;
}

.schedule_grid__title {
	grid-area: title;
}

.schedule_grid__detail {
	grid-area: detail;
}
```

```css
.card {

	display: grid;
	grid-template-rows: auto 1fr auto;

	background-color: #ffffff;
	border: 1px solid #d1d5db;
	border-radius: 0.5rem;
	padding: 1rem;

	@media (min-width: 48rem) {
		grid-template-columns: minmax( 0, 2fr ) minmax( 12rem, 1fr );
	}

	.card__hero {

		align-items: center;
		display: grid;
		gap: 0.75rem;
		grid-template-columns: auto 1fr;

		@media (min-width: 48rem) {
			grid-template-columns: 1fr;
		}

		.card__hero-image {
			aspect-ratio: 1;
			border-radius: 50%;
			object-fit: cover;
		}

		.card__hero-pill {
			background-color: #dbeafe;
			color: #1e3a8a;
			padding: 0.25rem 0.5rem;
		}
	}

	.card__body {

		font-size: 1rem;
		line-height: 1.5;
	}
}
```

```css
.dialog {

	border: 0;
	max-width: 32rem;
	padding: 0;

	&[open] {
		display: grid;
	}

	.dialog__footer {

		display: flex;
		gap: 0.75rem;
		justify-content: flex-end;
		padding: 1rem;

		@media (min-width: 40rem) {
			padding: 1.5rem;
		}
	}
}
```

## HTML

Parent elements with multiple logical child blocks have a blank line after the opening tag and between sibling blocks. Short lists with simple items are a compact exception: keep each `li` on one line and keep the items adjacent. For multiline opening tags, the contents are indented one additional tab beyond the attributes. Do not add a trailing blank line before an HTML closing tag.

```html
<main id="notification-settings" aria-labelledby="notification-settings-title">

	<!-- Keep the heading and help text together as the form's accessible context. -->

	<header>

		<h1 id="notification-settings-title">Notification settings</h1>

		<p id="notification-settings-help">
			Choose which updates should reach your team.
		</p>
	</header>

	<form aria-describedby="notification-settings-help" action="/settings/notifications" method="post">

		<fieldset>

			<legend>Delivery channels</legend>

			<label>
				<input name="channels[]" type="checkbox" value="email">
				Email summaries
			</label>

			<label>
				<input name="channels[]" type="checkbox" value="sms">
				Text alerts
			</label>
		</fieldset>

		<button type="submit">Save notification settings</button>
	</form>
</main>
```

```html
<nav aria-label="Project sections">
	<ul>
		<li><a aria-current="page" href="/overview">Overview</a></li>
		<li><a href="/activity">Activity</a></li>
		<li><a href="/members">Members</a></li>
	</ul>
</nav>
```

```html
<dialog aria-labelledby="archive-dialog-title" id="archive-dialog">

	<!-- Use the dialog form so native cancel and confirm behavior remains available. -->

	<form method="dialog">

		<h2 id="archive-dialog-title">Archive this report?</h2>

		<p>The report will leave the active workspace.</p>

		<div>
			<button value="cancel">Cancel</button>
			<button autofocus value="confirm">Archive report</button>
		</div>
	</form>
</dialog>
```

```html
<table>

	<!-- Keep the caption and scope attributes available to assistive technology. -->

	<caption>Upcoming maintenance windows</caption>

	<thead>
		<tr>
			<th scope="col">Service</th>
			<th scope="col">Window</th>
			<th scope="col">Owner</th>
		</tr>
	</thead>

	<tbody>

		<tr>
			<th scope="row">Search index</th>
			<td>June 12, 2026</td>
			<td>Platform team</td>
		</tr>

		<tr>
			<th scope="row">Billing export</th>
			<td>June 19, 2026</td>
			<td>Finance team</td>
		</tr>
	</tbody>
</table>
```

```html
<section aria-labelledby="import-status-title">

	<!-- Use a polite live region so status changes do not steal focus. -->

	<h2 id="import-status-title">Import status</h2>

	<p aria-live="polite" id="import-status-message">
		The import is ready to begin.
	</p>

	<progress aria-label="Import progress" max="100" value="0">0%</progress>
</section>
```

```html
<article aria-labelledby="release-note-title">

	<header>

		<p>Version 4.2</p>

		<h2 id="release-note-title">Scheduled exports are now available</h2>
	</header>

	<p>Send a report to a saved destination without opening the dashboard.</p>

	<a href="/exports">Manage scheduled exports</a>
</article>
```

## PHP

```php
<?php

/**
 * Formats an invoice reference for display.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 *
 * @param array $invoice Invoice data containing the invoice series and number.
 * @return string Formatted invoice reference.
 */
function get_invoice_reference( array $invoice ) : string {

	return sprintf(
		'%s-%06d',
		$invoice['series'],
		$invoice['number']
	);
}
```

```php
<?php

/**
 * Returns the enabled regions.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 * @since July 18, 2026 Added a safe default for missing enabled keys.
 *
 * @param array $regions Region records.
 * @return array Enabled region records.
 */
function get_enabled_regions( array $regions ) : array {

	return array_values(
		array_filter(
			$regions,
			static function ( array $region ) : bool {
				return true === ( $region['enabled'] ?? false );
			}
		)
	);
}
```

```php
<?php

/**
 * Returns a customer label.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 *
 * @param array $customer Customer data.
 * @return string Customer label.
 */
function get_customer_label( array $customer ) : string {

	return sprintf(
		'%s (%s)',
		$customer['name'],
		$customer['code']
	);
}
```

```php
<?php

/**
 * Builds a synchronization payload for an account.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 *
 * @param int $account_id Account ID.
 * @return array Synchronization payload containing account, contact, and refresh data.
 */
function build_sync_payload( int $account_id ) : array {

	$account = get_account( $account_id );

	return [
		'account' => $account,
		'contact' => $account['contact'],
		'refreshed_at' => time(),
	];
}
```

```php
<?php

/**
 * Writes an audit event.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 *
 * @param string $event Event name.
 * @param array $context Event context.
 * @param int $actor_id Actor ID.
 */
function write_audit_event( string $event, array $context, int $actor_id ) : void {

	// Pass the original context through so the hook can record the event accurately.
	do_action(
		'project_audit_event',
		$event,
		$context,
		$actor_id
	);
}
```

```php
<?php

/**
 * Registers the invoice status filter.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 */
function register_invoice_status_filter() : void {
	add_filter( 'invoice_status_label', 'format_invoice_status_label' );
}

/**
 * Formats an invoice status label.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 *
 * @param string $status Invoice status.
 * @return string Formatted invoice status.
 */
function format_invoice_status_label( string $status ) : string {

	return match ( $status ) {
		'paid' => 'Paid in full',
		'pending' => 'Awaiting payment',
		'refunded' => 'Refunded',
		default => 'Status unavailable',
	};
}
```

```php
<?php

/**
 * Gets a subscription label.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 *
 * @param array $subscription Subscription data.
 * @return string Subscription label.
 */
function get_subscription_label( array $subscription ) : string {

	if ( false === ( $subscription['enabled'] ?? false ) ) {
		return 'Disabled';
	}

	if ( '' === ( $subscription['email'] ?? false ) ) {
		return 'Email required';
	}

	if ( 'trial' === ( $subscription['status'] ?? '' ) ) {
		return 'Trial';
	}

	return $subscription['label'] ?? '';
}
```

```php
<?php if ( $reservation['confirmed'] ) : ?>

	<section aria-labelledby="reservation-confirmed-title">

		<h2 id="reservation-confirmed-title">Reservation confirmed</h2>

		<p>Your reservation code is <?php echo esc_html( $reservation['code'] ); ?>.</p>
	</section>

<?php else : ?>

	<section aria-labelledby="reservation-pending-title">

		<h2 id="reservation-pending-title">Reservation pending</h2>

		<p>We are waiting for the venue to confirm your request.</p>
	</section>

<?php endif; ?>
```

## JavaScript

```js
/**
 * Builds an accessible alert item.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 * @since July 18, 2026 Added a practical object guard.
 *
 * @param {object} alert Alert data.
 * @return {object} Alert item with live-region metadata.
 */
function buildAlertItem( alert ) {

	if ( null === alert || 'object' !== typeof alert ) {
		return {};
	}

	return {
		...alert,
		ariaLive: alert.severity === `critical` ? `assertive` : `polite`,
	};
}
```

```js
/**
 * Groups tasks by their calendar date.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 * @since July 18, 2026 Added an array guard and corrected the object return documentation.
 *
 * @param {array} tasks Task records.
 * @return {object} Tasks keyed by calendar date.
 */
function groupTasksByDay( tasks ) {

	if ( false === Array.isArray( tasks ) ) {
		return {};
	}

	return tasks.reduce( function ( groupedTasks, task ) {

		const day = task.dueDate.slice( 0, 10 );

		groupedTasks[ day ] = undefined === groupedTasks[ day ]
			? []
			: groupedTasks[ day ];

		groupedTasks[ day ].push( task );

		return groupedTasks;
	}, {} );
}
```

```js
/**
 * Sorts queue entries by priority without changing the source array.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 * @since July 18, 2026 Added a practical array guard.
 *
 * @param {array} entries Queue entries.
 * @return {array} Sorted queue entries.
 */
function sortQueueEntries( entries ) {

	if ( false === Array.isArray( entries ) ) {
		return [];
	}

	// Copy before sorting so callers keep the original queue order.
	return entries.slice().sort( function ( firstEntry, secondEntry ) {
		return firstEntry.priority - secondEntry.priority;
	} );
}
```

```js
/**
 * Loads dashboard data.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 * @since July 18, 2026 Added an endpoint type guard.
 *
 * @param {string} endpoint Dashboard endpoint.
 * @return {Promise} Dashboard data.
 */
async function loadDashboardData( endpoint ) {

	if ( 'string' !== typeof endpoint ) {
		return;
	}

	return fetch( endpoint ).then( function ( response ) {
		return response.json();
	} );
}
```

```js
/**
 * Returns a connection status message.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 * @since July 18, 2026 Added boolean parameter guards.
 *
 * @param {boolean} isOffline Whether the connection is offline.
 * @param {boolean} hasError Whether the last request failed.
 * @return {string} Connection status message.
 */
function getConnectionMessage( isOffline, hasError ) {

	if ( 'boolean' !== typeof isOffline || 'boolean' !== typeof hasError ) {
		return ``;
	}

	return ( isOffline || hasError )
		? `The service is temporarily unavailable.`
		: `The service is ready.`;
}
```

```js
/**
 * Reveals a panel when its controls are usable.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 * @since July 18, 2026 Added element guards and synchronized expanded state.
 *
 * @param {HTMLButtonElement} trigger Panel trigger.
 * @param {HTMLElement} panel Panel element.
 */
function revealPanel( trigger, panel ) {

	if ( false === trigger instanceof HTMLButtonElement ) {
		return;
	}

	if ( false === panel instanceof HTMLElement ) {
		return;
	}

	if ( `true` === trigger.getAttribute( `aria-expanded` ) ) {
		return;
	}

	panel.hidden = false;
	trigger.setAttribute( `aria-expanded`, `${true}` );
	trigger.focus();
}
```

```js
/**
 * Binds keyboard behavior to a dismissible panel.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 * @since July 18, 2026 Added a concise HTMLElement guard before using the panel.
 *
 * @param {HTMLElement} panel Panel element.
 */
function bindDismissiblePanel( panel ) {

	// Ignore values that are not DOM elements before registering a listener.
	if ( false === panel instanceof HTMLElement ) {
		return;
	}

	panel.addEventListener( `keydown`, function ( event ) {

		if ( event.key !== `Escape` ) {
			return;
		}

		panel.hidden = true;
	} );
}
```

```js
/**
 * Builds a query string for the search endpoint.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 * @since July 18, 2026 Added a filter object guard.
 *
 * @param {object} filters Search filters.
 * @return {string} Encoded query string.
 */
function buildSearchQuery( filters ) {

	if ( null === filters || 'object' !== typeof filters ) {
		return ``;
	}

	return new URLSearchParams( {
		phrase: filters.phrase,
		owner: filters.owner,
		state: filters.state,
	} ).toString();
}
```

```js
/**
 * Sends a summary metric to the renderer.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 * @since July 18, 2026 Added practical parameter type guards.
 *
 * @param {HTMLElement} root Summary root.
 * @param {string} label Metric label.
 * @param {number} value Metric value.
 */
function renderSummaryMetric( root, label, value ) {

	if ( false === root instanceof HTMLElement ) {
		return;
	}

	if ( 'string' !== typeof label ) {
		return;
	}

	if ( 'number' !== typeof value ) {
		return;
	}

	renderMetric(
		root,
		label,
		value
	);
}
```

```js
// Keep the workflow order aligned with its lifecycle.
const workflow = {
	queued: queueWorkflow,
	running: runWorkflow,
	complete: finishWorkflow,
};
```

## Combined PHP and HTML

```php
<?php if ( $profile['show_contact'] ) : ?>

	<!-- Keep contact values in a definition list so labels remain associated. -->

	<section aria-labelledby="profile-contact-title">

		<h2 id="profile-contact-title">Contact details</h2>

		<dl>

			<div>
				<dt>Email</dt>
				<dd><?php echo esc_html( $profile['email'] ); ?></dd>
			</div>

			<div>
				<dt>Phone</dt>
				<dd><?php echo esc_html( $profile['phone'] ); ?></dd>
			</div>
		</dl>
	</section>

<?php endif; ?>
```

## Combined HTML, CSS, and JavaScript

```html
<button
	aria-controls="command-palette"
	aria-expanded="false"
	id="command-palette-trigger"
	type="button">
		Open command palette
</button>

<div
	aria-labelledby="command-palette-title"
	aria-modal="true"
	hidden
	id="command-palette"
	role="dialog">

		<h2 id="command-palette-title">Command palette</h2>

		<button id="command-palette-close" type="button">Close command palette</button>

		<input aria-label="Search commands" id="command-palette-search" type="search">
</div>
```

```css
.command_palette {

	background-color: #111827;
	color: #f9fafb;

	border: 1px solid #374151;
	padding: 1rem;
}

.command_palette[hidden] {
	display: none;
}
```

```js
/**
 * Toggles the command palette and synchronizes its accessible state.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 * @since July 18, 2026 Synchronized palette state and focus behavior.
 *
 * @param {HTMLButtonElement} trigger Palette trigger.
 * @param {HTMLElement} palette Palette element.
 */
function toggleCommandPalette( trigger, palette ) {

	if ( false === trigger instanceof HTMLButtonElement ) {
		return;
	}

	if ( false === palette instanceof HTMLElement ) {
		return;
	}

	palette.hidden = false === palette.hidden;
	trigger.setAttribute( `aria-expanded`, `${false === palette.hidden}` );

	if ( false === palette.hidden ) {

		// Focus the search field after the palette opens.
		const searchInput = palette.querySelector( `#command-palette-search` );

		if ( searchInput instanceof HTMLInputElement ) {
			searchInput.focus();
		}

		return;
	}

	trigger.focus();
}
```

```js
/**
 * Binds command palette controls.
 *
 * @since Unknown
 * @since July 18, 2026 Updated canonical example to follow the current coding standards.
 * @since July 18, 2026 Added control element guards.
 *
 * @param {HTMLButtonElement} trigger Palette trigger.
 * @param {HTMLButtonElement} closeButton Palette close button.
 * @param {HTMLElement} palette Palette element.
 */
function bindCommandPalette( trigger, closeButton, palette ) {

	if ( false === trigger instanceof HTMLButtonElement ) {
		return;
	}

	if ( false === closeButton instanceof HTMLButtonElement ) {
		return;
	}

	if ( false === palette instanceof HTMLElement ) {
		return;
	}

	// Route both controls through the same state transition.
	trigger.addEventListener( `click`, function () {
		toggleCommandPalette( trigger, palette );
	} );

	closeButton.addEventListener( `click`, function () {
		toggleCommandPalette( trigger, palette );
	} );
}
```
