# Conversation-Derived Examples

These examples teach the user's preferred code shape. The bad examples are intentionally included so an agent can recognize and avoid the patterns.

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

## Strings and template literals

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

renderUserName(userName);
```

Good: pass the source value directly.

```js
renderUserName(user.profile.displayName);
```

Bad: aliases before a call only make the reader track additional names.

```js
function submitForm( form, settings ) {
	const endpoint = settings.api.baseUrl;
	const formData = new FormData(form);
	const requestUrl = `${endpoint}/forms/${settings.formId}`;

	return fetch(requestUrl, {
		method: `POST`,
		body: formData,
	});
}
```

Good: pass direct values and constructed values where they are needed.

```js
function submitForm( form, settings ) {
	return fetch(`${settings.api.baseUrl}/forms/${settings.formId}`, {
		method: `POST`,
		body: new FormData(form),
	});
}
```

Bad: a convenience alias does not improve this call.

```js
function updateCard( card, data ) {
	const cardElement = card.element;

	replaceCardContent(cardElement, data.content);
}
```

Good: use direct access. If context needs explanation, add a comment instead of an alias.

```js
function updateCard( card, data ) {
	// Replace the existing card content in the card element.
	replaceCardContent(card.element, data.content);
}
```

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

Bad: a computation is repeated unnecessarily.

```js
function renderUser( user ) {
	return {
		name: getDisplayName(user),
		label: `User: ${getDisplayName(user)}`,
		ariaLabel: getDisplayName(user),
	};
}
```

Good: store the computed result because it is reused.

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

Bad: a fetch or other expensive task is repeated while constructing an object.

```js
function buildProfile( userId ) {
	return {
		profile: fetchProfile(userId),
		permissions: getPermissions(fetchProfile(userId)),
		summary: createSummary(fetchProfile(userId)),
	};
}
```

Good: perform the work once and reuse its result.

```js
function buildProfile( userId ) {
	const profile = fetchProfile(userId);

	return {
		profile,
		permissions: getPermissions(profile),
		summary: createSummary(profile),
	};
}
```

## Function-call layout

Good: one or two parameters may remain inline.

```js
replaceCardContent(card.element, data.content);
```

Good: more than two parameters are split across lines. The trailing comma is used only when the target language and version support it.

```js
createCard(
	card.element,
	data.content,
	data.heading,
	data.status,
);
```

If support is not confirmed, omit the trailing comma rather than introducing unsupported syntax.

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
 * @param string $message Message to log.
 */
function log_admin_message( $message ) {
	error_log( $message );
}
```
