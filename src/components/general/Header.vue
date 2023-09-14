<script setup lang="ts">
import { reactive } from 'vue'

const menu = reactive([
  {
    name: 'Home',
    route: '/'
  },
  {
    name: 'About',
    route: '/about'
  },
  {
    name: 'Documents',
    route: '/documents'
  }
])
</script>

<template>
  <header>
    <div class="container">
      <a-row justify="space-between">
        <a-col>
          <router-link to="/">
            <img src="@/assets/images/builder.png" width="60" />
          </router-link>
        </a-col>
        <a-col>
          <nav class="mt-4">
            <router-link class="ml-10" v-for="(item, i) in menu" :key="i" :to="item.route">
              {{ item.name }}
            </router-link>
          </nav>
        </a-col>
      </a-row>
    </div>
  </header>
</template>

<style lang="sass" scoped>
header
  padding: 10px
  background: #fff
  border-bottom: 1px solid #ddd
  margin-bottom: 20px
</style>