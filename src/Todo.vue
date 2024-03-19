<script setup lang="ts">
import { ref } from 'vue'
import { useQuery } from '@tanstack/vue-query'
import { useIntersectionObserver } from '@vueuse/core'

const props = defineProps({
  id: { type: Number, required: true }
})
const sleep = (delay) => {
   return new Promise((resolve) => setTimeout(resolve, delay))
}
const { id } = props
const target = ref(null)
const targetIsVisible = ref(false)

useIntersectionObserver(
  target,
  ([{ isIntersecting }], _observerElement) => {
    if (!targetIsVisible.value) {
      targetIsVisible.value = isIntersecting
    }
  },
)
const url = `https://dummyjson.com/todos/${id}`
const { data, error, isFetching, isPending, isFetched } = useQuery({
  queryKey: ['todo', id],
  async queryFn() {
    await sleep(3000)
    return await fetch(url).then(
      (response) => response.json(),
    )
  },
  enabled: targetIsVisible
})
</script>
<template>
  {{ targetIsVisible }}
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