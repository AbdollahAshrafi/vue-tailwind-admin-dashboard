<template>
  <ComponentCard title="کمک‌های غیرمالی" className="mb-6">
    <Datatable :data="flatRows" :columns="columns" :items-per-page="10" :searchable="true">
      <template #date="{ value }"><span class="text-gray-800 dark:text-white/90">{{ value }}</span></template>
      <template #donor="{ value }"><span class="text-gray-800 dark:text-white/90">{{ value }}</span></template>
      <template #items="{ item }">
        <div class="inline-block min-w-[260px]">
          <table class="w-full text-sm">
            <thead>
              <tr class="text-gray-500 dark:text-gray-400">
                <th class="text-right py-1">نام قلم</th>
                <th class="text-right py-1 w-20">تعداد</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(itm, idx) in item.raw.items" :key="idx" class="border-t border-gray-100 dark:border-gray-800">
                <td class="py-1 text-gray-800 dark:text-white/90">{{ itm.name }}</td>
                <td class="py-1 text-gray-800 dark:text-white/90">{{ itm.count }}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <p v-if="item.raw.note" class="mt-2 text-xs text-gray-500 dark:text-gray-400">توضیح: {{ item.raw.note }}</p>
      </template>
    </Datatable>
  </ComponentCard>
</template>

<script setup lang="ts">
import ComponentCard from '@/components/common/ComponentCard.vue'
import Datatable from '@/components/tables/Datatable.vue'

interface NonMonetaryItem { name: string; count: number }
interface NonMonetaryHelpRow { id: number; date: string; donor: string; items: NonMonetaryItem[]; note?: string }

interface Props { items: NonMonetaryHelpRow[] }

const props = defineProps<Props>()

// Flatten rows for searching/sorting while keeping raw access for slot
const flatRows = props.items.map(r => ({
  id: r.id,
  date: r.date,
  donor: r.donor,
  items: r.items.map(i => `${i.name}×${i.count}`).join('، '),
  raw: r,
}))

const columns = [
  { key: 'date', label: 'تاریخ', sortable: true },
  { key: 'donor', label: 'اهداکننده', sortable: true },
  { key: 'items', label: 'اقلام تحویل‌شده', sortable: false },
]
</script> 