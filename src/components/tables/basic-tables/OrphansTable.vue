<template>
  <Datatable
    :data="users"
    :columns="columns"
    :items-per-page="5"
    :searchable="true"
  >
    <!-- Custom cell template for orphan information -->
    <template #orphan="{ item }">
              <div class="flex items-center gap-3">
                <div class="w-10 h-10 overflow-hidden rounded-full">
          <img :src="item.orphan.avatar" :alt="item.orphan.name" />
                </div>
                <div>
                  <span class="block font-medium text-gray-800 text-theme-sm dark:text-white/90">
            {{ item.orphan.name }}
                  </span>
                  <span class="block text-gray-500 text-theme-xs dark:text-gray-400">
            {{ item.orphan.code }}
                  </span>
                </div>
              </div>
    </template>

    <!-- Custom cell template for guardian -->
    <template #guardian="{ value }">
      <p class="text-gray-500 text-theme-sm dark:text-gray-400">{{ value }}</p>
    </template>

    <!-- Custom cell template for supervisor -->
    <template #supervisor="{ value }">
      <p class="text-gray-500 text-theme-sm dark:text-gray-400">{{ value }}</p>
    </template>

    <!-- Custom cell template for last updated -->
    <template #lastUpdated="{ value }">
      <p class="text-gray-500 text-theme-sm dark:text-gray-400">{{ value }}</p>
    </template>

    
  </Datatable>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import Datatable from '@/components/tables/Datatable.vue'

interface Orphan {
  name: string
  code: string
  avatar: string
}

interface OrphanRecord {
  orphan: Orphan
  guardian: string
  supervisor: string
  lastUpdated: string
}

const users = ref<OrphanRecord[]>([
  {
    orphan: {
      name: 'فاطمه قادری',
      code: 'YT-1023',
      avatar: '/images/user/user-17.jpg',
    },
    guardian: 'علی قادری',
    supervisor: 'خانم احمدی',
    lastUpdated: '1404-08-05',
  },
  {
    orphan: {
      name: 'حسن کریمی',
      code: 'YT-1088',
      avatar: '/images/user/user-18.jpg',
    },
    guardian: 'سعید کریمی',
    supervisor: 'آقای جعفری',
    lastUpdated: '1404-08-04',
  },
  {
    orphan: {
      name: 'زهرا موسوی',
      code: 'YT-1099',
      avatar: '/images/user/user-19.jpg',
    },
    guardian: 'مریم موسوی',
    supervisor: 'خانم عباسی',
    lastUpdated: '1404-07-29',
  },
  {
    orphan: {
      name: 'محمد امیری',
      code: 'YT-1110',
      avatar: '/images/user/user-20.jpg',
    },
    guardian: 'رضا امیری',
    supervisor: 'آقای فتحی',
    lastUpdated: '1404-07-22',
  },
  {
    orphan: {
      name: 'نگار رضایی',
      code: 'YT-1135',
      avatar: '/images/user/user-21.jpg',
    },
    guardian: 'سمیه رضایی',
    supervisor: 'خانم رسولی',
    lastUpdated: '1404-08-01',
  },
  {
    orphan: {
      name: 'علی احمدی',
      code: 'YT-1140',
      avatar: '/images/user/user-22.jpg',
    },
    guardian: 'محمد احمدی',
    supervisor: 'آقای رضایی',
    lastUpdated: '1404-07-30',
  },
  {
    orphan: {
      name: 'مریم حسینی',
      code: 'YT-1145',
      avatar: '/images/user/user-23.jpg',
    },
    guardian: 'حسین حسینی',
    supervisor: 'خانم محمدی',
    lastUpdated: '1404-08-02',
  },
  {
    orphan: {
      name: 'رضا نوری',
      code: 'YT-1150',
      avatar: '/images/user/user-24.jpg',
    },
    guardian: 'علی نوری',
    supervisor: 'آقای کریمی',
    lastUpdated: '1404-07-25',
  },
  {
    orphan: {
      name: 'سارا جعفری',
      code: 'YT-1155',
      avatar: '/images/user/user-25.jpg',
    },
    guardian: 'احمد جعفری',
    supervisor: 'خانم موسوی',
    lastUpdated: '1404-08-03',
  },
  {
    orphan: {
      name: 'امیر رضایی',
      code: 'YT-1160',
      avatar: '/images/user/user-26.jpg',
    },
    guardian: 'محمد رضایی',
    supervisor: 'آقای احمدی',
    lastUpdated: '1404-07-28',
  },
])

const columns = [
  { key: 'orphan', label: 'یتیم', sortable: true },
  { key: 'guardian', label: 'قیم', sortable: true },
  { key: 'supervisor', label: 'مسئول', sortable: true },
  { key: 'lastUpdated', label: 'آخرین بروزرسانی', sortable: true },
  // { key: 'actions', label: 'عملیات', sortable: false }
]
</script>

