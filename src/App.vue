<template>
  <div>
    <TodoList />
    <SelectWithSearch
      v-model="state.value" 
      :values="state.values"
    />
  </div>
</template>

<script lang="ts">
import { createComponent, reactive } from '@vue/composition-api'
import TodoList from '@/components/TodoList.vue'
import SelectWithSearch from '@/components/SelectWithSearch/SelectWithSearch.vue'

export default createComponent({
  name: 'app',
  components: {
    TodoList,
    SelectWithSearch
  },
  setup() {
    const state = reactive({
      value: 'Toyota',
      values: ['Toyota', 'Porsche', 'Seat', 'BMV', 'Tesla']
    })
    return { state }
  }
})
</script>
