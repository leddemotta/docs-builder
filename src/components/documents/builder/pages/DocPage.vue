<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps({
  page: Object,
  number: Number
})

// defineProps<{
//   title?: string
//   likes?: number
// }>()

const { page, number } = props

</script>

<template>
  <div>
  PAGE {{ number }} <br>
    {{ page.content }}
  </div>
</template>

<style lang="sass" scoped></style>
