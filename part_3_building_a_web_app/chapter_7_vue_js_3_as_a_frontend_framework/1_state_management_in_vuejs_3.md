## 7.2 State Management in Vue.js 3

Vue.js 3 provides 6 essential state-management techniques/patterns that must be used in combination in order to unlock the full potential of the framework.

Each state-management technique/pattern must be carefully used where it mostly makes sense.

Assuming the usage of the **composition-api**, those 6 state-management techniques are:

1. the local state technique (using ref and reactive functions)
2. the props + events technique (props down + events up)
3. the provide + inject technique
4. the shared state store technique (with pinia)
5. the using composables technique 
6. the localStorage technique (to persist state in the user's browser)

### 1. **Local State**: 


This is the simplest form of state management in Vue.js. Each component has its own local state, managed with `ref` and `reactive` functions. The state is not shared with other components unless passed explicitly.

{% hint type = "tip" %}

Only use this technique if you want to manage state in one component locally.

{% endhint %}

```js
<script setup>
import { ref } from 'vue';
const count = ref(0);
const increment = () => { count.value++ };
</script>
```

### 2. **Props + Events**: 

This is a common pattern for parent-child component communication. The parent passes data to the child via props, and the child communicates changes to the parent via events.

### 3. **Provide + Inject**: 

This is a way to pass data from a parent component to descendant components through a dependency injection mechanism, without going through intermediate components. This can be useful for avoiding prop drilling.

### 4. **Shared State Store (Pinia)**: 

Pinia is a state management library for Vue.js that provides a centralized store for shared state. It's an alternative to Vuex and is designed to be more intuitive and flexible.

### 5. **Using Composables**: 

Composables are reusable logic functions in Vue.js Composition API. They can be used to share stateful logic between components.

### 6. **LocalStorage**: 

This is a way to persist state across browser sessions by storing it in the user's browser. This can be useful for things like remembering a user's preferences or saving the state of a form.

