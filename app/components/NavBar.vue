<script setup lang="ts">
import type { NavigationMenuItem } from '@nuxt/ui'

const route = useRoute()

const items = computed<NavigationMenuItem[]>(() => [
  {
    label: 'Book',
    to: 'book',
    // active: route.path.startsWith('/docs/getting-started')
  },
  {
    label: 'My Bookings',
    to: 'mybookings',
    // active: route.path.startsWith('/docs/components')
  },
])
</script>

<template>
  <UHeader>
    <template #title>
      <Logo class="h-6 w-auto" />
    </template>

    <UNavigationMenu :items="items" />

    <template #right>
      <UColorModeButton />
        
      
    <UButton
        label="Logout"
        color="neutral"
        variant="outline"
        @click=""
         class="bg-gray-100 text-gray-700 transition-colors duration-200 hover:bg-blue-600 hover:text-white cursor-pointer"
    />
      
    </template>
  </UHeader>
</template>