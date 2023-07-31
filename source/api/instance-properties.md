title: Instance Properties
type: api
order: 3
---

### vm.$el

- **Type:** `HTMLElement`
- **Read only**

The DOM element that the ViewModel is managing.

### vm.$data

- **Type:** `Object`

The data object that the ViewModel is observing. You can swap it with a new object. The ViewModel proxies access to the properties on its data object.

### vm.$options

- **Type:** `Object`

The instantiation options used for the current ViewModel. This is useful when you want to include custom properties in the options:

``` js
new Vue({
    customOption: 'foo',
    created: function () {
        console.log(this.$options.customOption) // 'foo'
    }
})
```

### vm.$

- **Type:** `Object`
- **Read only**

An object that holds child ViewModels that have `v-ref` registered. For more details see [v-ref](/api/directives.html#v-ref).

### vm.$index

- **Type:** `Number`

The Array index of a child ViewModel created by `v-repeat`. For more details see [v-repeat](/api/directives.html#v-repeat).

### vm.$parent

- **Type:** `Object ViewModel`
- **Read only**

The parent ViewModel, if the current ViewModel has one.

### vm.$root

- **Type:** `Object ViewModel`
- **Read only**

The root ViewModel. If the current ViewModel has no parents this value will be itself.

### vm.$compiler

- **Type:** `Object Compiler`
- **Read only**

The Compiler instance of the ViewModel. This object maintains some internal state of the ViewModel such as data bindings and in most cases you should avoid touching it.