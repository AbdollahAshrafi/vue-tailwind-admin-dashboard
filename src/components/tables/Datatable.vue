<template>
    <div class="datatable">
      <!-- Search Bar -->
      <div class="mb-4">
        <SearchBar v-model="searchQuery" placeholder="جستجو در ایتام..." />
      </div>
  
      <!-- Table -->
      <div class="overflow-hidden rounded-xl border border-gray-200 bg-white dark:border-gray-800 dark:bg-white/[0.03]">
        <div class="max-w-full overflow-x-auto custom-scrollbar">
          <table class="min-w-full">
            <thead>
              <tr class="border-b border-gray-200 dark:border-gray-700">
                <th
                  v-for="column in columns"
                  :key="column.key"
                  @click="column.sortable !== false ? sortBy(column.key) : null"
                  :class="[
                    'px-5 py-3 text-right sm:px-6',
                    column.sortable !== false ? 'cursor-pointer hover:bg-gray-50 dark:hover:bg-gray-700' : ''
                  ]"
                >
                  <div class="flex items-center justify-between">
                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">
                      {{ column.label }}
                    </p>
                    <div v-if="column.sortable !== false" class="flex flex-col">
                      <svg
                        v-if="sortKey === column.key && sortOrder === 'asc'"
                        class="w-4 h-4 text-blue-500"
                        fill="currentColor"
                        viewBox="0 0 20 20"
                      >
                        <path fill-rule="evenodd" d="M14.707 12.707a1 1 0 01-1.414 0L10 9.414l-3.293 3.293a1 1 0 01-1.414-1.414l4-4a1 1 0 011.414 0l4 4a1 1 0 010 1.414z" clip-rule="evenodd" />
                      </svg>
                      <svg
                        v-else-if="sortKey === column.key && sortOrder === 'desc'"
                        class="w-4 h-4 text-blue-500"
                        fill="currentColor"
                        viewBox="0 0 20 20"
                      >
                        <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
                      </svg>
                      <svg
                        v-else
                        class="w-4 h-4 text-gray-300"
                        fill="currentColor"
                        viewBox="0 0 20 20"
                      >
                        <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
                      </svg>
                    </div>
                  </div>
                </th>
              </tr>
            </thead>
            <tbody class="divide-y divide-gray-200 dark:divide-gray-700">
              <tr
                v-for="(item, index) in paginatedData"
                :key="index"
                class="border-t border-gray-100 dark:border-gray-800 hover:bg-gray-50 dark:hover:bg-gray-700/50"
              >
                <td
                  v-for="column in columns"
                  :key="column.key"
                  class="px-5 py-4 sm:px-6"
                >
                  <slot :name="column.key" :item="item" :value="getNestedValue(item, column.key)">
                    <span class="text-gray-500 text-theme-sm dark:text-gray-400">
                      {{ getNestedValue(item, column.key) }}
                    </span>
                  </slot>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
  
      <!-- Pagination -->
      <div class="mt-4 flex items-center justify-between">
        <div class="text-sm text-gray-700 dark:text-gray-300">
          نمایش {{ startIndex + 1 }} تا {{ endIndex }} از {{ filteredData.length }} مورد
        </div>
        <div class="flex space-x-2 space-x-reverse">
          <button
            @click="previousPage"
            :disabled="currentPage === 1"
            class="px-3 py-1 text-sm border border-gray-300 rounded disabled:opacity-50 disabled:cursor-not-allowed hover:bg-gray-50 dark:border-gray-600 dark:hover:bg-gray-700 dark:text-white"
          >
            قبلی
          </button>
          <span class="px-3 py-1 text-sm dark:text-white">
            صفحه {{ currentPage }} از {{ totalPages }}
          </span>
          <button
            @click="nextPage"
            :disabled="currentPage === totalPages"
            class="px-3 py-1 text-sm border border-gray-300 rounded disabled:opacity-50 disabled:cursor-not-allowed hover:bg-gray-50 dark:border-gray-600 dark:hover:bg-gray-700 dark:text-white"
          >
            بعدی
          </button>
        </div>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref, computed, watch } from 'vue'
  import SearchBar from '@/components/layout/header/SearchBar.vue'

  
  interface Column {
    key: string
    label: string
    sortable?: boolean
  }
  
  interface Props {
    data: any[]
    columns: Column[]
    itemsPerPage?: number
    searchable?: boolean
  }
  
  const props = withDefaults(defineProps<Props>(), {
    itemsPerPage: 10,
    searchable: true
  })
  
  const searchQuery = ref('')
  const currentPage = ref(1)
  const sortKey = ref('')
  const sortOrder = ref<'asc' | 'desc'>('asc')
  
  // Helper function to get nested object values
  const getNestedValue = (obj: any, path: string) => {
    return path.split('.').reduce((current, key) => current?.[key], obj)
  }
  
  // Filter data based on search query
  const filteredData = computed(() => {
    if (!props.searchable || !searchQuery.value) {
      return props.data
    }
    
    return props.data.filter(item => {
      return Object.values(item).some(value => {
        if (typeof value === 'object' && value !== null) {
          return Object.values(value).some(nestedValue => 
            String(nestedValue).toLowerCase().includes(searchQuery.value.toLowerCase())
          )
        }
        return String(value).toLowerCase().includes(searchQuery.value.toLowerCase())
      })
    })
  })
  
  // Sort data
  const sortedData = computed(() => {
    if (!sortKey.value) {
      return filteredData.value
    }
    
    return [...filteredData.value].sort((a, b) => {
      const aVal = getNestedValue(a, sortKey.value)
      const bVal = getNestedValue(b, sortKey.value)
      
      if (aVal < bVal) return sortOrder.value === 'asc' ? -1 : 1
      if (aVal > bVal) return sortOrder.value === 'asc' ? 1 : -1
      return 0
    })
  })
  
  // Pagination
  const totalPages = computed(() => Math.ceil(sortedData.value.length / props.itemsPerPage))
  const startIndex = computed(() => (currentPage.value - 1) * props.itemsPerPage)
  const endIndex = computed(() => Math.min(startIndex.value + props.itemsPerPage, sortedData.value.length))
  const paginatedData = computed(() => 
    sortedData.value.slice(startIndex.value, endIndex.value)
  )
  
  // Methods
  const sortBy = (key: string) => {
    if (sortKey.value === key) {
      sortOrder.value = sortOrder.value === 'asc' ? 'desc' : 'asc'
    } else {
      sortKey.value = key
      sortOrder.value = 'asc'
    }
    currentPage.value = 1
  }
  
  const nextPage = () => {
    if (currentPage.value < totalPages.value) {
      currentPage.value++
    }
  }
  
  const previousPage = () => {
    if (currentPage.value > 1) {
      currentPage.value--
    }
  }
  
  // Reset to first page when search changes
  watch(searchQuery, () => {
    currentPage.value = 1
  })
  </script>