<template>
  <div class="todo-list">
    <div class="todos">
      <h3>There are {{ state.count }} items:</h3>
      <ul>
        <li v-for="(item, i) in state.items" :key="i"> {{ item }}</li>
      </ul>
    </div>
    <div class="form">
      <input v-model="state.newItem">
      <button @click="addItem">Add Item</button>
    </div>
  </div>
</template>

<script lang="ts">
import { createComponent, reactive, computed } from '@vue/composition-api'

export default createComponent({
  setup() {
    const state = reactive({
      items: ['foo', 'bar', 'baz'],
      newItem: '',
      count: computed(() => state.items.length)
    })

    function addItem() {
      state.items.push(state.newItem)
      state.newItem = ''
    }

    return { state, addItem }
  }
})

</script>
<style lang="stylus" scoped>
.todo-list
  display flex
  align-items center
.todos, .form
  padding 1rem
h3
  margin 0
</style>