<script setup lang="ts">
import { reactive, ref } from 'vue'

const people = reactive([
  {
    name: 'Calvin Hawkins',
    email: 'calvin.hawkins@example.com',
    image:
      'https://images.unsplash.com/photo-1491528323818-fdd1faba62cc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80'
  },
  {
    name: 'Kristen Ramos',
    email: 'kristen.ramos@example.com',
    image:
      'https://images.unsplash.com/photo-1550525811-e5869dd03032?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80'
  },
  {
    name: 'Ted Fox',
    email: 'ted.fox@example.com',
    image:
      'https://images.unsplash.com/photo-1500648767791-00dcc994a43e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80'
  }
])

const value = ref('')
</script>

<template>
  <section class="container">
    <a-input v-model:value="value" placeholder="Basic usage" />
    <ul class="divide-y divide-gray-200">
      <li v-for="person in people" :key="person.email" class="py-4 flex">
        <img class="h-10 w-10 rounded-full" :src="person.image" alt="" />
        <div class="ml-3">
          <div class="text-sm font-medium text-gray-900">{{ person.name }}</div>
          <div class="text-sm text-gray-500">{{ person.email }}</div>
        </div>
      </li>
    </ul>
  </section>
</template>

<style lang="sass" scoped></style>
