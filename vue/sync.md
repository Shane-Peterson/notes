# .sync 修饰符

![eventBus](./images/eventbus.jpg)

在有些情况下，我们可能需要对一个 prop 进行“双向绑定”。不幸的是，真正的双向绑定会带来维护上的问题，因为子组件可以变更父组件，且在父组件和子组件两侧都没有明显的变更来源。

这也是为什么尤雨溪推荐以 update:myPropName 的模式触发事件取而代之。

**场景描述**

爸爸给儿子钱，儿子要花钱怎么办，

答：儿子打电话（触发事件）向爸爸要钱。

```html
<template>
  <div className="app">
    App.vue 我现在有 {{ total }}
    <hr/>
    <Child :money="total" v-on:update:money="total = $event"/>
  </div>
</template>

<script>
import Child from "./Child.vue"

export default {
  data() {
    return {total: 10000}
  },
  components: {Child}
}
</script>

<style>
.app {
  border: 3px solid red;
  padding: 10px;
}
</style>
```

```html
<template>
  <div class="child">
    {{ money }}
    <button @click="$emit('update:money', money-100)">
      <span>花钱</span>
    </button>
  </div>
</template>

<script>
export default {
  props: ["money"]
}
</script>

<style>
.child {
  border: 3px solid green
}
</style>
```

```jsx
import Vue from 'vue'
import App from './App.vue'

new Vue({
  render: h => h(App)
}).$mount('#app')
```

Vue 规则：组件不能修改 props 外部数据

Vue 规则：`$emit` 可以触发事件，并传参

Vue 规则：`$event` 可以获取 `$emit` 的参数

由于这种场景很常见，所以尤雨溪发明了 .sync

```html
<Child :money.sync="total"/>
```

`:money.sync="total"` 等价于 `:money="total" v-on:update:money="total=$event"`