<script setup>
import { ref } from 'vue'

const count = ref(0)
</script>

<template>
  <div v-on:click="count++">
    Hello from home

    {{ count }}
  </div>
</template>
