<script setup lang="ts">
import { useQuery } from '@tanstack/vue-query'
import { VueQueryDevtools } from '@tanstack/vue-query-devtools'
import RemoteItem from './RemoteItem.vue'
import { type WhoaResponse } from './types/whoa'


const useWhoaApi = () => useQuery<WhoaResponse, any>({
  queryKey: ["whoa", 0],
  queryFn: async () => {
    const response = await fetch("https://whoa.onrender.com/whoas/ordered/0")
    const whoa = await response.json()
    if (Math.random() < .05) {
      throw Error("Not so fast!")
    } else {
      return whoa
    }
  }
})

</script>

<template>
  <RemoteItem :queryHook="useWhoaApi">
    <template #default="{ data }">
      <a href="#"><img :src="data.poster" width="240px" /></a>
      <video controls>
        <source :src="data.video['360p']" />
      </video>
    </template>
    <template #error="{ error }">
      <div>{{ error }}</div>
    </template>
    <template #loading>
      <div>Loading...</div>
    </template>
  </RemoteItem>
  <VueQueryDevtools />
</template>
