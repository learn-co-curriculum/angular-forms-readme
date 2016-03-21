# Built-in form validation methods

## Overview

You might've been thinking that Angular didn't have much more to offer - you were wrong! Angular helps us out massively with a quite common issue a lot of developers have to deal with - forms!

## Objectives

- Describe Angular form validation and states
- Write a submit function
- Create multiple inputs with different validation types
- Display state Objects in the View

## Angular Forms

We're already two-way binding our input values to our controller via `ng-model`, but Angular allows us to take our form integration much, much further.

When we assign a `name` to our form and input elements, Angular begins its magic and gives us access to a lot of useful tools to validate the user input. Why do we validate? So we don't get incorrect data! Whilst we would still validate the data on the server, we can save the user time (and server resources) by validating on the client first. Imagine if we could signup to Facebook with just numbers in our name - it wouldn't make any sense. Angular makes it easy for us to prevent the user from making these types of requests. 

Let's take this example form:

```html
<form>
	<input ng-model="ctrl.username" />
	<input ng-model="ctrl.password" />
</form>
```

And add name elements to our `<form>` and `<input />`s.

```html
<form name="form">
	<input
		name="username"
		ng-model="ctrl.username" />
	<input
        name="password"
        ng-model="ctrl.password" />
</form>
```

We now get quite an awesome object added to our `$scope`:

```js
$scope.form = {
	username: {
		$untouched: true,
		$touched: false,
		$pristine: true,
		$dirty: false,
		$valid: true,
		$invalid: false,
		$error: {}
	},
	password: {
        $untouched: true,
        $touched: false,
        $pristine: true,
        $dirty: false,
        $valid: true,
        $invalid: false,
        $error: {}
    }
};
```

As you can see we get quite a big, detailed object back. What do all of these mean?

- `$untouched` - this is initially `true`, set to `false` when the user goes in and then out of the input
- `$touched` - opposite of `$untouched`
- `$pristine` - this is initially `true`, set to `false` when the user changes the input's value (the user does not have to go out of the input for this to change)
- `$dirty` - opposite of `$pristine`
- `$valid` - if the field's value matches all validation rules applied to it
- `$invalid` - opposite of `$valid`
- `$error` - an object of all errors that the input is currently failing on

Whilst `$pristine` and `$touched` offer us some cool features, the core of the power is in the `$error` and `$valid` items.

By default, inputs in HTML allow us to set validation rules on them - for instance, required and maxlength.

Angular will check all of these, and let us know in our object whether or not the input passes these validation rules - this allows us to show messages if the user hasn't entered the correct information.

If we have this form:

```html
<form name="form">
	<input
		name="username"
		required="required"
		ng-model="ctrl.username" />
</form>
```

We will get this object:

```js
$scope.form = {
	username: {
		$untouched: true,
		$touched: false,
		$pristine: true,
		$dirty: false,
		$valid: false,
		$invalid: true,
		$error: {
			required: true
		}
	}
};
```

You can see here that we have an error in our input - it's required, and not filled in! We can see all the individual errors that the input fails on in `$error`, but also get a general value on whether or not our field is valid by looking at `$valid`.

Let's display the error in the view, if the user exists our input and still leaves it blank. We can do this by combining the `$touched` and `$error.required` values.

```html
<form name="form">
	<input
		name="username"
		required="required"
		ng-model="ctrl.username" />

	<div ng-if="form.username.$touched && form.username.$error.required">
		Username is required!
	</div>
</form>
```

This will display our error if the user goes into our input and then leaves it blank. This is better user experience because it means the error won't immediately be shown when the user loads the page. Simple!
