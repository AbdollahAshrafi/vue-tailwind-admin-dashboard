<template>
  <div>
    <div class="p-5 mb-6 border border-gray-200 rounded-2xl dark:border-gray-800 lg:p-6">
      <div class="flex flex-col gap-6 lg:flex-row lg:items-start lg:justify-between">
        <div>
          <h4 class="text-lg font-semibold text-gray-800 dark:text-white/90 lg:mb-6">
            آخرین پرداخت‌ها
          </h4>
        </div>
        <button @click="showAllPayments" class="edit-button">
          <svg
            class="fill-current"
            width="18"
            height="18"
            viewBox="0 0 18 18"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M9 1.5C4.85786 1.5 1.5 4.85786 1.5 9C1.5 13.1421 4.85786 16.5 9 16.5C13.1421 16.5 16.5 13.1421 16.5 9C16.5 4.85786 13.1421 1.5 9 1.5ZM9 15C5.68629 15 3 12.3137 3 9C3 5.68629 5.68629 3 9 3C12.3137 3 15 5.68629 15 9C15 12.3137 12.3137 15 9 15Z"
              fill=""
            />
            <path
              d="M9 4.5C8.58579 4.5 8.25 4.83579 8.25 5.25V8.25H5.25C4.83579 8.25 4.5 8.58579 4.5 9C4.5 9.41421 4.83579 9.75 5.25 9.75H8.25V12.75C8.25 13.1642 8.58579 13.5 9 13.5C9.41421 13.5 9.75 13.1642 9.75 12.75V9.75H12.75C13.1642 9.75 13.5 9.41421 13.5 9C13.5 8.58579 13.1642 8.25 12.75 8.25H9.75V5.25C9.75 4.83579 9.41421 4.5 9 4.5Z"
              fill=""
            />
          </svg>
          مشاهده همه
        </button>
      </div>

      <div class="overflow-x-auto">
        <table class="w-full">
          <thead>
            <tr class="border-b border-gray-200 dark:border-gray-700">
              <th class="text-right py-3 px-4 text-sm font-medium text-gray-500 dark:text-gray-400">تاریخ پرداخت</th>
              <th class="text-right py-3 px-4 text-sm font-medium text-gray-500 dark:text-gray-400">مبلغ</th>
              <th class="text-right py-3 px-4 text-sm font-medium text-gray-500 dark:text-gray-400">نوع پرداخت</th>
              <th class="text-right py-3 px-4 text-sm font-medium text-gray-500 dark:text-gray-400">پرداخت‌کننده</th>
              <th class="text-right py-3 px-4 text-sm font-medium text-gray-500 dark:text-gray-400">توضیحات</th>
              <th class="text-right py-3 px-4 text-sm font-medium text-gray-500 dark:text-gray-400">وضعیت</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="payment in payments" :key="payment.id" class="border-b border-gray-100 dark:border-gray-800">
              <td class="py-3 px-4 text-sm text-gray-800 dark:text-white/90">{{ payment.date }}</td>
              <td class="py-3 px-4 text-sm text-gray-800 dark:text-white/90">{{ payment.amount.toLocaleString() }} تومان</td>
              <td class="py-3 px-4 text-sm text-gray-800 dark:text-white/90">{{ payment.type }}</td>
              <td class="py-3 px-4 text-sm text-gray-800 dark:text-white/90">{{ payment.payer }}</td>
              <td class="py-3 px-4 text-sm text-gray-800 dark:text-white/90">{{ payment.description }}</td>
              <td class="py-3 px-4">
                <span :class="getStatusClass(payment.status)" class="px-2 py-1 text-xs rounded-full">
                  {{ payment.status }}
                </span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
interface Payment {
  id: number
  date: string
  amount: number
  type: string
  payer: string
  description: string
  status: string
}

interface Props {
  payments: Payment[]
}

const props = defineProps<Props>()

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

const showAllPayments = () => {
  // Implement show all payments logic
  console.log('Show all payments')
}
</script> 