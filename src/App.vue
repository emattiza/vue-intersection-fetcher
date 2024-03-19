<script setup lang="ts">
import { defineComponent } from 'vue'
import { useQueryClient } from '@tanstack/vue-query'
import { VueQueryDevtools } from '@tanstack/vue-query-devtools'
import Todo from './Todo.vue'

const queryClient = useQueryClient()
const refetch = () => {
  queryClient.invalidateQueries(["todos"])
}
</script>

<template>
  <div>
    <button @click="refetch">Refetch all</button>
  </div>
  <hr />
  <template v-for="id in 10">
    <Todo :id="id" />
  </template>
  <VueQueryDevtools />
</template>
