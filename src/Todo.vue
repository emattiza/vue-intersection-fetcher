<script lang="ts">
import { defineComponent, ref } from 'vue'
import { useQuery } from '@tanstack/vue-query'
import { useIntersectionObserver } from '@vueuse/core'

export default defineComponent({
  name: 'Todo',
  props: {
    id: { type: Number, required: true }
  },
  setup(props) {
    const { id } = props
    const target = ref(null)
    const targetIsVisible = ref(false)

    const { stop } = useIntersectionObserver(
      target,
      ([{ isIntersecting }], observerElement) => {
        targetIsVisible.value = isIntersecting
      },
    )
    const url = `https://dummyjson.com/todos/${id}`
    const { data, error, isFetching, isLoading } = useQuery({
      queryKey: ['todo', id],
      async queryFn() {
        return await fetch(url).then(
          (response) => response.json(),
        )
      },
      enabled: targetIsVisible.value
    })

    return {
      data,
      target,
      targetIsVisible,
      error,
      isFetching,
      isLoading,
    }
  },
})
</script>
<template>
  <div ref="target" class="wrapper">
    <template v-if="isLoading">Loading...</template>
    <template v-else-if="error">
    'An error has occurred: {{ error.message }}
    </template>
    <template v-else>
       <div>ID: {{ data.id }}</div>
       <div>Content: {{ data.todo}}</div>
       <div>User: {{data.userId}}</div>
    </template>
  </div>
</template>
<style scoped>
.wrapper {
  height: 200px
}
</style>