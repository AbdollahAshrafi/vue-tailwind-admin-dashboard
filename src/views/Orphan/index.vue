<template>
    <AdminLayout>
      <PageBreadcrumb :pageTitle="currentPageTitle" />
      <div class="space-y-5 sm:space-y-6">
        <!-- <ComponentCard title="لیست ایتام"> -->
          <OrphansTable />
        <!-- </ComponentCard> -->
      </div>
    </AdminLayout>
  </template>
  
  <script setup>
  import { ref } from "vue";
  import PageBreadcrumb from "@/components/common/PageBreadcrumb.vue";
  import AdminLayout from "@/components/layout/AdminLayout.vue";
  import ComponentCard from "@/components/common/ComponentCard.vue";
  import OrphansTable from "@/components/tables/basic-tables/OrphansTable.vue";
  const currentPageTitle = ref("لیست ایتام");
  </script>
  