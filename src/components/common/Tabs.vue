<template>
  <div>
    <div class="flex border-b border-gray-200 dark:border-gray-800 mb-4 overflow-x-auto">
      <button
        v-for="(tab, idx) in tabs"
        :key="tab.key || idx"
        @click="activeIndex = idx"
        :class="[
          'px-4 py-2 -mb-px whitespace-nowrap',
          activeIndex === idx
            ? 'border-b-2 border-brand-500 text-brand-600 dark:text-brand-400'
            : 'text-gray-600 dark:text-gray-400 hover:text-gray-800 dark:hover:text-gray-200'
        ]"
      >
        <span class="text-sm font-medium">{{ tab.title }}</span>
      </button>
    </div>
    <div>
      <slot :name="tabs[activeIndex]?.slot || tabs[activeIndex]?.key"></slot>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

export interface TabDef {
  key: string
  title: string
  slot?: string
}

const props = defineProps<{ tabs: TabDef[]; initial?: number }>()
const activeIndex = ref(props.initial ?? 0)
</script> 