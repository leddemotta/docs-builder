<script setup lang="ts">
import { ref } from 'vue'
import { Button } from 'ant-design-vue'
import { VueDraggableNext as draggable } from 'vue-draggable-next'
import DocPage from './builder/pages/DocPage.vue'

const pagesArr = ref([
  {
    content: 'test1'
  }
])

const startDrag = () => {}
const endDrag = () => {}
</script>

<template>
  <div class="builder-container">
    <draggable class="dragArea list-group w-full" :list="pagesArr" @change="startDrag">
      <DocPage
        class="page mb-5 list-group-item border m-1 p-3 rounded-md"
        v-for="(page, i) in pagesArr"
        :key="i"
        :number="i + 1"
        :page="page"
      />
    </draggable>

    <div class="text-center mt-5 mb-20">
      <Button @click="pagesArr.push({ content: 'test' + pagesArr.length })">
        ADICIONAR PÁGINA
      </Button>
    </div>

    {{ pagesArr }}
  </div>
</template>

<style lang="sass">
.page
  border-color: #ddd
  transition:.3s
  &:hover
    border-color: #aaa
.doc-skeleton
  border: 2px solid #eee
  height: 800px
  width: 100%
  border-radius: 10px
</style>
