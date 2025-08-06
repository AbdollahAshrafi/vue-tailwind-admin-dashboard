<template>
  <div dir="rtl">
    <ThemeProvider>
      <SidebarProvider>
        <RouterView />
      </SidebarProvider>
    </ThemeProvider>
  </div>
</template>

<script setup lang="ts">
import ThemeProvider from './components/layout/ThemeProvider.vue'
import SidebarProvider from './components/layout/SidebarProvider.vue'
</script>
