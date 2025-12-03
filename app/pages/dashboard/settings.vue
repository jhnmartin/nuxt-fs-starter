<script lang="ts" setup>
import type { NavigationMenuItem } from '@nuxt/ui';

definePageMeta({
  layout: 'dashboard',
  title: 'Settings',
  description: 'Settings',
  toolbarLinks: [
    {
      label: 'General',
      to: '/dashboard/settings',
      exact: true
    },
    {
      label: 'Members',
      to: '/dashboard/settings/members'
    },
    {
      label: 'Notifications',
      to: '/dashboard/settings/notifications'
    },
    {
      label: 'Security',
      to: '/dashboard/settings/security'
    }
  ] satisfies NavigationMenuItem[]
});
</script>

<template>
  <div>
    <NuxtPage />
  </div>
</template>
