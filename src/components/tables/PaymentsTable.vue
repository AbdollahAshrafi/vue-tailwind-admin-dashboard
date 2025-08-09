<template>
  <ComponentCard title="کمک‌های مالی" className="mb-6">
    <Datatable :data="payments" :columns="columns" :items-per-page="10" :searchable="true">
      <template #date="{ value }"><span class="text-gray-800 dark:text-white/90">{{ value }}</span></template>
      <template #amount="{ value }"><span class="text-gray-800 dark:text-white/90">{{ value.toLocaleString() }} تومان</span></template>
      <template #type="{ value }"><span class="text-gray-800 dark:text-white/90">{{ value }}</span></template>
      <template #payer="{ value }"><span class="text-gray-800 dark:text-white/90">{{ value }}</span></template>
      <template #description="{ value }"><span class="text-gray-800 dark:text-white/90">{{ value }}</span></template>
      <template #status="{ value }">
        <span :class="getStatusClass(value)" class="px-2 py-1 text-xs rounded-full">{{ value }}</span>
      </template>
    </Datatable>
  </ComponentCard>
</template>

<script setup lang="ts">
import ComponentCard from '@/components/common/ComponentCard.vue'
import Datatable from '@/components/tables/Datatable.vue'

interface Payment {
  id: number
  date: string
  amount: number
  type: string
  payer: string
  description: string
  status: string
}

interface Props { payments: Payment[] }

defineProps<Props>()

const columns = [
  { key: 'date', label: 'تاریخ پرداخت', sortable: true },
  { key: 'amount', label: 'مبلغ', sortable: true },
  { key: 'type', label: 'نوع پرداخت', sortable: true },
  { key: 'payer', label: 'پرداخت‌کننده', sortable: true },
  { key: 'description', label: 'توضیحات', sortable: false },
  { key: 'status', label: 'وضعیت', sortable: true },
]

const getStatusClass = (status: string) => {
  switch (status) {
    case 'پرداخت شده':
      return 'bg-green-100 text-green-800 dark:bg-green-900/20 dark:text-green-400'
    case 'در انتظار':
      return 'bg-yellow-100 text-yellow-800 dark:bg-yellow-900/20 dark:text-yellow-400'
    case 'ناموفق':
      return 'bg-red-100 text-red-800 dark:bg-red-900/20 dark:text-red-400'
    default:
      return 'bg-gray-100 text-gray-800 dark:bg-gray-900/20 dark:text-gray-400'
  }
}
</script> 