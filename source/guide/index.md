title: Introduction
type: guide
order: 2
---

Vue.js (pronounced /vjuː/, like **view**) is a library for building interactive web interfaces. The goal of Vue.js is to provide the benefits of **reactive data binding** and **composable view components** with an API that is as simple as possible.

Vue.js itself is not a full-blown framework - it is focused on the view layer only. It is therefore very easy to pick up and to integrate with other libraries or existing projects. On the other hand, when used in combination with proper tooling and supporting libraries, Vue.js is also perfectly capable of powering sophisticated Single-Page Applications.

If you are an experienced frontend developer and want to know how Vue.js compares to other libraries/frameworks, check out the [Comparison with Other Frameworks](comparison.html); if you are more interested about how Vue.js approaches larger-scale applications, check out the section on [Building Larger-Scale Applications](application.html).

## Reactive Data Binding

At the core of Vue.js is a reactive data-binding system that makes it extremely simple to keep your data and the DOM in sync. When using jQuery to manually manipulate the DOM, the code we write is often imperative, repetitive and error-prone. Vue.js embraces the concept of **data-driven view**. In plain words, it means we use special syntax in our normal HTML templates to "bind" the DOM to the underlying data. Once the bindings are created, the DOM will then be kept in sync with the data. Whenever you modify the data, the DOM updates accordingly. As a result, most of our application logic is now directly manipulating data, rather than messing around with DOM updates. This makes our code easier to write, easier to reason about and easier to maintain.

![MVVM](/images/mvvm.png)

For the simplest possible example:

``` html
<!-- this is our View -->
<div id="example-1">
  Hello {{ name }}!
</div>
```

``` js
// this is our Model
var exampleData = {
  name: 'Vue.js'
}

// create a Vue instance, or, a "ViewModel"
// which links the View and the Model
var exampleVM = new Vue({
  el: '#example-1',
  data: data
})
```

Result:
{% raw %}
<div id="example-1" class="demo">Hello {{ name }}!</div>
<script>
var exampleData = {
  name: 'Vue.js'
}
var exampleVM = new Vue({
  el: '#example-1',
  data: exampleData
})
</script>
{% endraw %}

This looks pretty similar to just rendering a template, but Vue.js has done a lot of work under the hood. The data and the DOM are now linked, and everything is now **reactive**. How do we know? Just open up your browser developer console and modify `exampleData.name`. You should see the rendered example above update accordingly.

Note that we didn't have to write any DOM-manipulating code: the HTML template, enhanced with the bindings, is a declarative mapping of the underlying data state, which is in turn just plain JavaScript objects. Our view is entirely data-driven.

Let's look at a second example:

``` html
<div id="example-2">
  <p v-if="greeting">Hello!</p>
</div>
```

``` js
var exampleVM2 = new Vue({
  el: '#example-2',
  data: {
    greeting: true
  }
})
```

{% raw %}
<div id="example-2" class="demo">
  <span v-if="greeting">Hello!</span>
</div>
<script>
var exampleVM2 = new Vue({
  el: '#example-2',
  data: {
    greeting: true
  }
})
</script>
{% endraw %}

Here we are encoutering something new. The `v-if` attribute you are seeing are called **Directives**. Directives are prefixed with `v-` to indicate that they are special attributes provided by Vue.js, and as you may have guessed, they apply special reactive behavior to the rendered DOM. Go ahead and set `exampleVM2.greeting` to `false` in the console. You should see the "Hello!" message disappear.

This second example demonstrates that not only can we bind DOM text to the data, we can also bind the **structure** of the DOM to the data. Moreover, Vue.js also provides a powerful transition effect system that can automatically apply transition effects when elements are inserted/removed by Vue.

There are quite a few other directives, each with its own special functionality. For example the `v-for` directive for displaying items in an Array, or the `v-bind` directive for binding HTML attributes. We will discuss the full data-binding syntax with more details later.

## Component System

The Component System is another important concept in Vue.js, becaues it's an abstraction that allows us to build large-scale applications composed of small, self-contained, and often reusable components. If we think about it, almost any type of application interface can be abstracted into a tree of components:

![Component Tree](/images/components.png)

In fact, a typical large application built with Vue.js would form exactly what is on the right - a tree of components. We will talk a lot more about components later in the guide, but here's an (imaginary) example of what an app's template would look like with components:

``` html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

You may have noticed that Vue.js components are very similar to **Custom Elements**, which is part of the [Web Components Spec](http://www.w3.org/wiki/WebComponents/). In fact, Vue.js' component syntax is loosely modeled after the spec. For example, Vue components implement the [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) and the `is` special attribute. However, there are a few key differences:

1. The Web Components Spec is still very much a work in progress, and is not natively implemented in every browser. In comparison, Vue.js components don't require any polyfills and works consistently in all supported browsers (IE9 and above). When needed, Vue.js components can also be wrapped inside a native custom element.

2. Vue.js components provide important features that are not available in plain custom elements, most notably cross-component data flow, custom event communication and dynamic component switching with transition effects.

The component system is the foundation for building large apps with Vue.js. In addition, the Vue.js ecosystem also provides advanced tooling and various supporting libraries that can be put together to create a more "framework" like system.

We have briefly introduced the two most important concepts in Vue.js, and will obviously cover more details on each topic in later sections of this guide. But for now, let's go back to ground zero and start with the basics: [The Vue Instance](instance.html).
