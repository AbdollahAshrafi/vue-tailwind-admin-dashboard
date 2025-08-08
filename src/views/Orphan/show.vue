<template>
  <admin-layout>
    <PageBreadcrumb :pageTitle="currentPageTitle" />

    <div
      class="rounded-2xl border border-gray-200 bg-white p-5 dark:border-gray-800 dark:bg-white/[0.03] lg:p-6"
    >
      <h3 class="mb-5 text-lg font-semibold text-gray-800 dark:text-white/90 lg:mb-7">اطلاعات یتیم</h3>
      
      <!-- Orphan Profile Card -->
      <OrphanProfileCard :orphan="orphanData" />
      
      <!-- Orphan Personal Info Card -->
      <FlexiblePersonalInfoCard 
        title="اطلاعات شخصی یتیم"
        :data="orphanPersonalInfo"
        :fields="orphanFields"
        @update="updateOrphanInfo"
      />
      
      <!-- Father Info Card -->
      <FlexiblePersonalInfoCard 
        title="اطلاعات پدر متوفی"
        :data="fatherData"
        :fields="fatherFields"
        @update="updateFatherInfo"
      />
      
      <!-- Mother Info Card -->
      <FlexiblePersonalInfoCard 
        title="اطلاعات مادر"
        :data="motherData"
        :fields="motherFields"
        @update="updateMotherInfo"
      />
      
      <!-- Mother's Husband Info Card (only if mother is remarried) -->
      <FlexiblePersonalInfoCard 
        v-if="motherData.marriageStatus === 'متاهل'"
        title="اطلاعات همسر مادر"
        :data="motherHusbandData"
        :fields="husbandFields"
        @update="updateHusbandInfo"
      />
      
      <!-- Visits Table -->
      <VisitsTable :visits="visitsData" />
      
      <!-- Payments Table -->
      <PaymentsTable :payments="paymentsData" />
    </div>
  </admin-layout>
</template>

<script setup lang="ts">
import AdminLayout from '../../components/layout/AdminLayout.vue'
import PageBreadcrumb from '@/components/common/PageBreadcrumb.vue'
import { ref } from 'vue'
import OrphanProfileCard from '../../components/profile/OrphanProfileCard.vue'
import FlexiblePersonalInfoCard from '../../components/profile/FlexiblePersonalInfoCard.vue'
import VisitsTable from '../../components/tables/VisitsTable.vue'
import PaymentsTable from '../../components/tables/PaymentsTable.vue'

const currentPageTitle = ref('محسن شریفیان')

// Sample data for orphan profile
const orphanData = ref({
  name: 'محسن شریفیان',
  code: 'YT-1023',
  age: 12,
  avatar: '/images/user/user-17.jpg',
  birthDate: '1392-03-15'
})

// Sample data for orphan personal info
const orphanPersonalInfo = ref({
  name: 'محسن شریفیان',
  code: 'YT-1023',
  age: 12,
  birthDate: '1392-03-15',
  caretaker: 'علی شریفیان',
  supervisor: 'خانم احمدی',
  address: 'تهران، خیابان ولیعصر، پلاک 123',
  phone: '09123456789'
})

// Field configuration for orphan
const orphanFields = ref([
  { key: 'name', label: 'نام', type: 'text' as const },
  { key: 'code', label: 'کد یتیم', type: 'text' as const },
  { key: 'age', label: 'سن', type: 'number' as const },
  { key: 'birthDate', label: 'تاریخ تولد', type: 'date' as const },
  { key: 'caretaker', label: 'قیم', type: 'text' as const },
  { key: 'supervisor', label: 'مسئول', type: 'text' as const },
  { key: 'address', label: 'آدرس', type: 'text' as const, fullWidth: true },
  { key: 'phone', label: 'شماره تماس', type: 'text' as const }
])

// Sample data for father
const fatherData = ref({
  name: 'احمد',
  lastName: 'شریفیان',
  nationalId: '0012345678',
  deathDate: '1400-05-20',
  deathAge: 45,
  occupation: 'راننده',
  address: 'تهران، خیابان ولیعصر، پلاک 123',
  deathCause: 'تصادف رانندگی'
})

// Field configuration for father
const fatherFields = ref([
  { key: 'name', label: 'نام', type: 'text' as const },
  { key: 'lastName', label: 'نام خانوادگی', type: 'text' as const },
  { key: 'nationalId', label: 'شماره شناسنامه', type: 'text' as const },
  { key: 'deathDate', label: 'تاریخ فوت', type: 'date' as const },
  { key: 'deathAge', label: 'سن در زمان فوت', type: 'number' as const },
  { key: 'occupation', label: 'شغل', type: 'text' as const },
  { key: 'address', label: 'آدرس', type: 'text' as const, fullWidth: true },
  { key: 'deathCause', label: 'علت فوت', type: 'text' as const, fullWidth: true }
])

// Sample data for mother
const motherData = ref({
  name: 'فاطمه',
  lastName: 'محمدی',
  nationalId: '0012345679',
  marriageStatus: 'متاهل',
  age: 38,
  occupation: 'خانه‌دار',
  address: 'تهران، خیابان ولیعصر، پلاک 123',
  phone: '09123456789'
})

// Field configuration for mother
const motherFields = ref([
  { key: 'name', label: 'نام', type: 'text' as const },
  { key: 'lastName', label: 'نام خانوادگی', type: 'text' as const },
  { key: 'nationalId', label: 'شماره شناسنامه', type: 'text' as const },
  { key: 'age', label: 'سن', type: 'number' as const },
  { key: 'marriageStatus', label: 'وضعیت تاهل', type: 'select' as const, options: [
    { value: 'مجرد', label: 'مجرد' },
    { value: 'متاهل', label: 'متاهل' },
    { value: 'مطلقه', label: 'مطلقه' },
    { value: 'بیوه', label: 'بیوه' }
  ]},
  { key: 'occupation', label: 'شغل', type: 'text' as const },
  { key: 'address', label: 'آدرس', type: 'text' as const, fullWidth: true },
  { key: 'phone', label: 'شماره تماس', type: 'text' as const }
])

// Sample data for mother's husband
const motherHusbandData = ref({
  name: 'محمد',
  lastName: 'کریمی',
  nationalId: '0012345680',
  age: 42,
  occupation: 'کارمند',
  marriageDate: '1401-08-15',
  address: 'تهران، خیابان ولیعصر، پلاک 123',
  phone: '09123456790'
})

// Field configuration for husband
const husbandFields = ref([
  { key: 'name', label: 'نام', type: 'text' as const },
  { key: 'lastName', label: 'نام خانوادگی', type: 'text' as const },
  { key: 'nationalId', label: 'شماره شناسنامه', type: 'text' as const },
  { key: 'age', label: 'سن', type: 'number' as const },
  { key: 'occupation', label: 'شغل', type: 'text' as const },
  { key: 'marriageDate', label: 'تاریخ ازدواج', type: 'date' as const },
  { key: 'address', label: 'آدرس', type: 'text' as const, fullWidth: true },
  { key: 'phone', label: 'شماره تماس', type: 'text' as const }
])

// Sample data for visits
const visitsData = ref([
  {
    id: 1,
    date: '1404-08-05',
    visitor: 'خانم احمدی',
    type: 'بازدید ماهانه',
    description: 'بررسی وضعیت تحصیلی و سلامت',
    status: 'انجام شده'
  },
  {
    id: 2,
    date: '1404-07-25',
    visitor: 'آقای جعفری',
    type: 'بازدید پزشکی',
    description: 'معاینه سلامت عمومی',
    status: 'انجام شده'
  },
  {
    id: 3,
    date: '1404-08-10',
    visitor: 'خانم موسوی',
    type: 'بازدید آموزشی',
    description: 'بررسی پیشرفت تحصیلی',
    status: 'در انتظار'
  }
])

// Sample data for payments
const paymentsData = ref([
  {
    id: 1,
    date: '1404-08-01',
    amount: 500000,
    type: 'کمک هزینه تحصیلی',
    payer: 'موسسه خیریه',
    description: 'پرداخت شهریه مدرسه',
    status: 'پرداخت شده'
  },
  {
    id: 2,
    date: '1404-07-15',
    amount: 300000,
    type: 'کمک هزینه معیشتی',
    payer: 'موسسه خیریه',
    description: 'کمک هزینه ماهانه',
    status: 'پرداخت شده'
  },
  {
    id: 3,
    date: '1404-08-15',
    amount: 200000,
    type: 'کمک هزینه درمانی',
    payer: 'موسسه خیریه',
    description: 'هزینه دارو و درمان',
    status: 'در انتظار'
  }
])

// Update handlers
const updateOrphanInfo = (data: Record<string, string | number>) => {
  Object.assign(orphanPersonalInfo.value, data)
  console.log('Orphan info updated:', data)
}

const updateFatherInfo = (data: Record<string, string | number>) => {
  Object.assign(fatherData.value, data)
  console.log('Father info updated:', data)
}

const updateMotherInfo = (data: Record<string, string | number>) => {
  Object.assign(motherData.value, data)
  console.log('Mother info updated:', data)
}

const updateHusbandInfo = (data: Record<string, string | number>) => {
  Object.assign(motherHusbandData.value, data)
  console.log('Husband info updated:', data)
}
</script>
  