# Conversation-Derived Examples

The canonical example files are [`good-examples.md`](good-examples.md) and [`bad-examples.md`](bad-examples.md). This file preserves earlier conversation-derived examples as supporting history. Read the canonical files first.

These historical examples teach the user's preferred code shape. The bad examples are intentionally included so an agent can recognize and avoid the patterns. Every function example includes documentation unless the missing documentation is the behavior being demonstrated.

The JavaScript examples assume a target that supports the syntax shown. When writing real project code, verify support for `const`, template literals, arrow functions, and trailing commas in function calls before using them.

## CSS grouping and contextual breathing room

Bad: properties are not grouped by function, and the block has no visual separation.

```css
#overlay {
	background: #fff;
	padding: 10px;
	position: absolute;
	color: #777;
	z-index: 1;
}
```

Good: related properties are grouped first, then alphabetized within each group. The blank line after the opening brace separates meaningful groups.

```css
#overlay {

	background: #fff;
	color: #777;

	padding: 10px;
	position: absolute;
	z-index: 1;
}
```

Good: a trivial one-property block stays compact.

```css
#overlay {
	background: #fff;
}
```

Good: a parent at-rule containing multiple child blocks has breathing room before the first child and between siblings, without a trailing blank line before the parent closes.

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

	.orbit-card__eyebrow {

		color: CanvasText;
	}
}
```

## HTML and PHP alternative syntax

Bad: PHP is used as a brace-based code island around HTML, with unnecessary PHP indentation and XHTML-style HTML syntax.

```php
<?php

	if ( $visible ) {
		echo '<p>Visible</p>';
	}

?>
<img src="image.jpg" />
```

Good: use PHP alternative syntax for template control, indent the HTML in context, and do not self-close HTML elements.

```php
<?php if ( $visible ) : ?>
	<p>Visible</p>
<?php endif; ?>

<img src="image.jpg">
```

Good: when a standalone PHP opening tag is followed by a DocBlock, leave a blank line after the tag while keeping the DocBlock directly adjacent to the declaration.

```php
<?php

/**
 * Encodes binary data as unpadded URL-safe Base64.
 *
 * @since Unknown
 *
 * @param string $value Binary data.
 * @return string URL-safe Base64 data.
 */
function constellation_base64url_encode( string $value ) : string {

	return rtrim(
		strtr(
			base64_encode( $value ),
			'+/',
			'-_'
		),
		'='
	);
}
```

## JavaScript object and array shape

Good: a very small object may remain compact.

```js
const point = { x: 10, y: 20 };
```

Good: a larger object puts each property on its own line and keeps intentional order.

```js
const settings = {
	animation: true,
	duration: 300,
	placement: `top`,
	visible: false,
};
```

Bad: do not alphabetize an object when its order is intentional.

```js
const menu = {
	open: openMenu,
	load: loadMenu,
	close: closeMenu,
};
```

The lifecycle order is intentional, so preserve it.

## Strings, interpolation, and concatenation

Avoid string concatenation in every supported language when native interpolation, templating, formatting, or parameterization is available.

Bad: JavaScript string concatenation creates unnecessary quote and spacing decisions.

```js
const message = 'Hello, ' + name + '!';
```

Bad: escaping an apostrophe is avoidable when a template literal expresses the string clearly.

```js
const message = 'I can\'t save this file.';
```

Good: use a template literal by default for JavaScript strings.

```js
const message = `Hello, ${name}!`;
const warning = `I can't save this file.`;
```

Bad: PHP concatenation is unnecessary when interpolation expresses the value clearly.

```php
$message = 'Hello, ' . $name . '!';
```

Good:

```php
$message = "Hello, {$name}!";
```

If a language or target version provides no viable interpolation, templating, formatting, or parameterized-string alternative, concatenation may be used only as a documented compatibility constraint.

## Variable declarations and single-use aliases

Bad: comma-separated declarations make variables harder to reorder.

```js
const firstName = `Ada`, lastName = `Lovelace`, fullName = `${firstName} ${lastName}`;
```

Good: declare each variable separately.

```js
const firstName = `Ada`;
const lastName = `Lovelace`;
const fullName = `${firstName} ${lastName}`;
```

Bad: a single-use property alias adds no meaning.

```js
const userName = user.profile.displayName;

renderUserName( userName );
```

Good: pass the source value directly.

```js
renderUserName( user.profile.displayName );
```

Bad: aliases before a call only make the reader track additional names. The form examples use these explicit shapes:

```js
/**
 * @typedef {Object} SubmissionApiSettings
 * @property {string} baseUrl API base URL.
 */

/**
 * @typedef {Object} SubmissionSettings
 * @property {SubmissionApiSettings} api API settings.
 * @property {string} formId Form identifier.
 */
```

```js
/**
 * Submits a form.
 *
 * @since Unknown
 *
 * @param {HTMLFormElement} form Form to submit.
 * @param {SubmissionSettings} settings Submission settings.
 * @return {Promise<Response>} Form submission request.
 */
function submitForm( form, settings ) {

	const endpoint = settings.api.baseUrl;
	const formData = new FormData( form );
	const requestUrl = `${endpoint}/forms/${settings.formId}`;

	return fetch( requestUrl, {
		method: `POST`,
		body: formData,
	} );
}
```

Good: pass direct values and constructed values where they are needed.

```js
/**
 * Submits a form.
 *
 * @since Unknown
 *
 * @param {HTMLFormElement} form Form to submit.
 * @param {SubmissionSettings} settings Submission settings.
 * @return {Promise<Response>} Form submission request.
 */
function submitForm( form, settings ) {

	return fetch( `${settings.api.baseUrl}/forms/${settings.formId}`, {
		method: `POST`,
		body: new FormData( form ),
	} );
}
```

Bad: a convenience alias does not improve this call. The card examples use these explicit shapes:

```js
/**
 * @typedef {Object} CardData
 * @property {Element} element Card element.
 */

/**
 * @typedef {Object} UpdatedCardData
 * @property {string} content Updated card content.
 */
```

```js
/**
 * Updates a card.
 *
 * @since Unknown
 *
 * @param {CardData} card Card data.
 * @param {UpdatedCardData} data Updated card data.
 */
function updateCard( card, data ) {

	const cardElement = card.element;

	replaceCardContent( cardElement, data.content );
}
```

Good: use direct access. If context needs explanation, add a comment instead of an alias.

```js
/**
 * Updates a card.
 *
 * @since Unknown
 *
 * @param {CardData} card Card data.
 * @param {UpdatedCardData} data Updated card data.
 */
function updateCard( card, data ) {

	// Replace the existing card content in the card element.
	replaceCardContent( card.element, data.content );
}
```

## Pure one-use computations and exact JSDoc shapes

Bad: this function uses a simple one-use computation, a one-use filtered result, and a one-use ternary alias. Its documentation also hides the actual input and output shapes behind generic types.

```js
/**
 * Folds incoming signals into a short-lived orbit of attention.
 *
 * @since Unknown
 *
 * @param {Array<Object>} signals Signal records with a label and start timestamp.
 * @param {number} [now=Date.now()] Current time as a Unix timestamp in milliseconds.
 * @return {Array<Object>} Signals ordered by start time with display metadata.
 */
function foldBeaconSignals( signals, now = Date.now() ) {

	const horizon = now + 15 * 60 * 1000;
	const visibleSignals = signals
		.slice()
		.filter( function ( signal ) {
			return signal.startsAt >= now && signal.startsAt <= horizon;
		} )
		.sort( function ( firstSignal, secondSignal ) {
			return firstSignal.startsAt - secondSignal.startsAt;
		} );

	return visibleSignals.map( function ( signal, index ) {
		const minutesUntil = Math.ceil( ( signal.startsAt - now ) / 60000 );
		const urgency = minutesUntil <= 2 ? `flicker` : `steady`;

		return {
			...signal,
			ariaLabel: `${signal.label}; begins in ${minutesUntil} minutes; ${urgency}.`,
			priority: index + 1,
		};
	} );
}
```

Good: define the actual record shapes, inline pure one-use values, and document that the function accepts an array and returns an array of concrete records.

```js
/**
 * @typedef {Object} BeaconSignal
 * @property {string} label Signal label.
 * @property {number} startsAt Signal start timestamp in milliseconds.
 */

/**
 * @typedef {Object} FoldedBeaconSignal
 * @property {string} label Signal label.
 * @property {number} startsAt Signal start timestamp in milliseconds.
 * @property {string} ariaLabel Accessible signal label.
 * @property {number} priority Display priority.
 */

/**
 * Folds incoming signals into a short-lived orbit of attention.
 *
 * @since Unknown
 *
 * @param {BeaconSignal[]} signals Signal records with a label and start timestamp.
 * @param {number} [now=Date.now()] Current time as a Unix timestamp in milliseconds.
 * @return {FoldedBeaconSignal[]} Signals ordered by start time with display metadata.
 */
function foldBeaconSignals( signals, now = Date.now() ) {

	return signals
		.slice()
		.filter( function ( signal ) {
			return signal.startsAt >= now &&
				signal.startsAt <= now + 15 * 60 * 1000;
		} )
		.sort( function ( firstSignal, secondSignal ) {
			return firstSignal.startsAt - secondSignal.startsAt;
		} )
		.map( function ( signal, index ) {

			const minutesUntil = Math.ceil( ( signal.startsAt - now ) / 60000 );

			return {
				...signal,
				ariaLabel: `${signal.label}; begins in ${minutesUntil} minutes; ${
					minutesUntil <= 2
						? `flicker`
						: `steady`
				}.`,
				priority: index + 1,
			};
		} );
}
```

The `horizon`, `visibleSignals`, and `urgency` locals are not retained because each is a pure value used once. `minutesUntil` remains because it is used twice and prevents the calculation from being repeated.

## DRY: repeated literals versus repeated work

Repeating a simple literal may be acceptable when a variable would add no meaning:

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

Bad: a computation is repeated unnecessarily. The user examples use these explicit shapes:

```js
/**
 * @typedef {Object} UserData
 * @property {string} displayName User display name.
 */

/**
 * @typedef {Object} RenderedUserData
 * @property {string} name User name.
 * @property {string} label User label.
 * @property {string} ariaLabel Accessible user label.
 */
```

```js
/**
 * Renders a user.
 *
 * @since Unknown
 *
 * @param {UserData} user User data.
 * @return {RenderedUserData} Rendered user data.
 */
function renderUser( user ) {

	return {
		name: getDisplayName( user ),
		label: `User: ${getDisplayName( user )}`,
		ariaLabel: getDisplayName( user ),
	};
}
```

Good: store the computed result because it is reused.

```js
/**
 * Renders a user.
 *
 * @since Unknown
 *
 * @param {UserData} user User data.
 * @return {RenderedUserData} Rendered user data.
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

Bad: a fetch or other expensive task is repeated while constructing an object. These examples use explicit profile shapes:

```js
/**
 * @typedef {Object} UserProfile
 * @property {number} id User ID.
 * @property {string} name User name.
 */

/**
 * @typedef {Object} ProfileResult
 * @property {UserProfile} profile User profile.
 * @property {string[]} permissions User permissions.
 * @property {string} summary Profile summary.
 */
```

```js
/**
 * Builds a profile.
 *
 * @since Unknown
 *
 * @param {number} userId User ID.
 * @return {ProfileResult} Profile data.
 */
function buildProfile( userId ) {

	return {
		profile: fetchProfile( userId ),
		permissions: getPermissions( fetchProfile( userId ) ),
		summary: createSummary( fetchProfile( userId ) ),
	};
}
```

Good: perform the work once and reuse its result.

```js
/**
 * Builds a profile.
 *
 * @since Unknown
 *
 * @param {number} userId User ID.
 * @return {ProfileResult} Profile data.
 */
function buildProfile( userId ) {

	const profile = fetchProfile( userId );

	return {
		profile,
		permissions: getPermissions( profile ),
		summary: createSummary( profile ),
	};
}
```

## Function-call layout

Good: one or two parameters may remain inline.

```js
replaceCardContent( card.element, data.content );
```

Good: more than two parameters are split across lines. WordPress spacing is preserved inside the parentheses, and the trailing comma is used only when the target language and version support it.

```js
createCard(
	card.element,
	data.content,
	data.heading,
	data.status,
);
```

If support is not confirmed, omit the trailing comma rather than introducing unsupported syntax.

For an object argument, the closing brace and function parenthesis are separated:

```js
return fetch( requestUrl, {
	method: `POST`,
	body: formData,
} );
```

## Documented JavaScript interaction

This example demonstrates the required function-call spacing and documentation shape. Accessibility behavior should still be reviewed against the complete accessibility requirements of the project.

```js
/**
 * Initializes accordion controls.
 *
 * @since Unknown
 *
 * @param {Document|Element} root Root containing the accordion controls.
 */
function initAccordion( root ) {

	root.querySelectorAll( `[data-accordion-trigger]` ).forEach( ( trigger ) => {

		trigger.addEventListener( `click`, () => {
			const panel = document.getElementById( trigger.getAttribute( `aria-controls` ) );

			if ( ! panel ) {
				return;
			}

			const isExpanded = trigger.getAttribute( `aria-expanded` ) === `true`;

			trigger.setAttribute( `aria-expanded`, `${! isExpanded}` );
			panel.hidden = isExpanded;
		} );
	} );
}
```

## Ternaries

Good: a simple test does not need unnecessary parentheses.

```js
const label = value ? `Yes` : `No`;
```

Good: combined tests use parentheses for clarity.

```js
const label = ( isEnabled && isVisible ) ? `Show` : `Hide`;
```

Good: a long ternary puts the question mark and colon on continuation lines.

```js
const result = condition
	? valueWhenTrue
	: valueWhenFalse;
```

## PHP documentation

Good: document a return value when the function returns one.

```php
/**
 * Gets the user display name.
 *
 * @since Unknown
 *
 * @param int $user_id User ID.
 * @return string User display name.
 */
function get_user_display_name( $user_id ) {

	return get_the_author_meta( 'display_name', $user_id );
}
```

Good: omit `@return` when the function has no return value.

```php
/**
 * Logs an administrative message.
 *
 * @since Unknown
 *
 * @param string $message Message to log.
 */
function log_admin_message( $message ) {

	error_log( $message );
}
```
