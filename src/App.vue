<script lang="ts">
import { defineComponent } from 'vue'
import { useQuery } from '@tanstack/vue-query'
import { VueQueryDevtools } from '@tanstack/vue-query-devtools'
import Todo from './Todo.vue'

export default defineComponent({
  name: 'App',
  components: { VueQueryDevtools, Todo },
  setup() {
  },
})
</script>

<template>
  <template v-for="id in 10">
    <Todo :id="id" />
  </template>
  <VueQueryDevtools />
</template>
