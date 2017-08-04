# AngularJS

# Directives

Directives are a functionnality of Angular called directly from the DOM.

When you want to create a small component using a controller, you need to create the component using HTML, then create a controller for this component, and finally bind the two together.

The HTML code for the component then looks like this:

```js
<div ng-controller="testController" ng-init="value = val; etc = etc">
	Your component
</div>
```

Directives, on the other hand, act as the creation of new HTML elements.

When you create your component using a directive, you just call it by its name as if it was a HTML element, and you bind its values using HTML attributes.

For example, let's say we create a directive named `progress-bar`, which creates an HTML progress bar, and takes two values as parameters: `value` and `max-value`.

```js
angular
	.module('testmodule')
	.directive('progressBar', progressBar);

	progressBar.$inject = [DEPENDENCIES];

	// We define the directive in a separate function called by .directive(), but you can also directly declare it like this:
	// .directive('progressBar', function(DEPENDENCIES) { // Your code });
	function progressBar(DEPENDENCIES) {
		return {
			replace: true,
			scope: {
				value: '=',
				maxValue: '='
			},
			template: `<div class="progress-bar">
							</div class="progress" style="width: {{ value }}"></div>
						</div>`,
			// You can also use this if your template is defined in a separate HTML document:
			// templateUrl: 'PATH_TO_TEMPLATE.html'
			link: function($scope, elm, $attrs) {
			}
		};
	}

```

To create the component, we just call it like if it was an HTML element, and we bind the variables using HTML attributes.

```js
<test-directive value="5" max-value="25"></test-directive>
```

Directives are not meant to be used for big components, it's better to use a Controller for this, but they are perfect when you just need to add some functionnality to an element, that you want to use at multiple locations in your code.


### TODO

## Tips and tricks

To see the current scope of an element on Google Chrome, inspect the element using the Chrome Developer Tools and type this in the console:
```js
angular.element($0).scope()
```

To stop a click event from propagating to the parent elements, use `$event.stropPropagation();`:

```html
<div ng-click="a()">
	<button ng-click="b(); $event.stopPropagation();">
		When we click on the button it doesn't trigger a click on the div so we only call b()
	</button>
	When we click on the div we call a()
</div>
```
