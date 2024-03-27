<script setup lang="ts" generic="T">
import { type UseQueryReturnType } from '@tanstack/vue-query';


const props = defineProps<{
    queryHook: () => UseQueryReturnType<T, Error>
}>()
const { data, status, error } = props.queryHook()
</script>
<template>
    <slot v-if="status === 'success' && data" :data="data">{{ data }}</slot>
    <slot v-if="status === 'error' && error" name="error" :error="error">{{ error }}</slot>
    <slot v-if="status === 'pending'" name="loading">Loading...</slot>
</template>