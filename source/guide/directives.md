title: Directives
type: guide
order: 3
---

## Synopsis

If you have not used AngularJS before, you probably don't know what a directive is. Essentially, a directive is some special token in the markup that tells the library to do something to a DOM element. In Vue.js, the concept of directive is drastically simpler than that in Angular. A Vue.js directive can only appear in the form of a prefixed HTML attribute that takes the following format:

``` html
<element
  prefix-directiveId="[argument:] expression [| filters...]">
</element>
```

## A Simple Example

``` html
<div v-text="message"></div>
```

Here the prefix is `v` which is the default. The directive ID is `text` and the expression is `message`. This directive instructs Vue.js to update the div's `textContent` whenever the `message` property on the Vue instance changes.

## Inline Expressions

``` html
<div v-text="'hello ' + user.firstName + ' ' + user.lastName"></div>
```

Here we are using a computed expression instead of a single property key. Vue.js automatically tracks the properties an expression depends on and refreshes the directive whenever a dependency changes. Thanks to async batch updates, even when multiple dependencies change, an expression will only be updated once every event loop.

You should use expressions wisely and avoid putting too much logic in your templates, especially statements with side effects (with the exception of event listener expressions). To discourage the overuse of logic inside templates, Vue.js inline expressions are limited to **one statement only**. For bindings that require more complicated operations, use [Computed Properties](/guide/computed.html) instead.

<p class="tip">For security reasons, in inline expressions you can only access properties and methods present on the current context Vue instance and its parents.</p>

## Argument

``` html
<div v-on="click : clickHandler"></div>
```

Some directives require an argument before the keypath or expression. In this example the `click` argument indicates we want the `v-on` directive to listen for a click event and then call the `clickHandler` method of the ViewModel instance.

## Filters

Filters can be appended to directive keypaths or expressions to further process the value before updating the DOM. Filters are denoted by a single pipe (`|`) as in shell scripts. For more details see [Filters in Depth](/guide/filters.html).

## Multiple Clauses

You can create multiple bindings of the same directive in a single attribute, separated by commas. Under the hood they are bound as multiple directive instances.

``` html
<div v-on="
  click   : onClick,
  keyup   : onKeyup,
  keydown : onKeydown
">
</div>
```

## Literal Directives

Some directives don't create data bindings - they simply take the attribute value as a literal string. For example the `v-ref` directive:

``` html
<my-component v-ref="some-string-id"></my-component>
```

Here `"some-string-id"` is not a reactive expression - Vue.js will not attempt to look it up in the component's data.

In some cases you can also use the mustache syntax to make a literal directive reactive:

``` html
<div v-show="showMsg" v-transition="{{dynamicTransitionId}}"></div>
```

However, note that `v-transition` is the only directive that has this feature. Mustache expressions in other literal directives, e.g. `v-ref` and `v-el`, are evaluated **only once**. After the directive has been compiled, it will no longer react to value changes.

A full list of literal directives can be found in the [API reference](/api/directives.html#Literal_Directives).

## Empty Directives

Some directives don't even expect an attribute value - they simply do something to the element once and only once. For example the `v-pre` directive:

``` html
<div v-pre>
  <!-- markup in here will not be compiled -->
</div>
```

A full list of empty directives can be found in the [API reference](/api/directives.html#Empty_Directives).

## Understanding Async Updates

You can think of directives as mappings of your data state to the DOM state. However, it is important to understand that in Vue.js, the directive update process is asynchronous by default. For example, when you set `vm.someData = 'new value'`, the DOM will not update immediately. Vue.js buffers all data changes happening in the same event loop, and execute any necessary DOM updates asynchronously in the next "tick". Internally it uses `MutationObserver` if available and falls back to `setTimeout(fn, 0)`. This prevents multiple changes to the same piece of data from triggering duplicate updates.

This behavior can be tricky when you want to do something that depends on the updated DOM state. Although Vue.js generally encourages developers to think in a "data-driven" way and avoid touching the DOM directly, sometimes you might just want to use that handy jQuery plugin you've always been using. In order to wait until Vue.js has finished updating the DOM after a data change, you can use `Vue.nextTick(callback)` immediately after the data is changed - when the callback is called, the DOM would have been updated. For example:

``` html
<div id="example">{{msg}}</div>
```

``` js
var vm = new Vue({
  el: '#example',
  data: {
    msg: '123'
  }
})
vm.msg = 'new message' // change data
vm.$el.textContent === 'new message' // false
Vue.nextTick(function () {
  vm.$el.textContent === 'new message' // true
})
```

Alternatively, you can turn off async updates by setting `Vue.config.async = false`.

Next, let's talk about [Filters](/guide/filters.html).
