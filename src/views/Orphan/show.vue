<template>
    <admin-layout>
      <PageBreadcrumb :pageTitle="currentPageTitle" />
  
      <div
        class="rounded-2xl border border-gray-200 bg-white p-5 dark:border-gray-800 dark:bg-white/[0.03] lg:p-6"
      >
        <h3 class="mb-5 text-lg font-semibold text-gray-800 dark:text-white/90 lg:mb-7">اطلاعات یتیم</h3>
      
      <!-- Orphan Profile Card -->
      <OrphanProfileCard :orphan="orphanData" />
      
      <Tabs :tabs="tabs" :initial="0">
        <template #personal>
          <!-- Orphan Personal Info Card -->
          <PersonalInfoCard 
            title="اطلاعات شخص یتیم"
            :data="orphanPersonalInfo"
            :fields="orphanFields"
            @update="updateOrphanInfo"
          />
          
          <!-- Father Info Card -->
          <PersonalInfoCard 
            title="اطلاعات پدر مرحوم"
            :data="fatherData"
            :fields="fatherFields"
            @update="updateFatherInfo"
          />
          
          <!-- Mother Info Card -->
          <PersonalInfoCard 
            title="اطلاعات مادر"
            :data="motherData"
            :fields="motherFields"
            @update="updateMotherInfo"
          />
          
          <!-- Mother's Husband Info Card (only if mother is remarried) -->
          <PersonalInfoCard 
            v-if="motherData.remarriageStatus === 'انجام داده'"
            title="اطلاعات همسر مادر"
            :data="motherHusbandData"
            :fields="husbandFields"
            @update="updateHusbandInfo"
          />
        </template>
        
        <template #visits>
          <VisitsTable :visits="visitsData" />
        </template>
        
        <template #monetary>
          <PaymentsTable :payments="paymentsData" />
        </template>
        
        <template #nonmonetary>
          <NonMonetaryHelpTable :items="nonMonetaryData" />
        </template>
      </Tabs>
      </div>
    </admin-layout>
  </template>
  
  <script setup lang="ts">
  import AdminLayout from '../../components/layout/AdminLayout.vue'
    import PageBreadcrumb from '@/components/common/PageBreadcrumb.vue'
  import { ref } from 'vue'
 import OrphanProfileCard from '../../components/profile/OrphanProfileCard.vue'
  import PersonalInfoCard from '../../components/profile/PersonalInfoCard.vue'
 import VisitsTable from '../../components/tables/VisitsTable.vue'
 import PaymentsTable from '../../components/tables/PaymentsTable.vue'
 import Tabs from '@/components/common/Tabs.vue'
 import NonMonetaryHelpTable from '@/components/tables/NonMonetaryHelpTable.vue'

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
  nationalId: '0012345678',
  idCardNumber: '123456789',
  education: 'ابتدایی',
  caretaker: 'علی شریفیان',
  supervisor: 'خانم احمدی',
  address: 'تهران، خیابان ولیعصر، پلاک 123',
  phone: '09123456789'
})

// Field configuration for orphan
const orphanFields = ref([
  { key: 'name', label: 'نام و نام خانوادگی', type: 'text' as const },
  { key: 'code', label: 'کد یتیم', type: 'text' as const },
  { key: 'age', label: 'سن', type: 'number' as const },
  { key: 'birthDate', label: 'تاریخ تولد', type: 'date' as const },
  { key: 'nationalId', label: 'کد ملی', type: 'text' as const },
  { key: 'idCardNumber', label: 'شماره شناسنامه', type: 'text' as const },
  { key: 'education', label: 'تحصیلات', type: 'text' as const },
  { key: 'caretaker', label: 'قیم', type: 'text' as const },
  { key: 'supervisor', label: 'مسئول', type: 'text' as const },
  { key: 'address', label: 'آدرس', type: 'text' as const, fullWidth: true },
  { key: 'phone', label: 'شماره تماس', type: 'text' as const }
])

// Sample data for father
const fatherData = ref({
  name: 'احمد شریفیان',
  fatherName: 'محمد',
  nationalId: '0012345678',
  birthDate: '1355-06-20',
  idCardNumber: '123456789',
  education: 'دیپلم',
  occupation: 'راننده',
  deathCause: 'تصادف رانندگی',
  numberOfWives: 1,
  deathDate: '1400-05-20',
  religion: 'شیعه',
  marriageDate: '1375-08-15'
})

// Field configuration for father
const fatherFields = ref([
  { key: 'name', label: 'نام و نام خانوادگی', type: 'text' as const },
  { key: 'fatherName', label: 'نام پدر', type: 'text' as const },
  { key: 'nationalId', label: 'کد ملی', type: 'text' as const },
  { key: 'birthDate', label: 'تاریخ تولد', type: 'date' as const },
  { key: 'idCardNumber', label: 'شماره شناسنامه', type: 'text' as const },
  { key: 'education', label: 'تحصیلات', type: 'text' as const },
  { key: 'occupation', label: 'شغل در زمان حیات', type: 'text' as const },
  { key: 'deathCause', label: 'علت فوت', type: 'text' as const },
  { key: 'numberOfWives', label: 'تعداد همسر', type: 'number' as const },
  { key: 'deathDate', label: 'تاریخ فوت', type: 'date' as const },
  { key: 'religion', label: 'مذهب', type: 'text' as const },
  { key: 'marriageDate', label: 'تاریخ عقد', type: 'date' as const }
])

// Sample data for mother
const motherData = ref({
  name: 'فاطمه محمدی',
  fatherName: 'علی',
  nationalId: '0012345679',
  birthDate: '1360-03-10',
  idCardNumber: '123456790',
  education: 'دیپلم',
  occupation: 'خانه‌دار',
  deathCause: '',
  numberOfHusbands: 1,
  deathDate: '',
  religion: 'شیعه',
  marriageDate: '1375-08-15',
  physicalCondition: 'سالم',
  moralStatus: 'خوب',
  mentalStatus: 'خوب',
  remarriageStatus: 'انجام داده',
  intentionToRemarry: 'خیر'
})

// Field configuration for mother
const motherFields = ref([
  { key: 'name', label: 'نام و نام خانوادگی', type: 'text' as const },
  { key: 'fatherName', label: 'نام پدر', type: 'text' as const },
  { key: 'nationalId', label: 'کد ملی', type: 'text' as const },
  { key: 'birthDate', label: 'تاریخ تولد', type: 'date' as const },
  { key: 'idCardNumber', label: 'شماره شناسنامه', type: 'text' as const },
  { key: 'education', label: 'تحصیلات', type: 'text' as const },
  { key: 'occupation', label: 'شغل در زمان حیات', type: 'text' as const },
  { key: 'deathCause', label: 'علت فوت', type: 'text' as const },
  { key: 'numberOfHusbands', label: 'تعداد همسر', type: 'number' as const },
  { key: 'deathDate', label: 'تاریخ فوت', type: 'date' as const },
  { key: 'religion', label: 'مذهب', type: 'text' as const },
  { key: 'marriageDate', label: 'تاریخ عقد', type: 'date' as const },
  { key: 'physicalCondition', label: 'وضعیت جسمانی', type: 'text' as const },
  { key: 'moralStatus', label: 'وضعیت اخلاقی و رفتاری', type: 'text' as const },
  { key: 'mentalStatus', label: 'وضعیت روحی و روانی', type: 'text' as const },
  { key: 'remarriageStatus', label: 'ازدواج مجدد', type: 'select' as const, options: [
    { value: 'انجام داده', label: 'انجام داده' },
    { value: 'انجام نداده', label: 'انجام نداده' }
  ]},
  { key: 'intentionToRemarry', label: 'آیا قصد ازدواج مجدد دارید؟', type: 'select' as const, options: [
    { value: 'بلی', label: 'بلی' },
    { value: 'خیر', label: 'خیر' }
  ]}
])

// Sample data for mother's husband
const motherHusbandData = ref({
  name: 'محمد کریمی',
  fatherName: 'حسین',
  nationalId: '0012345680',
  birthDate: '1358-12-05',
  idCardNumber: '123456791',
  education: 'کارشناسی',
  occupation: 'کارمند',
  nationality: 'ایرانی',
  numberOfWives: 1,
  religion: 'شیعه',
  marriageDate: '1401-08-15'
})

// Field configuration for husband
const husbandFields = ref([
  { key: 'name', label: 'نام و نام خانوادگی', type: 'text' as const },
  { key: 'fatherName', label: 'نام پدر', type: 'text' as const },
  { key: 'nationalId', label: 'کد ملی', type: 'text' as const },
  { key: 'birthDate', label: 'تاریخ تولد', type: 'date' as const },
  { key: 'idCardNumber', label: 'شماره شناسنامه', type: 'text' as const },
  { key: 'education', label: 'تحصیلات', type: 'text' as const },
  { key: 'occupation', label: 'شغل', type: 'text' as const },
  { key: 'nationality', label: 'ملیت', type: 'text' as const },
  { key: 'numberOfWives', label: 'تعداد همسر', type: 'number' as const },
  { key: 'religion', label: 'مذهب', type: 'text' as const },
  { key: 'marriageDate', label: 'تاریخ عقد', type: 'date' as const }
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

const tabs = ref([
  { key: 'personal', title: 'اطلاعات شخصی', slot: 'personal' },
  { key: 'visits', title: 'بازدیدها', slot: 'visits' },
  { key: 'monetary', title: 'کمک‌های مالی', slot: 'monetary' },
  { key: 'nonmonetary', title: 'کمک‌های غیرمالی', slot: 'nonmonetary' },
])

// Sample data for non-monetary help
const nonMonetaryData = ref([
  {
    id: 1,
    date: '1404-07-20',
    donor: 'موسسه خیریه',
    items: [
      { name: 'کیف مدرسه', count: 1 },
      { name: 'دفتر 100 برگ', count: 4 },
      { name: 'مداد', count: 10 },
    ],
    note: 'بسته لوازم‌التحریر مهر'
  },
  {
    id: 2,
    date: '1404-08-03',
    donor: 'خیرین محلی',
    items: [
      { name: 'لباس زمستانی', count: 2 },
      { name: 'کفش', count: 1 }
    ]
  }
])

// component name for linter (multi-word)
// eslint-disable-next-line vue/require-name-property
defineOptions({ name: 'OrphanShowPage' })
  </script>
  