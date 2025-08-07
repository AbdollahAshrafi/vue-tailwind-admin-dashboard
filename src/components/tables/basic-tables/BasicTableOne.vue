<template>
  <div
    class="overflow-hidden rounded-xl border border-gray-200 bg-white dark:border-gray-800 dark:bg-white/[0.03]"
  >
    <div class="max-w-full overflow-x-auto custom-scrollbar">
      <table class="min-w-full">
        <thead>
          <tr class="border-b border-gray-200 dark:border-gray-700">
            <th class="px-5 py-3 text-right w-3/11 sm:px-6">
              <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">یتیم</p>
            </th>
            <th class="px-5 py-3 text-right w-2/11 sm:px-6">
              <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">قیم</p>
            </th>
            <th class="px-5 py-3 text-right w-2/11 sm:px-6">
              <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">مسئول</p>
            </th>
            <th class="px-5 py-3 text-right w-2/11 sm:px-6">
              <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">آخرین بروزرسانی</p>
            </th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-200 dark:divide-gray-700">
          <tr
            v-for="(user, index) in users"
            :key="index"
            class="border-t border-gray-100 dark:border-gray-800"
          >
            <!-- یتیم -->
            <td class="px-5 py-4 sm:px-6">
              <div class="flex items-center gap-3">
                <div class="w-10 h-10 overflow-hidden rounded-full">
                  <img :src="user.orphan.avatar" :alt="user.orphan.name" />
                </div>
                <div>
                  <span class="block font-medium text-gray-800 text-theme-sm dark:text-white/90">
                    {{ user.orphan.name }}
                  </span>
                  <span class="block text-gray-500 text-theme-xs dark:text-gray-400">
                    {{ user.orphan.code }}
                  </span>
                </div>
              </div>
            </td>

            <!-- قیم -->
            <td class="px-5 py-4 sm:px-6">
              <p class="text-gray-500 text-theme-sm dark:text-gray-400">{{ user.guardian }}</p>
            </td>

            <!-- مسئول -->
            <td class="px-5 py-4 sm:px-6">
              <p class="text-gray-500 text-theme-sm dark:text-gray-400">{{ user.supervisor }}</p>
            </td>

            <!-- آخرین بروزرسانی -->
            <td class="px-5 py-4 sm:px-6">
              <p class="text-gray-500 text-theme-sm dark:text-gray-400">{{ user.lastUpdated }}</p>
            </td>

          </tr>
        </tbody>

      </table>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const users = ref([
  {
    orphan: {
      name: 'فاطمه قادری',
      code: 'YT-1023',
      avatar: '/images/user/user-17.jpg',
    },
    guardian: 'علی قادری',
    supervisor: 'خانم احمدی',
    lastUpdated: '2025-08-05',
  },
  {
    orphan: {
      name: 'حسن کریمی',
      code: 'YT-1088',
      avatar: '/images/user/user-18.jpg',
    },
    guardian: 'سعید کریمی',
    supervisor: 'آقای جعفری',
    lastUpdated: '2025-08-04',
  },
  {
    orphan: {
      name: 'زهرا موسوی',
      code: 'YT-1099',
      avatar: '/images/user/user-19.jpg',
    },
    guardian: 'مریم موسوی',
    supervisor: 'خانم عباسی',
    lastUpdated: '2025-07-29',
  },
  {
    orphan: {
      name: 'محمد امیری',
      code: 'YT-1110',
      avatar: '/images/user/user-20.jpg',
    },
    guardian: 'رضا امیری',
    supervisor: 'آقای فتحی',
    lastUpdated: '2025-07-22',
  },
  {
    orphan: {
      name: 'نگار رضایی',
      code: 'YT-1135',
      avatar: '/images/user/user-21.jpg',
    },
    guardian: 'سمیه رضایی',
    supervisor: 'خانم رسولی',
    lastUpdated: '2025-08-01',
  },
])

</script>

