# Good Examples

These examples are the canonical positive reference for Aubrey Portwood's coding
standards. They are intentionally distinct from the examples in the WordPress
snapshots and the conversation history.

Verify the target language and version before using version-sensitive syntax.
These examples do not use trailing commas in function calls.

## CSS

```css
.account_panel {

	display: grid;
	gap: 1rem;
	grid-template-columns: minmax(0, 1fr) auto;

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

	.account_panel__details {

		display: grid;
		grid-template-columns: repeat(2, minmax(0, 1fr));
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
	margin: 0;
}
```

```css
@media (prefers-reduced-motion: reduce) {

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

	box-shadow: 0 0.25rem 1rem rgb(23 32 51 / 12%);
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
	grid-template-columns: 8rem minmax(0, 1fr);

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

## HTML

```html
<main id="notification-settings" aria-labelledby="notification-settings-title">
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
		<li>
			<a aria-current="page" href="/overview">Overview</a>
		</li>
		<li>
			<a href="/activity">Activity</a>
		</li>
		<li>
			<a href="/members">Members</a>
		</li>
	</ul>
</nav>
```

```html
<dialog aria-labelledby="archive-dialog-title" id="archive-dialog">
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
 *
 * @param array{series: string, number: int} $invoice Invoice data.
 * @return string Formatted invoice reference.
 */
function get_invoice_reference( array $invoice ): string {
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
 *
 * @param array<int, array{code: string, enabled: bool}> $regions Region records.
 * @return array<int, array{code: string, enabled: bool}> Enabled region records.
 */
function get_enabled_regions( array $regions ): array {
	return array_values(
		array_filter(
			$regions,
			static function ( array $region ): bool {
				return $region['enabled'];
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
 *
 * @param array{code: string, name: string} $customer Customer data.
 * @return string Customer label.
 */
function get_customer_label( array $customer ): string {
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
 *
 * @param int $account_id Account ID.
 * @return array{
 *     account: array{id: int, contact: array{email: string}},
 *     contact: array{email: string},
 *     refreshed_at: int
 * } Synchronization payload.
 */
function build_sync_payload( int $account_id ): array {

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
 *
 * @param string $event Event name.
 * @param array{source: string, request_id: string} $context Event context.
 * @param int $actor_id Actor ID.
 */
function write_audit_event( string $event, array $context, int $actor_id ): void {
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
 */
function register_invoice_status_filter(): void {
	add_filter( 'invoice_status_label', 'format_invoice_status_label' );
}

/**
 * Formats an invoice status label.
 *
 * @since Unknown
 *
 * @param string $status Invoice status.
 * @return string Formatted invoice status.
 */
function format_invoice_status_label( string $status ): string {
	return match ( $status ) {
		'paid' => 'Paid in full',
		'pending' => 'Awaiting payment',
		'refunded' => 'Refunded',
		default => 'Status unavailable',
	};
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
 * @typedef {Object} AlertRecord
 * @property {string} id Alert identifier.
 * @property {string} message Alert message.
 * @property {`critical`|`info`} severity Alert severity.
 */

/**
 * @typedef {Object} AlertItem
 * @property {string} id Alert identifier.
 * @property {string} message Alert message.
 * @property {`critical`|`info`} severity Alert severity.
 * @property {`assertive`|`polite`} ariaLive Live-region politeness.
 */

/**
 * Builds an accessible alert item.
 *
 * @since Unknown
 *
 * @param {AlertRecord} alert Alert data.
 * @return {AlertItem} Alert item with live-region metadata.
 */
function buildAlertItem( alert ) {
	return {
		...alert,
		ariaLive: alert.severity === `critical` ? `assertive` : `polite`,
	};
}
```

```js
/**
 * @typedef {Object} TaskRecord
 * @property {string} id Task identifier.
 * @property {string} dueDate Due date in ISO format.
 * @property {string} title Task title.
 */

/**
 * Groups tasks by their calendar date.
 *
 * @since Unknown
 *
 * @param {TaskRecord[]} tasks Task records.
 * @return {Record<string, TaskRecord[]>} Tasks keyed by calendar date.
 */
function groupTasksByDay( tasks ) {
	return tasks.reduce( function ( groupedTasks, task ) {
		const day = task.dueDate.slice( 0, 10 );

		if ( ! groupedTasks[ day ] ) {
			groupedTasks[ day ] = [];
		}

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
 *
 * @param {Array<{ id: string, priority: number }>} entries Queue entries.
 * @return {Array<{ id: string, priority: number }>} Sorted queue entries.
 */
function sortQueueEntries( entries ) {
	return entries.slice().sort( function ( firstEntry, secondEntry ) {
		return firstEntry.priority - secondEntry.priority;
	} );
}
```

```js
/**
 * @typedef {Object} DashboardData
 * @property {string} title Dashboard title.
 * @property {number} activeUsers Active user count.
 * @property {number} pendingJobs Pending job count.
 */

/**
 * Loads dashboard data.
 *
 * @since Unknown
 *
 * @param {string} endpoint Dashboard endpoint.
 * @return {Promise<DashboardData>} Dashboard data.
 */
async function loadDashboardData( endpoint ) {
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
 *
 * @param {boolean} isOffline Whether the connection is offline.
 * @param {boolean} hasError Whether the last request failed.
 * @return {string} Connection status message.
 */
function getConnectionMessage( isOffline, hasError ) {
	return ( isOffline || hasError )
		? `The service is temporarily unavailable.`
		: `The service is ready.`;
}
```

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
		if ( event.key !== `Escape` ) {
			return;
		}

		panel.hidden = true;
	} );
}
```

```js
/**
 * @typedef {Object} SearchFilters
 * @property {string} phrase Search phrase.
 * @property {string} owner Owner identifier.
 * @property {string} state Search state.
 */

/**
 * Builds a query string for the search endpoint.
 *
 * @since Unknown
 *
 * @param {SearchFilters} filters Search filters.
 * @return {string} Encoded query string.
 */
function buildSearchQuery( filters ) {
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
 *
 * @param {HTMLElement} root Summary root.
 * @param {string} label Metric label.
 * @param {number} value Metric value.
 */
function renderSummaryMetric( root, label, value ) {
	renderMetric(
		root,
		label,
		value
	);
}
```

```js
const workflow = {
	queued: queueWorkflow,
	running: runWorkflow,
	complete: finishWorkflow,
};
```

## Combined PHP and HTML

```php
<?php if ( $profile['show_contact'] ) : ?>

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
	type="button"
>
	Open command palette
</button>

<div
	aria-labelledby="command-palette-title"
	aria-modal="true"
	hidden
	id="command-palette"
	role="dialog"
>
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

```js
/**
 * Binds command palette controls.
 *
 * @since Unknown
 *
 * @param {HTMLButtonElement} trigger Palette trigger.
 * @param {HTMLButtonElement} closeButton Palette close button.
 * @param {HTMLElement} palette Palette element.
 */
function bindCommandPalette( trigger, closeButton, palette ) {

	trigger.addEventListener( `click`, function () {
		toggleCommandPalette( trigger, palette );
	} );

	closeButton.addEventListener( `click`, function () {
		toggleCommandPalette( trigger, palette );
	} );
}
```
