<template>
  <div>
    <h2 class="text-xl font-bold mb-4">Example Usage of FlexiblePersonalInfoCard</h2>
    
    <!-- Example 1: Basic person info -->
    <FlexiblePersonalInfoCard 
      title="اطلاعات شخصی"
      :data="basicPersonData"
      :fields="basicPersonFields"
      @update="updateBasicPerson"
    />
    
    <!-- Example 2: Student with custom fields -->
    <FlexiblePersonalInfoCard 
      title="اطلاعات دانش‌آموز"
      :data="studentData"
      :fields="studentFields"
      @update="updateStudent"
    />
    
    <!-- Example 3: Employee with complex fields -->
    <FlexiblePersonalInfoCard 
      title="اطلاعات کارمند"
      :data="employeeData"
      :fields="employeeFields"
      @update="updateEmployee"
    />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import FlexiblePersonalInfoCard from './FlexiblePersonalInfoCard.vue'

// Example 1: Basic person
const basicPersonData = ref({
  name: 'علی احمدی',
  age: 25,
  phone: '09123456789',
  email: 'ali@example.com'
})

const basicPersonFields = ref([
  { key: 'name', label: 'نام', type: 'text' as const },
  { key: 'age', label: 'سن', type: 'number' as const },
  { key: 'phone', label: 'تلفن', type: 'text' as const },
  { key: 'email', label: 'ایمیل', type: 'text' as const }
])

// Example 2: Student with custom fields
const studentData = ref({
  name: 'فاطمه کریمی',
  studentId: 'ST-2024-001',
  grade: 'دهم',
  school: 'دبیرستان نمونه دولتی',
  parentPhone: '09123456789',
  address: 'تهران، خیابان ولیعصر'
})

const studentFields = ref([
  { key: 'name', label: 'نام دانش‌آموز', type: 'text' as const },
  { key: 'studentId', label: 'شماره دانش‌آموزی', type: 'text' as const },
  { key: 'grade', label: 'پایه تحصیلی', type: 'select' as const, options: [
    { value: 'هفتم', label: 'هفتم' },
    { value: 'هشتم', label: 'هشتم' },
    { value: 'نهم', label: 'نهم' },
    { value: 'دهم', label: 'دهم' },
    { value: 'یازدهم', label: 'یازدهم' },
    { value: 'دوازدهم', label: 'دوازدهم' }
  ]},
  { key: 'school', label: 'مدرسه', type: 'text' as const },
  { key: 'parentPhone', label: 'تلفن والدین', type: 'text' as const },
  { key: 'address', label: 'آدرس', type: 'textarea' as const, fullWidth: true, rows: 2 }
])

// Example 3: Employee with complex fields
const employeeData = ref({
  name: 'محمد رضایی',
  employeeId: 'EMP-2024-001',
  department: 'فنی و مهندسی',
  position: 'برنامه‌نویس ارشد',
  salary: 15000000,
  hireDate: '1400-03-15',
  status: 'فعال',
  notes: 'کارمند نمونه ماه'
})

const employeeFields = ref([
  { key: 'name', label: 'نام کارمند', type: 'text' as const },
  { key: 'employeeId', label: 'شماره کارمندی', type: 'text' as const },
  { key: 'department', label: 'دپارتمان', type: 'select' as const, options: [
    { value: 'فنی و مهندسی', label: 'فنی و مهندسی' },
    { value: 'مالی', label: 'مالی' },
    { value: 'فروش', label: 'فروش' },
    { value: 'پشتیبانی', label: 'پشتیبانی' }
  ]},
  { key: 'position', label: 'سمت', type: 'text' as const },
  { key: 'salary', label: 'حقوق', type: 'amount' as const },
  { key: 'hireDate', label: 'تاریخ استخدام', type: 'date' as const },
  { key: 'status', label: 'وضعیت', type: 'status' as const },
  { key: 'notes', label: 'یادداشت', type: 'textarea' as const, fullWidth: true, rows: 3 }
])

// Update handlers
const updateBasicPerson = (data: Record<string, string | number>) => {
  Object.assign(basicPersonData.value, data)
  console.log('Basic person updated:', data)
}

const updateStudent = (data: Record<string, string | number>) => {
  Object.assign(studentData.value, data)
  console.log('Student updated:', data)
}

const updateEmployee = (data: Record<string, string | number>) => {
  Object.assign(employeeData.value, data)
  console.log('Employee updated:', data)
}
</script> 