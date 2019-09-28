<template>
  <div ref="selectRef">
    <div>{{ value }}</div>
    <div v-if="isOpen">
      <input v-model.trim="filterText">
      <a
        class="selectOption"
        v-for="(filteredValue, i) in filteredValues"
        @click="onSelect(filteredValue)"
        :key="i"
        :class="{ selected: value === filteredValue }"
      >{{ filteredValue }}</a>
    </div>
  </div>
</template>

<script lang="ts">
import {
  createComponent, reactive, computed, PropType, ref, onMounted, onUnmounted
} from '@vue/composition-api'

export default createComponent({
  props: {
    value: {
      type: String,
      required: true
    },
    values: {
      type: Array as PropType<string[]>,
      required: true
    }
  },
  setup(props, context) {

    const { filterText, filteredValues } = useFilterValues(props.values)
    const { isOpen, selectRef } = useToggleDropdown()

    function onSelect(newValue: string) {
      filterText.value = ''
      isOpen.value = false
      context.emit('input', newValue)
    }

    return {
      filterText,
      filteredValues,
      isOpen,
      selectRef,
      onSelect
    }
  }
})

function useFilterValues(values: string[]) {
  const filterText = ref('')
  const filteredValues = computed(() => values.filter(v =>
    !filterText.value
    || v.toLowerCase().includes(filterText.value.toLowerCase())
  ))
  return { filterText, filteredValues }
}

function useToggleDropdown() {
  const isOpen = ref(false)
  const selectRef = ref(null)

  function onWindowClick({ target }) {
    isOpen.value = (selectRef.value as any).contains(target)
  }

  onMounted(() => window.addEventListener('click', onWindowClick))
  onUnmounted(() => window.removeEventListener('click', onWindowClick))

  return { isOpen, selectRef }
}
</script>

<style scoped lang="stylus">
.selected
  font-weight bold
.selectOption
  display block
</style>