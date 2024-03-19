<script setup lang="ts">
import { defineComponent, ref } from 'vue'
import { useQuery } from '@tanstack/vue-query'
import { useIntersectionObserver } from '@vueuse/core'

const props = defineProps({
  id: { type: Number, required: true }
})
const { id } = props
const target = ref(null)
const targetIsVisible = ref(false)

useIntersectionObserver(
  target,
  ([{ isIntersecting }], _observerElement) => {
    targetIsVisible.value = isIntersecting
  },
)
const url = `https://dummyjson.com/todos/${id}`
const { data, error, isFetching, isPending } = useQuery({
  queryKey: ['todo', id],
  async queryFn() {
    return await fetch(url).then(
      (response) => response.json(),
    )
  },
  enabled: targetIsVisible.value
})
</script>
<template>
  <div ref="target" class="wrapper">
    <template v-if="isPending">Loading...</template>
    <template v-else-if="error">
      'An error has occurred: {{ error.message }}
    </template>
    <template v-else>
      <div>ID: {{ data.id }}</div>
      <div>Content: {{ data.todo }}</div>
      <div>User: {{ data.userId }}</div>
    </template>
  </div>
</template>
<style scoped>
.wrapper {
  height: 200px
}
</style>